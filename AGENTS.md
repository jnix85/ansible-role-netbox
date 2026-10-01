# Project Context: ansible-role-netbox

**Type:** Ansible Role / Infrastructure Automation  
**Target:** Debian 12 (Bookworm), Debian 13 (Trixie), Ubuntu  
**Service:** [NetBox](https://netboxlabs.com) DCIM / IPAM via netbox-docker

---

## Overview

Deploys NetBox active/standby pair with scheduled pg_dump/media replication and failover scripting.
Serves as the IPAM backend for Proxmox VE Software-Defined Networking (SDN).

---

## Repository Layout

```
.
├── AGENTS.md                         # AI source of truth
├── CLAUDE.md -> AGENTS.md            # Symlink for Claude Code
├── README.md                         # Human documentation
├── ansible.cfg                       # Inventory and SSH settings (wires shared-inventory)
├── requirements.yml                  # Collections
├── inventory/
│   ├── hosts.yml                     # netbox_servers group
│   └── group_vars/                   # all/ and netbox_servers/
├── playbooks/
│   └── site.yml                      # Main playbook
└── roles/
    └── netbox/                       # Role implementation
```

---

## Common Commands

```bash
# Syntax check
ansible-playbook -i inventory/hosts.yml playbooks/site.yml --syntax-check

# Dry-run
ansible-playbook -i inventory/hosts.yml playbooks/site.yml --check --diff
```
