# NFS Mount Server And Client Configuration

## Install NFS (Network File System) on Both Servers

```bash
yum install nfs-utils vim -y
```

## Configure the NFS Server

Edit the exports file:

```bash
vim /etc/exports
```

Add the following line:

```bash
/var/www/html/calls 172.16.14.74(rw,sync,no_subtree_check)
```

Start and enable NFS services on the NFS server:

```bash
systemctl start nfs-server
systemctl enable nfs-server

exportfs -a

systemctl restart nfs-server
```

## Configure the NFS Client

Create the directory to mount the NFS share:

```bash
mkdir -p /var/www/html/calls
```

Mount the NFS share on the client server:

```bash
mount -t nfs 172.16.14.52:/var/www/html/calls /var/www/html/calls
```

## Automatic Mount on Boot

Edit the `fstab` file:

```bash
vim /etc/fstab
```

Add the following line:

```bash
172.16.14.52:/var/www/html/calls /mnt/voice_calls nfs defaults 0 0
