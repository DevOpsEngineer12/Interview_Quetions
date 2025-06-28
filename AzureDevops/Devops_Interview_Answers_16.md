
# DevOps Interview Answers (3-5 Years Experience)

## CI/CD & Workflow Automation

### ✅ What are the differences between release pipelines and build pipelines in Azure DevOps?
- **Build Pipelines** focus on building, testing, and packaging the code.
- **Release Pipelines** manage the deployment of built artifacts to different environments.
- Build pipelines use YAML or classic UI; release pipelines are often visual and environment-driven.

### ✅ How do you manage pipeline secrets securely without exposing them in logs?
- Store secrets in Azure Key Vault or mark them as secret pipeline variables.
- Avoid echoing secrets; use `env:` or secret variable references without logging.
- Ensure logs are scrubbed with secret masking features enabled.

### ✅ How would you implement conditional pipeline stages based on environment?
- Use the `condition:` keyword in YAML.
- Example: `condition: eq(variables['Build.SourceBranch'], 'refs/heads/main')` for production.

### ✅ Explain Blue-Green and Canary deployment strategies with examples.
- **Blue-Green**: Deploy to Green (idle), run tests, switch traffic from Blue to Green if successful.
- **Canary**: Deploy to a small subset of users (e.g., 10%), gradually increase while monitoring.

---

## 🚀 Docker & Containerization

### ✅ What’s your approach to optimizing Docker images for production use?
- Use multi-stage builds and minimal base images like Alpine or Distroless.
- Remove unnecessary dependencies and cache layers.
- Use `.dockerignore` to avoid copying unwanted files.

### ✅ How do you perform vulnerability scanning on Docker images in CI?
- Integrate tools like **Trivy**, **Grype**, **Snyk**, or **Aqua Security**.
- Scan during the build phase; fail builds if critical vulnerabilities are found.

### ✅ What are the risks of using base images from public registries?
- Potential vulnerabilities or outdated packages.
- Lack of auditing or security guarantees.
- Possibility of malicious code injection in unverified images.

---

## 🚀 Kubernetes & AKS (Azure Kubernetes Service)

### ✅ How do you troubleshoot CrashLoopBackOff errors in a pod?
- Use `kubectl describe pod` and `kubectl logs <pod>` to identify root cause.
- Check for failed liveness/readiness probes, misconfigured commands, or missing configs.

### ✅ What’s the difference between ClusterIP, NodePort, and LoadBalancer services?
- **ClusterIP**: Internal access only.
- **NodePort**: Exposes service on a port across all nodes.
- **LoadBalancer**: Exposes service via external Azure Load Balancer.

### ✅ How do you enforce resource limits and quotas at a namespace level?
- Define `LimitRange` and `ResourceQuota` in each namespace.
- Prevents resource abuse and ensures fair allocation.

### ✅ Explain how ConfigMaps and Secrets differ in Kubernetes and how to use them securely.
- **ConfigMaps** store non-sensitive configs (e.g., environment variables).
- **Secrets** store sensitive data (e.g., API keys) in base64 format.
- Mount securely via volumes or environment variables. Use external secret stores like Azure Key Vault.

---

## 🚀 Infrastructure as Code (Terraform)

### ✅ How do you structure your Terraform code for managing multiple environments?
- Use folders per environment (`dev/`, `prod/`) or workspaces.
- Share modules but customize variables per environment.
- Isolate backends and state files.

### ✅ What is a remote backend and how do you secure Terraform state in Azure?
- Store state in Azure Storage Account with encryption and RBAC.
- Enable blob versioning and soft delete.
- Lock state using the Azure Blob backend’s built-in locking.

### ✅ How do you implement tagging policies for all Azure resources via Terraform?
- Define tags as locals or variables.
- Use a `default_tags` block for consistency.
- Combine with Azure Policies to enforce tagging rules.

---

## 🚀 Monitoring, Logging & Alerts

### ✅ How do you use Azure Monitor and Log Analytics for centralized observability?
- Enable Azure Monitor on AKS and other services.
- Collect logs and metrics in Log Analytics workspace.
- Use Kusto Query Language (KQL) for analysis.

### ✅ What’s your approach to creating alert rules for high memory usage in AKS?
- Create metric-based alerts for container memory usage.
- Use Azure Monitor and define thresholds.
- Route alerts to action groups (email, SMS, Teams).

### ✅ How do you monitor pipeline failures and notify your team proactively?
- Enable pipeline email notifications.
- Use webhooks or Azure DevOps REST API to push alerts to Teams/Slack.
- Monitor pipeline health using Azure DevOps Analytics.

### ✅ What tools have you used to analyze logs across distributed microservices?
- **Azure Monitor + Application Insights** for tracing.
- **ELK stack (Elasticsearch, Logstash, Kibana)**.
- **Grafana with Loki**, Fluent Bit for log forwarding.

---

## 🚀 Scripting & Troubleshooting

### ✅ Write a Bash/Python script to monitor disk usage and send alerts.

```bash
#!/bin/bash
THRESHOLD=80
USAGE=$(df / | grep / | awk '{print $5}' | sed 's/%//')
if [ "$USAGE" -gt "$THRESHOLD" ]; then
  echo "Disk usage at $USAGE%" | mail -s "Disk Alert" admin@example.com
fi
```

### ✅ How do you automate log rotation and archival in Linux-based systems?
- Use `logrotate` with cron jobs.
- Configure rotation frequency, retention, compression.
- Archive old logs to remote storage.

### ✅ How would you troubleshoot slow container startup in AKS?
- Check image pull time and size.
- Analyze app initialization logs.
- Validate liveness and readiness probes.
- Monitor CPU/memory throttling.

### ✅ Describe how you’d debug a failed deployment that passes all pipeline stages but fails in production.
- Compare environment variables/configs between stages.
- Use `kubectl logs` and `kubectl describe` for pod-level diagnostics.
- Validate secrets and connectivity.
- Monitor with Application Insights or Log Analytics.

---
