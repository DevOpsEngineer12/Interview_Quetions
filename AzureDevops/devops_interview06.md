## CI/CD & Azure DevOps Pipelines

### ✅ How would you implement artifact versioning across multiple stages in Azure DevOps?

- Use build number or Git commit hash in the artifact name.
- Set version using `name` in the pipeline:
  ```yaml
  name: $(Build.BuildId)-$(Build.SourceBranchName)
  ```
- Publish and consume artifacts explicitly using `PublishBuildArtifacts` and `DownloadBuildArtifacts`.

### ✅ What’s the best way to trigger downstream pipelines only if upstream tests pass?

- Use `pipeline resources` with `trigger: true` only when tests succeed.
- Ensure test stage has `condition: succeeded()`:
  ```yaml
  resources:
    pipelines:
      - pipeline: upstream
        source: upstream-pipeline
        trigger:
          branches:
            include:
              - main
  ```

### ✅ How do you handle secrets rotation in pipelines without downtime?

- Integrate Azure Key Vault with variable groups.
- Access secrets dynamically using Key Vault references.
- Rotate in Key Vault, no pipeline change needed.
- Add retries or fallback mechanisms during transient access failures.

### ✅ How would you parallelize test execution to reduce pipeline time?

- Use `strategy.matrix` to split tests across jobs:
  ```yaml
  strategy:
    matrix:
      python3.8:
        python.version: '3.8'
      python3.9:
        python.version: '3.9'
  ```
- Alternatively, split test files and run in parallel containers.

---

## Infrastructure as Code (Terraform & Bicep)

### ✅ Compare Terraform and Bicep — when would you choose one over the other?

- **Terraform**:
  - Multi-cloud support.
  - Rich ecosystem/modules.
  - Use for complex, cross-cloud setups.
- **Bicep**:
  - Native Azure support.
  - Simpler syntax.
  - Use for Azure-only teams and tight ARM integration.

### ✅ How do you use remote state and state locking in Terraform with Azure backend?

- Use Azure Storage Account as backend:
  ```hcl
  backend "azurerm" {
    resource_group_name  = "tfstate-rg"
    storage_account_name = "tfstateacct"
    container_name       = "tfstate"
    key                  = "prod.terraform.tfstate"
  }
  ```
- State locking is handled automatically via blob leases.

### ✅ What is the approach to implement conditional resource creation in Terraform?

- Use `count` or `for_each` with condition:
  ```hcl
  resource "azurerm_public_ip" "example" {
    count = var.enable_ip ? 1 : 0
    ...
  }
  ```

### ✅ How would you handle secure storage of sensitive variables in Terraform?

- Use environment variables or secure pipeline variables.
- Use `sensitive = true` for inputs/outputs.
- Avoid committing secrets in VCS.
- Reference secrets from Azure Key Vault.

---

## Azure Cloud & Security

### ✅ What tools or services would you use to audit identity and access in Azure?

- Azure AD Sign-in logs.
- Azure Activity Logs.
- Microsoft Defender for Cloud.
- Azure Privileged Identity Management (PIM).

### ✅ How do you set up Private Link between an Azure Web App and a Key Vault?

- Enable Private Endpoint for the Key Vault.
- Configure Private DNS zone.
- Use VNET Integration in the Web App.
- Restrict public access to Key Vault.

### ✅ What’s the purpose of Azure Policy, and how do you enforce tag compliance?

- Azure Policy ensures resources follow governance rules.
- Use built-in policies like `Require tag and its value`.
- Assign policy to subscription/resource group.
- Example with Terraform:
  ```hcl
  policy_assignment {
    name                 = "enforce-tags"
    policy_definition_id = data.azurerm_policy_definition.tag_policy.id
  }
  ```

### ✅ How do you manage workload identities in Azure Kubernetes Service (AKS)?

- Use **Azure AD Workload Identity** or **Managed Identity**.
- Bind pod identity to Azure resources.
- Annotate pods with client ID.
- Securely access Key Vault, Storage, etc., without secrets.

---

## Containers & Kubernetes

### ✅ How do you patch a Kubernetes deployment with zero downtime?

- Use `kubectl patch` or `kubectl edit`.
- Ensure `RollingUpdate` strategy is used.
- Set `maxUnavailable=0` and `maxSurge=1` for smooth rollout.
- Validate readiness/liveness probes.

### ✅ What’s the difference between initContainers and sidecars?

- **initContainers**: Run before main containers. Used for setup tasks.
- **sidecars**: Run alongside the app. Used for logging, proxies, service mesh.

### ✅ How do you implement horizontal pod autoscaling based on queue depth in AKS?

- Expose custom metric (e.g., queue length) via Prometheus.
- Use `KEDA` (Kubernetes-based Event Driven Autoscaler).
- Deploy ScaledObject:
  ```yaml
  apiVersion: keda.sh/v1alpha1
  kind: ScaledObject
  spec:
    triggers:
    - type: azure-queue
      metadata:
        queueName: my-queue
  ```

### ✅ What steps would you take to troubleshoot DNS issues inside a Kubernetes cluster?

- Check CoreDNS pod logs.
- Run `nslookup` or `dig` from inside a pod.
- Verify DNS resolution via `kubectl exec`.
- Check `resolv.conf` and `kube-dns` config.
- Ensure correct service name/namespace.

