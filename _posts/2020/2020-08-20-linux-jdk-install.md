---
layout: post  
title:  Linux下安装jdk笔记  
category: linux  
tags: [linux]  
excerpt:   
---

##1、下载合适的 JDK 。
注意：这里需要下载 Linux 版本。这里以jdk-8u201-linux-x64.tar.gz为例。
  
##2、创建目录

在/usr/目录下创建java目录，

    mkdir /usr/java
    cd /usr/java
把下载的文件 jdk-8u201-linux-x64.tar.gz 放在/usr/java/目录下。
  
##3. 解压 JDK

    tar -zxvf jdk-8u201-linux-x64.tar.gz

##4. 设置环境变量

修改 vim /etc/profile

在 profile 文件最下面添加如下内容并保存：

    set java environment
    JAVA_HOME=/usr/java/jdk1.8.0_201       
    JRE_HOME=/usr/java/jdk1.8.0_201/jre     
    CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
    PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
    export JAVA_HOME JRE_HOME CLASS_PATH PATH
 
让修改生效：

    source /etc/profile

##5. 测试
    java -version


>java version "1.8.0_201"
Java(TM) SE Runtime Environment (build 1.8.0_201-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.201-b09, mixed mode)

显示 java 版本信息，则说明 JDK 安装成功




