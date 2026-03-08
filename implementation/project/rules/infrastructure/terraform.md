# Infrastructure / Terraform

- Terraform ~1.14 targeting AWS
- State stored in S3 (separate bucket per environment: dev, staging, production)
- **Before making any infrastructure changes**, always:
  1. Check existing AWS resources for the target environment
  2. Verify Terraform S3 state backend is accessible and working correctly
  3. Confirm no resources will be accidentally destroyed or duplicated
- Always run `terraform plan` before `terraform apply`
- GitHub Actions uses OIDC — no long-lived AWS credentials stored anywhere
- Three environments: `dev`, `staging`, `production`

## Key modules
- `cognito/` — User and admin Cognito pools
- `s3/ + cloudfront/` — Website hosting with CDN
- `github-actions-iam/` — OIDC-based IAM roles for CI/CD
- `naming/ + tagging/` — Shared conventions applied across all resources

## CI/CD
- `terraform-plan.yml` — runs on PRs: validate → Trivy + Checkov scans → plan
- `terraform-apply.yml` — applies after plan approval

## Patterns and Best Practices

See `.claude/rules/infrastructure/terraform-patterns.md` for comprehensive AWS/Terraform patterns including:
- **Module selection** — favor official Terraform Registry modules over custom code
- Naming conventions (`{project}-{environment}-{component}`)
- Tagging module usage
- API Gateway declarative routes
- Secrets management (Flow A: ephemeral, Flow B: external seeding)
- IAM least-privilege patterns
- VPC endpoints for private access
- Observability (CloudWatch, X-Ray, Slack alerts)
- Safety guardrails and security patterns

## Style Guide Compliance

**All Terraform code follows HashiCorp's official style guide:**
- See `.claude/rules/infrastructure/style-verification.md` for verification checklist
- Code formatting: two spaces, proper block ordering
- Naming: lowercase with underscores, descriptive nouns
- Variables: Always include `type` and `description`
- Outputs: Always include `description`
- Prefer `for_each` over `count` for stable addressing

**Validation commands:**
```bash
terraform fmt -recursive  # Format code
terraform validate       # Validate syntax
```
