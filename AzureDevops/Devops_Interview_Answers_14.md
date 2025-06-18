
## CI/CD & Workflow Optimization

### ✅ How do you design a multi-stage YAML pipeline in Azure DevOps for microservices deployment?
- Define reusable YAML templates for build, test, and deploy.
- Use `stages`, `jobs`, and `steps` to modularize logic.
- Reference shared templates via `template` keyword.
- Use pipeline variables and environment conditions for stage-specific logic.
- Integrate with Key Vault and service connections securely.

### ✅ What’s the difference between build and release pipelines? When do you use classic vs YAML?
| Pipeline Type | Purpose | Use Case |
|---------------|---------|----------|
| Build Pipeline | Compile, test, package code | CI for microservices |
| Release Pipeline | Deploy artifacts | Legacy or manual approvals |
| YAML | Unified CI/CD | Preferred for IaC and automation |
| Classic | GUI-based | Used when migrating from legacy systems |

### ✅ How do you handle secret rotation in CI/CD without redeploying the entire pipeline?
- Store secrets in Azure Key Vault.
- Reference secrets dynamically during runtime.
- Use managed identity to access secrets.
- Avoid hardcoding secrets in pipeline YAML.

### ✅ Describe a time you resolved a pipeline stuck in a pending or failed state.
- Identified blocked agent pool due to offline agents.
- Cleared hanging jobs from the queue.
- Restarted self-hosted agent service.
- Fixed resource constraints on the host VM.

## 🚀 Kubernetes (AKS) & Container Strategy

### ✅ How do you perform a rolling restart of pods in a live production cluster?
- Use `kubectl rollout restart deployment <name>`.
- Ensures zero downtime via controlled replacement of pods.
- Leverage readiness probes and replicas to maintain availability.

### ✅ How do you debug a pod that fails to schedule due to node resource constraints?
- Check pod events with `kubectl describe pod`.
- Review `kubectl describe node` for allocatable resources.
- Use `kubectl top node` for real-time metrics.
- Adjust requests/limits or use Cluster Autoscaler.

### ✅ Explain how to configure HPA (Horizontal Pod Autoscaler) in AKS with real metrics.
- Define `cpu` or `memory` in resource requests/limits.
- Use:
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
spec:
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```
- Enable Metrics Server in the cluster.

### ✅ What’s your strategy for cluster upgrades with zero downtime in Azure Kubernetes Service?
- Use `aks upgrade` with `--node-image-only` for minor updates.
- Use `availability zones` and `multiple node pools`.
- Drain and upgrade nodes incrementally.
- Validate upgrade in staging before prod.

## 🚀 Azure Infrastructure & Security

### ✅ What’s the difference between Azure Blueprints and ARM templates?
| Feature | Azure Blueprints | ARM Templates |
|---------|------------------|---------------|
| Purpose | Governance at scale | Resource provisioning |
| Content | Policies, RBAC, Templates | Only resources |
| Use Case | Subscription governance | IaC deployments |

### ✅ How do you isolate workloads in Azure using VNets, NSGs, and Subnets?
- Use separate subnets per workload.
- Apply NSGs to filter traffic between subnets.
- Use VNet peering with route tables for segmentation.
- Leverage Azure Firewall for control.

### ✅ How do you implement identity-based access using Azure AD and RBAC for DevOps teams?
- Assign RBAC roles (e.g., Contributor, Reader) at appropriate scope.
- Use Azure AD groups for team-based access.
- Integrate with Azure DevOps using service principals or managed identities.

### ✅ Explain how you would handle disaster recovery across two Azure regions.
- Replicate resources using Azure Site Recovery or geo-redundant storage.
- Deploy infrastructure via ARM or Terraform in secondary region.
- Use Traffic Manager or Front Door for failover.
- Automate failover with runbooks.

## 🚀 Infrastructure as Code (Terraform)

### ✅ How do you implement CI for Terraform to check for drift and validate changes before apply?
- Use `terraform plan` in CI pipelines.
- Compare plan output against current state.
- Integrate with GitHub Actions or Azure Pipelines.
- Include manual approval before `terraform apply`.

### ✅ What’s your approach to managing secrets in Terraform (e.g., for Azure providers)?
- Use environment variables or encrypted tfvars.
- Store secrets in Azure Key Vault.
- Reference them securely in provider blocks.

### ✅ How do you organize Terraform state files securely in a large team environment?
- Store in Azure Blob Storage backend with state locking enabled.
- Use separate state per environment.
- Apply RBAC on the storage container.

### ✅ Explain your workflow for rolling back a faulty Terraform deployment in production.
- Use versioned infrastructure with Git tags.
- Roll back by re-applying last known good commit.
- Backup state file before major change.
- Use `terraform destroy` selectively if needed.

## 🚀 Monitoring, Logging & Observability

### ✅ How do you use Azure Log Analytics to create dashboards for AKS performance?
- Enable monitoring via Azure Monitor for Containers.
- Use prebuilt queries or KQL to create dashboards.
- Track CPU, memory, pod count, error rates.

### ✅ What’s the difference between real-time monitoring and log ingestion in Azure Monitor?
| Feature | Real-Time Monitoring | Log Ingestion |
|---------|----------------------|---------------|
| Use Case | Alerting, metrics | Deep troubleshooting |
| Tools | Metrics, alerts | Log Analytics, KQL |
| Latency | Near real-time | Delayed (ingested data) |

### ✅ How do you identify and troubleshoot memory leaks in containerized workloads?
- Use `kubectl top pods` or Prometheus.
- Analyze heap dumps or memory profiling.
- Set memory limits to detect OOMKilled errors.
- Restart pods and analyze logs.

### ✅ Have you integrated Prometheus with Azure Monitor or Grafana? If yes, how?
- Use Azure Monitor Managed Prometheus (Preview).
- Scrape metrics via Prometheus in AKS.
- Connect to Grafana via Azure Monitor plugin.
