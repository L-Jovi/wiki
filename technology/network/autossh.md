---
title: AutoSSH
description: AutoSSH 可以完成从公网反向代理到本地进行 IP 和 Port 的映射
published: true
date: 2021-01-01T15:02:46.031Z
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

这里我们以端口重定向为例，将公网的某台 VPS 反向代理到本地某个运行中的程序，并将该任务添加到 Linux 守护进程 init.d 中。

`vi /etc/init/autossh.conf`

```
#!/bin/bash

while true
do
autossh -M0 -v -N -R [port]:localhost:[port] [user name]@[ip address]
done
```