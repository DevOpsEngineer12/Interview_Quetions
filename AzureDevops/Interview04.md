## Azure DevOps & Release Engineering

### ✅ How do you implement approval gates between different stages in Azure Pipelines?
Use **pre-deployment approvals** configured in **Environments** or directly within classic release pipelines. Define manual approvers or groups. YAML-based pipelines leverage environment protection rules with `approvals` and `checks` defined in the environment configuration.

### ✅ How can you optimize a long-running pipeline with parallel jobs and caching?
- Use **multi-job parallelism** with `jobs` running in parallel.
- Implement **caching** using `Cache` task for dependencies like `npm`, `pip`, or `Maven`.
- Use **matrix strategy** to run different combinations concurrently.
- Use **job dependencies** to reduce sequential execution.

### ✅ How do you enforce quality checks on pull requests using branch policies in Azure Repos?
- Enable **branch policies** on the target branch.
- Require **minimum number of reviewers**, **status checks**, and **linked work items**.
- Enforce **build validation** to run pipelines on PRs.
- Require **comment resolution** before merge.

### ✅ How do you use deployment groups vs service connections in Azure DevOps?
- **Deployment Groups**: Used for agent-based deployments to on-prem or VM-based environments.
- **Service Connections**: Used to authenticate and deploy to external systems like Azure, AWS, etc. (agentless).
- Use **deployment groups** for granular VM management, **service connections** for managed cloud infra.

### ✅ How would you implement conditional tasks in Azure Pipelines based on environment variables?
Use `condition` expressions in the YAML pipeline tasks. Example:
```yaml
- script: echo "Run only in dev"
  condition: eq(variables['Build.SourceBranchName'], 'dev')
```
Or use runtime parameters and conditions like:
```yaml
- ${{ if eq(parameters.env, 'prod') }}:
  - script: echo "Running in production"
```

### ✅ How do you securely consume secrets stored in Azure Key Vault within YAML pipelines?
- Create a **service connection** to Azure.
- Link **Key Vault** using the `azureKeyVault` variable group.
- Mark secrets and reference them like `$(mySecret)`.

### ✅ Explain the difference between checkout: self and checkout: none in Azure DevOps.
- `checkout: self`: Checks out the current repo code.
- `checkout: none`: Skips checkout. Useful when running pipelines that don’t require repo code, saving time.

### ✅ How would you design a matrix build strategy for testing across OS and Node versions?
```yaml
strategy:
  matrix:
    linux-node14:
      os: ubuntu-latest
      node: 14
    windows-node16:
      os: windows-latest
      node: 16
jobs:
- job: Build
  pool:
    vmImage: $(os)
  steps:
    - script: node --version
```

---

## Containers & Azure Kubernetes Service (AKS)

### ✅ What are pod disruption budgets and when would you configure one?
- A **Pod Disruption Budget (PDB)** limits the number of pods that can be disrupted during voluntary actions (e.g., node upgrade).
- Configure for **HA services** to ensure minimum pods remain available during disruptions.

### ✅ How do you auto-scale AKS pods based on custom metrics (like queue length or memory usage)?
- Use **KEDA (Kubernetes Event-driven Autoscaler)** for custom/event-driven scaling.
- Define **ScaledObjects** targeting metrics like queue length.
- Use **Prometheus Adapter** to expose custom metrics and scale with HPA.

### ✅ What are the pros and cons of using Azure CNI vs Kubenet in AKS networking?
- **Azure CNI**:
  - Pros: Full VNET integration, better control, NSGs support.
  - Cons: Consumes more IPs.
- **Kubenet**:
  - Pros: Fewer IPs consumed, simpler setup.
  - Cons: Less control, less secure.

### ✅ How do you handle stuck or orphaned pods in a production AKS cluster?
- Use `kubectl describe pod` and `kubectl logs` for debugging.
- Check for `CrashLoopBackOff`, `ImagePullBackOff`.
- Delete and recreate stuck pods.
- Use **Liveness/Readiness probes** and **pod affinity rules** for auto-healing.

### ✅ What’s your approach to debugging DNS resolution failures inside AKS pods?
- Use tools like `nslookup`, `dig`, or `busybox` to test DNS.
- Check CoreDNS logs and configmap.
- Verify `kube-dns` service is running and not resource-constrained.

### ✅ How do you secure service-to-service communication in AKS without exposing public endpoints?
- Use **internal services** and **NetworkPolicies**.
- Enable **mTLS** with a service mesh like Istio.
- Use **Azure Private Link** or **internal load balancers**.

### ✅ Explain how Istio or Linkerd works in managing traffic within Kubernetes clusters.
- Deploys a **sidecar proxy (Envoy)** with each pod.
- Handles **routing, retries, canary, traffic splitting**.
- Enables **observability**, **mTLS**, and **policy enforcement**.

### ✅ How would you handle certificate rotation for internal TLS in a service mesh setup?
- Use **Istio Citadel** or **cert-manager** with automatic renewal.
- Define short TTLs and auto-rotate.
- Restart sidecars after certificate refresh if not hot-reloaded.

---

## Infrastructure as Code (Terraform + Bicep)

### ✅ What are the differences between Terraform and Bicep in Azure provisioning?
- **Terraform**: Multi-cloud, mature ecosystem, stateful.
- **Bicep**: Native to Azure, simpler syntax, better ARM template integration.
- Terraform is better for hybrid scenarios, Bicep for Azure-focused setups.

### ✅ How do you implement DR (disaster recovery) setup using IaC in Azure?
- Use IaC to replicate infra in **secondary region**.
- Define **paired region resources** (e.g., Azure SQL with Geo-Replication).
- Set up **Traffic Manager** or **Front Door** for failover.

