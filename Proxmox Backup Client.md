### Outline:
The proxmox host can't be easily be backed up by the Proxmox Backup Server and the default GUI,
so the proxmox backup client can be used to backup data from the proxmox host, or any other linux machine

#### Steps:
1. Open the shell as root
2. Run the following command   ```
   ```
   proxmox-backup-client backup root.pxar:/ --repository <ipBackupServer>:<nameOfDatastore>

   # proxmox-backup-client backup root.pxar:/ --repository 192.168.x.x:prox
   ```


### Automate it:
1. Create a script:
```
#!/bin/bash
PBS_PASSWORD='pw'
PBS_FINGERPRINT='ac:eb:7f:77:22:ea....'
export PBS_PASSWORD
export PBS_FINGERPRINT

proxmox-backup-client backup root.pxar:/ --repository <ipBackupServer>:<nameOfDatastore>
```
2. Add it to [[cron|crontab]] or [[Anacron]] 
	- f.e. in crontab: 20 7 * * * /root/scripts/pbs.sh