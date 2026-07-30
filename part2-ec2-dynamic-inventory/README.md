# AWS EC2 Infrastructure Automation & Web Server Deployment

Automated dynamic node discovery and Apache web server deployment across AWS EC2 instances using Ansible, Python `boto3`, and the `amazon.aws.aws_ec2` inventory plugin.

---

## 📌 Project Overview

This repository provides an automated infrastructure management workflow designed to inspect and manage dynamic AWS EC2 instances without hardcoding IP addresses. 

### Key Capabilities
* **Dynamic Node Discovery:** Uses `amazon.aws.aws_ec2` inventory plugin to query AWS API real-time for instance states, public/private IPs, and tags.
* **EC2 Metadata Inspection:** Includes a dedicated playbook (`gather_ec2_info.yml`) to collect and report instance parameters, network configurations, and facts.
* **Idempotent Web Deployment:** Deploys and manages Apache HTTP servers (`install_apache.yml`), automatically resolving service conflicts (e.g., competing processes bound to port 80).
* **Cluster Verification:** Validates service status across target nodes via Ansible ad-hoc executions.

---

## 📁 Repository Structure

```text
.
├── ec2.aws_ec2.yml       # AWS EC2 Dynamic Inventory plugin configuration
├── gather_ec2_info.yml   # Playbook to collect dynamic metadata & EC2 facts
├── install_apache.yml    # Playbook for installing, configuring, and starting Apache
├── ansible.cfg           # Ansible environment configuration settings
├── .gitignore            # Security filtering (excludes *.pem keys and AWS credentials)
└── README.md             # Project documentation