### ✅ How do you write a reusable Terraform module for provisioning private AKS clusters?
- Define `main.tf`, `variables.tf`, `outputs.tf` in a module folder.
- Accept VNET/subnet, RBAC, and private endpoint inputs.
- Output kubeconfig and identity.

### ✅ What’s your approach to managing secrets and connection strings in IaC workflows?
- Store secrets in **Azure Key Vault**.
- Reference via Terraform's `azurerm_key_vault_secret`.
- Avoid hardcoding; use environment variables or secret providers.

### ✅ How do you ensure zero-downtime deployment while provisioning AKS clusters with Terraform?
- Use **blue-green** AKS strategy.
- Provision new AKS in parallel, switch traffic via DNS or Front Door.
- Avoid destructive `terraform apply` in-place upgrades.

### ✅ What are some Terraform design best practices for multi-team infrastructure management?
- Use **modular structure** and **workspaces**.
- Follow **naming conventions**.
- Use **remote backends** for state.
- Manage secrets via **Key Vault**.
- Implement **code reviews** and **linting tools**.

### ✅ How do you use terraform import to bring existing Azure resources into Terraform state?
- Use `terraform import <resource> <id>`.
- Example: `terraform import azurerm_resource_group.example /subscriptions/xxx/resourceGroups/mygroup`
- Follow with `terraform plan` to check drift.

### ✅ Explain remote state locking and backend configuration using Azure Storage Account.
- Configure backend:
```hcl
backend "azurerm" {
  storage_account_name = "tfstateprod"
  container_name       = "tfstate"
  key                  = "prod.terraform.tfstate"
}
```
- State is stored in Azure Storage, locking via **blob lease** to avoid race conditions.

---

## GitOps & Secret Management

### ✅ How do you integrate Azure Key Vault with ArgoCD or FluxCD for secrets injection?
- Use **external-secrets** controller to sync secrets from Key Vault.
- ArgoCD/FluxCD reads Kubernetes secrets synced by the controller.
- Ensure RBAC and access policies are in place.

### ✅ How do you manage multiple environments in a GitOps-based deployment flow?
- Use **separate overlays** (Kustomize) or **branches/folders** for each environment.
- Apply **ArgoCD Applicationsets** or Flux environments for automation.
- Parameterize config using `values.yaml`, `envsubst`, or Helm.

### ✅ What’s your rollback plan if Git contains a misconfigured value pushed accidentally?
- Use `git revert` or roll back to a previous commit.
- ArgoCD automatically re-syncs the corrected config.
- Enable **auto-sync** with manual pause if needed.

### ✅ How do you set up auto-sync and drift detection in ArgoCD for AKS deployments?
- Enable `syncPolicy: automated` in ArgoCD Application.
- Use `selfHeal: true` and `prune: true`.
- ArgoCD constantly watches and reverts drift from Git source.

### ✅ What’s your strategy to securely handle sensitive Helm chart values in a GitOps workflow?
- Use **sealed-secrets**, **SOPS**, or **external-secrets**.
- Avoid plaintext secrets in Git.
- Decrypt at runtime using Kubernetes controllers.

### ✅ How would you roll back to a previous Git commit in GitOps during a failed rollout?
- Use `git revert` or checkout the last working commit.
- Push the change to Git, ArgoCD auto-syncs.
- Optionally disable auto-sync during troubleshooting.

---

## Monitoring, Security & Real-Time Debugging

### ✅ How do you monitor SSL certificate expiry for services hosted in Azure App Gateway or AKS?
- Use **Azure Monitor** or **Log Analytics queries** for App Gateway certs.
- For AKS Ingress certs, use **cert-manager** + Prometheus + Alertmanager.
- Optional: Use external tools like **SSL Labs API** or custom scripts.

### ✅ What tools do you use for container runtime security (e.g., seccomp, AppArmor)?
- **Seccomp**: Restricts syscalls.
- **AppArmor**: Defines security profiles per pod/container.
- Use **PodSecurityPolicy** or **OPA/Gatekeeper** for enforcing policies.
- Use tools like **Aqua**, **Sysdig**, **Falco** for runtime scanning.

### ✅ How do you trace and fix a memory leak issue reported in a Java-based container running in AKS?
- Enable **heap dumps** and **GC logs**.
- Use **Prometheus + Grafana** to monitor memory metrics.
- Analyze with **VisualVM** or **Eclipse MAT**.
- Use `kubectl top pod` to detect trends and restart if needed.

### ✅ Describe a situation where you handled a production incident. How did you coordinate the fix?
- During a release, the app crashed due to misconfigured env vars.
- Identified the issue from logs, reverted the Helm release.
- Notified stakeholders, created a postmortem.
- Added a pre-deployment check in the CI/CD pipeline to catch similar issues early.

### ✅ How do you handle intermittent 502/504 errors in an AKS ingress controller setup?
- Check NGINX ingress logs for backend timeouts.
- Increase `proxy-read-timeout`, `proxy-send-timeout`.
- Analyze backend pod health/readiness.

### ✅ How do you configure and visualize AKS metrics in Azure Monitor and Grafana?
- Enable **Container Insights** in Azure Monitor.
- Export metrics to **Log Analytics workspace**.
- Use **Azure Monitor data source** in Grafana.

### ✅ Describe your strategy for root cause analysis when CPU throttling affects application latency.
- Use `kubectl top` or Prometheus to find affected pods.
- Compare `limits` vs `requests` for CPU.
- Analyze throttling metrics in Grafana.
- Adjust resource limits and apply HPA if needed.

