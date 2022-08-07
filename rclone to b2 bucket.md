### Outline:
Quick guide on how to setup a B2 storage as backup taget for proxmox

#### Steps:
1. Create a new Bucket in the Backblaze account (f.e. prox-backups)
	1. Save the keyID and the application key
2. Setup a Proxmox Backup Server instance
3. Add a new disk to the VM
4. Create a new Dataset with the new disk 
	1. Navigate to Storage/Disk
	2. Initialize the new disk with GPT
	3. Create a new directory
		1. Check add as datastore when saving
5. Enter shell
6. Install rclone
 ```bash 
	   apt install rclone
```

7. setup remote Backblaze repository
	1. rclone config
	2. add new
	3. enter a name (f.e. b2-backup)
	4. select service backblaze
	5. enter keyID and application key
	6. enter if you want to delete files on remote (true/false)
	7. skip advanced config
	8. finish setup
8. Setup a encryption repository (Repository that wraps around another repo and encrypts the files)
	1. rclone config
	2. add new
	3. enter a name (f.e. crypt)
	4. select crypt
	5. Enter which repository it should wrap (f.e b2-backup:)
	6. Enter encryption password 1 (save to password manager)
	7. Enter encryption password 2 (save to password manager)
	8. skip adavanced installation
	9. finish
9. Check if sync is working
	1. Navigate to /mnt/datastore/repositoryName (f.e. /mnt/datastore/b2-backup)
	2. Create a new file (f.e. touch test.txt)
	3. Sync it (you can --dry-run the command first)
        ```bash
            rclone sync /path/to/sync repositoryName:/folder
            f.e. rclone sync /mnt/datastore/b2-backup crypt: 
         ```
    4. If the command doesn't throw an error, check the remote Bucket for files 
10. Create a [[cron|cronjob]] for it


Further tips:
- Add ```--track-renames``` flag. rclone keeps track of file size and hashes and just moves in on the remote site, so it doesn't get uploaded again
- Add ```--progress``` or ```-P``` to see progress in the shell
- Add ```--verbose``` to see the remote copy messages in the log
- Use a static exlude file to ban file system lint
	- you can add a whole list of files and directories to exlude from a single file
	- ``` nano ~/.rclone/exclude.conf```
	- Add files (path), whole directories (/path/**), hidden directories and files (.hidden_file, .hidden_folder/**)
	- Add ```--exclude-from ~/.rclone/exclude.conf``` flag to exlude the whole list of files
- https://www.backblaze.com/blog/rclone-power-moves-for-backblaze-b2-cloud-storage/