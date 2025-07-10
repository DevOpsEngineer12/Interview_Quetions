# Azure & DevOps Engineer Interview Questions and Answers

## CI/CD with Azure DevOps & GitHub

### Q1. What is a CI/CD pipeline, and why is it important in DevOps?

**Answer:**  
A CI/CD pipeline automates the process of continuously integrating code changes, running tests, and deploying applications. CI (Continuous Integration) ensures code is regularly merged and validated, while CD (Continuous Delivery/Deployment) streamlines the rollout to production. This increases development speed, reduces manual errors, and ensures higher software quality.

### Q2. How do you implement a CI pipeline in Azure DevOps?

**Answer:**  
In Azure DevOps, a CI pipeline can be implemented using Azure Pipelines. You define the pipeline (using YAML or the classic editor), set up triggers (e.g. on code push or pull requests), and configure tasks for building, testing, and publishing artifacts. Integration with source control (e.g. GitHub or Azure Repos) is essential.

### Q3. Describe the stages in a typical Azure DevOps release pipeline.

**Answer:**  
Common stages include:  
- **Build**: Compile code and run tests.  
- **Release/Deploy**: Deploy artifacts to various environments (Dev, QA, Staging, Prod).  
- **Post-deployment**: Run smoke tests, health checks, or monitoring scripts.  
Gates and approvals may be set between stages for quality control.

### Q4. How do you manage pipeline as code using YAML in Azure DevOps?

**Answer:**  
Define pipeline steps and jobs in a YAML file (`azure-pipelines.yml`) stored in version control. This allows pipelines to be versioned, reviewed, and reused. YAML supports templates, variables, and conditional logic for robust configuration.

### Q5. How do you trigger a pipeline on a specific branch or tag in Azure DevOps?

**Answer:**  
Define triggers in the YAML file using the `trigger` (for branches) or `pr` (for pull requests) keywords. For tags, use the `trigger: tags` section. Example:
```yaml
trigger:
  branches:
    include:
      - main
  tags:
    include:
      - v*
```

### Q6. How would you integrate GitHub Actions with Azure?

**Answer:**  
Use Azure login and deployment actions from the GitHub Marketplace (e.g. `azure/login`, `azure/webapps-deploy`). Store credentials in GitHub Secrets and add workflow steps to build and deploy to Azure resources.

### Q7. How do you manage secrets in Azure DevOps or GitHub?

**Answer:**  
Store secrets in Azure DevOps Library variable groups (marked as secret) or in GitHub Actions Secrets. Reference them securely in pipelines to avoid exposing sensitive data in code or logs.

### Q8. What is an environment in Azure DevOps, and how is it used?

**Answer:**  
An environment in Azure DevOps is a logical deployment target (e.g. Dev, Test, Prod). Environments can track deployments, approvals, and resource health, and are used to manage releases and permissions for different stages.

### Q9. How can you perform canary or blue/green deployments using Azure DevOps?

**Answer:**  
Set up multiple deployment slots or environments, then incrementally route traffic (canary) or switch between two identical setups (blue/green). Use pipeline tasks, traffic managers, and approval gates to control rollout and validate each deployment phase.

### Q10. How do you handle rollbacks in Azure DevOps?

**Answer:**  
Maintain versioned artifacts and releases. If a deployment fails, redeploy a previous stable release or automate rollback scripts. Azure DevOps can be configured to redeploy known-good builds on failures.

### Q11. What’s the difference between classic and YAML pipelines?

**Answer:**  
Classic pipelines use a GUI for configuration, while YAML pipelines are defined as code in the repository. YAML enables versioning, reuse, and greater flexibility, and is preferred for modern DevOps practices.

### Q12. What are self-hosted agents and when would you use them?

**Answer:**  
Self-hosted agents run on your own infrastructure, providing control over the environment, installed tools, and network access. Use them for custom dependencies, private networking, or specialized hardware needs.

### Q13. How do you implement approvals and gates in Azure DevOps?

**Answer:**  
Configure approvals and gates in pipeline environments or release stages. Approvals require manual checks before progressing, while gates can include external checks, service health, or custom scripts.

### Q14. How do you manage pipeline variables and variable groups?

**Answer:**  
Variables can be set directly in YAML, in the pipeline UI, or in variable groups (for sharing across pipelines). Variable groups can link to Azure Key Vault for secrets management.

