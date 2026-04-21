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
║                      by  H a c k S t o n e                       ║
╚═══════════════════════════════════════════════════════════════════╝
```

# OpenStack Private Cloud Infrastructure

### Production-grade Private Cloud — OpenStack 2024.1 on a real 2-node architecture
### Deployed via Kolla-Ansible · Containerized with Docker · Built from scratch

<br>

[![OpenStack](https://img.shields.io/badge/OpenStack-2024.1_Caracal-ED1944?style=for-the-badge&logo=openstack&logoColor=white)](https://www.openstack.org/)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04_LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Ansible](https://img.shields.io/badge/Ansible-Kolla_2.15-EE0000?style=for-the-badge&logo=ansible&logoColor=white)](https://www.ansible.com/)
[![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org/)
[![License](https://img.shields.io/badge/License-MIT-22c55e?style=for-the-badge)](LICENSE)

<br>

> *"Most students deploy applications on cloud platforms.*
> *This project builds the cloud platform itself."*

<br>

[Overview](#-overview) •
[Architecture](#-architecture) •
[Tech Stack](#-technology-stack) •
[Services](#-openstack-services) •
[Installation](#-installation-guide) •
[Known Issues](#-known-issues--fixes) •
[Demo](#-demo-guide) •
[Results](#-project-results)

---

</div>

## What is This?

This is a **fully functional private cloud infrastructure** built from scratch using OpenStack — the same open-source platform powering infrastructure at **CERN, Walmart, BMW**, and hundreds of Fortune 500 enterprises.

Deployed on **2 physical machines** acting as real data center nodes, fully containerized with Docker, and automated using Kolla-Ansible. No cloud credits. No paid services. Just hardware, Linux, and engineering.

```
┌─────────────────────────────────────────────────────────┐
│  What this cloud delivers:                              │
│                                                         │
│  ◆  Provision VMs in under 60 seconds                  │
│  ◆  Multi-tenant isolation with RBAC                   │
│  ◆  Persistent block storage (like AWS EBS)            │
│  ◆  Self-service web dashboard (like AWS Console)      │
│  ◆  REST API + OpenStack CLI access                    │
│  ◆  Horizontal scaling by adding compute nodes         │
│  ◆  Cost: ₹0/month vs ₹10,000+/month on AWS           │
└─────────────────────────────────────────────────────────┘
```

---

## Cost Comparison

| Feature | AWS / Azure | This Private Cloud |
|---------|-------------|-------------------|
| VM Provisioning | ✅ | ✅ |
| Block Storage | ✅ | ✅ |
| Web Dashboard | ✅ | ✅ |
| Virtual Networking | ✅ | ✅ |
| Monthly Cost | ₹8,000 – ₹15,000 | **₹0 recurring** |
| Data Sovereignty | ❌ Third-party | ✅ On-premise |
| Full Customization | ❌ Limited | ✅ Complete control |

---

## Architecture

```
┌──────────────────────────────────────────────────────────────────────┐
│                         NETWORK LAYER                                │
│                      [ 10.134.227.0/24 ]                             │
└────────────────────────┬──────────────────────────┬──────────────────┘
                         │                          │
            ┌────────────▼──────────┐  ┌────────────▼──────────┐
            │   CONTROLLER NODE     │  │    COMPUTE NODE       │
            │   10.134.227.10       │  │    10.134.227.20      │
            │   12GB RAM  |  i5     │  │   16GB RAM  |  i5     │
            │                       │  │                       │
            │  ┌─────────────────┐  │  │  ┌─────────────────┐  │
            │  │  Keystone       │  │  │  │  Nova Compute   │  │
            │  │  Glance         │  │◄─┼─►│  Neutron Agent  │  │
            │  │  Nova API       │  │  │  │  Cinder Volume  │  │
            │  │  Neutron Server │  │  │  │  Open vSwitch   │  │
            │  │  Cinder API     │  │  │  │                 │  │
            │  │  Horizon        │  │  │  │  [ VMs run      │  │
            │  │  Heat           │  │  │  │    here ]       │  │
            │  │  Placement      │  │  │  │                 │  │
            │  │  MariaDB        │  │  │  └─────────────────┘  │
            │  │  RabbitMQ       │  │  │                       │
            │  └─────────────────┘  │  └───────────────────────┘
            └───────────────────────┘
