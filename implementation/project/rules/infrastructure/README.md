# Infrastructure Rules Overview

This directory contains Terraform/AWS patterns and best practices that Claude automatically applies when working on infrastructure tickets.

## Files

- **`terraform.md`** - Quick reference for Terraform basics and safety checks
- **`terraform-patterns.md`** - Comprehensive AWS/Terraform patterns (naming, tagging, modules, API Gateway, etc.)
- **`secrets-management.md`** - Flow A (ephemeral) and Flow B (external) secrets patterns
- **`workflow-examples.md`** - Real-world examples showing patterns in action
- **`style-verification.md`** - Terraform style guide verification checklist (HashiCorp style guide + terraform-skill)

## How It Works

When you use `/start-ticket <number>` on an infrastructure ticket, Claude automatically:

1. **Reads these pattern files** to understand conventions
2. **Checks existing AWS resources** before making changes
3. **Verifies Terraform state backend** is accessible
4. **Applies patterns automatically** when generating code:
   - **Favors official Terraform Registry modules** over custom code
   - Uses naming module: `{project}-{environment}-{component}`
   - Applies tagging module to all resources
   - Uses Flow A for DB secrets, Flow B for API keys
   - Follows least-privilege IAM patterns
   - Uses declarative API Gateway routes
   - Includes observability (CloudWatch, X-Ray, Slack)

5. **Validates patterns** during `/verify`:
   - Verifies official Terraform Registry modules are used when available
   - Checks naming conventions
   - Verifies tagging usage
   - Reviews secrets flow (A vs B)
   - Validates IAM policies
   - Ensures style guide compliance (block ordering, naming, descriptions)
   - Runs `terraform fmt` and `terraform validate`

6. **Documents in PR** when using `/raise-pr`:
   - Notes which patterns were used
   - Documents secret seeding steps (if Flow B)
   - Includes infrastructure checklist

## Quick Reference

### Module Selection

**Always favor official Terraform Registry modules:**

```hcl
# ✅ GOOD - Use terraform-aws-modules
module "s3_bucket" {
  source  = "terraform-aws-modules/s3-bucket/aws"
  version = "~> 5.0"
  
  bucket = var.bucket_name
}

# ❌ BAD - Don't write custom code when registry module exists
resource "aws_s3_bucket" "custom" {
  # Avoid unless no suitable registry module exists
}
```

**Priority:** Registry modules → Community modules → Custom modules (only when necessary)

### Naming
```hcl
module "naming" {
  source = "./modules/naming"
  project     = var.project
  environment = var.environment
}

# Use: module.naming.api_lambda_health → "platform-dev-api-health"
```

### Tagging
```hcl
module "tagging" {
  source = "./modules/tagging"
  project         = var.project
  environment     = var.environment
  repository      = var.repository
  additional_tags = var.tags
}

# Use: tags = module.tagging.tags
```

### Secrets - Flow A (Ephemeral)
```hcl
resource "random_password" "db_password" {
  length = 16
}

resource "aws_secretsmanager_secret" "db_app" {
  name = "/${var.environment}/${var.project}/db-app"
  tags = merge(module.tagging.tags, { flow = "A" })
}
```

### Secrets - Flow B (External)
```hcl
resource "aws_secretsmanager_secret" "stripe_api" {
  name = "/${var.environment}/${var.project}/stripe-api"
  tags = merge(module.tagging.tags, { flow = "B" })
}

# Seed externally: export STRIPE_API_KEY='...' && node scripts/secrets.js seed
```

### API Gateway Routes
```hcl
variable "api_routes" {
  type = map(object({
    method     = string
    path       = string
    lambda_key = string
  }))
  default = {
    "health" = {
      method     = "GET"
      path       = "/api/health"
      lambda_key = "health"
    }
  }
}
```

## Style Guide Compliance

All Terraform code follows:
- **HashiCorp Terraform Style Guide** - Official HashiCorp conventions
- **terraform-skill best practices** - Testing, modules, CI/CD patterns

**Verification checklist:** See `.claude/rules/infrastructure/style-verification.md`

**Key requirements:**
- Two spaces indentation
- Block ordering: `count`/`for_each` first, `tags` last, `lifecycle` last
- Resource names: lowercase with underscores, descriptive nouns
- Variables/outputs: Always include `type` and `description`
- Prefer `for_each` over `count` for stable addressing

## See Also

- `.claude/WORKFLOWS.md` - How workflows integrate these patterns
- `.claude/rules/common/git-workflow.md` - Git conventions
- `.claude/rules/code-style.md` - Code style guidelines
- `.claude/rules/infrastructure/style-verification.md` - Style guide checklist
