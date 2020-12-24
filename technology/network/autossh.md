---
title: AutoSSH
description: AutoSSH 可以完成从公网反向代理到本地进行 IP 和 Port 的映射
published: true
date: 2020-12-24T17:16:04.411Z
tags: network, autossh
editor: markdown
dateCreated: 2020-11-30T15:00:22.007Z
---

# AutoSSH

AutoSSH 可以完成从公网反向代理到本地进行 IP 和 Port 的映射。


## 安装

前往 https://www.harding.motd.ca/autossh/ 完成下载

```
gunzip -c autossh-1.4e.tgz | tar xvf -
cd autossh-1.4e
./configure
make
# copy binary to where you wish it, or "make install" will install it under /usr/local by default.
# examine autossh.host for example wrapper script and options
```


## 守护进程

### PM2

> 该小节内容需要完善...
{.is-warning}

### init.d

这里我们以端口重定向做演示，将公网的某台虚拟机反向代理到本地某个运行中的程序。

**/etc/init/autossh.conf**
```
#!/bin/bash

while true
do
autossh -M0 -v -N -R [port]:localhost:[port] [user name]@[ip address]
done
```