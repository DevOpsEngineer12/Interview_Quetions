## CI/CD & Azure DevOps Pipelines

### ✅ How do you use YAML templates in Azure DevOps for multi-project pipelines?

- Create centralized reusable templates for build, test, deploy.
- Reference templates using `template` keyword:
  ```yaml
  - template: templates/build.yml
    parameters:
      buildConfiguration: 'Release'
  ```
- Promote standardization and reduce duplication across projects.

### ✅ How do you pass secrets securely between pipeline stages?

- Store secrets in Azure Key Vault or Pipeline variable groups marked as secret.
- Use `isOutput: true` to pass values between jobs:
  ```yaml
  jobs:
    - job: GenerateSecret
      variables:
        secretVar: $(mySecret)
      steps:
        - script: echo "##vso[task.setvariable variable=sharedSecret;isOutput=true]$secretVar"

    - job: UseSecret
      dependsOn: GenerateSecret
      variables:
        sharedSecret: $[ dependencies.GenerateSecret.outputs['sharedSecret'] ]
  ```

### ✅ What’s your approach to implementing environment-specific approvals in release pipelines?

- Define environments in Azure DevOps.
- Use **pre-deployment approvals** in environment settings.
- Assign approvers based on RBAC.
- Use `checks:` in YAML pipelines for environment gates.

### ✅ How would you create rollback logic if a deployment stage fails?

- Use a separate rollback job triggered on failure:
  ```yaml
  - job: rollback
    condition: failed()
    steps:
      - script: ./rollback.sh
  ```
- Store deployment metadata for rollback.
- Use Helm rollback or deploy previous artifact version.

---

## Kubernetes (AKS) & Container Orchestration

### ✅ How do you troubleshoot a pod stuck in CrashLoopBackOff state?

- Check logs using `kubectl logs <pod>`.
- Describe pod for event details:
  ```bash
  kubectl describe pod <pod>
  ```
- Look for issues like failing readiness probe, missing config, or permission errors.
- Reproduce locally using Docker for faster debugging.

### ✅ What are taints and tolerations, and how do you use them in node scheduling?

- **Taints** prevent pods from being scheduled unless they tolerate it.
- Use for dedicating nodes to specific workloads.
- Example:
  ```bash
  kubectl taint nodes node1 key=value:NoSchedule
  ```
  Add toleration in pod spec:
  ```yaml
  tolerations:
    - key: "key"
      operator: "Equal"
      value: "value"
      effect: "NoSchedule"
  ```

### ✅ How would you restrict pod-to-pod communication using Network Policies?

- Define `NetworkPolicy` to allow traffic from specific pods/namespaces.
  ```yaml
  kind: NetworkPolicy
  spec:
    podSelector:
      matchLabels:
        app: frontend
    ingress:
      - from:
          - podSelector:
              matchLabels:
                app: backend
  ```
- Ensure CNI plugin (like Azure CNI) supports network policies.

### ✅ How do you handle Helm chart upgrades with stateful workloads?

- Use `helm upgrade --atomic` to avoid broken upgrades.
- Take backups (e.g., PVC snapshots) before upgrades.
- Test upgrade in staging first.
- Use `post-upgrade` hooks for migration.

---

## Infrastructure as Code (Terraform & Bicep)

### ✅ How do you modularize Terraform code for DRY (Don't Repeat Yourself) principles?

- Break code into reusable modules:
  ```hcl
  module "network" {
    source = "./modules/network"
    vnet_name = "prod-vnet"
  }
  ```
- Store shared modules in Git or Terraform Registry.
- Pass only necessary inputs and expose outputs.

### ✅ What’s the difference between count and for\_each in Terraform?

- `count`: used with numeric values.
- `for_each`: used with maps/sets to create distinct resources.
- `for_each` provides better key-based indexing.

### ✅ How do you manage secure storage of Terraform state in Azure Blob backend?

- Use Azure Storage Account with state locking:
  ```hcl
  backend "azurerm" {
    storage_account_name = "tfstateacct"
    container_name       = "tfstate"
    key                  = "env.terraform.tfstate"
  }
  ```
- Enable RBAC and private access to Storage Account.

### ✅ How would you provision an AKS cluster with custom node pools using Terraform?

- Define `azurerm_kubernetes_cluster_node_pool` resources:
  ```hcl
  resource "azurerm_kubernetes_cluster_node_pool" "userpool" {
    name                  = "usernp"
    kubernetes_cluster_id = azurerm_kubernetes_cluster.aks.id
    vm_size               = "Standard_DS2_v2"
    node_count            = 2
    mode                  = "User"
  }
  ```
- Define default node pool in main AKS resource.

---

## Monitoring, Logging & Troubleshooting

### ✅ How do you set up custom alerts for AKS using Azure Monitor?

- Enable monitoring on AKS.
- Define alert rules for metrics (CPU, memory, etc.).
- Use log analytics queries for specific events.
- Route alerts to Action Groups (email, webhook, etc.).

### ✅ What tools do you use for log aggregation and visualization in Azure?

- Azure Monitor Logs / Log Analytics.
- Azure Monitor Workbook.
- Container insights.
- Grafana with Azure Monitor or Loki.

### ✅ How do you troubleshoot a degraded AKS node?

- Check node status with `kubectl get nodes`.
- SSH into the node (if VMSS access allowed).
- Look at `kubelet`, `containerd`, and system logs.
- Verify resource exhaustion or disk issues.

### ✅ How would you integrate Prometheus and Grafana with AKS in a production setup?

- Deploy Prometheus via Helm:
  ```bash
  helm install kube-prometheus-stack prometheus-community/kube-prometheus-stack
  ```
- Configure Grafana dashboards.
- Enable persistent volume for Prometheus.
- Expose Grafana via Ingress or Internal LB.

