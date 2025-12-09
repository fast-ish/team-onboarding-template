# Decision Guide

How to choose the right options when onboarding your team.

## Team Name

| Choose | When |
|--------|------|
| **Short, descriptive** | `payments`, `search`, `auth` |
| **With domain prefix** | `platform-core`, `ml-inference` |

Rules:
- Lowercase letters, numbers, hyphens only
- Must start with a letter
- Keep it short (used in namespace names)

## Team Alias

2-4 character code used for:
- Service naming prefixes
- Cost allocation
- Metric tagging

Examples: `pay`, `srch`, `auth`, `plat`

## Cost Center

Required for billing. Format: `XX-0000`

- First two letters: Department code
- Last four digits: Team/project code

Contact Finance if you don't have a cost center.

## Resource Tier

| Choose | When |
|--------|------|
| **Starter** | New team, POC projects, small workloads |
| **Standard** | Most production teams (default) |
| **Large** | High-traffic APIs, data processing |
| **Enterprise** | Mission-critical, high-scale systems |

You can request tier upgrades later via platform team.

## Integrations

### Grafana

| Choose | When |
|--------|------|
| **Enable** | Production services, need observability (recommended) |
| **Disable** | Internal tools, cost-sensitive projects |

### AWS Access

| Choose | When |
|--------|------|
| **Enable** | Need S3, SQS, DynamoDB, etc. (recommended) |
| **Disable** | Kubernetes-only workloads |

## Environments

| Environment | Purpose |
|-------------|---------|
| **dev** | Always included, local testing |
| **staging** | Pre-production testing |
| **production** | Live traffic |

Most teams should include all environments.

## Team Members

- **Team Lead**: Primary contact, has admin access
- **Team Members**: All get developer role

Add members later via PR to GitOps repo.

## Quick Recommendations

### New Product Team
- Tier: Standard
- Grafana: Enable
- AWS: Enable
- Environments: All

### Platform/Infra Team
- Tier: Large
- Grafana: Enable
- AWS: Enable
- Environments: All

### Internal Tools Team
- Tier: Starter
- Grafana: Disable
- AWS: Disable
- Environments: dev, staging

### Data/ML Team
- Tier: Large or Enterprise
- Grafana: Enable
- AWS: Enable
- Environments: All
