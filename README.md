# Test Servers With Vagrant

This project automates the deployment of multiple virtual machines using Vagrant and VirtualBox.

## 📋 Prerequisites

- **Vagrant** (version 2.4.x or higher)
- **VirtualBox** (version 7.1 or higher) 
- **Git** (for repository cloning)

## 🚀 Quick Start

1. **Clone the repository:**
```bash
git clone https://github.com/KnYaZ-95/vagrant.git
cd vagrant
```
2. **Install Virtualbox provider:**
```bash
vagrant install plugin virtualbox
```
3. **Start virtual machines:**
```bash
vagrant up
```

## ⚙️ Configuration

**All settings are located in the vm_config.yaml.example file:**
```yaml
box: "ubuntu/jammy64"   # Image
memory: 3072   # RAM in mb
cpus: 2  # CPU cores
bridge_interface: "Your interface" # Bridge interface that you use

# User section
login: "test"
password: "openssl passwd -6 -salt salt1234"
public_key: ssh-rsa AAAA

# List of vitual machines
vms:
  - name: "test-server-1"
    hostname: "test-1"
    ip: "192.168.0.2"
...
```

## 🗑️ Cleanup

To completely remove virtual machines:
```bash
vagrant destroy -f
```
