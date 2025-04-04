# ansible-backups

## Run

```bash
ansible-playbook -i server.inventory.ini wp_backup_playbook.yaml
```

## Ad-hoc command

```bash
ansible -i server.inventory.ini MyServerGroup -m command -a "free -h"
```

## Cron

```bash
5 4 * * * /opt/homebrew/bin/ansible-playbook -i /Users/doriangonzalez/Workspace/ansible-backups/server.inventory.ini /Users/doriangonzalez/Workspace/ansible-backups/wp_backup_playbook.yaml
```
