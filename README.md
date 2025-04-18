# ansible-backups

## Run WP Backups

```bash
ansible-playbook -i server.inventory.ini wp_backup_playbook.yaml
```

## Run WP check services

```bash
ansible-playbook -i server_checks.inventory.ini wp_checkservices_playbook.yaml
```

## Ad-hoc command

```bash
ansible -i server.inventory.ini MyServerGroup -m command -a "free -h"
```

## Cron

```bash
5 4 * * * /opt/homebrew/bin/ansible-playbook -i /Users/doriangonzalez/Workspace/ansible-backups/server.inventory.ini /Users/doriangonzalez/Workspace/ansible-backups/wp_backup_playbook.yaml
```
