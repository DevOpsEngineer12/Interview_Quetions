# Azure Kubernetes Service (AKS) Interview Questions & Answers

## AKS Architecture

1. **What is AKS?**  
   Managed Kubernetes service on Azure, abstracts control plane management.

2. **Describe the AKS control plane.**  
   Managed by Azure, includes API server, scheduler, etcd; not user-accessible.

3. **What are node pools in AKS?**  
   Groups of VMs (nodes) for running workloads; can have multiple pools.

4. **Difference between system and user node pools?**  
   System pools run critical system pods; user pools run application workloads.

5. **How does AKS handle Kubernetes version upgrades?**  
   Azure provides upgrade commands; supports node pool and cluster upgrades.

6. **What is a Virtual Node in AKS?**  
   Integrates with Azure Container Instances for serverless, burstable pods.

7. **How does AKS integrate with Azure AD?**  
   Supports AAD for Kubernetes RBAC and API authentication.

8. **What is the role of kubelet in AKS?**  
   Node agent managing pod lifecycle and node communication.

9. **How is etcd managed in AKS?**  
   Azure manages etcd as part of the control plane.

10. **AKS vs self-hosted Kubernetes on Azure?**  
    AKS abstracts control plane, patching, scaling; self-hosted requires full management.

11. **How does AKS support multi-region deployments?**  
    Deploy multiple clusters in different regions; use Azure Traffic Manager.

12. **What is the default orchestrator version in AKS?**  
    The latest stable Kubernetes version supported by Azure.

13. **How do you manage node OS upgrades in AKS?**  
    Azure provides node image upgrades; can be automated or manual.

14. **What is the maximum number of nodes per AKS cluster?**  
    Varies by region and SKU; typically up to 5000 nodes.

15. **How does AKS handle high availability?**  
    Multi-zone support, multiple node pools, managed control plane.

16. **What is the AKS API server endpoint?**  
    The public/private endpoint for cluster management.

17. **How do you enable private clusters in AKS?**  
    Configure during creation; API server accessible only within VNet.

18. **What is Azure Policy for AKS?**  
    Enforces governance and compliance on AKS resources.

19. **How does AKS support Windows containers?**  
    By adding Windows node pools to the cluster.

20. **What is the role of CoreDNS in AKS?**  
    Provides DNS resolution for services and pods.

## Deployments & Scaling

21. **How do you deploy applications to AKS?**  
    Using `kubectl`, Helm, or CI/CD pipelines with YAML manifests.

22. **What is a Deployment in Kubernetes?**  
    Manages stateless application replicas, rolling updates, and rollbacks.

23. **How do you perform a rolling update in AKS?**  
    Update deployment image/version; Kubernetes handles gradual pod replacement.

24. **What is Cluster Autoscaler in AKS?**  
    Automatically adjusts node count based on pending pods and resource needs.

25. **How do you scale pods in AKS?**  
    Manually (`kubectl scale`) or automatically with Horizontal Pod Autoscaler (HPA).

26. **Difference between HPA and VPA?**  
    HPA scales pod count; VPA adjusts pod resource requests/limits.

27. **How do you perform blue-green or canary deployments in AKS?**  
    Use multiple deployments/services or tools like Argo Rollouts/Flagger.

28. **How do you manage stateful workloads in AKS?**  
    Use StatefulSets and persistent volumes (Azure Disks/Files).

29. **How do you upgrade applications with zero downtime?**  
    Use rolling updates, readiness/liveness probes, and proper resource requests.

30. **How do you handle node pool upgrades?**  
    Upgrade node pools individually; cordon/drain nodes to minimize disruption.

31. **What is a DaemonSet?**  
    Ensures a copy of a pod runs on all (or some) nodes.

32. **How do you use Jobs and CronJobs in AKS?**  
    For batch and scheduled tasks.

33. **What is a ReplicaSet?**  
    Maintains a stable set of replica pods.

