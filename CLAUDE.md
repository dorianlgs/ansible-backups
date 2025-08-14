# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Architecture

This is an Ansible-based infrastructure automation system for managing WordPress hosting environments. The system provides comprehensive backup, monitoring, and server provisioning capabilities for multiple WordPress sites.

### Core Components

**Ansible Playbooks:**
- `wp_backup_playbook.yaml` - Main backup orchestration (databases, files, configs)
- `wp_checkservices_playbook.yaml` - Service health monitoring (Caddy, PHP-FPM, MySQL)
- `wp_create_server_playbook.yaml` - Full server provisioning with site migration
- `wp_create_server_without_copy_playbook.yaml` - Server provisioning without site data
- `main_photos_backup_playbook.yaml` - Dedicated photo backup with WOL automation

**Inventory Management:**
- Inventory files define server groups with specific configurations
- Each server entry includes: PHP version, database names, WordPress paths, credentials
- Template file (`server.inventory.template.ini`) shows required variable structure

**Backup Infrastructure:**
- Local sync to `server_syncs/` directory organized by server name
- Remote backup using Restic to Backblaze B2 storage
- Automated retention policy (configurable via `backup_retention_keep_last_number`)

### Data Flow Architecture

1. **Backup Process**: Database dumps → Local file sync → Restic cloud backup → Cleanup
2. **Server Creation**: System setup → WordPress deployment → Database migration → Web server config
3. **Photo Backup**: Wake-on-LAN → Local backup execution → System shutdown

## Common Commands

### WordPress Backups
```bash
ansible-playbook -i server.inventory.template.ini wp_backup_playbook.yaml
```

### Service Health Checks
```bash
ansible-playbook -i server_checks.inventory.ini wp_checkservices_playbook.yaml
```

### Server Provisioning
```bash
# Full server with site migration
ansible-playbook -i server_create.inventory.ini wp_create_server_playbook.yaml

# Clean server setup only
ansible-playbook -i server_create_without_copy.inventory.ini wp_create_server_without_copy_playbook.yaml
```

### Photo Backup System
```bash
ansible-playbook -i server_main_photos.inventory.ini main_photos_backup_playbook.yaml
```

### Backup Management
```bash
# Check backup repository integrity
./check_backup_repos.sh

# View backup snapshots
./show_snapshots.sh
```

### Manual Operations
```bash
# Ad-hoc server commands
ansible -i server.inventory.ini MyServerGroup -m command -a "free -h"

# Local WordPress restoration
./restore_wp_local.sh
```

## Configuration Requirements

### Inventory Variables
Each server entry requires:
- `servername` - Unique identifier for backup organization
- `php_version` - PHP version for service management
- `database_names` - JSON array of databases to backup
- `wp_folder_locations` - JSON array of WordPress installation paths
- `db_user_name` / `db_user_password` - Database credentials

### Global Variables
- `backup_retention_keep_last_number` - Number of backups to retain
- `restic_repo` - Restic repository URL
- `aws_access_key_id` / `aws_secret_access_key` - Backup storage credentials

### Required Files
- `pass.txt` - Restic password for WordPress backups
- `main_photos_pass.txt` - Restic password for photo backups
- `wp_files.zip` - WordPress installation archive (for server creation)
- `wp_db.sql` - WordPress database template (for server creation)

## Server Technology Stack

- **Web Server**: Caddy (automatic HTTPS, reverse proxy)
- **PHP**: PHP-FPM (version specified per server)
- **Database**: MySQL with secure installation
- **Backup Storage**: Restic + Backblaze B2
- **OS**: Ubuntu/Debian (apt-based package management)

## File Organization

- Root level: Playbooks and inventory templates
- `server_syncs/`: Organized backup storage by server name
- Individual server directories contain: databases (.sql), configs (Caddyfile, php.ini, www.conf), WordPress files
- Do not read the .txt files
- Do not read the .sh files
- Do not read the .ini files