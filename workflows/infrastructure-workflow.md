# Infrastructure Workflow

How to safely make infrastructure changes with Terraform.

## Prerequisites

Before making infrastructure changes:

1. **Check existing AWS resources** for the target environment
   ```bash
   aws rds describe-db-instances --query 'DBInstances[*].DBInstanceIdentifier'
   aws ec2 describe-vpcs --query 'Vpcs[*].{ID:VpcId,Name:Tags[?Key==`Name`].Value|[0]}'
   ```

2. **Verify Terraform S3 state backend** is accessible and working correctly
   ```bash
   terraform init -backend-config=environments/dev/dev.s3.tfbackend
   terraform state list
   ```

3. **Ensure no resources will be accidentally destroyed or duplicated**
   ```bash
   terraform plan -var-file=environments/dev/terraform.tfvars
   # Review plan carefully for destroy/create operations
   ```

## Terraform Patterns

### Module Selection

**Always favor official Terraform Registry modules:**

```hcl
# ✅ GOOD
module "s3_bucket" {
  source  = "terraform-aws-modules/s3-bucket/aws"
  version = "~> 5.0"
}

# ❌ BAD - Custom code when registry module exists
resource "aws_s3_bucket" "custom" {
  # Avoid unless necessary
}
```

### Naming Convention

**Pattern:** `{project}-{environment}-{component}`

```hcl
prefix = "${var.project}-${var.environment}"
name = "${local.prefix}-api-health"
```

### Style Guide Compliance

- Run `terraform fmt -recursive`
- Run `terraform validate`
- Check block ordering (count/for_each first, tags last, lifecycle last)
- Verify naming conventions (lowercase with underscores)
- Ensure all variables/outputs have descriptions
- Prefer `for_each` over `count` when appropriate

## Verification Checklist

Before applying infrastructure changes:

- [ ] Existing AWS resources checked
- [ ] Terraform S3 state backend verified
- [ ] Terraform plan reviewed (no accidental destruction/duplication)
- [ ] Official Terraform Registry modules used when available
- [ ] Naming convention followed: `{project}-{environment}-{component}`
- [ ] Tagging module applied to all resources
- [ ] Secrets use Flow A (ephemeral) or Flow B (external) appropriately
- [ ] IAM policies follow least-privilege
- [ ] Code formatted with `terraform fmt`
- [ ] Configuration validated with `terraform validate`
