# Mount a CIFS/Samba share in read-write mode in Linux

#cifs #samba #linux #filesystem  

-----

Mount a CIFS/Samba share in Linux with read & write permissions

## Prerequisites

[[install-the-cifs-utils-package|cifs-utils]]

## Credentials

If credentials are required, create a credentials file in a secure folder

```bash
vim /secrets/domain_creds
```

And populate it with your domain/host Credentials

```
username=<username>
password=<password>
domain=<domain or hostname>
```

Then limit the permissions to prevent reads by other users

```bash
chmod 600 /secrets/domain_creds
```

## Mount

Edit `/etc/fstab` and append a line to ensure the shared folder will be mounted during startup

```bash
sudo vim /etc/fstab
```

Append line in the format:

```
<remote_path> <local_path> cifs credentials=<cred_file>,uid=<uid>,gid=<gid>,vers=3.0,iocharset=utf8,file_mode=0777,dir_mode=0777,noperm    0    0
```

Example:

```
//10.0.0.4/Library /mnt/library cifs credentials=/secrets/my_server,uid=1000,gid=1000,vers=3.0,iocharset=utf8,file_mode=0777,dir_mode=0777,noperm    0    0
```

Save the changes and then mount:

```bash
sudo mount -a
```






