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

## CI/CD & Azure DevOps

### ✅ How do you manage multi-stage YAML pipelines in Azure DevOps?

- Define pipeline stages using `stages:` block.
- Use templates to separate logic and promote reusability.
- Configure approvals and conditions for each stage.
- Use `dependsOn` to control execution flow.

### ✅ What is the difference between classic release pipelines and YAML pipelines?

| Feature         | Classic Release Pipelines | YAML Pipelines               |
| --------------- | ------------------------- | ---------------------------- |
| UI              | GUI-based                 | Code-based (YAML)            |
| Version Control | Limited                   | Fully version-controlled     |
| Flexibility     | Less flexible             | Highly customizable          |
| Reusability     | Manual cloning            | Templates and reusable logic |

### ✅ How do you implement conditional deployment to different environments (Dev/Test/Prod)?

- Use `condition:` expression in stages/jobs.
- Evaluate pipeline variables (e.g., `Build.SourceBranchName`).
- Example:
  ```yaml
  condition: and(succeeded(), eq(variables['Build.SourceBranchName'], 'main'))
  ```

### ✅ Explain how you integrate Azure Key Vault secrets into a pipeline securely.

- Use Azure Key Vault task or variable group linked to Key Vault.
- Grant pipeline identity `Get` access on secrets.
- Secrets are injected into environment as secure variables.

---

## Docker & Containerization

### ✅ What is the benefit of using Alpine base images in Docker?

- Lightweight (\~5MB), reducing image size.
- Smaller attack surface for security.
- Faster build and deployment time.
- Ideal for microservices and minimal dependencies.

### ✅ How do you manage versioning of Docker images in CI workflows?

- Use Git commit SHA, tags, or build numbers as image tags.
- Push latest and specific version tags.
- Example:
  ```sh
  docker build -t myapp:$(Build.BuildId) .
  docker tag myapp:$(Build.BuildId) myapp:latest
  ```

### ✅ How would you build and push Docker images from an Azure DevOps pipeline?

- Use `Docker@2` task:
  ```yaml
  - task: Docker@2
    inputs:
      command: buildAndPush
      repository: myacr.azurecr.io/myapp
      dockerfile: Dockerfile
      containerRegistry: myACRConnection
      tags: latest
  ```

### ✅ How do you handle secret injection into containers during runtime?

- Use Kubernetes Secrets mounted as volumes or env vars.
- Use Azure Key Vault with CSI driver.
- Avoid hardcoding; use entrypoint scripts to load secrets dynamically.

---

## Kubernetes & AKS

### ✅ What’s the difference between ConfigMaps and Secrets in Kubernetes?

| Feature  | ConfigMap         | Secret                |
| -------- | ----------------- | --------------------- |
| Purpose  | Store config data | Store sensitive data  |
| Encoding | Plain text        | Base64 encoded        |
| Use Case | App settings      | Passwords, tokens     |
| Security | Not encrypted     | Better access control |

### ✅ How do you perform rolling updates in AKS with zero downtime?

- Use Deployment with rolling update strategy.
- Set `maxUnavailable=0` and `maxSurge=1`.
- Ensure readiness probes are defined.
- Monitor rollout with `kubectl rollout status`.

### ✅ What are init containers and where would you use them?

- Init containers run before app containers start.
- Useful for pre-setup tasks like config download, waiting for services.
- Defined under `initContainers` in PodSpec.

### ✅ How do you troubleshoot CrashLoopBackOff issues in pods?

- Describe pod using `kubectl describe pod <pod>`.
- Check logs with `kubectl logs <pod> -c <container>`.
- Validate readiness/liveness probes.
- Check image startup command, resource limits.

---

## Azure Infrastructure & Automation

### ✅ How do you provision infrastructure in Azure using Terraform?

- Write `.tf` files for resources (e.g., azurerm\_\*).
- Authenticate via Service Principal.
- Run `terraform init`, `plan`, `apply`.
- Use remote backend (Azure Storage) for state.

### ✅ What is the use of remote state in Terraform and how do you manage it securely?

- Remote state enables collaboration and state locking.
- Use Azure Storage with SAS/RBAC.
- Encrypt state at rest; avoid storing credentials in code.

### ✅ How do you apply resource tagging in Terraform for Azure cost control?

- Define `tags` block for each resource:
  ```hcl
  tags = {
    environment = "dev"
    owner       = "mahesh"
  }
  ```
- Use locals or variables for tag reuse.

### ✅ How do you manage identity and access in Azure across multiple environments?

- Use Azure AD groups and RBAC.
- Assign roles at subscription/resource group level.
- Leverage Azure Blueprints or Terraform for consistent RBAC setup.

---

## Real-World Debugging Scenarios

### ✅ What steps would you take if deployment to Azure Web App suddenly fails during CI?

- Review pipeline logs for errors.
- Check Azure DevOps agent availability and permissions.
- Validate App Service health and configuration.
- Test manual deployment to isolate CI issue.

### ✅ How do you monitor AKS logs and metrics effectively?

- Enable Azure Monitor for Containers.
- Use Log Analytics queries for logs.
- Monitor metrics with Grafana dashboards.
- Alert on thresholds (e.g., CPU, memory, pod restarts).

### ✅ What is your process when pipeline performance slows down drastically?

- Analyze pipeline stage duration.
- Review agent specs (CPU, memory).
- Optimize scripts, dependencies, and caching.
- Use parallel jobs and artifact reuse.

### ✅ How do you identify bottlenecks in CI/CD pipelines?

- Visualize pipeline timeline in Azure DevOps.
- Use job duration analytics.
- Profile long-running scripts or builds.
- Split monolithic pipelines into smaller reusable templates.

---

