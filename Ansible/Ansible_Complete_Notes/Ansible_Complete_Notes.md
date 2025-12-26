
# **Complete Ansible Notes**

---

## 1️⃣ Introduction to Ansible

### What is Ansible?
- Open-source IT automation engine
- Automates:
	- Provisioning
	- Configuration management
	- Application deployment
	- Orchestration
- Agentless, simple, and powerful

### How Ansible Works
- **Agentless:** No software needed on managed nodes
- Control node pushes **modules** to managed nodes via SSH/WinRM
- Modules are **idempotent**
- For network devices, modules run on control node

**Comparison with Shell Scripting**
- Shell scripting works only on Linux
- Complex as script size grows
- Not idempotent
- Ansible is scalable, modular, and readable

**Image Placeholder:** `ansible_architecture.png`

---

## 2️⃣ Why Ansible?

- Easy to learn (YAML syntax)
- Agentless
- Secure (SSH + keys)
- Fast automation
- Used for servers, cloud, apps, CI/CD

---

## 3️⃣ Ansible Architecture

**Components**
1. **Control Node:** Executes playbooks, manages tasks
2. **Managed Nodes:** Hosts managed by Ansible
3. **Inventory:** Defines hosts and groups
4. **Modules:** Units of work (copy, yum, service)
5. **Playbooks:** YAML files with automation logic
6. **Plugins:** Extend Ansible functionality (connection, lookup, callback)

**Architecture Flow:**


Control Node
|
| (SSH / WinRM)
↓
Managed Nodes
|
| (Modules Executed)
↓
Desired State Achieved


**Image Placeholder:** `ansible_workflow.png`

---

## 4️⃣ Passwordless Authentication

### EC2 Instances
- Using public key:
```bash
ssh-copy-id -f "-o IdentityFile <PATH_TO_PEM>" ubuntu@<INSTANCE-IP>
```


Using password:

Edit /etc/ssh/sshd_config.d/60-cloudimg-settings.conf

PasswordAuthentication yes

sudo systemctl restart sshd

5️⃣ Inventory (VERY IMPORTANT)

Types

Static (INI/YAML)

Dynamic (cloud, generated via scripts/plugins)

INI Example:

[webservers]
web1.example.com
web2.example.com

[dbservers]
db1.example.com
db2.example.com

[all:vars]
ansible_user=admin
ansible_ssh_private_key_file=/path/key.pem


YAML Example:

all:
	vars:
		ansible_user: admin
		ansible_ssh_private_key_file: /path/key.pem
	children:
		webservers:
			hosts:
				web1.example.com:
				web2.example.com:


Features

Host groups

Nested groups

Host variables

Image Placeholder: ansible_inventory.png

6️⃣ ansible.cfg Configuration

Default settings: inventory, remote_user, roles_path

SSH settings: ssh_args, timeout

Forks: Number of parallel tasks

Privilege escalation: become, become_user, become_method

Pipelining: Reduces SSH operations

Configuration precedence:
ANSIBLE_CONFIG → Current dir → ~/.ansible.cfg → /etc/ansible/ansible.cfg

7️⃣ Passwordless SSH Setup

SSH key generation: ssh-keygen

Copy key: ssh-copy-id

Optional: configure ansible.cfg for default private key

8️⃣ Ad-Hoc Commands

Syntax:

ansible <host-pattern> -m <module> -a "<args>"


Common Modules

ping

command

shell

copy

file

service

user

9️⃣ YAML Basics

Strings, numbers, booleans

List

fruits:
	- Apple
	- Orange


Dictionary

person:
	name: John
	age: 30


Nested lists/dictionaries

🔟 Ansible Playbooks (CORE)

Components

Play

Tasks

Handlers

Variables

Tags

Facts

Conditionals

Loops

Error handling

Example:

- name: Install web server
	hosts: webservers
	vars:
		http_port: 80
	tasks:
		- name: Install nginx
			apt:
				name: nginx
				state: present

