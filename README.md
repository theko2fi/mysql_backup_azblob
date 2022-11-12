mysql_backup_azblob
===================

Ansible role to schedule full MySQL/MariaDB database backup via cron job. It also offers the ability to upload the backup file to Azure Blob Storage container.

Role Variables
--------------

This role support the following variables:
```
| variable             | description                             | default value                 |
|----------------------|-----------------------------------------|-------------------------------|
| mysql_backup_dir     | database backup destination path        | /db/backup                    |
| backup_user          | user account used for the backup job    | root                          |
| minute               | Minute when the job should run          | "01"                          |
| hour                 | Hour when the job should run            | "01"                          |
| day                  | Day when the job should run             | "*"                           |
| month                | Month when the job should run           | "*"                           |
| weekday              | Weekday when the job should run         | "*"                           |
| azcopy_user          | owner of azcopy executable file         | value of backup_user          |
| azcopy_group         | group of azcopy executable file         | value of backup_user          |
| azcopy_bin_path      | download path of azcopy binary file     | value of mysql_backup_dir     |
| storage_resource_uri | storage resource uri in Azure           | N/A                           |
| blob_sas_token       | the SAS token generated in Azure        | N/A                           |
```
All of these parameters are optional, but if you want to enable the uploading to Azure Blob storage, you MUST specify the following ones:
  - storage_resource_uri
  - blob_sas_token

Examples Playbook
----------------

This sample playbook will dump all mysql/mariadb databases, every day at 01:01 and store the file to `/backup/dumps` directory :

    - hosts: all
      roles:
         - role: theko2fi.mysql_backup_azblob
      vars:
        mysql_backup_dir: /backup/dumps

If you would like to schedule the backup to a different time, let's say every day at 04:02, you could do this:

```
    - hosts: all
      roles:
         - role: theko2fi.mysql_backup_azblob
      vars:
        mysql_backup_dir: /backup/dumps
        minute: 04
        hour: 02
```
Example of playbook with upload to Azure blob storage enabled:

```
    - hosts: all
      become: true
      roles:
         - role: theko2fi.mysql_backup_azblob
      vars:
        blob_sas_token: "sp=rcw&st=2022-11-10T21:47:37Z&se=2022-11-11T05:47:37Z&spr=https&sv=2021-06-08&sr=c&sig=YWJjZGVmZw%3d%3d&sig=Rcp6gQRfV7WDlURdVTqCa%2bqEArnfJxDgE%2bKH3TCChIs%3d"
        storage_resource_uri: "https://backupblobexample.blob.core.windows.net/dbbackup"
```

License
-------

BSD

Author Information
------------------

Created by Kenneth KOFFI, you can reach him on the following platforms:

- medium:   https://medium.com/@theko2fi
- github:   https://github.com/theko2fi
- linkedin: https://www.linkedin.com/in/kenneth-koffi-6b1218178/
