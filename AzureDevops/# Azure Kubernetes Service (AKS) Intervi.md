# Azure Kubernetes Service (AKS) Interview Questions & Answers

## AKS Architecture

1. **What is Azure Kubernetes Service (AKS)?**
   - Managed Kubernetes service by Azure, abstracts control plane management, simplifies cluster operations.

2. **Explain the AKS control plane and node pool separation.**
   - Control plane managed by Azure (free), node pools are user-managed VMs (billed).

3. **What are system and user node pools in AKS?**
   - System node pools run critical system pods; user node pools run application workloads.

4. **How does AKS handle Kubernetes version upgrades?**
   - Azure provides upgrade commands; supports node pool and cluster upgrades with minimal downtime.

5. **What is Virtual Node in AKS?**
   - Integrates with Azure Container Instances for burstable, serverless pods.

6. **How does AKS integrate with Azure Active Directory (AAD)?**
   - Supports AAD for Kubernetes RBAC and API authentication.

7. **What is the role of the kubelet in AKS?**
   - Agent on each node, manages pod lifecycle and node communication with control plane.

8. **How does AKS manage etcd?**
   - Azure manages etcd as part of the control plane; users have no direct access.

9. **What is the difference between managed and self-hosted Kubernetes on Azure?**
   - Managed (AKS) abstracts control plane, patching, scaling; self-hosted requires full management.

10. **How does AKS support multi-region deployments?**
    - AKS clusters are regional; multi-region requires multiple clusters and traffic management.

## Deployments & Scaling

11. **How do you deploy applications to AKS?**
    - Using `kubectl`, Helm, or CI/CD pipelines with manifests (YAML).

12. **What is a Deployment in Kubernetes?**
    - Manages stateless application replicas, rolling updates, and rollbacks.

13. **How do you perform a rolling update in AKS?**
    - Update deployment image/version; Kubernetes handles pod replacement gradually.

14. **What is Cluster Autoscaler in AKS?**
    - Automatically adjusts node count based on pending pods and resource needs.

15. **How do you scale pods in AKS?**
    - Manually (`kubectl scale`), or automatically with Horizontal Pod Autoscaler (HPA).

16. **What is the difference between HPA and VPA?**
    - HPA scales pod count; VPA adjusts pod resource requests/limits.

17. **How do you perform blue-green or canary deployments in AKS?**
    - Use multiple deployments/services or tools like Argo Rollouts/Flagger.

18. **How do you manage stateful workloads in AKS?**
    - Use StatefulSets and persistent volumes (Azure Disks/Files).

19. **How do you upgrade applications with zero downtime?**
    - Use rolling updates, readiness/liveness probes, and proper resource requests.

20. **How do you handle node pool upgrades?**
    - Upgrade node pools individually; cordon/drain nodes to minimize disruption.

## Networking

21. **What are the networking options in AKS?**
    - Kubenet (basic), Azure CNI (advanced, assigns VNet IPs to pods).

22. **How do you expose applications externally in AKS?**
    - Use Services of type LoadBalancer, Ingress controllers (e.g., NGINX, AGIC).

23. **What is an Ingress controller?**
    - Manages external HTTP/S access to services, supports routing, TLS, etc.

24. **How does AKS support private clusters?**
    - Control plane endpoint is private, accessible only within VNet.

25. **How do you implement network policies in AKS?**
    - Use Azure or Calico network policies to control pod-to-pod traffic.

26. **What is the role of Azure Application Gateway Ingress Controller (AGIC)?**
    - Integrates Azure Application Gateway as an Ingress for AKS.

27. **How do you enable DNS for services in AKS?**
    - CoreDNS runs as a deployment; services get DNS names within the cluster.

28. **How do you connect AKS to on-premises networks?**
    - Use VPN Gateway, ExpressRoute, or VNet peering.

29. **What is IP address management in AKS?**
    - Plan for pod/service IPs, use Azure CNI for VNet integration, manage IP exhaustion.

30. **How do you secure traffic between pods?**
    - Use network policies, mTLS (with service mesh), and private endpoints.

## Security

31. **How do you manage secrets in AKS?**
    - Kubernetes Secrets, Azure Key Vault integration, or CSI driver.

32. **How does RBAC work in AKS?**
    - Role-based access control for Kubernetes resources, integrates with AAD.

33. **How do you restrict access to the AKS API server?**
    - Use private clusters, authorized IP ranges, and AAD authentication.

34. **How do you implement pod security in AKS?**
    - Pod Security Policies (deprecated), Azure Policy, OPA/Gatekeeper.

35. **How do you scan container images for vulnerabilities?**
    - Use Azure Security Center, ACR Tasks, or third-party scanners.

36. **How do you enable encryption at rest in AKS?**
    - Azure-managed disks are encrypted; enable customer-managed keys for advanced scenarios.

37. **How do you enable encryption in transit?**
    - Use HTTPS, mTLS (service mesh), and secure Ingress controllers.

38. **How do you isolate workloads in AKS?**
    - Use namespaces, network policies, and separate node pools.

39. **How do you audit activity in AKS?**
    - Enable Kubernetes audit logs, Azure Monitor, and Azure Policy.

40. **How do you manage service account permissions?**
    - Use least privilege, bind roles to service accounts, and avoid default accounts.

## Monitoring & Troubleshooting

41. **How do you monitor AKS clusters?**
    - Azure Monitor, Container Insights, Prometheus/Grafana.

42. **How do you collect logs from AKS?**
    - Azure Log Analytics, Fluent Bit, or custom logging solutions.

43. **How do you troubleshoot pod failures?**
    - Use `kubectl describe`, `kubectl logs`, events, and resource metrics.

44. **How do you debug networking issues in AKS?**
    - Check network policies, service endpoints, DNS, and use `kubectl exec` for connectivity tests.

45. **How do you monitor cluster health?**
    - Node/pod status, resource usage, alerts via Azure Monitor.

46. **How do you handle node failures in AKS?**
    - Cluster autoscaler replaces unhealthy nodes; use node taints/tolerations.

## Helm & Service Mesh

47. **What is Helm and how is it used in AKS?**
    - Package manager for Kubernetes; deploys applications as charts.

48. **What is a service mesh and why use one in AKS?**
    - Layer for traffic management, security, and observability (e.g., Istio, Linkerd).

49. **How do you enable Istio in AKS?**
    - Install via Helm or manifests; configure sidecar injection and gateways.

50. **What are common production challenges in AKS and how do you address them?**
    - Scaling, upgrades, security, cost management, monitoring, and disaster recovery; use automation, policies, and best practices.

