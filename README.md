# Ansible Backups

A collection of Ansible playbooks for managing WordPress backups, server creation, and monitoring.

## Playbooks

### WordPress Backups
```bash
ansible-playbook -i server.inventory.template.ini wp_backup_playbook.yaml
```

### WordPress Service Checks
```bash
ansible-playbook -i server_checks.inventory.ini wp_checkservices_playbook.yaml
```

### Main Photos Backup
```bash
ansible-playbook -i server_main_photos.inventory.ini main_photos_backup_playbook.yaml
```

### WordPress Server Creation
```bash
ansible-playbook -i server_create.inventory.ini wp_create_server_playbook.yaml
```

### WordPress Server Creation (Without Copy)
```bash
ansible-playbook -i server_create.inventory.ini wp_create_server_without_copy_playbook.yaml
```

## Ad-hoc Commands

Example command to check memory usage:
```bash
ansible -i server.inventory.ini MyServerGroup -m command -a "free -h"
```

## Automation

### Cron Schedule
Daily backup at 4:05 AM:
```bash
5 4 * * * /opt/homebrew/bin/ansible-playbook -i /Users/doriangonzalez/Workspace/ansible-backups/server.inventory.template.ini /Users/doriangonzalez/Workspace/ansible-backups/wp_backup_playbook.yaml
```
