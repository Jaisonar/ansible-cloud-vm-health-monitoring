# Ansible-Driven Cloud VM Health Monitoring

Agentless framework to monitor AWS EC2 VM health using Ansible.

## Features
- Real-time CPU, memory, and disk monitoring across multiple EC2 instances
- Automated EC2 instance discovery, deterministic tagging, and SSH key propagation
- Modular, reusable Ansible playbooks leveraging Python (`venv + boto3`) and Ansible collections
- Structured configurations using `ansible.cfg`, `group_vars`, and Jinja2 templates
- Fully automated workflow for discovery, tagging, SSH setup, metric collection, and reporting
