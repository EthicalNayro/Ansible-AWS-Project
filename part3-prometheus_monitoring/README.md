# AWS EC2 Automated Monitoring with Prometheus & Node Exporter (Ansible)

An automated deployment pipeline using Ansible Roles and Jinja2 templating to provision a complete Prometheus monitoring stack across AWS EC2 instances.

---

## 📌 Project Overview

This project automates the deployment of a system monitoring infrastructure:
- **Prometheus Server:** Deployed on the Control Node EC2 instance to collect, store, and query system metrics.
- **Target Node:** Monitored EC2 instance running `node_exporter` to expose host-level hardware and OS metrics.
- **Dynamic Scrape Configuration:** Utilizes Jinja2 templates (`.j2`) to automatically evaluate inventory targets and generate `prometheus.yml` scrape targets dynamically.

---

## 🏗️ Repository Hierarchy

```text
prometheus_monitoring/
├── inventory                   # Ansible inventory defining host groups
├── site.yml                    # Main Ansible playbook entrypoint
├── group_vars/                 # Group variables directory
└── roles/
    ├── prometheus/
    │   ├── tasks/
    │   │   └── main.yml        # Tasks to install, configure, and run Prometheus
    │   └── templates/
    │       ├── prometheus.service.j2 # Systemd unit template
    │       └── prometheus.yml.j2      # Jinja2 template for dynamic target discovery
    └── node_exporter/
        ├── tasks/
        │   └── main.yml        # Tasks to install and run Node Exporter
        └── templates/
            └── node_exporter.service.j2 # Systemd unit template
```

---

## ⚙️ Prerequisites

1. **AWS EC2 Instances (Ubuntu 22.04 LTS):**
   - **Prometheus Instance:** Serves as both Ansible Control Node & Prometheus host.
   - **Target Instance:** Monitored target running `node_exporter`.
2. **Security Group Rules:**
   - Port `22` (SSH) — Inbound from admin workstation / Ansible control node.
   - Port `9090` (Prometheus Web UI) — Inbound allowed on Prometheus instance.
   - Port `9100` (Node Exporter) — Inbound allowed from Prometheus instance.
3. **SSH Authentication:**
   - Key-based passwordless SSH access established between the Prometheus Instance and Target Instance.

---

## 🚀 Quick Start & Deployment

### 1. Clone the Repository
```bash
git clone <YOUR_REPOSITORY_URL>
cd prometheus_monitoring
```

### 2. Configure Inventory
Update the `inventory` file with your target server IP:

```ini
[prometheus]
localhost ansible_connection=local

[targets]
<TARGET_INSTANCE_IP> ansible_user=ubuntu
```

### 3. Run the Playbook
Execute the main playbook to deploy all roles across the infrastructure:

```bash
ansible-playbook -i inventory site.yml
```

---

## 📊 Verification & PromQL Queries

### 1. Access Prometheus Web UI
Open your browser and navigate to:
```text
http://<PROMETHEUS_PUBLIC_IP>:9090
```

### 2. Verify Targets Status
Navigate to **Status -> Targets** in the Prometheus web interface. Ensure the `node_exporter` job showing `<TARGET_IP>:9100` displays a state of **UP**.

### 3. Sample PromQL Queries

* **Target Availability Check:**
  ```promql
  up
  ```

* **CPU Utilization (%):**
  ```promql
  100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
  ```

* **Memory Usage (%):**
  ```promql
  (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100
  ```

---

## 🛠️ Roles Specification

### 1. `prometheus` Role
- Updates `apt` cache and installs required system dependencies (`wget`, `tar`, `apache2-utils`).
- Downloads and extracts Prometheus binary archive (v2.44.0).
- Renders `prometheus.yml` dynamically from `prometheus.yml.j2` based on inventory targets.
- Provisions `prometheus.service` systemd unit file and enables the service.

### 2. `node_exporter` Role
- Downloads and extracts Node Exporter binary archive (v1.6.1).
- Provisions `node_exporter.service` systemd unit file.
- Starts and enables `node_exporter` service exposing metrics on port 9100.