34. **How do you rollback a deployment in AKS?**  
    Use `kubectl rollout undo deployment/<name>`.

35. **How do you manage application configuration in AKS?**  
    Use ConfigMaps and Secrets.

36. **How do you use init containers?**  
    Run setup tasks before main containers start.

37. **How do you manage resource requests and limits?**  
    Define in pod specs to control CPU/memory usage.

38. **What is a PodDisruptionBudget?**  
    Limits the number of pods down during voluntary disruptions.

39. **How do you schedule pods on specific nodes?**  
    Use node selectors, affinities, and taints/tolerations.

40. **How do you use labels and annotations?**  
    For organizing, selecting, and describing Kubernetes objects.

## Networking

41. **What are the networking options in AKS?**  
    Kubenet (basic), Azure CNI (advanced, assigns VNet IPs to pods).

42. **How do you expose applications externally in AKS?**  
    Use Services of type LoadBalancer, Ingress controllers.

43. **What is an Ingress controller?**  
    Manages external HTTP/S access to services, supports routing, TLS, etc.

44. **How does AKS support private clusters?**  
    Control plane endpoint is private, accessible only within VNet.

45. **How do you implement network policies in AKS?**  
    Use Azure or Calico network policies to control pod-to-pod traffic.

46. **What is the role of Azure Application Gateway Ingress Controller (AGIC)?**  
    Integrates Azure Application Gateway as an Ingress for AKS.

47. **How do you enable DNS for services in AKS?**  
    CoreDNS runs as a deployment; services get DNS names within the cluster.

48. **How do you connect AKS to on-premises networks?**  
    Use VPN Gateway, ExpressRoute, or VNet peering.

49. **What is IP address management in AKS?**  
    Plan for pod/service IPs, use Azure CNI for VNet integration, manage IP exhaustion.

50. **How do you secure traffic between pods?**  
    Use network policies, mTLS (with service mesh), and private endpoints.

51. **What is a Service of type ClusterIP?**  
    Exposes service on a cluster-internal IP.

52. **What is a Service of type NodePort?**  
    Exposes service on each node’s IP at a static port.

53. **How do you use ExternalName services?**  
    Maps service to DNS name outside the cluster.

54. **How do you troubleshoot DNS issues in AKS?**  
    Check CoreDNS logs, use `nslookup`/`dig` inside pods.

55. **How do you restrict egress traffic from AKS?**  
    Use Azure Firewall, NSGs, or egress network policies.

56. **How do you enable multi-tenancy in AKS networking?**  
    Use namespaces, network policies, and separate node pools.

57. **What is the maximum number of pods per node in AKS?**  
    Depends on VM size and network plugin; typically up to 250.

58. **How do you use static IPs for services in AKS?**  
    Reserve public IP in Azure and reference in service manifest.

59. **How do you use Azure Private Link with AKS?**  
    For private connectivity to Azure services.

60. **How do you monitor network traffic in AKS?**  
    Use Azure Network Watcher, Container Insights, or third-party tools.

## Security

61. **How do you manage secrets in AKS?**  
    Kubernetes Secrets, Azure Key Vault integration, or CSI driver.

62. **How does RBAC work in AKS?**  
    Role-based access control for Kubernetes resources, integrates with AAD.

63. **How do you restrict access to the AKS API server?**  
    Use private clusters, authorized IP ranges, and AAD authentication.

64. **How do you implement pod security in AKS?**  
    Pod Security Policies (deprecated), Azure Policy, OPA/Gatekeeper.

65. **How do you scan container images for vulnerabilities?**  
    Use Azure Security Center, ACR Tasks, or third-party scanners.

66. **How do you enable encryption at rest in AKS?**  
    Azure-managed disks are encrypted; enable customer-managed keys for advanced scenarios.

67. **How do you enable encryption in transit?**  
    Use HTTPS, mTLS (service mesh), and secure Ingress controllers.

