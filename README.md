# ansible-backups

## Run WP Backups

```bash
ansible-playbook -i server.inventory.template.ini wp_backup_playbook.yaml
```

## Run WP check services

```bash
ansible-playbook -i server_checks.inventory.ini wp_checkservices_playbook.yaml
```

## Run Main Photos Backup

```bash
ansible-playbook -i server_main_photos.inventory.ini main_photos_backup_playbook.yaml
```

## Run Create WP Server

```bash
ansible-playbook -i server_create.inventory.ini wp_create_server_playbook.yaml
```


## Ad-hoc command

```bash
ansible -i server.inventory.ini MyServerGroup -m command -a "free -h"
```

## Cron

```bash
5 4 * * * /opt/homebrew/bin/ansible-playbook -i /Users/doriangonzalez/Workspace/ansible-backups/server.inventory.template.ini /Users/doriangonzalez/Workspace/ansible-backups/wp_backup_playbook.yaml
```
