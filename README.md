# Team Onboarding Template

> Self-service team onboarding via Backstage.

[![Backstage](https://img.shields.io/badge/Backstage-Template-blue)](https://backstage.io)
[![Platform](https://img.shields.io/badge/Type-Team%20Onboarding-green)]()

## What's Included

| Category | Resources |
|----------|-----------|
| **Kubernetes** | Namespace, ResourceQuota, LimitRange, NetworkPolicy |
| **RBAC** | Developer role, Viewer role, RoleBindings |
| **ArgoCD** | AppProject with team-scoped permissions |
| **Secrets** | ExternalSecrets SecretStores (Secrets Manager, Parameter Store) |
| **Service Account** | Workload SA with IRSA |
| **Backstage** | Team entity in catalog |
| **GitHub** | Team with repo access |
| **ECR** | Container registry with lifecycle policy |
| **Observability** | Grafana team configuration |

## Quick Start

1. Go to [Backstage Software Catalog](https://backstage.yourcompany.com/create)
2. Select "Onboard New Team"
3. Fill in team details
4. Submit and approve the PR

## Template Options

| Parameter | Description |
|-----------|-------------|
| `teamName` | Full team name (lowercase, alphanumeric, hyphens) |
| `teamAlias` | Short alias (2-4 characters) |
| `teamDescription` | Brief description |
| `costCenter` | Cost center code (XX-0000) |
| `slackChannel` | Team Slack channel |
| `oncallSchedule` | PagerDuty schedule ID |

### Resource Tiers

| Tier | CPU | Memory | Pods |
|------|-----|--------|------|
| Starter | 4 | 8Gi | 20 |
| Standard | 10 | 20Gi | 50 |
| Large | 20 | 40Gi | 100 |
| Enterprise | 40 | 80Gi | 200 |

### Integrations

| Integration | Default | Description |
|-------------|---------|-------------|
| **Grafana** | Yes | Team dashboards and alerts |
| **AWS Access** | Yes | IAM role for IRSA |
| **GitHub** | Yes | Team with repo access |
| **ECR** | Yes | Container registry |

## What Gets Created

```
teams/{teamName}/
├── namespace.yaml          # Namespace with pod security standards
├── resource-quota.yaml     # ResourceQuota based on tier
├── limit-range.yaml        # LimitRange with container defaults
├── network-policy.yaml     # NetworkPolicy for namespace isolation
├── rbac.yaml               # Roles and RoleBindings
├── service-account.yaml    # Workload SA with IRSA
├── argocd-project.yaml     # ArgoCD AppProject
├── external-secrets.yaml   # SecretStores (Secrets Manager, Parameter Store)
├── backstage.yaml          # Backstage team entity
├── grafana.yaml            # Grafana config
├── github.yaml             # GitHub team config (if enabled)
└── ecr.yaml                # ECR config (if enabled)
```

## Documentation

| Document | Description |
|----------|-------------|
| [Decision Guide](./docs/DECISIONS.md) | How to choose options |
| [Team Guide](./docs/index.md) | What teams get |

## Support

- **Slack**: #platform-help
- **Office Hours**: Thursdays 2-3pm

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2025-12 | Initial release |
