# Terraform Patterns Best Practices

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

## Style Guide Compliance

All Terraform code follows:
- **HashiCorp Terraform Style Guide** (use `/terraform-style-guide` skill)
- **terraform-skill best practices** (use `/terraform-skill` skill)

**Using Claude Skills for Terraform:**

When working with Terraform code, Claude automatically uses:
- `/terraform-style-guide` - HashiCorp's official style conventions (indentation, naming, block ordering)
- `/terraform-skill` - Testing strategies, module patterns, CI/CD, security best practices

**Verification checklist:**
- Code formatted with `terraform fmt`
- Configuration validated with `terraform validate`
- Block ordering (count/for_each first, tags last, lifecycle last)
- Naming conventions (lowercase with underscores)
- Variable/output descriptions required
- Prefer `for_each` over `count` when appropriate
- Verify against `/terraform-style-guide` skill
- Verify against `/terraform-skill` skill

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
