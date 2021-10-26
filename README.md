# nfs

* https://www.tecmint.com/install-nfs-server-on-centos-8/

## Install nfs-utils client & server in kernel
```
dnf install -y nfs-utils
systemctl enable nfs-server.service --now
```

## Mountage sharing
* /etc/nfs.conf – main configuration file for the NFS daemons and tools.
* /etc/nfsmount.conf – an NFS mount configuration file.

```
tee /etc/exports<<EOF
/var/www/html/symfony/  	192.168.0.0/24(rw,sync)
/mnt/backup			192.168.0.0/24(rw,sync,no_all_squash,root_squash)
EOF

exportfs -arv
exporting 192.168.0.0/24:/mnt/backup
exporting 192.168.0.0/24:/var/www/html/symfony
```
* rw – allows both read and write access on the file system.
* sync – tells the NFS server to write operations (writing information to the disk) when requested (applies by default).
* all_squash – maps all UIDs and GIDs from client requests to the anonymous user.
* no_all_squash – used to map all UIDs and GIDs from client requests to identical UIDs and GIDs on the NFS server.
* root_squash – maps requests from root user or UID/GID 0 from the client to the anonymous UID/GID.


* Display current sharing
```
exportfs -s
/var/www/html/symfony  192.168.0.0/24(sync,wdelay,hide,no_subtree_check,sec=sys,rw,secure,root_squash,no_all_squash)
/mnt/backup  192.168.0.0/24(sync,wdelay,hide,no_subtree_check,sec=sys,rw,secure,root_squash,no_all_squash)
```


## Set up the firewall
```
firewall-cmd --permanent --add-service=nfs
firewall-cmd --permanent --add-service=rpc-bind
firewall-cmd --permanent --add-service=mountd
firewall-cmd --reload
```


# On the client side
```
dnf install nfs-utils nfs4-acl-tools
showmount -e 192.168.0.40

```


## Linux
mkdir -p ./backup
mount -t nfs  192.168.0.40:/mnt/backup ./backup

## MacOSx
mkdir $PWD/symfony
sudo mount -t nfs -o resvport,rw   192.168.0.40:/var/www/html/symfony $PWD/symfony
```
