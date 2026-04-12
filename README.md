# private-cloud-openstack
Build your own private cloud from scratch.  Full OpenStack 2024.1 deployment on 2 nodes using  Kolla-Ansible, Docker, Nova, Neutron, Cinder and Horizon.

<div align="center">

```
╔═══════════════════════════════════════════════════════════════════╗
║                                                                   ║
║     ██████╗ ██████╗ ███████╗███╗   ██╗███████╗████████╗ █████╗   ║
║    ██╔═══██╗██╔══██╗██╔════╝████╗  ██║██╔════╝╚══██╔══╝██╔══██╗  ║
║    ██║   ██║██████╔╝█████╗  ██╔██╗ ██║███████╗   ██║   ███████║  ║
║    ██║   ██║██╔═══╝ ██╔══╝  ██║╚██╗██║╚════██║   ██║   ██╔══██║  ║
║    ╚██████╔╝██║     ███████╗██║ ╚████║███████║   ██║   ██║  ██║  ║
║     ╚═════╝ ╚═╝     ╚══════╝╚═╝  ╚═══╝╚══════╝   ╚═╝   ╚═╝  ╚═╝  ║
║                                                                   ║
║              P R I V A T E   C L O U D   P L A T F O R M         ║
║                                                                   ║
╚═══════════════════════════════════════════════════════════════════╝
```

# OpenStack Private Cloud Infrastructure

### Production-grade Private Cloud built on OpenStack 2024.1 — deployed via Kolla-Ansible on a real 2-node Controller-Compute architecture.

<br/>