### Q15. How do you troubleshoot a failed Azure DevOps build?

**Answer:**  
Examine build logs, identify the failed step, review error messages, validate environment settings, and rerun with diagnostic logs if needed. Fix underlying issues and test locally before retrying.

### Q16. What is an artifact in the context of CI/CD?

**Answer:**  
An artifact is a build output (e.g., binaries, Docker images, packages) produced by the pipeline and used for deployments or sharing between stages.

### Q17. How do you publish and consume artifacts across pipelines?

**Answer:**  
Use `PublishBuildArtifacts` and `DownloadBuildArtifacts` tasks in Azure Pipelines, or use pipeline resources to share artifacts between pipelines and stages.

### Q18. How do you use GitHub Actions for multi-stage deployments?

**Answer:**  
Define multiple jobs and environments in the workflow YAML. Use `needs` for job dependencies, and configure environment protection rules for approvals and secrets.

### Q19. How do you integrate Azure Key Vault with Azure DevOps?

**Answer:**  
Connect Azure Key Vault to a Variable Group in Azure DevOps. Reference secrets by name in pipelines, enabling secure retrieval without exposing values.

### Q20. Describe a scenario where you optimized a CI/CD pipeline for faster delivery.

**Answer:**  
I parallelized test jobs, implemented caching for dependencies, and moved static analysis to pre-commit hooks. This reduced build time by 40% and enabled faster, more reliable deployments.

## Terraform & Infrastructure as Code (IaC)

### Q1. What is Infrastructure as Code (IaC), and what are its benefits?

**Answer:**  
IaC is the management of infrastructure using code and automation instead of manual processes. Benefits include repeatability, version control, reduced errors, faster provisioning, and improved collaboration.

### Q2. Describe the Terraform workflow (init, plan, apply, destroy).

**Answer:**  
- `init`: Initializes the working directory and downloads providers.
- `plan`: Previews changes Terraform will make.
- `apply`: Executes proposed changes.
- `destroy`: Removes all managed resources.

### Q3. What is a Terraform state file and why is it important?

**Answer:**  
The state file (`terraform.tfstate`) tracks resource mappings and metadata, allowing Terraform to manage updates and detect drift. It is essential for consistent and reliable infrastructure management.

### Q4. How do you manage Terraform state remotely?

**Answer:**  
Use remote backends like Azure Storage, AWS S3, or Terraform Cloud. Remote state enables team collaboration, locking, and secure state management.

### Q5. How do you handle secrets in Terraform?

**Answer:**  
Avoid hardcoding secrets. Use environment variables, integrate with secret managers (e.g., Azure Key Vault), or use Terraform Cloud’s sensitive variables feature.

### Q6. What is the difference between terraform import and terraform state?

**Answer:**  
`terraform import` brings existing resources under Terraform management. `terraform state` commands manipulate the state file (move, remove, list resources) directly.

### Q7. How do you structure a Terraform project for multiple environments (e.g., dev, test, prod)?

**Answer:**  
Use separate directories, workspaces, or variable files for each environment. Share common code via modules, and maintain environment-specific configuration separately.

### Q8. How do you manage module reuse in Terraform?

**Answer:**  
Create modules as reusable code blocks, publish them locally or in registries, and reference them in root configuration with different variables as needed.

### Q9. Explain the concept of workspaces in Terraform.

**Answer:**  
Workspaces allow multiple state files for the same configuration, enabling separate environments (e.g. dev, prod) without code duplication.

### Q10. What are Terraform providers and how are they used?

**Answer:**  
Providers are plugins enabling Terraform to manage resources in a specific platform (Azure, AWS, etc.). Define required providers in the configuration and use their resources.

### Q11. How do you upgrade a Terraform provider?

**Answer:**  
Update the provider version in the `required_providers` block and run `terraform init -upgrade` to download the latest version.

### Q12. How do you manage dependencies between resources in Terraform?

**Answer:**  
Terraform infers dependencies from references. Use `depends_on` for explicit ordering if needed.

### Q13. What is the use of depends_on in Terraform?

**Answer:**  
`depends_on` forces Terraform to create or destroy resources in a specific order, overriding automatic dependency inference.

### Q14. How do you write a custom Terraform module?

**Answer:**  
Create a directory with `main.tf`, `variables.tf`, and `outputs.tf`. Define resources and variables, then reference the module in the root configuration.

