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

## Kubernetes (AKS) & Deployment Practices

### ✅ How would you perform a zero-downtime deployment in AKS using Helm?

- Use `helm upgrade` with readiness and liveness probes configured.
- Set `strategy.rollingUpdate` with maxUnavailable = 0.
- Use `preStop` hook and graceful termination.
- Optionally leverage Argo Rollouts for advanced strategies.

### ✅ What’s the difference between affinity and anti-affinity rules in pod scheduling?

| Type          | Description                                       |
| ------------- | ------------------------------------------------- |
| Affinity      | Schedule pods together on the same node           |
| Anti-Affinity | Avoid scheduling pods on the same node            |
| Use Case      | Grouping services vs. achieving high availability |

### ✅ How do you auto-scale AKS workloads based on CPU and memory?

- Enable Cluster Autoscaler for node pool scaling.
- Configure Horizontal Pod Autoscaler (HPA):
  ```yaml
  apiVersion: autoscaling/v2
  kind: HorizontalPodAutoscaler
  spec:
    metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          averageUtilization: 70
  ```

### ✅ What tools do you use to monitor and trace service communication within AKS?

- Azure Monitor for Containers
- Prometheus + Grafana
- Jaeger or OpenTelemetry for tracing
- Service Mesh (e.g., Istio) for deeper observability

---

## CI/CD Pipelines – Azure DevOps & GitOps

### ✅ How do you implement a gated check-in in Azure Repos?

- Enable branch policies with required reviewers.
- Add build validation using pipeline triggers.
- Require successful builds before merge.

### ✅ How do you integrate Terraform provisioning into Azure Pipelines?

- Use `Terraform CLI` or `Terraform Installer` task.
- Authenticate using Service Principal.
- Define `init`, `plan`, `apply` stages in pipeline.
- Store state in Azure backend.

### ✅ What’s your rollback strategy for a failed deployment in production?

- Use Helm rollback or previous artifact redeployment.
- Store release metadata (image tag, chart version).
- Trigger rollback pipeline stage using condition:
  ```yaml
  condition: failed()
  ```

### ✅ Compare Blue-Green and Canary deployments – when would you use each?

| Type       | Description                                | Use Case                              |
| ---------- | ------------------------------------------ | ------------------------------------- |
| Blue-Green | Two identical environments (only one live) | Fast rollback, major version upgrades |
| Canary     | Gradual traffic shift to new version       | Risk mitigation, real-time feedback   |

---

## Infrastructure as Code – Terraform

### ✅ How do you manage shared modules across teams in Terraform?

- Create versioned modules in shared Git repos or registries.
- Use semantic versioning (`source = git::...?ref=v1.2.0`).
- Enforce standards via PR validation.

### ✅ How do you structure Terraform code for multi-environment deployments?

- Use root folders per environment (`envs/dev`, `envs/prod`).
- Use shared modules directory.
- Pass environment-specific variables via tfvars.

### ✅ What’s your approach to state file locking and secure backend storage?

- Use Azure Storage Account backend with container-level access.
- Enable blob locking and encryption.
- Use RBAC and Key Vault for credential security.

### ✅ Explain how you use variables, locals, and outputs efficiently in large projects.

- **Variables**: external inputs (`terraform.tfvars`).
- **Locals**: computed values reused within modules.
- **Outputs**: export values to calling modules or pipelines.

---

## Troubleshooting & Debugging Scenarios

### ✅ A pod in AKS fails to mount a persistent volume. How do you troubleshoot it?

- Check PVC and PV status using `kubectl get pvc,pv`.
- Verify StorageClass and volume parameters.
- Check logs in `kube-controller-manager`.
- Use `kubectl describe pod` to view error messages.

### ✅ Your CI pipeline starts timing out. What steps will you take to identify the root cause?

- Check agent availability and system diagnostics.
- Review recent code changes and pipeline logs.
- Increase timeout temporarily for debugging.
- Split long-running jobs or optimize scripts.

### ✅ What is the impact of DNS resolution failure in Kubernetes and how to fix it?

- Pods may fail to connect to services or external endpoints.
- Check CoreDNS pod status and logs.
- Ensure `kube-dns` ConfigMap is valid.
- Restart DNS pods or nodes if needed.

### ✅ A Terraform apply fails due to dependency conflicts — how do you resolve it?

- Use `terraform graph` or `terraform plan` to inspect dependencies.
- Refactor resources with `depends_on` where needed.
- Split resources into logical modules.
- Ensure state consistency and correct order of creation.

---

