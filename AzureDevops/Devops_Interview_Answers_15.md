
## CI/CD & Automation (Azure DevOps)

### ✅ How do you manage pipeline failures and auto-retry specific stages in Azure DevOps?
- Use `dependsOn` and `condition` to control retry logic in YAML.
- Add `retry` logic in jobs using scripts with loop or `try/catch` patterns.
- Use `ContinueOnError` for non-blocking stages and alert for failures.

### ✅ What are pipeline templates in YAML and how do they improve maintainability?
- Templates modularize reusable stages, jobs, or steps.
- Promote DRY principle by extracting common logic (e.g., build/test/deploy).
- Easy to maintain changes across multiple pipelines.

### ✅ How do you implement approvals, gates, and environment checks in release pipelines?
- Use Azure Environments with pre-deployment approvals in YAML/classic.
- Add Gates to check service health or execute queries before deployment.
- Use branch filters, RBAC, and manual intervention where needed.

### ✅ How do you integrate Azure Key Vault secrets dynamically in your CI pipeline?
- Link Key Vault as variable group in Azure DevOps Library.
- Use `AzureKeyVault@2` task or pipeline variables referencing secret names.
- Use Managed Identity or SPN with proper Key Vault access.

---

## Kubernetes & Platform Engineering (AKS)

### ✅ How would you secure internal services in AKS using Network Policies?
- Define NetworkPolicy to restrict ingress/egress traffic.
- Use Calico or Azure-native network plugin for enforcement.
- Combine with NSGs and private endpoints for layered security.

### ✅ Explain the role of taints and tolerations in workload scheduling.
- Taints repel pods from nodes.
- Tolerations allow specific pods to tolerate the taints and schedule.
- Used for node-level isolation (e.g., GPU workloads, system-critical apps).

### ✅ How do you manage dynamic scaling in AKS based on CPU/memory metrics?
- Use HPA (Horizontal Pod Autoscaler) to scale replicas based on resource metrics.
- Use Cluster Autoscaler to scale nodes.
- Enable metrics-server for autoscaling support.

### ✅ What’s the difference between HPA and VPA? When would you use each?
- **HPA:** Scales number of pods based on metrics.
- **VPA:** Adjusts pod resource requests/limits based on usage.
- Use HPA for horizontal scaling and VPA for optimizing resource allocation.

---

## Infrastructure as Code (Terraform)

### ✅ How do you manage sensitive variables and secret keys in Terraform?
- Store secrets in Azure Key Vault or environment variables.
- Use `sensitive = true` in Terraform to avoid showing in output.
- Avoid hardcoding sensitive values in `.tf` files.

### ✅ What’s your strategy to handle Terraform drift detection and correction in Azure?
- Use `terraform plan` in CI to detect drift regularly.
- Enable alerts for drift in state vs actual infrastructure.
- Apply changes carefully with `terraform apply` after review.

### ✅ Explain how you handle provisioning dependencies across multiple modules.
- Use `depends_on` to enforce order.
- Pass outputs from one module to another using input variables.
- Define clear module boundaries and state separation.

### ✅ What are workspaces in Terraform, and how do they help in environment separation?
- Workspaces maintain separate state files per environment.
- Helpful for Dev, Test, Prod separation without duplicating code.
- Not a full replacement for directory-based strategies but useful for lightweight setups.

---

## Cloud Security & Azure Governance

### ✅ How do you enforce tagging and naming policies in Azure across teams?
- Define naming/tagging conventions in documentation.
- Enforce using Azure Policy and initiatives.
- Use custom policies or tools like Terraform to enforce standardization.

### ✅ What are Azure Policies and how do you prevent non-compliant deployments?
- Azure Policies define rules to enforce configurations (e.g., allowed SKUs, regions).
- Use `deny`, `audit`, `append` effects to control compliance.
- Apply policies at resource group/subscription/management group level.

### ✅ How do you handle Just-In-Time (JIT) VM access and why is it important?
- JIT VM Access enabled via Microsoft Defender for Cloud.
- Opens ports like RDP/SSH temporarily upon approval.
- Reduces attack surface and improves security posture.

### ✅ How do you configure RBAC for AKS at namespace level?
- Create Kubernetes RoleBindings/ClusterRoleBindings scoped to namespace.
- Use Azure AD integration for identity federation.
- Assign users/groups to roles using `kubectl` or manifests.

---

## Monitoring & Observability

### ✅ How do you use Azure Monitor to alert on abnormal CPU patterns in AKS?
- Enable Container Insights for AKS.
- Use Log Analytics queries to monitor CPU usage.
- Configure alert rules and action groups for notifications.

### ✅ What’s your approach to integrating Prometheus/Grafana with AKS clusters?
- Use kube-prometheus-stack Helm chart.
- Configure Prometheus scraping and dashboards in Grafana.
- Optionally export metrics to Azure Monitor using exporters.

### ✅ How would you centralize logs from all microservices in a containerized application?
- Use Fluent Bit or Log Analytics agent to forward logs.
- Use a sidecar or daemonset approach for collection.
- Centralize in Azure Log Analytics or ELK/Grafana stack.

### ✅ What’s the difference between logs, metrics, and traces?
- **Logs** – Discrete event records (e.g., error, request logs).
- **Metrics** – Aggregated numerical data (e.g., CPU usage).
- **Traces** – Track request flow across services (e.g., APM, OpenTelemetry).
