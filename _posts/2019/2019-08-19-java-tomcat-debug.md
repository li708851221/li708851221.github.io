---
layout: post
title: Java远程调试
category: java
tags: [linux]
excerpt: 使用idea进行tomcat远程调试
copyfromurl: 
copyfromname: 
---
## 1.配置remote

点击Edit Configurations，进入Run/Debug Configurations界面，然后点击左上角的+，选择Remote：

![](https://li708851221.github.io/assets/images/2019/java/debug/remote.jpg) 

修改图中的host地址为实际需要远程调试的服务器地址，端口填写一个未被占用的端口（端口不要填写成正常服务的端口）。

此处我们设置成了8001。记住这个端口！

在use module classpath 中选择你要调试的module。这里要保证你的本机程序和远程调试程序代码保持完全一致。

其他设置默认。

## 2.修改catalina文件

1.服务器是linux系统  

复制下面语句到服务器tomcat bin目录下catalina.sh文件中的开始位置。

    export JAVA_OPTS='-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8001'
    
2.服务器是windows系统  
    
复制下面语句到服务器tomcat bin目录下catalina.sh文件中的开始位置。
    
    set JAVA_OPTS=-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8001
    
注：address为自定义的端口号，这个端口需要和第一步中设置的端口保持一致。


## 3.开始调试

先启动tomcat，后启动本地程序，调试方法和本地程序调试完全一样，在想要调试的地方打上断点，就可以愉快的调试了。
    
    


