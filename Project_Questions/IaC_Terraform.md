## Terraform Interview Guide

### 1. What is IaC (Infrastructure as Code)?

IaC is the practice of managing and provisioning IT infrastructure (like servers, networks, databases, and containers) using **configuration files** rather than manual processes or interactive tools.

* **Analogy:** Instead of manually clicking buttons in a cloud provider's console (like building with Lego blocks by hand), you write a detailed blueprint (code) that automatically builds the entire infrastructure every time.
* **Key Benefit:** It makes infrastructure provisioning **repeatable, consistent, version-controlled** (like software), and dramatically reduces the risk of human error. 

### 2. How does Terraform work?

Terraform is a tool that allows you to define infrastructure using its proprietary language, **HashiCorp Configuration Language (HCL)**. It follows a three-step process:

1.  **Write:** You write configuration files (`.tf` files) that define the desired state of your infrastructure (e.g., "I want an Nginx container on port 8081").
2.  **Plan:** Terraform executes the `plan` command, which compares the **desired state** (your HCL code) with the **current state** (what actually exists in the cloud or Docker). It creates an execution plan detailing exactly what needs to be added, changed, or destroyed.
3.  **Apply:** You approve the plan by running the `apply` command. Terraform executes the plan, calling the necessary APIs (via Providers) to provision the resources and reach the desired state.

### 3. What is the Terraform state file?

The Terraform state file (`terraform.tfstate`) is the **source of truth** for your infrastructure.

* **Purpose:** It is a JSON file that acts as a map, recording a snapshot of the *real-world* infrastructure that Terraform has provisioned and is currently managing. It maps the resources defined in your HCL code to the unique IDs of the actual resources (like the container ID).
* **Importance:** Terraform uses the state file during the `plan` phase to understand what already exists, enabling it to calculate the minimal changes required (the diff) to get to the desired state.
* **Best Practice:** It should **never** be manually edited, and for team collaboration, it must be stored remotely (e.g., in an S3 bucket or Azure Storage) and locked to prevent simultaneous changes.

### 4. What is the difference between `apply` and `plan`?

This is a critical distinction for safety in IaC:

| Feature | `terraform plan` | `terraform apply` |
| :--- | :--- | :--- |
| **Purpose** | **Inspection and Validation.** It shows what *will* happen. | **Execution and Provisioning.** It makes changes happen. |
| **Safety** | High (Read-only operation). | Low (Writes changes to infrastructure). |
| **Output** | Shows a detailed list of additions (`+`), changes (`~`), and destructions (`-`). | Shows real-time progress, then a summary of resources added/changed/destroyed. |
| **Analogy** | A construction manager's review of the blueprint before starting work. | The act of physically building the structure. |

### 5. What are Terraform Providers?

Terraform is a versatile tool because it relies on Providers.

* **Definition:** A Provider is a plugin that Terraform uses to understand and communicate with an external API (Application Programming Interface).
* **Function:** The Provider acts as a translator. When you define an AWS server or a Docker container in HCL, the Provider translates those commands into the correct API calls (e.g., to Docker's daemon or AWS's EC2 service).
* **Example:** In your recent task, you used the **Docker Provider** to talk to the Docker daemon. Other common ones include the AWS Provider, Azure Provider, and Kubernetes Provider.

### 6. What is Resource Dependency?

Resource dependency dictates the **order** in which Terraform creates, updates, or destroys resources.

* **Implicit Dependency (Automatic):** When one resource uses the output of another, Terraform automatically knows the order. For example, if you define a container that needs a network to exist first, the network will be created before the container. (This is the most common and safest way.)
* **Explicit Dependency (Manual):** If there is no data tie between two resources, but one must be created before the other for logical reasons, you use the `depends_on` argument.
    * **Example:** `resource "aws_server" "web" { depends_on = [aws_database.primary] }` ensures the database is fully ready before the server starts trying to connect to it.

### 7. How do you handle secret variables (passwords, API keys)?

Security is paramount in IaC, and you **never** commit secrets directly to version control (like GitHub).

* **Local Use (Avoid):** You should never store secrets in `terraform.tfvars` or hardcode them directly into `main.tf`.
* **Best Practices for Secrets:**
    * **Environment Variables:** Use environment variables (like `TF_VAR_db_password`) for local testing, which keeps the secret outside of your HCL code.
    * **Dedicated Secret Managers:** In a professional environment, you integrate Terraform with tools designed specifically for secrets management, such as:
        * HashiCorp Vault
        * AWS Secrets Manager
        * Azure Key Vault

### 8. Explain the benefits of Terraform (Summary)

Terraform provides significant advantages over manual infrastructure management:

* **Consistency:** Eliminates configuration drift by ensuring every environment (Dev, Test, Prod) is provisioned identically from the same code.
* **Automation:** Reduces manual work and speeds up the provisioning process from days/hours to minutes.
* **Version Control:** Infrastructure configurations are treated as code, allowing you to use Git for tracking changes, reviewing them, and rolling back easily.
* **Drift Detection:** By comparing the desired state (HCL) with the current state (State File), Terraform can highlight undocumented changes made manually by team members.
* **Modularity:** You can reuse blocks of code (Modules) across different projects, promoting efficiency and standardization.
* **Cloud Agnostic:** Terraform can manage infrastructure across multiple clouds (AWS, Azure, GCP) and platforms (Docker, Kubernetes) from a single tool and language.