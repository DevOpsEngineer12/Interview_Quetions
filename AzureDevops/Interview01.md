# 🌐 Azure & Production Readiness

### ✅ How would you configure Azure Front Door with AKS for global routing and security?
- Use Azure Front Door as the global entry point with its Web Application Firewall (WAF) for security.
- Configure Front Door to route traffic to regional AKS clusters using backend pools.
- Enable HTTPS with custom domains and certificates.
- Use health probes and backend priority for failover and load balancing.
- Integrate with Azure Private Link or Application Gateway for enhanced security.

### ✅ How do you enforce tag policies across all Azure resources using Azure Policy?
- Define a built-in or custom Azure Policy with `effects` such as `audit`, `deny`, or `append` to control tag compliance.
- Assign policies at the management group or subscription level for organization-wide enforcement.
- Combine with Azure Initiative Definitions to bundle multiple policies.
- Use `remediation tasks` to apply tags to non-compliant resources.

### ✅ What's the difference between Azure Private Link and Service Endpoints — and where would you use each?
- **Private Link**: Provides private IP access from the VNet to Azure services. Use for strict security isolation.
- **Service Endpoints**: Extends VNet identity to Azure services over the public IP but secured path. Use for performance with less complexity.
- **Use Cases**:
  - Use Private Link for services with compliance or data exfiltration concerns.
  - Use Service Endpoints for simpler and more performant connections without full private integration.

### ✅ How do you handle log retention and cost control in Azure Monitor for a large-scale system?
- Set retention policies at the Log Analytics workspace level.
- Implement data collection rules to limit telemetry to necessary logs only.
- Use diagnostic settings to route logs to cheaper storage like Blob storage for long-term archival.
- Enable sampling in Application Insights.
- Create cost alerts and dashboards using Azure Cost Management.

---

# 🚀 CI/CD + GitHub Actions / Azure DevOps

### ✅ How do you ensure idempotency in pipeline tasks when re-running failed stages?
- Design scripts to check for the current state before executing actions (e.g., using `az cli`, `terraform plan`).
- Use declarative tools like Terraform or ARM templates that enforce state management.
- Mark steps as `condition: succeeded()` or use job-level dependencies to control reruns.

### ✅ What’s your approach for implementing approval gates and quality checks in a release pipeline?
- In Azure DevOps, use `environments` and set `pre-deployment approvals`.
- Integrate quality gates like SonarQube, security scans, and test coverage checks.
- Enforce manual or automated checks using environments and `checks` APIs in GitHub Actions.

### ✅ How would you manage multiple release pipelines for microservices with shared infrastructure?
- Use templated pipeline definitions and YAML templates for DRY configuration.
- Separate infra pipeline from service pipelines and use artifact/version promotion.
- Use environment-specific variables and deployment strategies (e.g., blue-green, canary) per service.

### ✅ How do you handle pipeline secrets rotation and versioning securely?
- Store secrets in Azure Key Vault or GitHub Secrets.
- Use managed identity to access secrets securely.
- Rotate secrets using automation and monitor access logs.
- Version secrets using tags or naming conventions (e.g., `db-password-v2`).

---

# 🚀 Kubernetes / AKS Deep Dive

### ✅ How do you manage certificate renewals for services hosted in AKS using cert-manager?
- Install cert-manager with Helm and configure ClusterIssuer/Issuer (e.g., Let's Encrypt).
- Certificates auto-renew based on configured TTL (default 30 days before expiry).
- Use annotations in Ingress resources to trigger certificate provisioning and updates.
- Monitor cert-manager logs and events for renewals and errors.

### ✅ Explain the steps to debug DNS resolution issues inside a pod in AKS.
1. `kubectl exec` into the pod and use `nslookup`/`dig` on the failing domain.
2. Check `/etc/resolv.conf` to verify DNS settings.
3. Validate CoreDNS pods are running using `kubectl get pods -n kube-system`.
4. Review CoreDNS logs with `kubectl logs`.
5. Ensure `networkPolicy` or `DNSPolicy` isn't blocking DNS access.

### ✅ What are the performance implications of running too many sidecars in a pod?
- Increases memory/CPU usage per pod, leading to higher node pressure.
- Can delay pod startup time and affect readiness.
- Complicates resource requests/limits tuning.
- May cause cascading failures if one sidecar fails.

### ✅ How do you validate AKS cluster upgrades in a blue-green strategy?
- Deploy the upgraded version to a parallel (green) AKS cluster or namespace.
- Route a portion of traffic using Azure Front Door or Ingress canary strategy.
- Run integration and smoke tests on green.
- Promote green to production after validation, then decommission blue.

---

# 🚀 GitOps & ArgoCD/FluxCD

### ✅ How would you secure GitOps workflows against unauthorized cluster changes?
- Use RBAC and admission controllers to block direct `kubectl` changes.
- Enable ArgoCD's "self-heal" and "auto-sync" to revert unauthorized changes.
- Integrate with SSO for access control.
- Use signed commits or Git commit verification with GPG.

### ✅ What happens if the Git source is updated manually during a deployment — how does ArgoCD respond?
- ArgoCD detects Git changes periodically or via webhook.
- If auto-sync is enabled, the deployment restarts with the latest commit.
- If not, the UI shows it as "OutOfSync" for manual sync.

### ✅ How do you use sync waves in ArgoCD to control deployment order?
- Use `sync-wave` annotation in manifests to define order (lower numbers first).
- Example: Apply CRDs before CRs, or deploy backend before frontend.
- Sync waves ensure resources are created in logical and dependency-based order.

### ✅ What’s your Git branching strategy for managing multi-environment deployments?
- Use `main` or `trunk` for production, `dev` for development, and `staging` for pre-prod.
- Maintain a folder structure per environment or repo-per-env model.
- Use ArgoCD `Application` CRs or `kustomize` overlays to target specific branches.

---

# 💸 Cost Optimization & Governance

### ✅ How do you monitor and control cost spikes in AKS and Azure Functions?
- Enable Azure Cost Management and set budgets with alerts.
- Use autoscaling policies wisely (KEDA for Functions, HPA for AKS).
- Right-size node pools and implement spot instances.
- Use usage analytics to detect anomalies and unused resources.

### ✅ What are the ways to automate idle resource cleanup in Azure?
- Use Azure Automation or Logic Apps to identify idle resources using activity logs.
- Schedule cleanup of unattached disks, idle VMs, or stale IPs.
- Tag resources with owner and TTL (time-to-live) policies, enforce using Azure Policy.

### ✅ Explain your approach to choosing between Azure App Services and Azure Container Apps.
- **App Services**: Ideal for web apps with low container complexity or legacy needs.
- **Container Apps**: Better for microservices, event-driven workloads, and scale-to-zero requirements.
- Container Apps support Dapr and KEDA, offering more flexibility for cloud-native apps.

### ✅ How do you use Azure Cost Management to enforce budget alerts across teams?
- Create and assign budgets per subscription, resource group, or service.
- Configure budget alerts via Action Groups (email, webhook, etc.).
- Use cost analysis dashboards to give visibility to teams.
- Tag resources by owner/team to filter and track usage per department.
