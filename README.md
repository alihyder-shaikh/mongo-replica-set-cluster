# MongoDB Replica Set Automation with Ansible & Podman

This project provides a **production-ready Ansible playbook** to deploy and manage a **MongoDB replica set** using **rootless Podman containers**.

The automation is designed to be **idempotent**, **secure**, and **environment-aware**, making it suitable for real-world DevOps and SRE workflows.

---

## 🚀 Features

- Rootless **Podman** installation and configuration
- MongoDB container deployment with persistent volumes
- Automatic **replica set initiation**
- Dynamic **replica member management**
- Environment-based inventories (`dev`, `prod`)
- Modular Ansible roles for clean separation of concerns
- Safe re-runs with replication state detection

---

## 📂 Project Structure

mongo-replica-set-cluster/
├── inventories/
│ ├── dev/
│ │ ├── hosts.ini
│ │ └── group_vars/
│ │ └── all.yml
│ └── prod/
├── roles/
│ ├── podman/ # Rootless Podman setup
│ ├── mongodb/ # MongoDB container deployment
│ └── replication/ # Replica set orchestration
├── site.yml # Entry point playbook
└── README.md


---

## ⚙️ How It Works

### 1️⃣ Podman Role
- Installs Podman and required dependencies
- Configures rootless containers using subuid/subgid mappings
- Enables user lingering for persistent services

### 2️⃣ MongoDB Role
- Pulls MongoDB image
- Renders container environment configuration
- Runs MongoDB containers with:
  - Port mapping
  - Persistent data & log volumes
  - Replica set configuration

### 3️⃣ Replication Role
- Checks current `rs.status()`
- Initiates replica set if not initialized
- Adds missing replica members dynamically
- Ensures idempotent behavior on re-runs

---

## 🧪 Inventory Example

```ini
[db_primary]
db1 ansible_host=server1 mongo_port=27017 mongo_container_name=mongo-primary

[db_replicas]
db2 ansible_host=server2 mongo_port=27018 mongo_container_name=mongo-replica-1
db3 ansible_host=server3 mongo_port=27019 mongo_container_name=mongo-replica-2

[all:vars]
ansible_user=ubuntu
ansible_become=true
