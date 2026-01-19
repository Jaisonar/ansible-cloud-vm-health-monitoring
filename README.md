# Ansible-Driven Cloud VM Health Monitoring

An agentless cloud monitoring solution built using Ansible to automatically discover AWS EC2 instances, collect system health metrics, and generate operational reports.

---

## Overview

This project demonstrates how Ansible can be used to build a scalable, agentless monitoring framework for cloud virtual machines. It focuses on automation, dynamic infrastructure discovery, and secure remote access without installing any agents on target systems.

---

## Key Features

- Agentless monitoring of CPU, memory, and disk utilization across multiple AWS EC2 instances
- Automated EC2 instance discovery using Ansible dynamic inventory
- Deterministic instance tagging and SSH key propagation using Bash scripts
- Secure, passwordless access to managed instances
- Modular Ansible playbooks leveraging Python (virtual environment + boto3) and Ansible collections
- Reusable and scalable configuration using `ansible.cfg`, `group_vars`, and Jinja2 templates
- Fully automated workflow covering discovery, tagging, SSH setup, metric collection, and reporting

---

## Getting Started

Refer to **[SETUP.md](SETUP.md)** for detailed environment setup and execution steps.
