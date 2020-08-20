---
layout: post  
title:  Linux Centtos 下安装mysql和postgresql笔记  
category: linux  
tags: [linux]  
excerpt:   
---

## 一、mysql安装
执行命令完成安装

    wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
    rpm -ivh mysql-community-release-el7-5.noarch.rpm
    yum update
    yum install mysql-server

权限设置：

    chown mysql:mysql -R /var/lib/mysql
初始化 MySQL：

    mysqld --initialize
启动 MySQL：

    systemctl start mysqld
查看 MySQL 运行状态：

    systemctl status mysqld

如果没有意外，mysql已经安装成功。  
然而是事情并没有那么简单。安装完成后，执行“systemctl start mysqld”命令启动mysql服务发现报错启动不了。

解决方式：查看mysql错误日志

/var/log/mysql/mysqld.log文件。//不同安装情况下日志位置可能不同。
错误如下

>2020-08-19T05:49:26.559072Z 1 [ERROR] [MY-012271] [InnoDB] The innodb_system data file 'ibdata1' must be writable

ibdata1文件没有写入权限。 解决方式如下。
 
    chmod -R 777 /var/lib/mysql

重新执行**systemctl start mysqld**命令启动mysql服务，服务正常启动。    
  
**更改mysql初始密码**

 通过/var/log/mysql/mysqld.log文件获取root初始密码为 2ZnH*w(nwm>S  
 
 >2020-08-19T05:48:58.056339Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: 2ZnH*w(nwm>S


    [root@linux ~]# mysql -uroot -p
    Enter password:

输入初始密码，登录。

    mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'xxxxxxxx';
    Query OK, 0 rows affected (0.11 sec)
    
    mysql> flush privileges;
    Query OK, 0 rows affected (0.01 sec)

执行更改密码语句。
此处参考https://www.cnblogs.com/wuotto/p/9682400.html，不同系统下修改方式不同。

然后使用navicat远程连接mysq还是连接不上  
一.考虑是否开启了防火墙未开启端口。云服务器考虑是不是网络安全组设置未开启相应端口。  
二.开放端口后重新链接报"Host 'xxxx' is not allowed to connect to this MySQL server"  
解决方法：  
修改mysql权限表 
    
        use mysql;   
        update user set host='%' where user='root';  
        flush privileges; 

##二、centos8安装postgresql记录  
###1.安装    

    # Install the repository RPM:  
    dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm  

    # Disable the built-in PostgreSQL module:  
    dnf -qy module disable postgresql  

    # Install PostgreSQL:  
    dnf install -y postgresql11-server  

    # Optionally initialize the database and enable automatic start:  
    /usr/pgsql-11/bin/postgresql-11-setup initdb  
    systemctl enable postgresql-11  
    systemctl start postgresql-11  

###2.修改远程连接限制    
修改/var/lib/pgsql/11/data/文件夹下    
**1.postgresql.conf文件**  

    # - Connection Settings -  
    listen_addresses = '*'		# what IP address(es) to listen on;
                        # comma-separated list of addresses;
                        # defaults to 'localhost'; use '*' for all
                        # (change requires restart)

**2.pg_hba.conf文件**   
添加   
>host    all             all             0.0.0.0/0               md5  


重新启动postgresql    

     systemctl restart postgresql  

###3.修改默认postgres用户密码  

步骤一：登录PostgreSQL  

    sudo -u postgres psql  

步骤二：执行改密码脚本  

    postgres=# ALTER USER postgres WITH PASSWORD 'postgresql';  
成功提示 ALTER ROLE 