### Q15. What are some Terraform best practices?

**Answer:**  
Use modules, remote state, input validation, separate environments, version control, and avoid hardcoding sensitive data. Enable state locking and regular state backups.

### Q16. How do you version control Terraform configurations?

**Answer:**  
Store Terraform files in a Git repository, use branches and pull requests for changes, and tag releases for tracking.

### Q17. How do you handle drift detection with Terraform?

**Answer:**  
Run `terraform plan` to compare actual infrastructure with the state file. Use automation or monitoring to alert on drift.

### Q18. What’s the difference between terraform plan and terraform apply?

**Answer:**  
`terraform plan` previews changes without applying them. `terraform apply` executes the proposed changes.

### Q19. How do you manage shared infrastructure resources across teams?

**Answer:**  
Use modules and remote state data sources. Enforce RBAC, and separate state files or workspaces for isolation.

### Q20. How do you integrate Terraform with Azure DevOps?

**Answer:**  
Use Azure Pipelines with the Terraform extension. Define tasks for `init`, `plan`, and `apply`, and store state in Azure Storage.

### Q21. How do you create a Virtual Machine using Terraform?

**Answer:**  
Define an `azurerm_virtual_machine` resource, configure image, size, network, and credentials, then run `terraform apply`.

### Q22. How do you deploy an Azure App Service using Terraform?

**Answer:**  
Create `azurerm_app_service_plan` and `azurerm_app_service` resources, configure the app settings, and deploy using `terraform apply`.

### Q23. What is the benefit of using locals and variables in Terraform?

**Answer:**  
Variables enable parameterization and reusability. Locals help compute intermediate values, improving code clarity and maintainability.

### Q24. How do you use a remote backend in Terraform (e.g., Azure Storage)?

**Answer:**  
Define the `backend` block in the configuration, specify storage details, and run `terraform init` to migrate state.

### Q25. What are the key differences between ARM templates and Terraform?

**Answer:**  
ARM templates are Azure-specific and JSON-based. Terraform is multi-cloud, uses HCL, and supports modular, reusable, and more readable configurations.

## Azure IaaS/PaaS Services

### Q1. What’s the difference between Azure IaaS and PaaS?

**Answer:**  
IaaS provides raw infrastructure (VMs, networks), requiring OS and runtime management. PaaS offers managed services (App Service, SQL DB), abstracting infrastructure for easier development and scaling.

### Q2. How do you provision an Azure Virtual Machine with Terraform?

**Answer:**  
Define `azurerm_virtual_machine`, network, and storage resources in Terraform. Configure the VM size, image, and credentials, then apply the configuration.

### Q3. How do you deploy an Azure Function App using Terraform?

**Answer:**  
Create a Storage Account, App Service Plan, and `azurerm_function_app` resource. Configure app settings and deployment packages.

### Q4. What is the use of Application Insights in Azure?

**Answer:**  
Application Insights enables real-time monitoring, telemetry, and diagnostics for applications, helping detect and resolve issues quickly.

### Q5. How do you monitor application performance in Azure?

**Answer:**  
Leverage Application Insights, Azure Monitor, and Log Analytics to track metrics, logs, and set up alerts on performance thresholds.

### Q6. How would you configure autoscaling for an Azure App Service?

**Answer:**  
Enable autoscale in the App Service Plan. Define scaling rules based on metrics (CPU, memory, HTTP queue length), schedule, or custom conditions.

### Q7. How do you secure an Azure Function App using managed identity?

**Answer:**  
Enable managed identity in Function App settings, assign necessary Azure roles, and use the identity for secure resource access (e.g., Key Vault, Storage).

### Q8. What is Azure Container App and how is it different from Azure Kubernetes Service?

**Answer:**  
Azure Container Apps is a serverless container platform for microservices, abstracting away Kubernetes management. AKS is a full-managed Kubernetes cluster, offering more control and flexibility.

### Q9. How do you configure networking for an Azure VM?

**Answer:**  
Create a subnet, network security group, and public IP. Attach the VM to the subnet and configure NSGs for traffic rules.

### Q10. What is the difference between Azure App Service and Azure Function App?

**Answer:**  
App Service runs persistent web apps and APIs. Function App is serverless, event-driven, and scales automatically with per-execution billing.

### Q11. How do you use Azure Front Door or Application Gateway with your services?

