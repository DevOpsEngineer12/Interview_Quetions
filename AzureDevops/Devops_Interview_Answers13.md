
## CI/CD & Workflow Automation

### ✅ How do you implement CI/CD pipelines using Azure DevOps or Jenkins for microservices?
- Use separate pipelines per microservice for modularity.
- Employ YAML templates for reusable logic (build/test/deploy).
- Integrate Docker build, Helm packaging, and deployment to AKS.
- Jenkins can use shared libraries and webhooks with Git triggers.

### ✅ What are pipeline variables and how are they used across stages in a YAML pipeline?
- Variables are key-value pairs used to store config data.
- Defined at pipeline, stage, or job level.
- Accessed via `$(var)` or `variables['var']` syntax.
- Useful for environment config, secrets (with Key Vault), and conditions.

### ✅ How do you integrate automated tests and quality gates into a pipeline?
- Use test tasks (e.g., `dotnet test`, `pytest`, etc.) in CI stage.
- Integrate SonarQube or other static analysis tools.
- Define quality gate policies to block deployments if tests fail.
- Publish test results for reporting.

### ✅ What’s the difference between deployment groups and environments in Azure DevOps?
- **Deployment Groups**: Agent-based, used in classic pipelines for VM deployments.
- **Environments**: Used in YAML pipelines with approval gates, resource visualization, and checks.
- Environments support Kubernetes, virtual machines, and generic targets.

---

## Containerization & Docker

### ✅ How do you minimize Docker image size and why is it important?
- Use minimal base images (e.g., Alpine).
- Remove unnecessary build tools in multi-stage builds.
- Clean up caches and temporary files.
- Smaller images improve build, pull, and startup times.

### ✅ Explain how to pass environment variables securely into Docker containers during build and run.
- During build: use `--build-arg` (not secure for secrets).
- During runtime: use `--env` or `env_file` with secure secret providers (e.g., Azure Key Vault with CSI).
- Avoid hardcoding secrets.

### ✅ What are the risks of exposing Docker daemon socket to containers?
- Containers can control the host Docker daemon (full access).
- Potential for container breakout and privilege escalation.
- Should be avoided unless absolutely necessary.

### ✅ How do you manage versioning of Docker images in CI/CD?
- Use tags based on commit SHA, branch name, or semantic version.
- Push to a container registry (e.g., ACR).
- Maintain `latest` for dev and stable tags for releases.

---

## Kubernetes & AKS

### ✅ How do you scale applications in AKS using HPA (Horizontal Pod Autoscaler)?
- Define an HPA object with `kubectl` or YAML.
- Configure CPU/memory thresholds and min/max replicas.
- Metrics collected via metrics-server or Prometheus adapter.

### ✅ What is a Kubernetes DaemonSet and when would you use it?
- Ensures a pod runs on every (or some) node(s).
- Used for logging agents, monitoring daemons, security tools.

### ✅ How do you handle application configuration using ConfigMaps and Secrets?
- ConfigMaps store non-sensitive config like app settings.
- Secrets store sensitive data (base64 encoded).
- Mounted as volumes or injected as environment variables.

### ✅ How do you upgrade AKS clusters without downtime?
- Upgrade nodes incrementally using Azure CLI or portal.
- Use `maxUnavailable` and `PodDisruptionBudget` for control.
- Ensure apps use multiple replicas and readiness probes.

---

## Infrastructure as Code & Azure

### ✅ How do you use Terraform to provision Azure resources like AKS, VNets, and Key Vault?
- Define modules or resources for each service.
- Use AzureRM provider and backend state in Azure Blob.
- Secure access with service principal or managed identity.
- Example: `resource "azurerm_kubernetes_cluster"`

### ✅ What is the difference between Terraform modules and workspaces?
- **Modules**: Reusable code blocks for infrastructure.
- **Workspaces**: Isolated state environments (e.g., dev, prod) using the same codebase.

### ✅ How do you handle sensitive data like client secrets in Terraform?
- Use `sensitive = true` for variables.
- Store secrets in Azure Key Vault and reference via data sources.
- Avoid plaintext in code or logs.

### ✅ What are some ways to implement tagging policies for cost tracking in Azure?
- Define required tags using Azure Policy.
- Enforce tag application via IaC.
- Automate validation with CI checks and Terraform policies.

---

## Real-World Troubleshooting

### ✅ How would you resolve a failing pod stuck in CrashLoopBackOff in production?
- Check logs using `kubectl logs`.
- Describe pod to check events and reasons.
- Check init containers, readiness probes, and dependencies.
- Redeploy or rollback image if issue persists.

### ✅ What would you check if a pipeline randomly fails with timeout errors?
- Agent availability and performance.
- External service dependencies (rate limits, timeouts).
- Increase timeout thresholds temporarily.
- Review logs and resource usage.

### ✅ How do you troubleshoot sudden spikes in AKS node CPU usage?
- Use Azure Monitor or Prometheus to identify offending pods.
- Check HPA and app logic (e.g., infinite loops, load spikes).
- Set resource limits to prevent node exhaustion.

### ✅ What logs and metrics would you review if a service is intermittently unavailable?
- Pod logs and events.
- Azure Load Balancer and Ingress logs.
- Application Insights traces.
- Check liveness/readiness probe failures.
