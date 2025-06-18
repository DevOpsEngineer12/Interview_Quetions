## CI/CD & Azure DevOps Pipelines

### ✅ How do you use YAML templates in Azure DevOps for multi-project pipelines?

- Create centralized reusable templates for build, test, deploy.
- Reference templates using `template` keyword:
  ```yaml
  - template: templates/build.yml
    parameters:
      buildConfiguration: 'Release'
  ```
- Promote standardization and reduce duplication across projects.

### ✅ How do you pass secrets securely between pipeline stages?

- Store secrets in Azure Key Vault or Pipeline variable groups marked as secret.
- Use `isOutput: true` to pass values between jobs:
  ```yaml
  jobs:
    - job: GenerateSecret
      variables:
        secretVar: $(mySecret)
      steps:
        - script: echo "##vso[task.setvariable variable=sharedSecret;isOutput=true]$secretVar"

    - job: UseSecret
      dependsOn: GenerateSecret
      variables:
        sharedSecret: $[ dependencies.GenerateSecret.outputs['sharedSecret'] ]
  ```

### ✅ What’s your approach to implementing environment-specific approvals in release pipelines?

- Define environments in Azure DevOps.
- Use **pre-deployment approvals** in environment settings.
- Assign approvers based on RBAC.
- Use `checks:` in YAML pipelines for environment gates.

### ✅ How would you create rollback logic if a deployment stage fails?

- Use a separate rollback job triggered on failure:
  ```yaml
  - job: rollback
    condition: failed()
    steps:
      - script: ./rollback.sh
  ```
- Store deployment metadata for rollback.
- Use Helm rollback or deploy previous artifact version.

### ✅ How would you implement a multi-stage pipeline in Azure DevOps for microservices?

- Use templates and variable groups for each microservice.
- Define separate stages: build → test → deploy → verify.
- Use service connections per environment.
- Example:
  ```yaml
  stages:
    - stage: Build
      jobs:
        - job: BuildServiceA
        - job: BuildServiceB
    - stage: Deploy
      dependsOn: Build
      jobs:
        - job: DeployToAKS
  ```

### ✅ How do you detect and act on drift in your infrastructure using CI/CD?

- Use Terraform `plan` and `show` to compare infrastructure state.
- Automate drift detection in pipeline with manual approval gates:
  ```yaml
  - script: terraform plan -detailed-exitcode
  ```
- Trigger alerts if drift is detected.

### ✅ How do you secure service connections and agent pools in Azure Pipelines?

- Use **Managed Identity** or **Service Principal** with minimal RBAC permissions.
- Restrict agent pool access using Project-level security.
- Enable job authorization for scoped access.

### ✅ How do you handle versioning and rollback strategies for Helm chart-based deployments?

- Tag Helm chart with Git commit or build number.
- Use `helm rollback <release> <revision>` on failure.
- Store Helm releases in a private chart repository (e.g., ACR).

---

## Kubernetes (AKS) & Production Reliability

### ✅ How do you manage config changes across multiple environments in Kubernetes?

- Use environment-specific `values.yaml` with Helm.
- Implement GitOps using ArgoCD or Flux.
- Separate namespaces per environment.
- Use sealed secrets or Azure Key Vault CSI Driver for secret management.

### ✅ What’s the role of PodDisruptionBudget and how does it help during node upgrades?

- Ensures minimum availability of pods during voluntary disruptions.
- Prevents all replicas from being evicted at once during node upgrades.
- Example:
  ```yaml
  apiVersion: policy/v1
  kind: PodDisruptionBudget
  spec:
    minAvailable: 2
    selector:
      matchLabels:
        app: myapp
  ```

### ✅ How do you configure network policies to isolate team environments in AKS?

- Use Kubernetes `NetworkPolicy` with namespace selectors.
- Isolate traffic between namespaces:
  ```yaml
  policyTypes: [Ingress, Egress]
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              name: dev-team-a
  ```
- Use Azure CNI with network policy support.

### ✅ What would you do if kubectl get pods shows ImagePullBackOff on a critical pod?

