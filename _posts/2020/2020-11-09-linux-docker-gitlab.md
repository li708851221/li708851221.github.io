---
layout: post  
title:  使用docker安装GitLab    
category: docker     
tags: [gitlab]    
excerpt:   
---

## 一. 下载镜像文件

    docker pull beginor/gitlab-ce:11.0.1-ce.0

## 二. 创建容器外文件    

创建GitLab 的配置 (etc) 、 日志 (log) 、数据 (data) 放到容器之外， 便于日后升级    
      
      mkdir -p /mnt/gitlab/etc
      mkdir -p /mnt/gitlab/log
      mkdir -p /mnt/gitlab/data


## 三. 运行GitLab容器    

进入/mnt/gitlab/etc目录，运行一下命令
      
      docker run \
          --detach \
          --publish 8443:443 \
          --publish 8090:80 \
          --name gitlab \
          --restart unless-stopped \
          -v /mnt/gitlab/etc:/etc/gitlab \
          -v /mnt/gitlab/log:/var/log/gitlab \
          -v /mnt/gitlab/data:/var/opt/gitlab \
          beginor/gitlab-ce:11.0.1-ce.0 

## 四. 修改/mnt/gitlab/etc/gitlab.rb   

把external_url改成部署机器的域名或者IP地址  

    vim /mnt/gitlab/etc/gitlab.rb

将external_url修改为 'http://真实IP'

## 五. 修改/mnt/gitlab/data/gitlab-rails/etc/gitlab.yml    

    vim /mnt/gitlab/data/gitlab-rails/etc/gitlab.yml

找到关键字 * ## Web server settings *

将host的值改成映射的外部主机ip地址和端口

## 六.重启docker容器    

先停止该容器，删掉该容器信息，重启完docke之后，重新运行GitLab容器

    docker ps
    docker stop id --id容器id
    docker rm id
    
   再次执行第三步运行GitLab容器
   
## 七.完成
   
   gitlab的web管理页面就可以正常访问    
   
   
     





