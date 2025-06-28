# DevOps Interview Q&A (3-5 Years Experience)

## Azure Repos & Pipelines

**Q: How do you enforce branch policies and ensure PR validation in Azure Repos?**  
A: Enforce branch policies by requiring pull request (PR) reviews, setting build validation (linking a pipeline to run on PRs), and enabling status checks. Use required reviewers, enforce work item linking, and block direct pushes to protected branches.

**Q: Explain how pipeline caching improves build performance.**  
A: Pipeline caching stores dependencies or build outputs from previous runs. On subsequent builds, the cache is restored, reducing time spent on tasks like package installation or compilation, thus speeding up the build process.

**Q: What’s the purpose of pipeline templates in YAML-based Azure DevOps workflows?**  
A: Pipeline templates promote reuse and consistency by allowing you to define common steps, jobs, or stages in a separate YAML file, which can be referenced across multiple pipelines.

---

## ☸️ Kubernetes (AKS) & Microservices

**Q: How do you implement horizontal pod autoscaling in AKS?**  
A: Deploy the Horizontal Pod Autoscaler (HPA) resource, specifying target metrics (CPU/memory/custom). Ensure the Metrics Server is installed. HPA automatically scales pods based on observed metrics.

**Q: How would you isolate traffic between environments in a shared AKS cluster?**  
A: Use Kubernetes namespaces for logical isolation, apply network policies to restrict traffic, and leverage Azure Network Policies or Calico for fine-grained control. Optionally, use separate ingress controllers per environment.

**Q: What is the difference between ClusterIP, NodePort, and LoadBalancer in Kubernetes?**  
A:  
- **ClusterIP:** Default, exposes service internally within the cluster.  
- **NodePort:** Exposes service on a static port on each node’s IP.  
- **LoadBalancer:** Provisions an external load balancer (e.g., Azure LB) to expose the service publicly.

---

## 📦 Docker & Container Lifecycle

**Q: How do you optimize a Docker image for production?**  
A: Use minimal base images (e.g., Alpine), multi-stage builds, `.dockerignore` files, minimize layers, avoid unnecessary packages, and pin dependency versions.

**Q: Explain how volumes work in Docker and when to use bind mounts vs named volumes.**  
A: Volumes persist data outside the container lifecycle.  
- **Bind mounts:** Map host directories/files; use for development or when direct host access is needed.  
- **Named volumes:** Managed by Docker; use for production data persistence and portability.

**Q: What’s your approach for handling logging in containers running in production?**  
A: Write logs to stdout/stderr, use a centralized logging solution (e.g., ELK, Azure Monitor), and avoid writing logs to local files inside containers.

---

## ☁️ Azure Cloud Architecture

**Q: How do you implement high availability for a multi-region app in Azure?**  
A: Deploy app instances in multiple Azure regions, use Azure Traffic Manager or Front Door for global load balancing, and replicate data across regions.

**Q: What’s the difference between Azure Key Vault and Managed Identity?**  
A:  
- **Key Vault:** Securely stores secrets, keys, and certificates.  
- **Managed Identity:** Provides an automatically managed identity for Azure resources to authenticate to services (like Key Vault) without credentials.

**Q: How would you set up automated scaling for Azure App Services?**  
A: Configure autoscale rules based on metrics (CPU, memory, HTTP queue length) in the Azure portal or via ARM templates, specifying min/max instance counts and scaling thresholds.

---

## 🛡️ Cloud Security & Identity

**Q: What is the Shared Responsibility Model in Azure?**  
A: Azure secures the underlying cloud infrastructure, while customers are responsible for securing their data, identities, applications, and configurations.

**Q: How do you use Azure Defender for cloud workload protection?**  
A: Enable Azure Defender in Security Center, configure policies, and monitor alerts for threats across VMs, containers, databases, and other resources.

**Q: How do you secure access to Azure DevOps artifacts and feeds?**  
A: Use Azure AD-based authentication, set granular permissions on feeds, enable PAT/token expiration policies, and audit access regularly.

---

## 📊 Monitoring, Telemetry & Resilience

**Q: What is Azure Monitor Logs and how does it differ from Metrics?**  
A:  
- **Logs:** Store detailed, queryable event and diagnostic data (structured/unstructured).  
- **Metrics:** Store numeric time-series data for near real-time monitoring.

**Q: How do you implement health probes for microservices in AKS?**  
A: Define `livenessProbe` and `readinessProbe` in pod specs, using HTTP, TCP, or command checks to monitor service health and availability.

**Q: How would you monitor SLA compliance for a business-critical service?**  
A: Set up Azure Monitor alerts on key metrics (uptime, response time), use Application Insights for end-to-end monitoring, and generate SLA compliance reports from