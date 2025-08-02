# 🧱 Base Vagrant Box

A minimal yet powerful Vagrant development environment using **Ubuntu**, **Docker**, and **Ansible**, ideal for local infrastructure automation, container orchestration, or provisioning workflows.

## 📦 Overview

This project spins up a VirtualBox-based Ubuntu VM with Docker and Ansible installed. Docker containers can be orchestrated from within the VM, and Ansible is preconfigured to manage both local containers and external systems.

You can use this base box to:

- Develop and test Ansible playbooks in a consistent Linux environment.
- Build Docker images or run containers in an isolated VM.
- Serve as a foundation for more complex multi-machine environments.

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/base-vagrant-box.git
cd base-vagrant-box
```

### 2. Add Your Environment Variables (Optional)

If you're using Docker Hub credentials or other secrets, create a `.env` file at the root of the project:

```bash
touch .env
```

Example `.env`:

```env
DOCKER_USERNAME=yourdockerusername
DOCKER_PASSWORD=yourdockerpassword
```

> 🛡️ These are loaded into the VM environment, not into version control.

---

### 3. Start the Vagrant Box

```bash
vagrant up
```

```bash
# testing command for timing  purposes
time echo '1' | vagrant up --debug-timestamp
```

The first time this runs, it will:

- Download the base Ubuntu box
- Provision it with Docker and Ansible
- Run any bootstrapping scripts or environment setup

---

### 4. SSH Into the VM

```bash
vagrant ssh
```

From here, you can:

- Run Ansible commands (`ansible-playbook`)
- Build or run Docker containers
- Access synced folders between host and guest

---

## ⚙️ Features

- ✅ Ubuntu 22.04 base image
- ✅ Docker CE + Docker Compose
- ✅ Ansible (latest version via pip)
- ✅ Synced folder support (`./data` or as configured)
- ✅ Environment variable injection from `.env`
- ✅ Optional port forwarding and network customization

---

## 🛠️ Configuration Options

Inside the `Vagrantfile`, you can customize:

- VM RAM, CPU
- Network type (private/public/bridged)
- Docker login automation
- Boot timeout duration
- Shell provisioning scripts

---

## 📁 Folder Structure

```
base-vagrant-box/
│
├── Vagrantfile          # Core configuration file
├── provision.sh         # Provisioning script (Docker, Ansible, etc.)
├── .env                 # (Optional) Environment variables
├── data/                # Shared folder between host and VM
└── README.md            # This file
```

---

## 🧪 Testing

Use the following to destroy and rebuild from scratch:

```bash
vagrant destroy -f && vagrant up
```

---

## 📝 License

MIT License. Feel free to fork and build upon it.

---

## 👨‍💻 Author

Created by morogoyo  
Pull requests and issues welcome!
