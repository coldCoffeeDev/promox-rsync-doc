### Outline:
How to remove directories/datastores

#### Steps:
1. Remove datastore
    ``` proxmox-backup-manager datastore remove store1 ``` or via GUI
2. Disable the mount
    ```systemctl disable mnt-datastore-<id>.mount ```
3. rm the service
    ``` rm /etc/systemd/system/<id>``` 
 4. Check in the ui if the directory and the datastore are deleted