# Team Onboarding Template

Backstage template for onboarding new teams to the platform.

## Structure

```
/template.yaml          # Backstage scaffolder definition
/skeleton/              # Generated GitOps resources
/docs/                  # Template-level documentation
```

## Key Files

- `template.yaml` - Template parameters and steps (scaffolder.backstage.io/v1beta3)
- `skeleton/teams/${{values.teamName}}/` - Generated manifest files:
  - `namespace.yaml` - Namespace with pod security standards
  - `resource-quota.yaml` - ResourceQuota based on tier
  - `limit-range.yaml` - LimitRange with container defaults
  - `network-policy.yaml` - NetworkPolicy for namespace isolation
  - `rbac.yaml` - Roles and RoleBindings for team members
  - `service-account.yaml` - Workload SA with IRSA annotation
  - `argocd-project.yaml` - ArgoCD AppProject scoped to team
  - `external-secrets.yaml` - SecretStores for AWS Secrets Manager and Parameter Store
  - `backstage.yaml` - Backstage Group entity
  - `grafana.yaml` - Grafana config (always included)
  - `github.yaml` - GitHub team config (conditional)
  - `ecr.yaml` - ECR config (conditional)

## Template Syntax

Uses Jinja2 via Backstage:
- Variables: `${{ values.teamName }}`, `${{ values.teamAlias }}`
- Conditionals: `{%- if values.enableGrafana %}...{%- endif %}`
- Loops: `{%- for member in values.teamMembers %}...{%- endfor %}`

## What Gets Created

### Kubernetes Resources
- Namespace with pod security standards
- ResourceQuota based on tier (starter/standard/large/enterprise)
- LimitRange with container defaults
- NetworkPolicy for namespace isolation
- RBAC roles (developer, viewer)
- RoleBindings for team members
- ServiceAccount for workloads with IRSA

### ArgoCD
- AppProject scoped to team namespace
- Source repo restrictions (`{teamName}-*`)
- Resource whitelists and blacklists
- Developer and viewer roles

### Secrets Management
- SecretStore for AWS Secrets Manager
- SecretStore for AWS Parameter Store
- Uses workload service account for auth

### Backstage Catalog
- Group entity for the team
- Team members linked to users

### Integrations (Optional, via Terraform)
- Grafana team configuration
- GitHub team ConfigMap (Terraform reads this to create GitHub team)
- ECR repository ConfigMap (Terraform reads this to create ECR repo)
- ECR pull secret via ExternalSecrets

## Template Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| teamName | string | Lowercase team name |
| teamAlias | string | 2-4 character alias |
| teamDescription | string | Team purpose |
| costCenter | string | Billing code (XX-0000) |
| tier | enum | Resource tier |
| teamLead | string | GitHub username |
| teamMembers | array | GitHub usernames |
| enableGrafana | boolean | Grafana integration |
| enableAwsAccess | boolean | IRSA for AWS |
| enableGitHub | boolean | GitHub team creation |
| enableEcr | boolean | ECR repository |

## Resource Tiers

| Tier | CPU | Memory | Pods |
|------|-----|--------|------|
| starter | 4 | 8Gi | 20 |
| standard | 10 | 20Gi | 50 |
| large | 20 | 40Gi | 100 |
| enterprise | 40 | 80Gi | 200 |

## Conventions

- Namespace: `team-{teamName}`
- Labels: `fasti.sh/team`, `fasti.sh/cost-center`
- Service account: `{teamName}-workload`
- Roles: `{teamName}-developer`, `{teamName}-viewer`

## GitOps Workflow

1. User fills form in Backstage
2. Template creates PR to `aws-idp-gitops` repo
3. PR reviewed and merged
4. ArgoCD syncs Kubernetes resources to cluster
5. Terraform reads ConfigMaps and provisions GitHub team/ECR repo
6. Team can start deploying services

## Terraform Integration

GitHub team and ECR repo are created via Terraform, not directly by Backstage.
The template creates ConfigMaps with `fasti.sh/managed-by: terraform` annotation.
Terraform watches for these ConfigMaps and provisions the corresponding resources.

## Don't

- Skip cost center (required for billing)
- Create custom quotas without justification
- Add external users to team members
