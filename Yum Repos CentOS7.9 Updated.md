# CentOS 7.9 Updated Vault Repositories

First delete the default CentOS-Base.repo file
```
rm -rf /etc/yum.repos.d/CentOS-Base.repo
```

Now create a new `CentOS-Base.repo` file and add the Repository configuration:

```bash
vi /etc/yum.repos.d/CentOS-Base.repo
```

Copy and paste the below Repository Configuration in CentOS-Base.repo file

```
[base]
name=CentOS-$releasever - Base
baseurl=https://vault.centos.org/centos/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#released updates
[updates]
name=CentOS-$releasever - Updates
baseurl=https://vault.centos.org/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that may be useful
[extras]
name=CentOS-$releasever - Extras
baseurl=https://vault.centos.org/centos/$releasever/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-$releasever - Plus
baseurl=https://vault.centos.org/centos/$releasever/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

```
Now test the repos by installing a random package 

```
yum install tree -y
```
