---
layout: post
title: Mac下自启动mongodb
img: mongodb.png # Add image post (optional)
tags: [mongodb]
---

安装mongodb
```
>brew install mongodb
```
链接plist文件
```
>ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
```
默认的plist文件指定的MongoDB配置文件是 /usr/local/etc/mongod.conf，配置中dbpath为/usr/local/var/mongdb，
修改dbpath为你自己的数据库路径