```

**Network Interfaces:**

| Interface | Purpose |
|-----------|---------|
| `ens33` | Management — API calls and service communication |
| `ens34` | Provider — VM external network access |

---

## Technology Stack

| Layer | Component | Version |
|-------|-----------|---------|
| Cloud Platform | OpenStack Caracal | 2024.1 |
| Operating System | Ubuntu LTS | 24.04 |
| Deployment Tool | Kolla-Ansible | 18.0.0 |
| Containerization | Docker | Latest |
| Hypervisor | KVM / QEMU | — |
| Networking | Open vSwitch | — |
| Block Storage | LVM + Cinder | — |
| Database | MariaDB | — |
| Message Queue | RabbitMQ | — |
| Automation | Ansible Core | 2.15.x |
| Language | Python | 3.12 |

---

## OpenStack Services

```
┌──────────────┬───────────────┬────────────────────────────────────────────┐
│  Service     │  Component    │  What It Does                              │
├──────────────┼───────────────┼────────────────────────────────────────────┤
│  Keystone    │  Identity     │  Auth, tokens, RBAC, multi-tenancy         │
│  Nova        │  Compute      │  VM lifecycle: create, start, stop, delete │
│  Neutron     │  Network      │  Virtual networks, floating IPs, security  │
│  Glance      │  Image        │  VM image storage and registry             │
│  Cinder      │  Volume       │  Persistent block storage (like AWS EBS)   │
│  Horizon     │  Dashboard    │  Web-based self-service portal             │
│  Heat        │  Orchestration│  Infrastructure-as-code templates          │
│  Placement   │  Scheduler    │  Resource inventory and VM scheduling      │
└──────────────┴───────────────┴────────────────────────────────────────────┘
```

---

## Hardware Requirements

| Node | RAM | CPU | Storage | Role |
|------|-----|-----|---------|------|
| Controller | 12GB min | 4 cores | 100GB | Cloud management plane |
| Compute | 16GB min | 4 cores | 150GB | VM hypervisor host |

> Both nodes run Ubuntu 24.04 LTS with Docker. Works on physical machines or VMware VMs with nested virtualization enabled.

---

## Installation Guide

### Step 1 — Prepare Both Nodes

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install required packages
sudo apt install -y python3-dev libffi-dev gcc libssl-dev python3-pip git

# Install Docker via Ubuntu repo (works on restricted networks)
sudo apt install -y docker.io
sudo systemctl enable docker && sudo systemctl start docker
sudo usermod -aG docker $USER && newgrp docker

# Add passwordless sudo
echo "$USER ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/$USER
```

### Step 2 — Configure Static IPs

**Controller Node** `/etc/netplan/00-installer-config.yaml`:

```yaml
network:
  version: 2
  ethernets:
    ens33:
      dhcp4: false
      addresses: [10.134.227.10/24]
      routes:
        - to: default
          via: 10.134.227.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
```

```bash
sudo netplan apply
```

Repeat on Compute Node with IP `10.134.227.20`.

### Step 3 — SSH Key Setup (Controller Only)

```bash
ssh-keygen -t rsa -b 4096
ssh-copy-id user@10.134.227.10   # Controller itself
ssh-copy-id user@10.134.227.20   # Compute node
```

### Step 4 — Install Kolla-Ansible

```bash
python3 -m venv ~/kolla-venv
source ~/kolla-venv/bin/activate

pip install --upgrade pip
pip install 'ansible-core>=2.15,<2.16'
pip install 'kolla-ansible==18.0.0'

sudo mkdir -p /etc/kolla
sudo chown $USER:$USER /etc/kolla

cp ~/kolla-venv/share/kolla-ansible/etc_examples/kolla/globals.yml /etc/kolla/globals.yml
cp ~/kolla-venv/share/kolla-ansible/etc_examples/kolla/passwords.yml /etc/kolla/passwords.yml
cp ~/kolla-venv/share/kolla-ansible/ansible/inventory/multinode ~/multinode

kolla-genpwd
```

### Step 5 — Install Ansible Collections (Python 3.12 Manual Method)

> ⚠️ Python 3.12 broke `ansible-galaxy` SSL handling. Install all collections manually via wget.

