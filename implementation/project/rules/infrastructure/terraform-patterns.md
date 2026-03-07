# Terraform AWS Patterns

Reference patterns and proven practices.

**Style Guide Compliance:** All code follows HashiCorp's official Terraform style guide and terraform-skill best practices.

**Claude Skills:** When working with Terraform code, Claude uses:
- `/terraform-style-guide` - HashiCorp's official style conventions
- `/terraform-skill` - Testing strategies, module patterns, CI/CD, security

## Module Selection Strategy

**Always favor official Terraform Registry modules over custom code.**

**Priority order:**
1. **Official Terraform Registry modules** (terraform-aws-modules, etc.)
2. **Community modules** (well-maintained, widely used)
3. **Custom modules** (only when no suitable registry module exists)

**Example:**

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

## Naming Convention

**Pattern:** `{project}-{environment}-{component}`

```hcl
prefix = "${var.project}-${var.environment}"
name = "${local.prefix}-api-health"
```

## Infrastructure Safety Checks

Before making infrastructure changes:

1. **Check existing AWS resources** for the target environment
2. **Verify Terraform S3 state backend** is accessible and working correctly
3. **Ensure no resources will be accidentally destroyed or duplicated**
4. **Review terraform plan** carefully before applying

## Secrets Management

- **Flow A (Ephemeral):** Terraform-generated secrets (e.g., DB passwords)
- **Flow B (External):** Externally-seeded secrets (e.g., API keys)

See `.claude/rules/infrastructure/secrets-management.md` for details.
