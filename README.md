# AWS Infrastructure Automation & Monitoring Suite with Ansible

An end-to-end DevOps repository demonstrating automated provisioning, dynamic cloud host discovery, and monitoring infrastructure stack deployment on AWS EC2 using Ansible, Jinja2, Prometheus, and Nginx.

---

## 📌 Project Overview

This repository contains a modular 3-part DevOps solution for automating AWS cloud environments:
1. **Part 1 — Web Server Provisioning:** Automated Nginx web server deployment with dynamic Jinja2 templating and service management.
2. **Part 2 — Dynamic Inventory Management:** Automated AWS EC2 instance discovery using the `amazon.aws.aws_ec2` plugin based on cloud tags.
3. **Part 3 — Automated Monitoring Stack:** Complete Prometheus and Node Exporter deployment using Ansible Roles and dynamic Jinja2 target resolution.

---

## 🏗️ Repository Structure

```text
Ansible-AWS-Project/
├── part1-ansible-nginx-setup/       # Part 1: Ansible playbook & Jinja2 templates for Nginx
├── part2-ec2-dynamic-inventory/     # Part 2: AWS EC2 Dynamic Inventory plugin setup & tag filters
├── part3-prometheus_monitoring/     # Part 3: Prometheus & Node Exporter Ansible Roles suite
└── README.md                        # Main repository documentation
```

---

## 📑 Project Modules Breakdown

### 🛠️ Part 1: Nginx Web Server Automation (`part1-ansible-nginx-setup`)
* **Objective:** Automate web server installation and dynamic content deployment.
* **Key Components:**
  * Ansible Playbooks for `apt` package management and service state enforcement.
  * Jinja2 HTML templates (`index.html.j2`) injecting system facts (hostname, IP, OS facts) at runtime.
  * Idempotent task execution ensuring zero configuration drift.

---

### 🌐 Part 2: AWS EC2 Dynamic Inventory (`part2-ec2-dynamic-inventory`)
* **Objective:** Replace static IP management with automated cloud resource discovery.
* **Key Components:**
  * Configuration of `amazon.aws.aws_ec2` inventory plugin.
  * Host grouping dynamically driven by AWS resource tags (`Env=development`, `Role=web`).
  * Seamless SSH key authentication across auto-discovered cloud instances.

---

### 📊 Part 3: Prometheus & Node Exporter Stack (`part3-prometheus_monitoring`)
* **Objective:** Automated, role-based monitoring stack deployment across control node and target hosts.
* **Architecture:**
  * **Control Node / Prometheus Server:** Runs Ansible execution and hosts Prometheus Web UI (Port 9090).
  * **Target Host:** Monitored instance running `node_exporter` (Port 9100).
* **Key Components:**
  * **Ansible Roles Hierarchy:** Separation of concerns into `roles/prometheus` and `roles/node_exporter`.
  * **Dynamic Scrape Config:** Jinja2 loop (`prometheus.yml.j2`) dynamically generating target scrape endpoints from Ansible inventory.
  * **Systemd Integration:** Provisioned systemd service daemons for automated startup and lifecycle management.
  * **PromQL Metrics Validation:** Real-time metrics querying (CPU utilization, Memory usage, Host reachability).

---

## ⚙️ Prerequisites & Tech Stack

* **Cloud Infrastructure:** AWS EC2 (Ubuntu 22.04 LTS instances)
* **Automation Engine:** Ansible 2.14+
* **Monitoring & Metrics:** Prometheus (v2.44.0), Node Exporter (v1.6.1)
* **Web Services:** Nginx
* **Security & Auth:** Key-based passwordless SSH authentication (RSA 4096-bit), AWS Security Groups

---

## 🚀 How to Run Each Module

Navigate to the relevant directory and execute the playbook instructions specified in each subfolder's `README.md`:

```bash
# To run Part 1 (Nginx Deployment)
cd part1-ansible-nginx-setup
ansible-playbook -i inventory playbook.yml

# To run Part 2 (Dynamic Inventory)
cd part2-ec2-dynamic-inventory
ansible-inventory -i aws_ec2.yml --graph

# To run Part 3 (Prometheus Stack)
cd part3-prometheus_monitoring
ansible-playbook -i inventory site.yml
```