**Answer:**  
Configure Front Door or Application Gateway to route, load balance, and secure incoming traffic to backend services. Define routing rules, health probes, and enable WAF if needed.

### Q12. How do you implement high availability in Azure?

**Answer:**  
Use Availability Sets, Zones, Load Balancers, and geo-replication. Architect applications for redundancy at compute, storage, and network layers.

### Q13. How do you back up and restore an Azure VM?

**Answer:**  
Use Azure Backup to schedule VM backups. Restore by creating a new VM from backups or restoring individual disks.

### Q14. What are Azure resource locks and how do they help?

**Answer:**  
Resource locks (`ReadOnly` and `Delete`) prevent accidental modification or deletion, improving governance and security.

### Q15. How do you use tags for resource management in Azure?

**Answer:**  
Assign key-value tags to resources for organization, cost management, automation, and policy enforcement. Use tags for reporting and access policies.

## Troubleshooting & Debugging

### Q1. How do you troubleshoot failed deployments in Azure DevOps?

**Answer:**  
Review pipeline logs, pinpoint failed steps, analyze error messages, check environment configurations, and rerun with diagnostics enabled.

### Q2. What steps do you take when a Terraform apply fails?

**Answer:**  
Read error output, check credentials and permissions, review the state file and dependencies, and fix issues before retrying.

### Q3. How do you identify issues in a VM that fails to provision?

**Answer:**  
Check deployment logs, Azure Activity Log, and boot diagnostics. Validate resource quotas, configurations, and network settings.

### Q4. How do you debug Azure Functions during runtime?

**Answer:**  
Enable Application Insights, review function logs and traces, use local debugging tools, and analyze invocation details.

### Q5. What is the process to troubleshoot performance issues in Azure App Service?

**Answer:**  
Analyze metrics (CPU, memory), enable Application Insights, review slow requests and dependencies, and optimize code or scaling settings.

### Q6. How do you investigate CI/CD pipeline slowness?

**Answer:**  
Profile each pipeline stage, identify bottlenecks, parallelize jobs, cache dependencies, and optimize build/test steps.

### Q7. What’s the process to revert infrastructure changes made with Terraform?

**Answer:**  
Revert configuration in version control and run `terraform apply`, or use `terraform destroy` for unwanted resources.

### Q8. How do you identify issues using Application Insights or Log Analytics?

**Answer:**  
Query logs for errors, examine traces and exceptions, set up alerts, and use dashboards for correlation and visualization.

### Q9. What logs do you check when an Azure deployment fails?

**Answer:**  
Check Activity Log, deployment logs, resource-specific logs, and diagnostics in Azure Monitor.

### Q10. How do you ensure logs are centralized for debugging?

**Answer:**  
Configure all resources to send logs to Log Analytics or a SIEM. Use diagnostic settings and log forwarding for aggregation.

## PowerShell / Azure CLI / Automation

### Q1. How do you use PowerShell to deploy Azure resources?

**Answer:**  
Use the `Az` PowerShell module, authenticate, and run commands like `New-AzResourceGroupDeployment` for ARM templates or direct resource creation.

### Q2. What is the difference between Azure PowerShell and Azure CLI?

**Answer:**  
Azure PowerShell is object-oriented and suited for automation in Windows environments. Azure CLI is cross-platform, command-based, and ideal for scripting and quick tasks.

### Q3. How do you write an idempotent PowerShell script?

**Answer:**  
Check for existing resources before creation, use `-WhatIf`, handle errors properly, and ensure repeated runs don’t create duplicates.

### Q4. How do you automate Azure DevOps using Azure CLI or REST API?

**Answer:**  
Use `az devops` CLI commands or call Azure DevOps REST APIs to automate pipeline, repo, and resource management tasks.

### Q5. How do you schedule scripts to run automatically in Azure?

**Answer:**  
Use Azure Automation Runbooks, Logic Apps, Functions (timer triggers), or scheduled pipelines in Azure DevOps.

### Q6. How do you pass secrets securely into a script?

**Answer:**  
Retrieve secrets at runtime from Azure Key Vault or environment variables, and avoid hardcoding sensitive data in scripts.

### Q7. How do you retrieve resource metadata using PowerShell?

**Answer:**  
Use commands like `Get-AzResource` or `Get-AzVM` to query and retrieve resource properties and metadata.

### Q8. How do you handle error handling in PowerShell scripts?

