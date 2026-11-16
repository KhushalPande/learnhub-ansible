# LearnHub - Video Demo Script (target: 4-5 minutes)

**0:00 Intro (20s)**
"Hi, I'm <NAME>, PRN <PRN>. My case study is LearnHub, an online learning platform, and I automated its deployment on AWS EC2 using Ansible."

**0:20 DevOps lifecycle & architecture (60s)**
- Show the lifecycle diagram in the report: Plan, Code, Build, Test, Release, Deploy, Operate, Monitor.
- Tools: GitHub Issues, Git/GitHub, GitHub Actions, ansible-lint + syntax-check, Git tags, Ansible, AWS EC2 + Nginx + MariaDB, Prometheus node exporter.
- "Ansible covers the Deploy and Operate stages - configuration as code."

**1:20 Project walkthrough (80s)**
- `ls` and `find roles -type f` - explain the role structure (reusable, one role per tier).
- `cat inventory.ini` - groups web, db, monitoring; parent group learnhub; localhost for demo, commented multi-server example.
- `cat group_vars/all.yml` - variables separated from logic.
- `cat site.yml` - 5 plays; roles; tags.
- Open one role's tasks (webserver) and the Jinja2 template - explain `notify` / handler.

**2:40 Execution (90s)**
- `ansible all -m ping` -> SUCCESS
- `ansible-playbook site.yml --syntax-check`
- `ansible-playbook site.yml` -> point at changed tasks and PLAY RECAP (failed=0)
- Run it again -> `changed=0`: "This is idempotency."

**4:10 Verification (40s)**
- `curl http://localhost/health`
- `systemctl status nginx mariadb prometheus-node-exporter`
- `sudo mysql -e "SHOW DATABASES;"` -> learnhub_db
- Browser: `http://<EC2-IP>/` shows LearnHub page.

**4:50 Concepts recap & close (20s)**
Inventory, playbook, roles, variables, templates, handlers, idempotency, tags. "Thank you."
