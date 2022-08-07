### Outline:
A quick guide to exlude certain disk from backups to proxmox backup server.

#### Steps:
There are 2 ways:
1. GUI
	1. Click on Container/VM,... in the proxmox gui
	2. Open Hardware
	3. Select and Edit Hard Disk
	4. Remove tick from checkbox "Backup"
2. SHELL
	1. Open the shell as root
	2. Navigate to /etc/pve/qemu-server
	3. Open the config file of the instance you want to alter
	4. add "backup=0," into the lines that represent the disks you want to skip
	   f.e. scsi1: local-zfs:vm-800-disk-1,backup=0,size=120G