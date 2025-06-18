## CI/CD & Azure DevOps Pipelines

### ✅ How would you design a pipeline in Azure DevOps that supports rollback and canary deployments?
- Use multi-stage pipelines with deployment strategies like `canary` and `blue-green`.
- For canary: gradually increase traffic using traffic-splitting in Azure App Gateway or Kubernetes service.
- Include post-deployment validation steps.
- Rollback logic: define conditional stage that triggers on failure to redeploy previous version.

### ✅ How do you ensure secure connection between Azure DevOps and external container registries like ACR or DockerHub?
- Use service connections with least-privilege access.
- Use Azure AD authentication with managed identity for ACR.
- Store DockerHub credentials as secure pipeline variables or in Azure Key Vault.

### ✅ How can you prevent accidental deployment to production in a multi-stage pipeline?
- Add manual approval checks for the production stage.
- Use environment-level protection with role-based approvals.
- Use condition checks like `if eq(variables['Build.SourceBranchName'], 'main')`.
- Require two-person rule via branch policies.

### ✅ Explain how you would manage pipeline templates and reuse logic across different microservices.
- Use YAML templates for jobs/stages/steps.
- Store templates in central repo or template folder.
- Import via `template:` keyword in child pipelines.
- Pass parameters for customization.

---

## Azure Cloud Infrastructure & Governance

### ✅ How do you implement custom RBAC roles in Azure for fine-grained access control?
- Define role in JSON with actions, scopes, and conditions.
- Example:
  ```json
  {
    "Name": "CustomReader",
    "Actions": ["Microsoft.Compute/virtualMachines/read"],
    "AssignableScopes": ["/subscriptions/xxx"]
  }
  ```
- Assign via Azure CLI or Portal.

### ✅ What’s the best way to manage and rotate Azure Key Vault secrets across environments?
- Use separate Key Vaults or prefixes per environment.
- Automate rotation using Event Grid + Azure Functions.
- Access secrets via managed identity.
- Set soft delete and purge protection.

### ✅ How would you enforce cost controls and alerting for unexpected Azure resource consumption?
- Use Azure Budgets with alert thresholds.
- Configure cost alerts via Azure Monitor.
- Tag resources for cost attribution.
- Apply Azure Policies to limit SKU sizes.

### ✅ Describe how to securely manage infrastructure using private endpoints and service endpoints in Azure.
- Use Private Endpoints for secure, private connectivity.
- Disable public access on PaaS services.
- Use Private DNS zones for resolution.
- For service endpoints, enable subnet-level access to PaaS services.

---

## Containerization & AKS (Azure Kubernetes Service)

### ✅ How do you handle persistent storage for stateful apps in AKS?
- Use Azure Disk (ReadWriteOnce) for single pod state.
- Use Azure Files (ReadWriteMany) for shared access.
- Define `PersistentVolume` and `PersistentVolumeClaim`.
- Use StorageClass for dynamic provisioning.

### ✅ What’s your strategy for blue-green deployment in AKS using Ingress or service routing?
- Deploy green version alongside blue using separate labels.
- Use ingress path or header-based routing to control traffic.
- Validate green deployment, then switch traffic.
- Optionally delete blue after success.

### ✅ How would you auto-scale AKS based on custom metrics (e.g., queue length or CPU usage)?
- Enable HPA with custom metrics using Prometheus Adapter.
- Use KEDA for event-driven scaling (e.g., Azure Queue, Kafka).
- Define ScaledObject with threshold and metric source.

### ✅ What’s your approach to managing different configurations (dev, staging, prod) for the same app in Kubernetes?
- Use `ConfigMaps` and `Secrets` per environment.
- Separate namespaces or Helm value files.
- Use Kustomize overlays or Helm templating.
- Integrate with GitOps (ArgoCD) for declarative control.

---

## Monitoring & Observability

### ✅ How do you implement distributed tracing for microservices in AKS?
- Use OpenTelemetry SDK in apps.
- Deploy Jaeger or Azure Monitor Application Insights.
- Correlate trace IDs across services.
- Export data to Log Analytics or Grafana.

### ✅ How would you set up end-to-end alerting with Azure Monitor and Action Groups?
- Enable monitoring for AKS, VMs, and App Services.
- Define metric or log-based alerts.
- Route alerts to Action Groups: email, webhook, ITSM.
- Integrate with PagerDuty or Teams for incident response.

### ✅ How do you investigate and fix a 500 error reported from an internal API hosted on Azure?
- Check Azure Monitor and App Insights traces.
- Inspect container logs (`kubectl logs`).
- Correlate requests via trace ID.
- Use retry and circuit breaker patterns to mitigate.

### ✅ How would you correlate logs and metrics across AKS and Azure Functions?
- Centralize logs in Log Analytics workspace.
- Use Application Insights for both services.
- Query using Kusto Query Language (KQL).
- Create dashboards in Azure Monitor or Grafana.

