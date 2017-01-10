---
title: ntopng 安装使用
date: 2016-12-16 15:07:57
tags: ntopng ntop  网络监控


---
##### centOS7 安装参考
https://devops.profitbricks.com/tutorials/install-ntopng-network-traffic-monitoring-tool-on-centos-7/

#### 步骤
#### Installing Ntopng
```
yum install epel-release
```

```
vim /etc/yum.repos.d/ntop.repo
```
Add the following content to the ntop.repo file:

```
[ntop]
name=ntop packages
baseurl=http://www.nmon.net/centos-stable/$releasever/$basearch/
enabled=1
gpgcheck=1
gpgkey=http://www.nmon.net/centos-stable/RPM-GPG-KEY-deri
[ntop-noarch]
name=ntop packages
baseurl=http://www.nmon.net/centos-stable/$releasever/noarch/
enabled=1
gpgcheck=1
gpgkey=http://www.nmon.net/centos-stable/RPM-GPG-KEY-deri
```

```
yum -y update
```

```
yum --enablerepo=epel install redis ntopng
```

#### Start the Ntopng and Redis Service
```
yum --enablerepo=epel install hiredis-devel
```

```
systemctl start redis.service
systemctl enable redis.service
```

```
systemctl start ntopng.service
systemctl enable ntopng.service
```

#### Configure Ntopng
Ntop will create a default configuration file at /etc/ntopng/ntopng.conf
However if you check the status, you’ll see that ntop gives you a "No Pro licence is found" error, and announces that it will return to community mode after 10 minutes.

To check the ntopng status, run
```
systemctl status ntopng
```
remove this warning message by editing the ntopng configuration file:
```
vim /etc/ntopng/ntopng.conf
```

Add/change the line shown below:
```
-G=/var/tmp/ntopng.pid\
--community
```

Save and exit the file, restart ntopng and check status again:
```
systemctl restart ntopng
systemctl status ntopng
```

Allow Ntopng Through the Firewall
```
firewall-cmd --permanent --add-port=3000/tcp

irewall-cmd --reload
```
#### Test Ntopng

http://your.server.ip:3000. Use the login information:

User: admin Password: admin

Enjoy...

编译安装参考：
https://linux.cn/article-5664-1.html
使用参考官网：
http://www.ntop.org/products/traffic-analysis/ntop/
docker版本部署参考：
https://github.com/Laisky/ntopng-docker
源码：
https://github.com/ntop/ntopng
redis:
https://redis.io/