```bash
# Download collections
wget https://github.com/ansible-collections/ansible.posix/archive/refs/heads/main.zip -O posix.zip
wget https://github.com/ansible-collections/community.general/archive/refs/heads/main.zip -O general.zip
wget https://github.com/ansible-collections/community.docker/archive/refs/heads/main.zip -O docker.zip
wget https://github.com/ansible-collections/ansible.netcommon/archive/refs/heads/main.zip -O netcommon.zip

sudo apt install -y unzip
unzip posix.zip && unzip general.zip && unzip docker.zip && unzip netcommon.zip

mkdir -p ~/.ansible/collections/ansible_collections/{ansible,community}

mv ansible.posix-main     ~/.ansible/collections/ansible_collections/ansible/posix
mv community.general-main ~/.ansible/collections/ansible_collections/community/general
mv community.docker-main  ~/.ansible/collections/ansible_collections/community/docker
mv ansible.netcommon-main ~/.ansible/collections/ansible_collections/ansible/netcommon

kolla-ansible install-deps
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

```yaml
kolla_base_distro: "ubuntu"
network_interface: "ens33"
neutron_external_interface: "ens34"
kolla_internal_vip_address: "10.134.227.10"
enable_haproxy: "no"
enable_cinder: "yes"
enable_cinder_backend_lvm: "yes"
cinder_volume_group: "cinder-volumes"
enable_horizon: "yes"
enable_docker_repo: false
docker_apt_package: "docker.io"
```

### Step 7 — Configure Inventory

```bash
nano ~/multinode
```

```ini
[control]
10.134.227.10 ansible_user=YOUR_USER ansible_become=true ansible_private_key_file=~/.ssh/id_rsa

[network]
10.134.227.10 ansible_user=YOUR_USER ansible_become=true ansible_private_key_file=~/.ssh/id_rsa

[compute]
10.134.227.20 ansible_user=YOUR_USER ansible_become=true ansible_private_key_file=~/.ssh/id_rsa

[storage]
10.134.227.20 ansible_user=YOUR_USER ansible_become=true ansible_private_key_file=~/.ssh/id_rsa

[monitoring]
10.134.227.10 ansible_user=YOUR_USER ansible_become=true ansible_private_key_file=~/.ssh/id_rsa

[deployment]
localhost ansible_connection=local
```

### Step 8 — Setup Cinder LVM (Compute Node)

```bash
sudo mkdir -p /var/lib/cinder
sudo dd if=/dev/zero of=/var/lib/cinder/cinder-volumes bs=1G count=39
sudo losetup /dev/loop0 /var/lib/cinder/cinder-volumes
sudo pvcreate /dev/loop0
sudo vgcreate cinder-volumes /dev/loop0

# Verify
sudo vgs
# Expected: cinder-volumes  1  0  39GB

# Make persistent across reboots
echo 'losetup /dev/loop0 /var/lib/cinder/cinder-volumes' | sudo tee -a /etc/rc.local
echo 'vgchange -ay cinder-volumes' | sudo tee -a /etc/rc.local
sudo chmod +x /etc/rc.local
```

### Step 9 — Patch Ubuntu 24.04 Support

```bash
sed -i 's/- "jammy"/- "jammy"\n    - "noble"\n    - "24.04"/' \
  ~/kolla-venv/share/kolla-ansible/ansible/roles/prechecks/vars/main.yml
```

### Step 10 — Deploy

```bash
# Bootstrap
kolla-ansible -i ~/multinode bootstrap-servers
# Expected: failed=0

# Pre-checks
kolla-ansible -i ~/multinode prechecks
# Expected: failed=0

# Deploy (30-60 minutes)
kolla-ansible -i ~/multinode deploy
# Expected: failed=0

# Post-deploy
kolla-ansible -i ~/multinode post-deploy
```

### Step 11 — Access Your Cloud

```bash
pip install python-openstackclient
source /etc/kolla/admin-openrc.sh
openstack service list
```

```
Browser:  http://10.134.227.10/dashboard
Username: admin
Domain:   Default
Password: grep keystone_admin_password /etc/kolla/passwords.yml
```

---

## Known Issues & Fixes

### Python 3.12 SSL Error with ansible-galaxy

```
Error: HTTPSConnection.__init__() got an unexpected keyword argument 'cert_file'
```

**Fix:** Install all collections manually via wget (see Step 5). This bypasses ansible-galaxy entirely.

---

### Ubuntu 24.04 Not Supported Error

```
Error: Ubuntu release 'noble' version '24.04' is not supported.
```

**Fix:**
```bash
sed -i 's/- "jammy"/- "jammy"\n    - "noble"\n    - "24.04"/' \
  ~/kolla-venv/share/kolla-ansible/ansible/roles/prechecks/vars/main.yml
