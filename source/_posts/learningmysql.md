---
title: 在阿里云centos7.0 上安装mysql最新版方法
tags: CentOS
---
## 使用rpm方法
1.在http://dev.mysql.com/downloads/mysql/#downloads 上选择
(1):(mysql-community-common-5.7.11-1.el7.x86_64.rpm)
(2):(mysql-community-libs-5.7.11-1.el7.x86_64.rpm)
(3):(mysql-community-client-5.7.11-1.el7.x86_64.rpm)
(4):(mysql-community-devel-5.7.11-1.el7.x86_64.rpm)
(5):(mysql-community-server-5.7.11-1.el7.x86_64.rpm)

5条命令来安装上面5个;
``` bash
[root@iZ28m9hwbnmZ ~]# wget -c http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-community-common-5.7.11-1.el7.x86_64.rpm
```
<!--more-->

2.接着按顺序用
``` bash
[root@iZ28m9hwbnmZ ~]#  rpm -ivh mysql-community-common-5.7.11-1.el7.x86_64.rpm
```
把这写装好
3.然后初始化数据库
``` bash
[root@iZ28m9hwbnmZ ~]# mysql_install_db --datadir=/var/lib/mysql
``` 
//必须指定datadir,执行后会生成~/.mysql_secret密码文件
网上还有一种方法适合新版本的
[root@iZ28m9hwbnmZ ~]# mysqld --initialize                         //新版的推荐此方法，执行生会在/var/log/mysqld.log生成随机密码
4.接着我是看到了网上别人写的
更改mysql数据库目录的所属用户及其所属组，然后启动mysql数据库
[root@iZ28m9hwbnmZ ~]# chown mysql:mysql /var/lib/mysql -R
[root@iZ28m9hwbnmZ ~]# systemctl start mysqld.service //启动mysql数据库服务 
5.之后设置密码和改密码
[root@iZ28m9hwbnmZ ~]# mysql -uroot -p'Il#/C1i2rHdR'
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.11

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> set password=password('******');
Query OK, 0 rows affected, 1 warning (0.00 sec)
## 最终完成了参考教程网站http://www.cnblogs.com/5201351/p/4912614.html
# 在阿里云上完全卸载mysql
## 第一种yum装的mysql卸载
命令
1.yum remove mysql mysql-server mysql-libs 
2.rm -rf /var/lib/mysql
3.rm /etc/my/cnf
查看还有没有mysql软件:
rpm -qa|gre mysql
对于rpm的安装方式的查询
[root@iZ28m9hwbnmZ ~]# rpm -qa|grep -i mysql
mysql-community-libs-5.7.11-1.el7.x86_64
mysql-community-common-5.7.11-1.el7.x86_64
mysql-community-client-5.7.11-1.el7.x86_64
mysql-community-devel-5.7.11-1.el7.x86_64
mysql57-community-release-el7-7.noarch
mysql-community-server-5.7.11-1.el7.x86_64
## 第二种rpm安装的mysql怎么卸载
卸载命令是
rpm -e mysql-community-libs-5.7.11-1.el7.x86_64
就是 rpm -e softname
2删除服务指令
chkconfig --list | grep -i mysql 和 chkconfig --del mysql
3.删除多余文件
先用whereis mysql;
查找路径
4.接着用 rm -rf path 删除文件和文件夹
