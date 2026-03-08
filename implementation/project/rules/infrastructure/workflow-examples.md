# Terraform Patterns in Workflows - Examples

This document shows how Terraform/AWS patterns are automatically applied when working on tickets.

## Example 1: Adding a New API Endpoint

**Ticket:** "Add user profile update endpoint"

### What Claude Will Do:

1. **During `/start-ticket 123`:**
   - Checks existing AWS resources (API Gateway, Lambdas)
   - Verifies Terraform state backend
   - Identifies that this needs a new Lambda function

2. **Creates Terraform code following patterns:**

```hcl
# Uses naming module
module "lambda_api" {
  lambda_names = {
    # ... existing lambdas
    profile = module.naming.api_lambda_profile  # platform-dev-api-profile
  }
}

# Uses tagging module
tags = module.tagging.tags  # Applies Project, Environment, Repository, ManagedBy

# Adds to declarative API routes
variable "api_routes" {
  default = {
    # ... existing routes
    "profile_update" = {
      method     = "PATCH"
      path       = "/api/profile"
      lambda_key = "profile"
    }
  }
}

# IAM policy follows least-privilege
resource "aws_iam_role_policy" "profile_lambda_secrets" {
  policy = jsonencode({
    Statement = [{
      Action   = ["secretsmanager:GetSecretValue"]
      Resource = [var.db_secret_arn]  # Only this specific secret
    }]
  })
}
```

3. **Commit message:**
   ```
   feat: add user profile update endpoint
   ```

4. **PR description:**
   - Simple description following template
   - References ticket number
   - Includes platform checkbox

---

## Example 2: Adding a Third-Party API Integration

**Ticket:** "Integrate Stripe payment API"

### What Claude Will Do:

1. **Identifies this needs Flow B secret (external API key)**

2. **Creates Flow B secret shell:**

```hcl
# Terraform creates empty shell
resource "aws_secretsmanager_secret" "stripe_api" {
  name = "/${var.environment}/${var.project}/stripe-api"
  description = "Stripe API credentials (Flow B - seeded externally)"
  
  tags = merge(
    module.tagging.tags,
    {
      flow    = "B"  # Marked as externally-seeded
      purpose = "payment-integration"
    }
  )
}

# Note: No secret_version - value seeded externally
```

3. **Adds IAM permission for Lambda:**

```hcl
resource "aws_iam_role_policy" "api_lambda_stripe_secret" {
  policy = jsonencode({
    Statement = [{
      Action   = ["secretsmanager:GetSecretValue"]
      Resource = [aws_secretsmanager_secret.stripe_api.arn]  # Specific ARN only
    }]
  })
}
```

4. **In Lambda environment:**

```hcl
environment {
  variables = {
    STRIPE_SECRET_ARN = aws_secretsmanager_secret.stripe_api.arn  # ARN, not value
  }
}
```

5. **Documents seeding step:**

```bash
# After terraform apply, seed the secret:
export STRIPE_API_KEY='{"api_key":"sk_test_..."}'
node scripts/secrets.js seed
```

---

## Example 3: Adding Database Migration Support

**Ticket:** "Add database migration task for Prisma"

### What Claude Will Do:

1. **Uses Flow A for migration credentials:**

```hcl
# Ephemeral password generation
resource "random_password" "migration_password" {
  length  = 16
  special = true
}

# Store in Secrets Manager (Flow A)
resource "aws_secretsmanager_secret" "db_migration" {
  name = "/${var.environment}/${var.project}/db-migration"
  tags = merge(module.tagging.tags, { flow = "A" })
}

resource "aws_secretsmanager_secret_version" "db_migration" {
  secret_id = aws_secretsmanager_secret.db_migration.id
  secret_string = jsonencode({
    url      = "postgresql://migration_user:${random_password.migration_password.result}@${db_endpoint}:5432/platform"
    username = "migration_user"
    password = random_password.migration_password.result
    host     = db_endpoint
    port     = 5432
    dbname   = "platform"
  })
}
```

2. **ECS task uses valueFrom pattern:**

```hcl
secrets = [
  {
    name      = "DATABASE_URL"
    valueFrom = "${aws_secretsmanager_secret.db_migration.arn}:url::"
  }
]
```

3. **IAM policy for ECS task:**

```hcl
{
  Action   = ["secretsmanager:GetSecretValue"]
  Resource = [aws_secretsmanager_secret.db_migration.arn]  # Only migration secret
}
```

---

## Example 4: Adding a New SQS Queue

**Ticket:** "Add queue for email notifications"

### What Claude Will Do:

1. **Follows naming convention:**

```hcl
module "sqs_email" {
  source = "./modules/sqs"
  
  queue_name = module.naming.sqs_email_notifications  # platform-dev-email-notifications
  tags       = module.tagging.tags  # Standard tags applied
}
```

2. **Adds DLQ (Dead Letter Queue):**

```hcl
module "sqs_email" {
  # ... queue config
  create_dlq = true
  dlq_name   = module.naming.sqs_email_notifications_dlq
}
```

3. **Lambda IAM policy:**

```hcl
resource "aws_iam_role_policy" "lambda_sqs" {
  policy = jsonencode({
    Statement = [{
      Action = [
        "sqs:SendMessage",
        "sqs:ReceiveMessage",
        "sqs:DeleteMessage"
      ]
      Resource = [module.sqs_email.queue_arn]  # Specific queue only
    }]
  })
}
```

---

## Example 5: Adding CloudWatch Alarms

**Ticket:** "Add monitoring for API errors"