```

---

### No KVM in VMware (Nested Virtualization)

```
Error: No valid host was found. Exhausted all hosts.
```

**Fix:**
```bash
sudo mkdir -p /etc/kolla/config/nova
sudo bash -c 'cat > /etc/kolla/config/nova/nova-compute.conf << EOF
[libvirt]
virt_type = qemu
cpu_mode = none
EOF'

kolla-ansible -i ~/multinode deploy --tags nova
```

---

### MariaDB Cannot Reach VIP Address

```
Error: Wait for MariaDB service to be ready through VIP — FAILED
```

**Fix:** Set VIP to Controller's actual IP in `globals.yml`:
```yaml
kolla_internal_vip_address: "10.134.227.10"
```

---

### Docker GPG Key 404

```
Error: curl (22) The requested URL returned error: 404
```

**Fix:** Use Ubuntu's built-in Docker instead of Docker's repo:
```bash
sudo apt install -y docker.io
```

---

### Two Default Gateways Conflict

```
Symptom: One node has internet but can't ping the other
```

**Fix:**
```bash
sudo ip route del default via <DHCP_GATEWAY>
sudo netplan apply
```

---

## Demo Guide

### Launch Your First VM

```bash
source /etc/kolla/admin-openrc.sh

# 1. Upload test image
wget http://download.cirros-cloud.net/0.6.2/cirros-0.6.2-x86_64-disk.img
openstack image create "CirrOS" \
  --file cirros-0.6.2-x86_64-disk.img \
  --disk-format qcow2 --container-format bare --public

# 2. Create flavor
openstack flavor create --ram 512 --vcpus 1 --disk 5 small.flavor

# 3. Create network
openstack network create demo-net
openstack subnet create --network demo-net \
  --subnet-range 192.168.100.0/24 demo-subnet

# 4. Launch VM
openstack server create \
  --image CirrOS --flavor small.flavor \
  --network demo-net my-first-vm

# 5. Monitor status
openstack server list

# 6. Attach storage
openstack volume create --size 1 my-volume
openstack server add volume my-first-vm my-volume
```

**Console Access via Horizon:**
```
Project → Compute → Instances → my-first-vm → Console
Login: cirros | Password: gocubsgo
```

---

## Project Results

```
────────────────────────────────────────────────────────
  Node              Tasks     Status
────────────────────────────────────────────────────────
  Controller        ok=281    failed=0   ✅ SUCCESS
  Compute           ok=84     failed=0   ✅ SUCCESS
  Localhost         ok=4      failed=0   ✅ SUCCESS
────────────────────────────────────────────────────────

  OpenStack Services Running  →  8 services
  VM Boot Time                →  < 60 seconds
  Storage Pool                →  39GB Cinder LVM
  Access Methods              →  Web Dashboard + CLI + API
  Monthly Cost                →  ₹0
────────────────────────────────────────────────────────
```

---

## Repository Structure

```
private-cloud-openstack/
│
├── README.md                       ← You are here
├── LICENSE
│
├── config/
│   ├── globals.yml                 ← Kolla global config
│   ├── multinode                   ← Ansible inventory
│   └── nova-compute.conf           ← QEMU config for VMware
│
├── scripts/
│   ├── install-collections.sh      ← Manual collection install
│   ├── bootstrap.sh
│   ├── deploy.sh
│   └── post-deploy.sh
│
└── docs/
    ├── architecture.md
    ├── troubleshooting.md
    └── demo-guide.md
```

---

## Author

<div align="center">

**Dhairya Pithadia**
*Cybersecurity Engineer · Cloud Builder · Founder of HackStone*

[![GitHub](https://img.shields.io/badge/GitHub-Hack--Stone-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Hack-Stone)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-dhairya--pithadia-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/dhairya-pithadia)
[![Website](https://img.shields.io/badge/Website-hackstone.in-orange?style=for-the-badge&logo=google-chrome&logoColor=white)](https://hackstone.in)

*Built with patience, persistence, and a lot of chai.*

</div>

---

<div align="center">

```
─────────────────────────────────────────────────────────────
  If this project helped you, drop a ⭐ on the repo.
  It helps others find this resource and keeps me motivated.
─────────────────────────────────────────────────────────────
```

`OpenStack` · `PrivateCloud` · `CloudComputing` · `DevOps` · `Linux` · `Docker` · `KollaAnsible` · `Infrastructure` · `IaaS`

</div>
