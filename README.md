# 2420_week_11_lab

##Authors
- Eric Cao A01025661
- Aaron Arcalas A01196893

## Description
This a service to backup files to a specific folder everyday friday at 01:00.

## Files within the repo
- **backup-script**: a script used to back up files
- **backup-service**: a script used in conjuction with the backup script
- **backup-timer**: a script used to activate the backup-service file at friday 01:00
- **backup-config**: a config file for backup variables

## Prerequisites
**Assumptions**: You have watched the following video. The video explains how to set up a Digital Ocean Droplet.
If you have not watched it here is the link [DigitalOcean](https://vimeo.com/758870226/f75da348fc?embedded=true&source=video_title&owner=17609105)

## Create the backup script in /opt
1. Cd into the directory `/opt`
2. Create a directory in `/opt` using `sudo mkdir /opt/service`

## Making the backupscript

```
#!/bin/bash
source /opt/service/back.conf

rsync -aPve "ssh -o StrictHostKeyChecking=no -i $ssh_key" $dir $backup_user@address:$dir2
```
**Note**: Make sure to give backup-script execute permission `chmod +x backup-script`

# Create the config file in /opt

```
address=(164.92.115.241)
dir=("/home/aaronuser/test")
dir2=("/home/aaronuser")
ssh-key=("~/.ssh/key-pair")
backup_user=("aaronuser)
```

## Create backup-service file in /opt

```
[Unit]
Description=Backup directories read from a config file to the given IP address

[Service]
Type=oneshot
ExecStart=/opt/service/backup-script

[Install]
WantedBy=multi-user.target
```

## Create backup-timer file in /opt

```
#!/bin/bash

[Unit]
Description=Timer to start the backup-script to back up files every Friday at 01:00

[Timer]
OnCalendar=Friday --* 01:00
Persistent=true
RandomizedDelaySec=10

[Install]
WantedBy=timers.target
```

## To activate service
1. Move file in `/etc/systemd/system` directory
2. Reload the service `sudo systemctl daemon-reload`
3. To start backup-service `sudo systemctl start backup.service`
4. Check status of service `sudo systemctl status backup.service`
5. Enable backup-service `sudo systemctl enable --now backup.service`
6. Enable backup-timer `sudo enable --now backup.timer
7. Check if backup-timer is running/active `systemctl list-timer`

