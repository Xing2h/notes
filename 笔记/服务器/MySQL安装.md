[TOC]



# CentOS7.6 安装mysql8.0

### 1、检查是否存在mariadb，如果存在就删除

```
rpm -qa | grep mariadb
rpm -e mariadb-libs-5.5.68-1.el7.x86_64 --nodeps
```

### 2、配置清华镜像

```
vim /etc/yum.repos.d/mysql-community.repo
//添加内容
[mysql-connectors-community]
name=MySQL Connectors Community
baseurl=https://mirrors.tuna.tsinghua.edu.cn/mysql/yum/mysql-connectors-community-el7-$basearch/
enabled=1
gpgcheck=1
gpgkey=https://repo.mysql.com/RPM-GPG-KEY-mysql

[mysql-tools-community]
name=MySQL Tools Community
baseurl=https://mirrors.tuna.tsinghua.edu.cn/mysql/yum/mysql-tools-community-el7-$basearch/
enabled=1
gpgcheck=1
gpgkey=https://repo.mysql.com/RPM-GPG-KEY-mysql

[mysql-5.6-community]
name=MySQL 5.6 Community Server
baseurl=https://mirrors.tuna.tsinghua.edu.cn/mysql/yum/mysql-5.6-community-el7-$basearch/
enabled=0
gpgcheck=1
gpgkey=https://repo.mysql.com/RPM-GPG-KEY-mysql

[mysql-5.7-community]
name=MySQL 5.7 Community Server
baseurl=https://mirrors.tuna.tsinghua.edu.cn/mysql/yum/mysql-5.7-community-el7-$basearch/
enabled=1
gpgcheck=1
gpgkey=https://repo.mysql.com/RPM-GPG-KEY-mysql

[mysql-8.0-community]
name=MySQL 8.0 Community Server
baseurl=https://mirrors.tuna.tsinghua.edu.cn/mysql/yum/mysql-8.0-community-el7-$basearch/
enabled=1
gpgcheck=1
gpgkey=https://repo.mysql.com/RPM-GPG-KEY-mysql
```

### 3、安装mysql-community-server

```
yum -y install mysql-community-server
```

### 4、修改my.cnf

```
vim /etc/my.cnf
#注释掉下面这两条
#datadir=/var/lib/mysql
#socket=/var/lib/mysql/mysql.sock
datadir=/home/mysql
socket=/home/mysql/mysql.sock
character-set-server=utf8
default_authentication_plugin=mysql_native_password
[client]
default-character-set=utf8
socket=/home/mysql/mysql.sock
[mysql]
default-character-set=utf8
socket=/home/mysql/mysql.sock
```

### 5、启动并修改密码

```
#启动mysql并设置开机自启
systemctl start mysqld && systemctl enable mysqld
#查看初始密码，mysql启动成功才会有日志生成，才能查看到密码
grep 'password' /var/log/mysqld.log
#使用查看到的密码登录mysql
mysql -uroot -p
#修改密码
alter user 'root'@'localhost' identified with mysql_native_password by 'Xzh0627..';
#退出
quit;
```

### 6、修改root权限，增加远程登录

```
#使用新密码登录MySQL
mysql -uroot -p
#设置权限
use mysql
update user set host = '%' where user='root';
flush privileges;
alter user 'root'@'%' identified with mysql_native_password by 'Xzh0627..';
flush privileges;
quit;
```

7、设置防火墙策略

```
#关闭防火墙
systemctl stop firewalld && systemctl disable firewalld
#开放3306端口
/sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT
```

密码：Xzh0627..
