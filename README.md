# Traefik Ansible Role Setup

This repository contains an Ansible role to automate the installation and configuration of Traefik, a modern reverse proxy and load balancer. The role supports both RHEL-based and Ubuntu-based systems and includes dynamic configuration for services and TLS certificates.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Files](#files)
- [Prerequisites](#prerequisites)
- [Setup](#setup)
  - [Step 1: Clone the Repository](#step-1-clone-the-repository)
  - [Step 2: Update Variables](#step-2-update-variables)
  - [Step 3: Run the Playbook](#step-3-run-the-playbook)
- [Role Details](#role-details)
- [Dynamic Configuration](#dynamic-configuration)
- [License](#license)

---

## Overview

This Ansible role automates the deployment of Traefik, including:
- Installing the Traefik binary.
- Configuring static and dynamic configurations.
- Setting up a systemd service for Traefik.
- Enabling HTTPS with Let's Encrypt certificates.
- Configuring a dashboard for monitoring.

---

## Features

- **Cross-Platform Support**: Works on both RHEL-based and Ubuntu-based systems.
- **Dynamic Configuration**: Supports multiple backend services with TLS.
- **Traefik Dashboard**: Optionally enables the Traefik dashboard for monitoring.
- **Systemd Integration**: Automatically sets up and starts Traefik as a systemd service.

---

## Files

### 1. `tasks/main.yml`
Defines the tasks to:
- Install prerequisites.
- Download and extract the Traefik binary.
- Configure Traefik with static and dynamic configurations.
- Set up a systemd service for Traefik.

### 2. `templates/traefik.yaml.j2`
A Jinja2 template for the static Traefik configuration, including:
- Entry points for HTTP and HTTPS.
- Providers for file-based configurations.
- Certificates resolvers for Let's Encrypt.
- Logging and API settings.

### 3. `templates/dynamic_config.yaml.j2`
A Jinja2 template for dynamic Traefik configuration, including:
- Routers for backend services.
- Services with load balancer configurations.
- TLS settings for secure connections.

### 4. `vars/main.yml`
Defines default variables for the role, including:
- Traefik version.
- Email for Let's Encrypt certificates.
- Domain names for services and the dashboard.
- Backend service configurations.

---

## Prerequisites

1. **Ansible**: Installed on the control node (version 2.9 or higher recommended).
2. **Target Nodes**: RHEL-based or Ubuntu-based systems with SSH access.
3. **DNS Configuration**: Ensure DNS records point to the target node for the configured domains.

---

## Setup

### Step 1: Clone the Repository

Clone this repository to your Ansible control node:

```bash
git clone https://github.com/your-username/traefik-ansible-role-setup.git
cd traefik-ansible-role-setup
```
### Step 2: Update Variables
Edit the vars/main.yml file to customize the configuration:
```bash
traefik_version: "2.11.7"
traefik_email: "admin@example.com"
traefik_domain: "example.com"
traefik_dashboard: true
traefik_dashboard_domain: "dashboard.example.com"
traefik_services:
  - name: "site"
    url: "http://192.168.1.100:80"
    route: "Host(`example.com`)"
    service_name: "site-service"
```
### Step 3: Run the Playbook
Run the Ansible playbook to deploy Traefik:
```bash
ansible-playbook -i inventory main.yml
```
## Role Details
- **Install Prerequisites**: Installs required packages like wget and tar.
- **Download Traefik**: Downloads the specified version of Traefik from GitHub.
- **Configure Traefik**: Templates static and dynamic configuration files.
- **Systemd Service**: Creates and starts a systemd service for Traefik.
---
Variables

- **traefik_version**: The version of Traefik to install.
- **traefik_email**: Email address for Let's Encrypt certificates.
- **traefik_domain**: The primary domain for the service.
- **traefik_dashboard**: Enables or disables the Traefik dashboard.
- **traefik_services**: A list of backend services to configure.

## Dynamic Configuration
The dynamic_config.yaml.j2 template allows you to define multiple backend services. Example configuration:
```bash
traefik_services:
  - name: "site"
    url: "http://192.168.1.100:80"
    route: "Host(`example.com`)"
    service_name: "site-service"
  - name: "api"
    url: "http://192.168.1.101:8080"
    route: "Host(`api.example.com`)"
    service_name: "api-service"
```
This will create routers and services for example.com and api.example.com.

## License
This project is licensed under the MIT License. See the LICENSE file for more details.