1️⃣1️⃣ Modules (MOST USED)

ping, command, shell, copy, fetch, file, template

service/systemd, yum/apt/package, user/group

cron, archive/unarchive, lineinfile, replace

1️⃣2️⃣ Variables

vars section, vars_files

host vars, group vars

extra vars (-e)

registered variables

variable precedence

1️⃣3️⃣ Facts

setup module gathers system info

custom facts possible

disable for performance

1️⃣4️⃣ Templates (Jinja2)

Variable substitution: {{ var }}

Loops, conditionals, filters

Use template module to deploy

1️⃣5️⃣ Conditionals

when condition

multiple conditions

logical operators

using facts

1️⃣6️⃣ Loops

loop / with_items

list loops, dictionary loops

nested loops

1️⃣7️⃣ Handlers

Triggered with notify

Run only on task changes

Restart services

1️⃣8️⃣ Tags

Run specific tasks or skip tasks

Faster execution

1️⃣9️⃣ Roles

Modular structure: tasks, handlers, files, templates, vars, defaults, meta

Reusable and maintainable

Example structure:

my_role/
├── defaults/
├── files/
├── handlers/
├── meta/
├── tasks/
├── templates/
├── vars/

2️⃣0️⃣ Push Roles to Galaxy

Initialize git, push to GitHub

Import into Ansible Galaxy

Use ansible-galaxy install <role> to reuse

🔐 2️⃣1️⃣ Ansible Vault

Encrypt secrets and files

Use ansible-vault create/edit/encrypt/decrypt

Vault password or password file

Best practices: do not commit secrets, separate env files

⚠️ 2️⃣2️⃣ Error Handling

ignore_errors

failed_when

changed_when

block / rescue / always

2️⃣3️⃣ Privilege Escalation

become, become_user

sudo / passwordless sudo

Only escalate where needed

2️⃣4️⃣ Cloud Inventory

AWS, Azure, GCP

Inventory plugins to fetch dynamic hosts

2️⃣5️⃣ Ansible with Cloud

EC2, S3, IAM (AWS)

Azure VM management

GCP automation

2️⃣6️⃣ Ansible Tower / AWX

Web UI, RBAC, job templates, scheduling

Auditing, credential management

Image Placeholder: tower_awx_ui.png

2️⃣7️⃣ Authentication in Tower / AWX

SSH keys

Machine credentials

Vault credentials

Cloud credentials

Git credentials

2️⃣8️⃣ Tower / AWX Performance Optimization

Forks, job slicing

Disable facts, async tasks

Execution environments, pipelining

Inventory optimization

2️⃣9️⃣ Ansible Best Practices

Use roles and tags

Secure secrets

Clean directory structure

Reusable playbooks

Version control with Git

3️⃣0️⃣ Debugging & Troubleshooting

Verbose mode -vvv

Debug module

Dry run --check

Diff mode --diff

Handle common errors

3️⃣1️⃣ Ansible with CI/CD

Jenkins integration

Pipeline execution

Automated deployments

Blue/Green deployments

3️⃣2️⃣ Advanced Topics

Custom modules, plugins, callback, lookup, filters

Collections

Execution Environments

AAP (Automation Platform)

3️⃣3️⃣ Security

SSH hardening

Vault usage

RBAC (Tower/AWX)

Secrets management

3️⃣4️⃣ Real-World Use Cases

Server provisioning

App deployment

Patch management

User creation

Monitoring setup

Backup automation

Suggested Images / Diagrams

ansible_architecture.png → Control node, managed nodes, modules flow

ansible_inventory.png → Static vs Dynamic inventory

ansible_workflow.png → Playbook execution flow

tower_awx_ui.png → Tower/AWX Web UI and job templates

End of Notes


---

I can also **create a zip with the images and ready-to-use Markdown** where you can open it in VSCode or Obsidian with all diagrams placeholders filled.  

Do you want me to do that next?
