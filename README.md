# LearnHub - DevOps Case Study & Ansible Automation

Configuration management for **LearnHub**, an online learning platform, using Ansible.
One command provisions the web tier (Nginx), database tier (MariaDB) and monitoring (Prometheus node exporter) on an Ubuntu EC2 instance.

## Project structure
```
learnhub-ansible/
├── ansible.cfg                # defaults: inventory, sudo, host key checking
├── inventory.ini              # groups: web, db, monitoring (+ learnhub parent group)
├── site.yml                   # master playbook (5 plays)
├── group_vars/all.yml         # all variables (app, ports, DB, courses)
└── roles/
    ├── common/                # baseline packages
    ├── webserver/             # nginx + templates (home page, site config, /health)
    ├── database/              # mariadb + database + user
    └── monitoring/            # prometheus-node-exporter
```

## Prerequisites
- Ubuntu 22.04/24.04 EC2 instance (control node = managed node)
- Security group: inbound **22** (SSH) and **80** (HTTP)
- `sudo apt update && sudo apt install -y ansible`

## Run
```bash
ansible --version
ansible-inventory --graph
ansible all -m ping
ansible-playbook site.yml --syntax-check
ansible-playbook site.yml            # first run  -> changes applied
ansible-playbook site.yml            # second run -> changed=0 (idempotent)
curl http://localhost/health
```
Open `http://<EC2-public-IP>/` in a browser.

## Run only one tier (tags)
```bash
ansible-playbook site.yml --tags web
ansible-playbook site.yml --tags db
ansible-playbook site.yml --tags monitoring
ansible-playbook site.yml --tags verify
```

## Notes
- `db_password` in `group_vars/all.yml` is a demo value; use `ansible-vault` in real projects.
- To move to a multi-server setup, change only `inventory.ini` (example is in the file comments).