**Answer:**  
Implement `try/catch/finally` blocks, check `$Error`, log errors, and set `$ErrorActionPreference` as appropriate.

### Q9. How do you create and assign roles using PowerShell?

**Answer:**  
Use `New-AzRoleAssignment` to assign built-in or custom roles to users, groups, or service principals.

### Q10. How do you update or rotate keys/secrets using automation?

**Answer:**  
Schedule scripts or use Azure Automation to generate new secrets, update Key Vault, and notify or update dependent resources.

## Source Code Management / Git / Versioning

### Q1. What is Git branching strategy and which do you prefer (GitFlow, trunk-based)?

**Answer:**  
A branching strategy defines how code changes are managed. I prefer trunk-based development for fast-paced teams, but GitFlow is suitable for structured release cycles.

### Q2. How do you handle merge conflicts in Git?

**Answer:**  
Fetch latest changes, rebase or merge, resolve conflicts in the code, test locally, and commit resolved changes.

### Q3. What is the difference between rebase and merge?

**Answer:**  
`merge` combines histories by creating a new commit. `rebase` moves commits to a new base, creating a linear history and rewriting commit history.

### Q4. How do you enforce commit policies in Azure Repos?

**Answer:**  
Set branch policies (required reviews, builds, work item linking) to ensure code quality and compliance before merges.

### Q5. How do you link work items to commits in Azure DevOps?

**Answer:**  
Reference work item IDs in commit messages or pull requests (e.g., `#123`). Azure DevOps automatically links them.

### Q6. How do you ensure code quality before a pull request is approved?

**Answer:**  
Enforce branch policies, require successful builds, run static analysis, mandate code reviews, and use automated tests.

### Q7. How do you use Git submodules or monorepos?

**Answer:**  
Submodules allow including other repositories as dependencies. Monorepos store multiple projects in a single repo for easier management.

### Q8. What is the difference between a release tag and branch?

**Answer:**  
A release tag marks a specific commit as a versioned release. A branch is a movable pointer for ongoing development.

### Q9. How do you automate semantic versioning in CI/CD?

**Answer:**  
Use tools like `semantic-release` or pipeline scripts to increment versions based on commit messages and automatically tag releases.

### Q10. What are GitHub Codespaces and how can they help developers?

**Answer:**  
GitHub Codespaces provide cloud-based development environments for instant onboarding, consistent setups, and remote collaboration.

## Scenario-based / Behavioral / Conceptual

### Q1. Describe a time you optimized a CI/CD pipeline.

**Answer:**  
I parallelized test jobs, implemented caching, and split long-running tests. This reduced build time by 50%, increasing feedback speed and release frequency.

### Q2. How do you approach infrastructure provisioning across dev/test/prod?

**Answer:**  
Use parameterized IaC templates, separate state and access controls, and enforce RBAC and policies for isolation and consistency.

### Q3. What’s your strategy for securing infrastructure code?

**Answer:**  
Enforce code reviews, automate security scans, avoid hardcoded secrets, use secret managers, and follow least privilege principles.

### Q4. Describe a complex deployment issue you resolved.

**Answer:**  
Resolved a blue/green deployment issue where traffic routing failed due to misconfigured health probes. Fixed probe settings and automated validation in the pipeline.

### Q5. How do you collaborate with developers for smooth deployments?

**Answer:**  
Maintain open communication, document processes, use shared channels, provide feedback via code reviews, and automate deployment/testing for transparency.

### Q6. How do you ensure minimal downtime during infrastructure updates?

**Answer:**  
Use blue/green or canary deployments, rolling updates, health checks, and automate rollback mechanisms.

### Q7. What is your process for onboarding new team members to DevOps workflows?

**Answer:**  
Provide onboarding documentation, conduct walkthroughs, assign mentors, set permissions, and share best practices.

### Q8. How do you stay updated with Azure and DevOps tools?

**Answer:**  
Follow official docs, blogs, community forums, attend webinars, and participate in relevant user groups.

### Q9. What are the most important KPIs for DevOps success?

**Answer:**  
Deployment frequency, lead time to production, mean time to recovery (MTTR), change failure rate, and system uptime.

### Q10. Why do you want to work in DevOps, and what drives you?

**Answer:**  
I enjoy automating processes, solving complex problems, and enabling efficient, reliable software delivery. Continuous learning and collaboration drive my passion for DevOps.