68. **How do you isolate workloads in AKS?**  
    Use namespaces, network policies, and separate node pools.

69. **How do you audit activity in AKS?**  
    Enable Kubernetes audit logs, Azure Monitor, and Azure Policy.

70. **How do you manage service account permissions?**  
    Use least privilege, bind roles to service accounts, and avoid default accounts.

71. **How do you use Azure AD Pod Identity?**  
    Assign managed identities to pods for secure Azure resource access.

72. **How do you restrict container capabilities?**  
    Use securityContext in pod specs.

73. **How do you prevent privilege escalation in pods?**  
    Set `allowPrivilegeEscalation: false` in securityContext.

74. **How do you enforce image provenance in AKS?**  
    Use Azure Policy to restrict allowed container registries.

75. **How do you rotate secrets in AKS?**  
    Update Kubernetes Secrets or integrate with Key Vault for automatic rotation.

76. **How do you secure Helm deployments?**  
    Use RBAC, restrict Tiller (Helm v2), or use Helm v3 (no Tiller).

77. **How do you use Azure Defender for AKS?**  
    Provides threat protection and security recommendations.

78. **How do you secure node access in AKS?**  
    Use SSH keys, restrict NSG rules, and use Azure Bastion.

79. **How do you manage image pull secrets?**  
    Store Docker registry credentials as Kubernetes Secrets.

80. **How do you enforce resource quotas in AKS?**  
    Define ResourceQuota objects per namespace.

## Monitoring & Troubleshooting

81. **How do you monitor AKS clusters?**  
    Azure Monitor, Container Insights, Prometheus/Grafana.

82. **How do you collect logs from AKS?**  
    Azure Log Analytics, Fluent Bit, or custom logging solutions.

83. **How do you troubleshoot pod failures?**  
    Use `kubectl describe`, `kubectl logs`, events, and resource metrics.

84. **How do you debug networking issues in AKS?**  
    Check network policies, service endpoints, DNS, and use `kubectl exec` for connectivity tests.

85. **How do you monitor cluster health?**  
    Node/pod status, resource usage, alerts via Azure Monitor.

86. **How do you handle node failures in AKS?**  
    Cluster autoscaler replaces unhealthy nodes; use node taints/tolerations.

87. **How do you monitor application performance in AKS?**  
    Use Application Insights, Prometheus, and custom metrics.

88. **How do you set up alerts for AKS?**  
    Azure Monitor alerts, Prometheus Alertmanager.

89. **How do you view resource usage in AKS?**  
    Use `kubectl top`, Azure Monitor, or Grafana dashboards.

90. **How do you troubleshoot cluster upgrades?**  
    Review upgrade logs, check node/pod status, and validate workloads post-upgrade.

## Helm & Service Mesh

91. **What is Helm and how is it used in AKS?**  
    Package manager for Kubernetes; deploys applications as charts.

92. **How do you create a Helm chart?**  
    Use `helm create <chart-name>` and customize templates.

93. **How do you manage Helm releases in AKS?**  
    Use `helm install`, `helm upgrade`, `helm rollback`, and `helm list`.

94. **How do you use Helm repositories?**  
    Add with `helm repo add`; fetch charts from public/private repos.

95. **How do you use Helm values files?**  
    Override default chart values during install/upgrade.

96. **What is a service mesh and why use one in AKS?**  
    Layer for traffic management, security, and observability (e.g., Istio, Linkerd).

97. **How do you enable Istio in AKS?**  
    Install via Helm or manifests; configure sidecar injection and gateways.

98. **What are the benefits of using a service mesh in AKS?**  
    mTLS, traffic shaping, observability, retries, circuit breaking.

99. **How do you monitor service mesh traffic in AKS?**  
    Use built-in dashboards (e.g., Kiali for Istio), Prometheus, Grafana.

100. **What are common production challenges in AKS and how do you address them?**  
     Scaling, upgrades, security, cost management, monitoring, disaster recovery; use automation, policies, and best practices.

