# Team Onboarding

## What is Team Onboarding?

Team onboarding is the process of setting up a new team on the platform with all the resources they need to start building and deploying services.

## What Teams Get

### Kubernetes Namespace

Each team gets a dedicated namespace with:

- **Pod Security Standards**: Baseline enforcement, restricted warnings
- **Resource Quotas**: CPU, memory, and pod limits based on tier
- **Limit Ranges**: Default container resource requests/limits
- **Network Policies**: Namespace isolation with controlled egress

### RBAC Roles

| Role | Permissions |
|------|-------------|
| **Developer** | Full access to workloads, services, configs, secrets |
| **Viewer** | Read-only access to resources |

### Service Account

A workload service account (`{team}-workload`) for:
- Running deployments and jobs
- AWS access via IRSA
- Pulling images from ECR

### ArgoCD AppProject

Each team gets an ArgoCD project that:
- Restricts deployments to team namespace only
- Allows sourcing from team repos (`{team}-*`)
- Whitelists common Kubernetes resources
- Blacklists resources that affect other teams (ResourceQuota, LimitRange)
- Provides developer and viewer roles

### Secrets Management

Teams get ExternalSecrets SecretStores for:
- **AWS Secrets Manager**: Store sensitive credentials
- **AWS Parameter Store**: Store configuration values

Access is scoped to `team/{teamName}/*` paths in AWS.

### Backstage Integration

- Team appears in Backstage catalog
- Team members linked to user profiles
- Ownership tracking for services

### GitHub Team (Optional)

When enabled, a ConfigMap is created that Terraform reads to provision:
- GitHub team in `fast-ish` org
- Team members added automatically
- Repo naming convention: `{team}-*`

*Note: Requires Terraform GitHub provider integration in the GitOps repo.*

### ECR Repository (Optional)

When enabled, a ConfigMap is created that Terraform reads to provision:
- ECR repository prefix: `team-{teamName}`
- Lifecycle policy keeps last 30 images
- Pull secret created via ExternalSecrets

*Note: Requires Terraform AWS provider integration in the GitOps repo.*

### Observability (Optional)

When Grafana integration is enabled:
- Team tag for all services
- Default dashboards
- Alerting configuration

## Resource Tiers

| Tier | Use Case | CPU | Memory | Pods |
|------|----------|-----|--------|------|
| **Starter** | Small teams, early projects | 4 | 8Gi | 20 |
| **Standard** | Most teams | 10 | 20Gi | 50 |
| **Large** | High-traffic services | 20 | 40Gi | 100 |
| **Enterprise** | Critical workloads | 40 | 80Gi | 200 |

## Network Policy

Teams can:
- Communicate within their namespace
- Receive traffic from ingress controller
- Receive probes from observability namespace
- Access external services (internet)
- Access DNS

Teams cannot:
- Access other team namespaces directly
- Access internal cluster services

## Getting Started

1. **Request Onboarding**: Use the Backstage template
2. **Wait for Approval**: Platform team reviews PR
3. **Access Namespace**: Use `kubectl` with your credentials
4. **Deploy Services**: Use service templates to create apps

## After Onboarding

Once onboarded, teams can:

1. Create services using golden path templates
2. Deploy to their namespace
3. Access Grafana dashboards
4. Manage their resources via GitOps

## Support

- **Slack**: #platform-help
- **Office Hours**: Thursdays 2-3pm
