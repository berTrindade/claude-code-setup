# Terraform Style Verification Checklist

This checklist ensures all Terraform code follows HashiCorp's official style guide and terraform-skill best practices.

## File Organization

✅ **Required Files:**
- [ ] `versions.tf` - Terraform and provider version constraints
- [ ] `providers.tf` - Provider configurations
- [ ] `main.tf` - Primary resources and data sources
- [ ] `variables.tf` - Input variable declarations (alphabetical)
- [ ] `outputs.tf` - Output value declarations (alphabetical)
- [ ] `locals.tf` - Local value declarations (if needed)

✅ **Module Structure:**
```
modules/<module>/
├── README.md       # Usage documentation
├── main.tf         # Primary resources
├── variables.tf    # Input variables with descriptions
├── outputs.tf      # Output values
├── versions.tf     # Provider version constraints
└── examples/       # Module usage examples
```

## Code Formatting

### Indentation
- [ ] Use **two spaces** per nesting level (no tabs)
- [ ] Align equals signs for consecutive arguments

### Block Organization
- [ ] `count` or `for_each` FIRST (blank line after)
- [ ] Other arguments follow
- [ ] `tags` as last real argument
- [ ] `depends_on` after tags (if needed)
- [ ] `lifecycle` at the very end (if needed)

**Example:**
```hcl
resource "aws_instance" "web" {
  count = var.create_instance ? 1 : 0

  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = "subnet-12345678"

  tags = {
    Name        = "web-server"
    Environment = "production"
  }

  depends_on = [aws_vpc.main]

  lifecycle {
    create_before_destroy = true
  }
}
```

## Naming Conventions

### Resource Names
- [ ] Use **lowercase with underscores** for all names
- [ ] Use **descriptive nouns** excluding the resource type
- [ ] Be specific and meaningful
- [ ] Resource names must be **singular**, not plural
- [ ] Use `"this"` for singleton resources (only one of that type)

**Good Examples:**
```hcl
resource "aws_instance" "web_api" {}           # ✅ Descriptive
resource "aws_vpc" "main" {}                   # ✅ Singleton
resource "aws_security_group" "web_servers" {} # ✅ Descriptive plural OK for SG
```

**Bad Examples:**
```hcl
resource "aws_instance" "webAPI-aws-instance" {} # ❌ Mixed case, hyphens
resource "aws_instance" "web_apis" {}            # ❌ Plural (use singular)
resource "aws_vpc" "vpc" {}                      # ❌ Redundant type name
```

### Variable Names
- [ ] Prefix with context when needed
- [ ] Use descriptive names

**Good:**
```hcl
variable "vpc_cidr_block" {}          # ✅ Contextual
variable "database_instance_class" {} # ✅ Contextual
```

**Bad:**
```hcl
variable "cidr" {}           # ❌ Too generic
variable "instance_class" {} # ❌ Ambiguous
```

## Variables

### Required Elements
- [ ] Every variable has `type` and `description`
- [ ] Sensitive variables marked with `sensitive = true`
- [ ] Validation blocks for complex constraints
- [ ] Default values where appropriate

**Example:**
```hcl
variable "instance_type" {
  description = "EC2 instance type for the web server"
  type        = string
  default     = "t2.micro"

  validation {
    condition     = contains(["t2.micro", "t2.small", "t2.medium"], var.instance_type)
    error_message = "Instance type must be t2.micro, t2.small, or t2.medium."
  }
}

variable "database_password" {
  description = "Password for the database admin user"
  type        = string
  sensitive   = true
}
```

### Variable Block Ordering
1. `description` (ALWAYS required)
2. `type`
3. `default`
4. `validation`
5. `nullable` (when setting to false)
6. `sensitive` (if true)

## Outputs

### Required Elements
- [ ] Every output has `description`
- [ ] Sensitive outputs marked with `sensitive = true`

**Example:**
```hcl
output "instance_id" {
  description = "ID of the EC2 instance"
  value       = aws_instance.web.id
}

output "database_password" {
  description = "Database administrator password"
  value       = aws_db_instance.main.password
  sensitive   = true
}
```

## Dynamic Resource Creation

### Prefer `for_each` over `count`

**When to use `for_each`:**
- Items may be reordered/removed
- Reference by key
- Multiple named resources

**When to use `count`:**
- Boolean condition (create or don't)
- Simple numeric replication

**Good:**
```hcl
# ✅ for_each for stable addressing
resource "aws_subnet" "private" {
  for_each = toset(var.availability_zones)

  availability_zone = each.key
  cidr_block        = cidrsubnet(var.vpc_cidr, 8, index(var.availability_zones, each.key))
}

# ✅ count for boolean condition
resource "aws_nat_gateway" "this" {
  count = var.create_nat_gateway ? 1 : 0
  # ...
}
```

**Bad:**
```hcl
# ❌ count for items that may be removed
resource "aws_subnet" "private" {
  count = length(var.availability_zones)

  availability_zone = var.availability_zones[count.index]
  # Removing middle AZ recreates all subsequent subnets
}
```

## Version Management

### Version Constraints
- [ ] Terraform version pinned: `required_version = "~> 1.14"`
- [ ] Provider versions use `~>` for flexibility: `version = "~> 5.0"`
- [ ] Module versions pinned appropriately

**Example:**
```hcl
terraform {
  required_version = "~> 1.14"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"  # Allow minor updates
    }
  }
}
```

## Security Best Practices

- [ ] Enable encryption at rest by default
- [ ] Configure private networking where applicable
- [ ] Apply principle of least privilege for security groups
- [ ] Enable logging and monitoring
- [ ] Never hardcode credentials or secrets
- [ ] Mark sensitive outputs with `sensitive = true`
- [ ] Use Secrets Manager for credentials (Flow A/B patterns)

## Code Review Checklist

Before committing Terraform code:

- [ ] Code formatted with `terraform fmt -recursive`
- [ ] Configuration validated with `terraform validate`
- [ ] Files organized according to standard structure
- [ ] All variables have `type` and `description`
- [ ] All outputs have `description`
- [ ] Resource names use descriptive nouns with underscores
- [ ] Version constraints pinned explicitly
- [ ] Sensitive values marked with `sensitive = true`
- [ ] No hardcoded credentials or secrets
- [ ] Security best practices applied
- [ ] Block ordering follows style guide (count/for_each first, tags last, lifecycle last)
- [ ] Uses `for_each` instead of `count` when appropriate
- [ ] Follows naming convention: `{project}-{environment}-{component}`
- [ ] Applies tagging module to all resources
- [ ] Secrets follow Flow A (ephemeral) or Flow B (external) patterns

## Validation Commands

Run before committing:

```bash
# Format code
terraform fmt -recursive

# Validate syntax
terraform validate

# Security scanning (CI)
trivy config .
checkov -d .
```

## Integration with Workflows

When using `/start-ticket`, `/verify`, or `/raise-pr`:

1. **Automatic checks:**
   - Code formatting (`terraform fmt`)
   - Syntax validation (`terraform validate`)
   - Security scanning (Trivy, Checkov in CI)

2. **Pattern verification:**
   - Naming conventions followed
   - Tagging module used
   - Secrets flow (A/B) appropriate
   - IAM least-privilege
   - Block ordering correct

3. **Documentation:**
   - PR includes style compliance notes
   - Secret seeding steps documented (if Flow B)
   - Pattern references included
