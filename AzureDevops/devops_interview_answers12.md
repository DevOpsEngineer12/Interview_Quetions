## Azure Platform & Infrastructure Management

### ✅ How do you design a scalable network architecture using Azure Virtual Networks?

- Use Hub-and-Spoke topology with peered VNets.
- Deploy Application Gateway or Azure Firewall in the hub.
- Use Network Security Groups (NSGs) and Azure Private Link for security.
- Implement Azure Bastion for secure VM access.

### ✅ Explain how Azure Policy helps in enforcing compliance across subscriptions.

- Define policies to enforce tagging, location, SKU, etc.
- Assign policies at subscription or management group levels.
- Use built-in or custom definitions.
- Audit, deny, or append configurations automatically.

### ✅ What’s the difference between Azure RBAC and Azure AD roles?

| Feature  | Azure RBAC                           | Azure AD Roles                 |
| -------- | ------------------------------------ | ------------------------------ |
| Scope    | Azure resources (e.g., VMs, storage) | Azure AD objects (e.g., users) |
| Use Case | Access to manage Azure services      | Access to manage identities    |
| Examples | Contributor, Reader, Owner           | User Administrator, App Admin  |

### ✅ How do you use Managed Identities in automation workflows?

- Assign System-Assigned or User-Assigned Managed Identity to VM, App Service, or Function App.
- Grant access via RBAC to required Azure resources (e.g., Key Vault).
- Use MSI in scripts or pipeline tasks (e.g., Azure CLI with `--identity`).

---

## CI/CD Pipeline & DevOps Workflow

### ✅ How do you design a secure CI/CD pipeline for multi-environment deployment using Azure DevOps?

- Use service connections with limited scope and managed identities.
- Define environment-specific variables securely.
- Use YAML templates with stage-level approvals and RBAC.
- Implement Azure Key Vault for secret management.

### ✅ What are self-hosted agents, and when would you use them over Microsoft-hosted agents?

- Self-hosted agents run on user-managed machines.
- Useful for: custom dependencies, network access to private resources, longer builds.
- Cost-effective for frequent, long-running jobs.

### ✅ How do you handle rollback strategies in automated deployments?

- Store and version artifacts.
- Use Helm rollback or redeploy previous build.
- Add rollback stage triggered on failure.
- Maintain environment state to revert changes.

### ✅ How can you implement approval gates and artifact retention in a release pipeline?

- Use environment approvals in multi-stage YAML or classic releases.
- Configure pre-deployment conditions.
- Enable artifact retention policies in pipeline settings.

---

## Docker & Container Management

### ✅ How do you handle secrets while building Docker images in a CI/CD pipeline?

- Use build arguments for temporary secret injection.
- Avoid secrets in Dockerfiles or layers.
- Leverage Azure Key Vault and environment variables in runtime.

### ✅ What are the implications of running containers in privileged mode?

- Grants full access to host system—security risk.
- Should be avoided in production unless necessary.
- Required only for certain system-level workloads.

### ✅ Explain how multi-stage builds improve security and performance.

- Separate build and runtime environments.
- Reduces final image size and surface area.
- Removes build tools and sensitive data from production image.

### ✅ How do you clean up unused Docker resources automatically in build pipelines?

- Use cleanup scripts in pipeline (e.g., `docker system prune`).
- Schedule cleanup tasks for self-hosted agents.
- Avoid caching unused images/artifacts.

---

## Kubernetes (AKS)

### ✅ How do you monitor pod health and ensure high availability in AKS?

- Use readiness and liveness probes.
- Deploy multiple replicas across availability zones.
- Enable Azure Monitor for Containers.
- Use HPA and Cluster Autoscaler.

### ✅ What’s the difference between ClusterIP, NodePort, and LoadBalancer services?

| Type          | Description                                    | Use Case                      |
| ------------- | ---------------------------------------------- | ----------------------------- |
| ClusterIP     | Internal cluster access only                   | Default for internal services |
| NodePort      | Exposes service on static port on each node    | Development or debugging      |
| LoadBalancer  | External access via Azure Load Balancer        | Production public services    |

### ✅ How do you manage secrets and config files for applications running in Kubernetes?

- Use ConfigMaps and Secrets.
- Mount them as environment variables or volumes.
- Integrate Azure Key Vault via CSI driver.

### ✅ How would you troubleshoot a pod failing due to image pull issues?

- Check error with `kubectl describe pod`.
- Verify image tag and registry access.
- Check for authentication issues (e.g., imagePullSecrets).
- Validate image existence in container registry.

---

## Azure Infrastructure & Automation

### ✅ What’s your approach to provisioning Azure infrastructure using Terraform?

- Use modular structure with environment-specific tfvars.
- Use remote state backend (Azure Storage) with locking.
- Authenticate with service principal or managed identity.
- Validate and lint using `tflint` and `terraform validate`.

### ✅ How do you implement tagging standards across Azure resources for cost governance?

- Define standard tags in Terraform locals or variables.
- Enforce with Azure Policy.
- Include mandatory tags (owner, environment, cost-center).
- Use automation to validate tag presence.

### ✅ Explain how Azure Key Vault integrates with CI/CD tools.

- Link Key Vault to pipeline via variable groups or Key Vault tasks.
- Use managed identity or service principal to access secrets.
- Secrets injected into pipeline securely at runtime.

### ✅ What’s the role of Azure Policy in enforcing compliance?

- Enforce governance across Azure environment.
- Deny non-compliant resources or enforce defaults.
- Audit deployments for policy violations.
- Enable policy initiatives for grouped controls.

---

## Troubleshooting & Real-Time Scenarios

### ✅ How do you investigate an intermittent pipeline failure that only occurs in staging?

- Compare pipeline logs across runs.
- Analyze environment-specific variables or services.
- Add logging or retry logic in test stages.
- Reproduce locally or isolate flaky tests.

### ✅ What tools do you use for centralized logging and monitoring in Azure?

- Azure Monitor and Log Analytics.
- Application Insights.
- Grafana with Azure Monitor plugin.
- Container Insights for AKS workloads.

### ✅ How do you identify memory leaks in containerized workloads?

- Monitor container memory with Prometheus/Azure Monitor.
- Use `kubectl top pods` for metrics.
- Analyze application heap dumps or profiling tools.
- Set memory limits and observe OOM events.

### ✅ What steps do you take to reduce container startup time?

- Use minimal base images (e.g., Alpine).
- Optimize application initialization code.
- Avoid large image sizes and excessive dependencies.
- Use image caching and pre-pulled images on nodes.

---