### What Claude Will Do:

1. **Uses monitoring module pattern:**

```hcl
module "monitoring" {
  source = "./modules/monitoring"
  
  api_gateway_id = module.apigateway.api_id
  
  # Slack integration
  slack_workspace_id = var.slack_workspace_id
  slack_channel_id   = var.slack_channel_id
  
  # Alarm thresholds
  api_5xx_threshold = 10
  api_4xx_threshold = 100
  api_latency_threshold = 5000
  
  tags = module.tagging.tags
}
```

2. **Follows observability pattern:**
   - CloudWatch alarms for 5xx/4xx errors
   - Latency monitoring
   - Slack notifications via AWS Chatbot
   - X-Ray tracing enabled on Lambdas

---

## Example 6: Adding VPC Endpoint for Private Access

**Ticket:** "Move Lambda to VPC for RDS access"

### What Claude Will Do:

1. **Adds VPC endpoint:**

```hcl
module "vpc" {
  vpc_endpoints = {
    secretsmanager = {
      service             = "secretsmanager"
      private_dns_enabled = true  # Auto-creates Route53 private zone
      tags                = { Name = "${module.naming.vpc}-secretsmanager" }
    }
  }
}
```

2. **Lambda VPC config:**

```hcl
module "lambda_api" {
  vpc_config = {
    subnet_ids         = module.vpc.private_subnets
    security_group_ids = [module.vpc.security_group_lambda_id]
  }
}
```

3. **Security group follows least-privilege:**

```hcl
resource "aws_security_group_rule" "lambda_to_rds" {
  type                     = "egress"
  from_port                = 5432
  to_port                  = 5432
  protocol                 = "tcp"
  source_security_group_id = module.vpc.security_group_rds_id  # Only RDS SG
}
```

---

## Automatic Pattern Application Checklist

When Claude works on infrastructure tickets, it will automatically:

### ✅ Naming
- [ ] Use `module.naming.*` for all resource names
- [ ] Follow `{project}-{environment}-{component}` pattern
- [ ] Never hardcode resource names

### ✅ Tagging
- [ ] Apply `module.tagging.tags` to all resources
- [ ] Add purpose-specific tags (e.g., `flow = "A"` or `flow = "B"`)

### ✅ Secrets
- [ ] Use Flow A (ephemeral) for database credentials
- [ ] Use Flow B (external) for third-party API keys
- [ ] Follow naming: `/{environment}/{project}/{purpose}`
- [ ] Mark secret ARNs as `sensitive = true` in outputs

### ✅ IAM
- [ ] Least-privilege policies (specific ARNs only)
- [ ] Separate policies per service/resource
- [ ] No wildcard permissions

### ✅ API Gateway
- [ ] Use declarative `api_routes` variable
- [ ] Use `terraform-aws-modules/apigateway-v2/aws`
- [ ] Map routes to Lambda integrations

### ✅ Safety
- [ ] Check existing AWS resources before changes
- [ ] Verify Terraform state backend
- [ ] Review plan for accidental destroys
- [ ] Use `force_destroy = false` for S3 buckets

### ✅ Observability
- [ ] Enable X-Ray tracing on Lambdas
- [ ] Configure CloudWatch access logging
- [ ] Add CloudWatch alarms
- [ ] Set up Slack notifications

---

## Workflow Integration

### `/start-ticket 123` (Infrastructure Ticket)

**Automatic checks:**
1. ✅ Reads `.claude/rules/infrastructure/terraform-patterns.md`
2. ✅ Reads `.claude/rules/infrastructure/secrets-management.md`
3. ✅ Checks existing AWS resources
4. ✅ Verifies Terraform state backend
5. ✅ Applies patterns automatically in code generation

### `/verify` (Infrastructure Changes)

**Automatic validation:**
1. ✅ Runs `terraform validate`
2. ✅ Checks naming conventions
3. ✅ Verifies tagging module usage
4. ✅ Validates secrets flow (A vs B)
5. ✅ Reviews IAM policies for least-privilege
6. ✅ Checks for safety guardrails

### `/raise-pr` (Infrastructure PR)

**Automatic PR creation:**
1. ✅ Follows PR template
2. ✅ Includes infrastructure checklist
3. ✅ Documents secret seeding steps (if Flow B)
4. ✅ Notes Terraform plan review requirements
5. ✅ References patterns used

---

## Real-World Example Flow

**You:** `/start-ticket 45` (Add Stripe payment integration)

**Claude automatically:**
1. Fetches issue #45
2. Identifies infrastructure needs (new Lambda, API route, secret)
3. Checks existing AWS resources
4. Creates branch `feature/45-stripe-integration` from `develop`
5. Generates code following all patterns:
   - ✅ Naming: `platform-dev-api-payments`
   - ✅ Tagging: Standard tags + `flow = "B"` for Stripe secret
   - ✅ Flow B secret: `/dev/platform/stripe-api` (empty shell)
   - ✅ IAM: Least-privilege policy for Stripe secret ARN only
   - ✅ API route: Adds to declarative `api_routes`
   - ✅ Lambda: Environment variable with secret ARN (not value)
6. Documents seeding step: `export STRIPE_API_KEY='...' && node scripts/secrets.js seed`
7. Waits for your approval

**You:** Approve and continue

**Claude:**
- Commits with: `feat: add stripe payment integration`
- Runs `/verify` → validates Terraform patterns
- Runs `/raise-pr` → creates PR with simple description following template

**Result:** All Terraform code follows established patterns automatically!