- Check image name and tag in the pod spec.
- Validate image exists in registry and permissions are correct.
- Inspect image pull secret if using a private registry.
- Check events with `kubectl describe pod <pod>` for pull failure reason.

---

## Infrastructure as Code (Terraform & Bicep)

### ✅ What are the trade-offs between using Terraform vs. Bicep on Azure?

- **Terraform**:
  - Multi-cloud support.
  - Mature module ecosystem.
  - State management and locking built-in.
- **Bicep**:
  - Native to Azure.
  - Easier syntax for ARM users.
  - No state management (uses Azure directly).

### ✅ How do you build reusable Terraform modules and structure them in monorepos?

- Organize modules in `modules/` directory.
- Use naming convention for module versions.
- Example:
  ```hcl
  module "storage" {
    source = "./modules/storage"
    name   = "devstore"
  }
  ```
- Use GitHub Actions to validate and test modules on PRs.

### ✅ How do you store Terraform state securely and manage remote locking in Azure?

- Use Azure Storage backend with blob locking.
- Enable encryption and RBAC on the storage account.
- Use separate containers or state files per environment.

### ✅ What’s your strategy for tagging, labeling, and cost-management through IAC?

- Apply `tags` block to all resources:
  ```hcl
  tags = {
    environment = "dev"
    owner       = "team-x"
    costcenter  = "12345"
  }
  ```
- Use Azure Policy to enforce mandatory tags.
- Automate tag propagation using modules.

---

## SRE, Monitoring & Troubleshooting

### ✅ How do you troubleshoot intermittent 502 errors in an AKS-hosted service?

- Check Ingress Controller logs (e.g., NGINX or AGIC).
- Inspect app logs for startup or timeout issues.
- Check backend probe and readiness probe status.
- Analyze metrics from Azure Monitor and App Insights.

### ✅ How do you integrate Azure Log Analytics with Prometheus/Grafana for AKS?

- Use Azure Monitor for containers with Prometheus scraping enabled.
- Export logs to Log Analytics.
- Connect Grafana to Log Analytics using Azure Monitor plugin.
- Visualize AKS metrics and custom dashboards.

### ✅ How do you detect and remediate memory leaks in containerized apps?

- Use `kubectl top pod` to check memory usage.
- Set memory limits and requests in pod specs.
- Enable alerts on memory usage via Prometheus.
- Use tools like `Valgrind`, `dotMemory`, or `GraalVM` for app profiling.

### ✅ Explain how you’d run a chaos test to validate resilience in your deployment.

- Use tools like **LitmusChaos**, **Chaos Mesh**, or **Azure Chaos Studio**.
- Simulate pod/network/node failures.
- Observe system behavior using observability stack.
- Validate recovery mechanisms and SLO adherence.

---

## Azure Services & Cloud Architecture

### ✅ How do you use Azure Application Gateway for traffic routing between microservices?

- Configure URL path-based routing rules.
- Use Azure App Gateway Ingress Controller (AGIC) for Kubernetes.
- Enable WAF for security.
- Use backend pools mapped to services or AKS Ingress.

### ✅ What is the difference between Azure Load Balancer and Azure Traffic Manager?

| Feature       | Azure Load Balancer              | Azure Traffic Manager    |
| ------------- | -------------------------------- | ------------------------ |
| Type          | Layer 4 (TCP/UDP)                | DNS-based global routing |
| Usage         | Internal/External Load Balancing | Multi-region failover    |
| Health Checks | TCP probes                       | DNS/HTTP/HTTPS probes    |
| Geo-awareness | ❌                                | ✅                        |

### ✅ How would you configure Azure Monitor to track resource health across subscriptions?

- Enable Diagnostic Settings on resources.
- Route logs/metrics to a centralized Log Analytics Workspace.
- Use Azure Monitor Workbooks and Alerts.
- Use Azure Lighthouse to view cross-subscription insights.

### ✅ Explain the use of Azure Blueprints in large-scale governance.

- Define and apply governance controls at scale (policies, RBAC, resources).
- Assign blueprints to subscriptions or management groups.
- Enforce standard environments for dev, test, and prod.
- Supports artifact versioning and lock assignments.

---

