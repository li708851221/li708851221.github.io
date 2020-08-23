---
layout: post  
title:  Linux下安装maven笔记  
category: linux  
tags: [linux]  
excerpt:   
---

## 一. 下载压缩包:

官网地址: http://maven.apache.org/download.cgi  
下载apache-maven-3.6.3-bin.tar.gz文件

## 二. 上传到linux的/usr/maven目录

    cd /usr/maven


## 三. 解压文件

    tar -zxvf apache-maven-3.6.3-bin.tar.gz

## 四.  配置环境变量

    vim /etc/profile
        
在 profile 文件最下面添加如下内容并保存：

    export MAVEN_HOME=/usr/local/apache-maven-3.6.1
    export PATH=$MAVEN_HOME/bin:$PATH 



## 五. 刷新环境变量 

    source /etc/profile

## 六. 检查版本

    mvn -v 
  
  
>Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: /usr/maven/apache-maven-3.6.3
Java version: 1.8.0_201, vendor: Oracle Corporation, runtime: /usr/java/jdk1.8.0_201/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "4.18.0-193.14.2.el8_2.x86_64", arch: "amd64", family: "unix"