[![OpenStack](https://img.shields.io/badge/OpenStack-2024.1_Caracal-ED1944?style=for-the-badge&logo=openstack&logoColor=white)](https://www.openstack.org/)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04_LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Ansible](https://img.shields.io/badge/Ansible-2.15-EE0000?style=for-the-badge&logo=ansible&logoColor=white)](https://www.ansible.com/)
[![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org/)
[![License](https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge)](LICENSE)

<br/>

> *"Most students deploy applications on cloud platforms. This project builds the cloud platform itself."*

<br/>

</div>

---

## Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Technology Stack](#-technology-stack)
- [OpenStack Services](#-openstack-services-deployed)
- [Hardware Requirements](#-hardware-requirements)
- [Quick Start](#-quick-start)
- [Installation Guide](#-installation-guide)
- [Known Issues & Fixes](#-known-issues--fixes)
- [Demo Guide](#-demo-guide)
- [Project Results](#-project-results)
- [Author](#-author)

---

## Overview

This project is a **fully functional private cloud infrastructure** built from scratch using OpenStack — the same open-source platform that powers the infrastructure of **CERN, Walmart, BMW**, and hundreds of Fortune 500 enterprises worldwide.

The cloud was deployed on **2 physical machines** acting as real data center nodes, containerized entirely with Docker, and automated using Kolla-Ansible.

```
What this cloud can do:

  ✦  Provision virtual machines in under 60 seconds
  ✦  Support multiple isolated users via role-based access control
  ✦  Attach and detach persistent block storage (like AWS EBS)
  ✦  Manage everything via a web browser dashboard
  ✦  Access via REST API and OpenStack CLI
  ✦  Scale horizontally by adding more compute nodes
```

**Cost Comparison:**

| Feature              | AWS / Azure          | This Private Cloud      |
|----------------------|----------------------|-------------------------|
| VM Provisioning      | ✅ Yes               | ✅ Yes                  |
| Block Storage        | ✅ Yes               | ✅ Yes                  |
| Web Dashboard        | ✅ Yes               | ✅ Yes                  |
| Virtual Networking   | ✅ Yes               | ✅ Yes                  |
| Monthly Cost         | ₹8,000 – ₹15,000     | ₹0 recurring            |
| Data Sovereignty     | ❌ Third-party       | ✅ On-premise           |
| Full Customization   | ❌ Limited           | ✅ Complete control     |

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                        NETWORK LAYER                                │
│                     [ 10.134.227.0/24 ]                             │
└───────────────────────┬─────────────────────────┬───────────────────┘
                        │                         │
           ┌────────────▼──────────┐   ┌──────────▼────────────┐
           │   CONTROLLER NODE     │   │    COMPUTE NODE       │
           │   12GB RAM  |  i5     │   │   16GB RAM  |  i5     │
           │   150GB Disk          │   │   150GB Disk          │
           │                       │   │                       │
           │  ┌─────────────────┐  │   │  ┌─────────────────┐  │
           │  │    Keystone     │  │   │  │  Nova Compute   │  │
           │  │    Glance       │  │◄──┼─►│  Neutron Agent  │  │
           │  │    Nova API     │  │   │  │  Cinder Volume  │  │
           │  │    Neutron Svr  │  │   │  │  Open vSwitch   │  │
           │  │    Cinder API   │  │   │  │                 │  │
           │  │    Horizon      │  │   │  │  [ VMs Live     │  │
           │  │    Heat         │  │   │  │    Here ]       │  │
           │  │    MariaDB      │  │   │  │                 │  │
           │  │    RabbitMQ     │  │   │  └─────────────────┘  │
           │  └─────────────────┘  │   │                       │
           └───────────────────────┘   └───────────────────────┘

           Controller IP: 10.134.227.10    Compute IP: 10.134.227.20
```

**Network Design:**

| Interface | Purpose                      | Notes                        |
|-----------|------------------------------|------------------------------|
| `ens33`   | Management Network           | API calls, service traffic   |
| `ens34`   | Provider / External Network  | VM internet access           |

---

## Technology Stack

| Layer            | Component                    | Version        |
|------------------|------------------------------|----------------|
| Cloud Platform   | OpenStack Caracal            | 2024.1         |
| Operating System | Ubuntu LTS                   | 24.04          |
| Deployment Tool  | Kolla-Ansible                | 18.0.0         |
| Containerization | Docker                       | Latest         |
| Hypervisor       | KVM / QEMU                   | —              |
| Networking       | Open vSwitch                 | —              |
| Block Storage    | LVM + Cinder                 | —              |
| Database         | MariaDB                      | —              |
| Message Queue    | RabbitMQ                     | —              |
| Automation       | Ansible Core                 | 2.15.x         |
| Language         | Python                       | 3.12           |

---

## OpenStack Services Deployed

```
┌──────────────┬───────────────────┬───────────────────────────────────────────┐
│  Service     │  Component        │  What It Does                             │
├──────────────┼───────────────────┼───────────────────────────────────────────┤
│  Keystone    │  Identity         │  Authentication, authorization, tokens     │
│  Nova        │  Compute          │  VM lifecycle: create, start, stop, delete │
│  Neutron     │  Network          │  Virtual networks, floating IPs, security  │
│  Glance      │  Image            │  VM image storage and registry             │
│  Cinder      │  Volume           │  Persistent block storage (like AWS EBS)   │
│  Horizon     │  Dashboard        │  Web-based self-service portal             │
│  Heat        │  Orchestration    │  Infrastructure-as-code templates          │
│  Placement   │  Scheduler        │  Resource inventory and allocation         │
└──────────────┴───────────────────┴───────────────────────────────────────────┘
```

---

## Hardware Requirements

| Node             | RAM        | CPU       | Storage    | Role                  |
|------------------|------------|-----------|------------|-----------------------|
| Controller Node  | 12GB min   | 4 cores   | 100GB      | Cloud management plane |
| Compute Node     | 16GB min   | 4 cores   | 150GB      | VM hypervisor host    |

> **Note:** Both nodes run Ubuntu 24.04 LTS with Docker installed. Controller requires more CPU for API processing. Compute requires more RAM for running multiple VMs simultaneously.

---

## Quick Start

```bash
# 1. Clone repository
git clone https://github.com/YOUR_USERNAME/openstack-private-cloud
cd openstack-private-cloud

# 2. Create Python virtual environment
python3 -m venv ~/kolla-venv
source ~/kolla-venv/bin/activate

# 3. Install dependencies
pip install 'ansible-core>=2.15,<2.16'
pip install 'kolla-ansible==18.0.0'

# 4. Install Ansible collections (manual method for Python 3.12)
bash scripts/install-collections.sh

# 5. Configure your environment
cp config/globals.yml.example config/globals.yml
cp config/multinode.example config/multinode
# Edit both files with your IPs and settings

# 6. Deploy OpenStack
bash scripts/bootstrap.sh
bash scripts/deploy.sh
bash scripts/post-deploy.sh

# 7. Access Horizon Dashboard
# Open browser: http://YOUR_CONTROLLER_IP/dashboard
# Username: admin
# Password: grep keystone_admin_password /etc/kolla/passwords.yml
```

---

## Installation Guide

### Step 1 — Prepare Both Nodes

Run on **both Controller and Compute:**

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install required packages
sudo apt install -y python3-dev libffi-dev gcc libssl-dev python3-pip git

# Install Docker (Ubuntu repository method — works on restricted networks)
sudo apt install -y docker.io
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER
newgrp docker

# Add passwordless sudo
echo "$USER ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/$USER
```

### Step 2 — Setup Network (Static IPs)

On **Controller Node:**

```yaml
# /etc/netplan/00-installer-config.yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: false
      addresses:
        - 10.134.227.10/24          # Change to your IP
      routes:
        - to: default
          via: 10.134.227.1         # Change to your gateway
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

```bash
sudo netplan apply
```

On **Compute Node** — same config but with IP `10.134.227.20`

### Step 3 — Configure SSH Keys

Run on **Controller Node only:**

```bash
# Generate SSH key
ssh-keygen -t rsa -b 4096

# Copy to both nodes (passwordless SSH)
ssh-copy-id dhairya@10.134.227.10   # Controller itself
ssh-copy-id dhairya@10.134.227.20   # Compute node

# Test
ssh dhairya@10.134.227.20           # Should login without password
```

### Step 4 — Setup Kolla-Ansible

```bash
# Create virtual environment
python3 -m venv ~/kolla-venv
source ~/kolla-venv/bin/activate

# Install packages
pip install --upgrade pip
pip install 'ansible-core>=2.15,<2.16'
pip install 'kolla-ansible==18.0.0'

# Create config directory
sudo mkdir -p /etc/kolla
sudo chown $USER:$USER /etc/kolla

# Copy config files
cp ~/kolla-venv/share/kolla-ansible/etc_examples/kolla/globals.yml /etc/kolla/globals.yml
cp ~/kolla-venv/share/kolla-ansible/etc_examples/kolla/passwords.yml /etc/kolla/passwords.yml
cp ~/kolla-venv/share/kolla-ansible/ansible/inventory/multinode ~/multinode

# Generate passwords
kolla-genpwd
```

### Step 5 — Install Ansible Collections (Python 3.12 Manual Method)

> Due to a Python 3.12 SSL compatibility bug in ansible-galaxy, collections must be installed manually via wget.

```bash
# Download all required collections
wget https://github.com/ansible-collections/ansible.posix/archive/refs/heads/main.zip -O posix.zip
wget https://github.com/ansible-collections/community.general/archive/refs/heads/main.zip -O general.zip
wget https://github.com/ansible-collections/community.docker/archive/refs/heads/main.zip -O docker.zip
wget https://github.com/ansible-collections/ansible.netcommon/archive/refs/heads/main.zip -O netcommon.zip

# Extract all
sudo apt install -y unzip
unzip posix.zip && unzip general.zip && unzip docker.zip && unzip netcommon.zip

# Install to correct location
mkdir -p ~/.ansible/collections/ansible_collections/ansible
mkdir -p ~/.ansible/collections/ansible_collections/community

mv ansible.posix-main     ~/.ansible/collections/ansible_collections/ansible/posix
mv community.general-main ~/.ansible/collections/ansible_collections/community/general
mv community.docker-main  ~/.ansible/collections/ansible_collections/community/docker
mv ansible.netcommon-main ~/.ansible/collections/ansible_collections/ansible/netcommon

# Install kolla deps (openstack.kolla)
kolla-ansible install-deps

# Verify all installed
ansible-galaxy collection list
```

Expected output:
```
ansible.posix       ✅
ansible.netcommon   ✅
community.general   ✅
community.docker    ✅
openstack.kolla     ✅
```

### Step 6 — Configure globals.yml

```bash
nano /etc/kolla/globals.yml
```

Set these values:

```yaml
kolla_base_distro: "ubuntu"
network_interface: "ens33"
neutron_external_interface: "ens34"
kolla_internal_vip_address: "10.134.227.10"    # Your Controller IP
enable_haproxy: "no"
enable_cinder: "yes"
enable_cinder_backend_lvm: "yes"
cinder_volume_group: "cinder-volumes"
enable_horizon: "yes"
enable_docker_repo: false
docker_apt_package: "docker.io"
```

### Step 7 — Configure multinode Inventory

```bash
nano ~/multinode
```

```ini
[control]
10.134.227.10 ansible_user=dhairya ansible_become=true ansible_private_key_file=~/.ssh/id_rsa

[network]
10.134.227.10 ansible_user=dhairya ansible_become=true ansible_private_key_file=~/.ssh/id_rsa

[compute]
10.134.227.20 ansible_user=dhairya ansible_become=true ansible_private_key_file=~/.ssh/id_rsa

[storage]
10.134.227.20 ansible_user=dhairya ansible_become=true ansible_private_key_file=~/.ssh/id_rsa

[monitoring]
10.134.227.10 ansible_user=dhairya ansible_become=true ansible_private_key_file=~/.ssh/id_rsa

[deployment]
localhost ansible_connection=local
```

### Step 8 — Setup Cinder LVM on Compute Node

SSH into Compute and run:

```bash
# Create storage file (39GB)
sudo mkdir -p /var/lib/cinder
sudo dd if=/dev/zero of=/var/lib/cinder/cinder-volumes bs=1G count=39

# Setup loop device
sudo losetup /dev/loop0 /var/lib/cinder/cinder-volumes

# Create LVM volume group
sudo pvcreate /dev/loop0
sudo vgcreate cinder-volumes /dev/loop0

# Verify
sudo vgs
# Should show: cinder-volumes  1  0  39GB

# Make persistent across reboots
sudo nano /etc/rc.local
# Add: losetup /dev/loop0 /var/lib/cinder/cinder-volumes
#      vgchange -ay cinder-volumes
sudo chmod +x /etc/rc.local
```

### Step 9 — Deploy OpenStack

```bash
# Patch Ubuntu 24.04 support (not officially supported in kolla-ansible 18.0.0)
sed -i 's/- "jammy"/- "jammy"\n    - "noble"\n    - "24.04"/' \
  ~/kolla-venv/share/kolla-ansible/ansible/roles/prechecks/vars/main.yml

# Bootstrap nodes
kolla-ansible -i ~/multinode bootstrap-servers
# Expected: failed=0 on all nodes

# Run pre-checks
kolla-ansible -i ~/multinode prechecks
# Expected: failed=0 on all nodes

# Deploy OpenStack (takes 30-60 minutes)
kolla-ansible -i ~/multinode deploy
# Expected: failed=0 on all nodes

# Post-deploy setup
kolla-ansible -i ~/multinode post-deploy
```

### Step 10 — Access Your Cloud

```bash
# Install OpenStack CLI
pip install python-openstackclient

# Load credentials
source /etc/kolla/admin-openrc.sh

# Verify all services running
openstack service list
openstack compute service list
```

Open browser:
```
URL:      http://10.134.227.10/dashboard
Username: admin
Domain:   Default
Password: grep keystone_admin_password /etc/kolla/passwords.yml
```

---

## Known Issues & Fixes

### Issue 1 — Python 3.12 SSL Error with ansible-galaxy

```
Error: HTTPSConnection.__init__() got an unexpected keyword argument 'cert_file'
```

**Root cause:** Python 3.12 removed the `cert_file` parameter from `HTTPSConnection`. Ansible Galaxy uses this internally.

**Fix:** Install all collections manually via wget (see Step 5 above). This completely bypasses ansible-galaxy's SSL handling.

---

### Issue 2 — Ubuntu 24.04 Noble Not Supported

```
Error: Ubuntu release 'noble' version '24.04' is not supported.
Supported releases are: jammy
```

**Fix:**

```bash
sed -i 's/- "jammy"/- "jammy"\n    - "noble"\n    - "24.04"/' \
  ~/kolla-venv/share/kolla-ansible/ansible/roles/prechecks/vars/main.yml
```

---

### Issue 3 — No KVM Available (VMware Nested Virtualization)

```
Error: No valid host was found. Exhausted all hosts.
```

**Root cause:** `/dev/kvm` does not exist inside VMware. KVM hardware acceleration unavailable in nested environment.

**Fix:**

```bash
sudo mkdir -p /etc/kolla/config/nova
sudo bash -c 'cat > /etc/kolla/config/nova/nova-compute.conf << EOF
[libvirt]
virt_type = qemu
cpu_mode = none
EOF'

# Redeploy Nova only
kolla-ansible -i ~/multinode deploy --tags nova
```

---

### Issue 4 — MariaDB Cannot Reach VIP Address

```
Error: Wait for MariaDB service to be ready through VIP — FAILED (retrying)
```

**Root cause:** VIP address `10.134.227.100` does not exist on any interface. HAProxy is disabled but VIP check still runs.

**Fix:** Set `kolla_internal_vip_address` to the Controller's actual IP directly:

```yaml
kolla_internal_vip_address: "10.134.227.10"
```

---

### Issue 5 — Docker GPG Key 404 Error

```
Error: curl (22) The requested URL returned error: 404
```

**Fix:** Use Ubuntu's built-in `docker.io` package instead of Docker's official repository:

```bash
sudo apt install -y docker.io
```

---

### Issue 6 — Two Default Gateways Conflict

```
Symptom: VM has internet but ping to other node fails
```

**Fix:** Remove conflicting DHCP route and enforce static only:

```bash
sudo ip route del default via <DHCP_GATEWAY>
sudo netplan apply
```

---

## Demo Guide

### Launching Your First VM

```bash
source /etc/kolla/admin-openrc.sh

# 1. Upload CirrOS test image
wget http://download.cirros-cloud.net/0.6.2/cirros-0.6.2-x86_64-disk.img
openstack image create "CirrOS" \
  --file cirros-0.6.2-x86_64-disk.img \
  --disk-format qcow2 \
  --container-format bare \
  --public

# 2. Create a VM flavor
openstack flavor create --ram 512 --vcpus 1 --disk 5 small.flavor

# 3. Create a virtual network
openstack network create demo-net
openstack subnet create \
  --network demo-net \
  --subnet-range 192.168.100.0/24 \
  demo-subnet

# 4. Launch a VM
openstack server create \
  --image CirrOS \
  --flavor small.flavor \
  --network demo-net \
  my-first-vm

# 5. Watch status (wait for ACTIVE)
openstack server list

# 6. Create and attach storage volume
openstack volume create --size 1 my-volume
openstack server add volume my-first-vm my-volume

# 7. Verify
openstack volume list
```

### VM Console Access

Via Horizon Dashboard:
```
Project → Compute → Instances → my-first-vm → Console

Login: cirros
Password: gocubsgo
```

### Create a New User

```bash
openstack user create --password student123 student1
openstack project create student-project
openstack role add --project student-project --user student1 member
```

### Check All Services

```bash
openstack service list
openstack compute service list
openstack network agent list
docker ps --format "table {{.Names}}\t{{.Status}}"
```

---

## Project Results

```
Deployment Summary:
──────────────────────────────────────────────────
  Controller  10.134.227.10  ok=281  failed=0  ✅
  Compute     10.134.227.20  ok=84   failed=0  ✅
  Localhost                  ok=4    failed=0  ✅
──────────────────────────────────────────────────

Services Running:     8 OpenStack services
VM Boot Time:         under 60 seconds
Storage Available:    39GB Cinder LVM pool
Users Supported:      Multi-tenant isolated
Dashboard:            Horizon web portal
API Access:           REST + CLI
```

---

## Skills Demonstrated

```
  Cloud Architecture      ─────────────────────── Infrastructure design
  Linux Administration    ─────────────────────── Ubuntu server management
  Container Orchestration ─────────────────────── Docker + Kolla-Ansible
  Network Engineering     ─────────────────────── OVS, VXLAN, Neutron
  Storage Management      ─────────────────────── LVM, Cinder volumes
  Automation              ─────────────────────── Ansible playbooks
  Distributed Systems     ─────────────────────── Multi-service debugging
  Production Ops          ─────────────────────── Real error resolution
```

---

## Repository Structure

```
openstack-private-cloud/
│
├── README.md                          ← You are here
│
├── config/
│   ├── globals.yml                    ← Kolla-Ansible global config
│   ├── multinode                      ← Ansible inventory file
│   └── nova-compute.conf              ← Nova QEMU config for VMware
│
├── scripts/
│   ├── install-collections.sh         ← Manual collection installer
│   ├── bootstrap.sh                   ← Bootstrap script
│   ├── deploy.sh                      ← Full deploy script
│   └── post-deploy.sh                 ← First VM setup script
│
├── docs/
│   ├── architecture.md                ← Detailed architecture docs
│   ├── installation.md                ← Step-by-step install guide
│   ├── troubleshooting.md             ← All errors and fixes
│   └── demo-guide.md                  ← Competition demo script
│
└── screenshots/
    ├── horizon-dashboard.png
    ├── vm-running.png
    ├── services-list.png
    └── architecture-diagram.png
```

---

## Author

<div align="center">

**Dhairya**
*Founder of HackStone*

[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/YOUR_USERNAME)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/dhairya-pithadia/)

*Built with patience, persistence, and a lot of chai.*

</div>

---

## License

```
MIT License — Free to use, modify, and distribute.
See LICENSE file for full details.
```

---

<div align="center">

```
─────────────────────────────────────────────────────────
  If this project helped you, consider giving it a ⭐
  It means a lot and helps others find this resource.
─────────────────────────────────────────────────────────
```

**OpenStack** • **CloudComputing** • **PrivateCloud** • **DevOps** • **Linux** • **Docker** • **Ansible** • **KollaAnsible** • **Virtualization** • **Infrastructure** • **IaaS** • **FinalYearProject**

</div>
