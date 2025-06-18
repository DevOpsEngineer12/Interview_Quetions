## Azure DevOps & Release Engineering

### ✅ How do you create pipeline triggers based on variable conditions or custom flags?

Use conditional expressions in pipeline triggers and task conditions:

- For runtime conditionals:
  ```yaml
  trigger:
    branches:
      include:
        - main
  pr: none

  parameters:
    - name: deploy
      type: boolean
      default: false

  jobs:
    - job: Deploy
      condition: eq(parameters.deploy, true)
  ```
- You can also use variable groups or templates with conditions.

### ✅ How would you perform blue-green deployment in Azure DevOps for a web application hosted on Azure App Service?

- Deploy to a **staging slot** (green).
- Perform validation tests.
- Swap staging and production slots after validation.
- Use Azure CLI or `AzureWebApp` task:
  ```yaml
  - task: AzureWebApp@1
    inputs:
      appName: 'my-app'
      slotName: 'staging'
  - task: AzureAppServiceManage@0
    inputs:
      action: 'Swap Slots'
      sourceSlot: 'staging'
      targetSlot: 'production'
  ```

### ✅ What’s your approach to implementing pipeline approvals and RBAC in Azure DevOps?

- Define **environments** and configure **approvals** via pipeline YAML or UI.
- Example YAML:
  ```yaml
  environment:
    name: 'prod'
    resourceName: 'prod'
  ```
- Configure environment checks for **manual approval**.
- Use Azure DevOps RBAC to grant permission to specific users/groups.

### ✅ How do you handle parallel deployment of multiple microservices with dependencies?

- Use a **matrix** or define jobs with **dependsOn**.
- Break into **independent jobs** per microservice.
- Use `condition` to gate dependent jobs.
- Example:
  ```yaml
  jobs:
    - job: ServiceA
      ...
    - job: ServiceB
      dependsOn: ServiceA
      condition: succeeded()
  ```

---

## Kubernetes (AKS)

### ✅ How do you configure and rotate secrets stored in Kubernetes Secrets linked to Azure Key Vault?

- Use **Azure Key Vault Provider for Secrets Store CSI Driver**:
  - Install CSI driver and Azure identity.
  - Mount secrets as volumes.
  - Automatically rotated by Key Vault.
- Example SecretProviderClass:
  ```yaml
  apiVersion: secrets-store.csi.x-k8s.io/v1
  kind: SecretProviderClass
  metadata:
    name: azure-kv
  spec:
    provider: azure
    parameters:
      usePodIdentity: "false"
      keyvaultName: "mykv"
      objects:  |
        array:
          - objectName: secret1
            objectType: secret
  ```

### ✅ Explain the difference between livenessProbe, readinessProbe, and startupProbe — with use cases.

- **livenessProbe**: Checks if the container is alive. If it fails, Kubernetes restarts the pod. Used for recovery.
- **readinessProbe**: Checks if the pod is ready to receive traffic. Does not restart on failure. Used for load balancing.
- **startupProbe**: Used during app startup. Once it succeeds, liveness/readiness begin. Prevents premature restarts.

### ✅ How do you debug an intermittent network issue in a multi-namespace AKS setup?

- Use `kubectl exec` with `curl`, `ping`, or `dig`.
- Check `NetworkPolicy`, DNS, and service discovery.
- Use `kube-proxy`, CoreDNS logs.
- Use **network tools** like `netshoot`, `tcpdump`, and **Azure NSG flow logs**.

### ✅ What is a Helm post-renderer and how do you use it for customizing chart installs?

- A **post-renderer** allows you to modify the rendered YAML before applying to the cluster.
- Useful for injecting annotations/labels or enforcing policies.
- Example usage:
  ```bash
  helm install mychart ./chart --post-renderer ./kustomize-wrapper.sh
  ```

---

## Azure Cloud & IaC (Terraform / Bicep)

### ✅ How do you manage secrets securely in Terraform when provisioning AKS and Azure Key Vault?

- Store secrets in Azure Key Vault.
- Use `azurerm_key_vault_secret` to fetch at runtime.
- Use **environment variables** or external secret managers to avoid hardcoding.
- Use `sensitive = true` for outputs and variables.

### ✅ How do you version and deploy infrastructure across staging and production with Terraform?

- Use **Terraform workspaces** or separate state files.
- Tag resources with environment name.
- Implement CI/CD with pipelines using `terraform workspace select` and `plan/apply` per stage.
- Example folder structure:
  ```
  /infra
    /staging
    /production
  ```

### ✅ What is the difference between soft delete and purge protection in Azure Key Vault — and how do you enforce it using IaC?

- **Soft Delete**: Retains deleted Key Vault items for recovery.
- **Purge Protection**: Prevents permanent deletion even by admins.
- Enforce in Terraform:
  ```hcl
  purge_protection_enabled = true
  soft_delete_enabled      = true
  ```

### ✅ Describe how you manage module reusability and DRY (Don't Repeat Yourself) principles in Terraform.

- Create **modules** for reusable infrastructure (e.g., AKS, VNET).
- Use **locals**, **variables**, and **outputs**.
- Use **terragrunt** or templates for dynamic configurations.
- Store modules in a shared Git repo or Terraform Registry.

---

## Monitoring, Observability & Production Incidents

### ✅ How do you detect and troubleshoot memory leaks in containerized workloads on AKS?

- Monitor with **Prometheus** and **Grafana**.
- Analyze memory with `kubectl top` and container logs.
- Enable **heap dumps** and analyze with **VisualVM** or **MAT**.
- Set memory limits and use liveness probes to restart if needed.

### ✅ How would you set up alerts for failed deployments and unhealthy pods using Azure Monitor?

- Use **Container Insights** for AKS.
- Create **Log Analytics queries** for failure patterns.
- Define **alert rules**:
  - Failed deployments.
  - Pods in CrashLoopBackOff or NotReady.
- Integrate with **Action Groups** for notifications (email, ITSM, webhook).

### ✅ What’s your approach to root cause analysis when a high-priority incident occurs post-deployment?

- Gather logs, metrics, and alerts from App Insights, Azure Monitor.
- Correlate deployment changes with issues.
- Check pipeline runs, recent config changes.
- Perform **blameless postmortem**, document lessons learned.
- Add pipeline tests and rollback mechanisms for prevention.

