
# DevOps Interview Answers – CI/CD, Kubernetes, Azure (3–5 YOE)

## CI/CD & Release Strategy

### ✅ How do you structure a monorepo CI/CD pipeline for multiple microservices?
- Use path-based triggers in YAML to trigger specific microservice pipelines.
- Share templates across microservices for common build/deploy logic.
- Define independent stages for each service in a centralized pipeline with conditional execution.

### ✅ What’s your approach to implementing progressive delivery in Azure DevOps?
- Use deployment rings (Dev → Test → Prod) with approval gates.
- Leverage feature flags for controlled rollout.
- Integrate with tools like Argo Rollouts or use traffic routing in Azure App Gateway/Ingress.

### ✅ How do you manage parallel jobs and dependencies in YAML pipelines?
- Use `dependsOn` for sequential dependencies.
- Define multiple jobs with `strategy: parallel`.
- Use output variables to pass data between dependent jobs.

---

## ☸️ Kubernetes & Platform Engineering

### ✅ What is the role of custom resource definitions (CRDs) in Kubernetes?
- CRDs allow you to define custom resources beyond the built-in K8s objects.
- Used by operators and controllers for domain-specific automation.

### ✅ How would you implement multi-tenant architecture securely in AKS?
- Separate tenants via namespaces.
- Apply Network Policies to isolate communication.
- Use Azure AD for RBAC and Kubernetes namespace-level roles.

### ✅ How do you handle node pool upgrades in a live production cluster?
- Upgrade node pools gradually using surge upgrades.
- Use PodDisruptionBudgets to prevent service interruption.
- Monitor health and rollback on failure.

---

## 📦 Containers & Runtime Optimization

### ✅ What are the best practices for reducing Docker image attack surface?
- Use minimal base images (e.g., Alpine, Distroless).
- Avoid running as root user.
- Remove build tools and cache in final image.

### ✅ How do you handle SIGTERM and graceful shutdowns in containers?
- Implement signal traps in app code to clean up resources.
- Define preStop hooks in Kubernetes deployments.
- Ensure graceful shutdown duration matches termination grace period.

### ✅ Explain the impact of container resource limits and how you monitor them.
- Limits prevent resource contention but can lead to throttling or OOMKills.
- Monitor with `kubectl top pods`, Prometheus, and Azure Monitor.
- Use resource requests/limits based on performance profiling.

---

## ☁️ Azure Infrastructure & Automation

### ✅ How do you use Terraform workspaces to manage multiple environments?
- Create a workspace per environment (e.g., dev, prod).
- Use shared modules but different `tfvars` per workspace.
- Workspaces isolate state files.

### ✅ What’s your process for implementing blueprints or landing zones in Azure?
- Use Azure Landing Zone ARM/Bicep modules or Terraform modules.
- Automate with Azure Blueprints or Azure Deployment Scripts.
- Include policies, RBAC, and networking by default.

### ✅ How would you manage infrastructure drift in a team environment?
- Use `terraform plan` in CI to detect drift.
- Enable notifications for unexpected changes.
- Reconcile manually or automate correction with `terraform apply` after validation.

---

## 🛡️ Cloud Security & Compliance

### ✅ How do you enforce CIS benchmarks in an AKS cluster?
- Use tools like Kubescape, kube-bench, and Azure Policy add-ons.
- Enable audit logging and restrict privileges.
- Continuously scan with Azure Defender for Kubernetes.

### ✅ What tools do you use to detect misconfigured cloud storage or exposed secrets?
- Azure Security Center and Defender for Cloud.
- Tools like Trivy, Checkov, and GitHub Advanced Security.
- Periodic secret scanning in source control and pipelines.

### ✅ How do you implement just-in-time (JIT) VM access in Azure?
- Enable JIT in Microsoft Defender for Cloud.
- Only open necessary ports for approved time windows.
- Logs are captured in Azure Activity Log.

---

## 📊 Monitoring & Proactive Ops

### ✅ How do you use KEDA for event-driven scaling in AKS?
- Deploy KEDA with scaler configurations for queues, HTTP, CPU, etc.
- Define `ScaledObject` for trigger sources like Azure Queue, Kafka.
- Auto-scales pods based on event metrics.

### ✅ What is your approach to observability in distributed systems?
- Use OpenTelemetry for tracing.
- Centralize logs with Fluent Bit → Log Analytics.
- Combine traces, metrics, logs in a unified dashboard (Grafana, Azure Monitor).

### ✅ How do you set up anomaly detection alerts for unusual deployment behavior?
- Use Azure Monitor with dynamic thresholds.
- Enable Smart Detection in Application Insights.
- Define custom alert rules based on baseline deviations.

---

## 🧠 Scenario-Based Debugging

### ✅ Your app is failing only during peak hours — what’s your step-by-step analysis?
- Analyze CPU/memory/network usage metrics.
- Inspect autoscaling behavior and HPA events.
- Review dependency timeouts or rate limits.
- Add profiling or instrumentation to narrow down performance hotspots.

### ✅ A Terraform state file is accidentally deleted — what do you do?
- Check for backups in Azure Storage (versioning enabled).
- Recreate state with `terraform import` if no backup.
- Rebuild and reinitialize the backend securely.

### ✅ How would you investigate high latency in a service running in AKS?
- Use `kubectl top`, `kubectl describe` to check pod health.
- Analyze network policies or DNS issues.
- Review logs, application traces, and resource pressure on the node.

