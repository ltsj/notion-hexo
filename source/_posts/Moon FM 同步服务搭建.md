---
categories: ''
tags: []
description: ''
permalink: ''
title: Moon FM 同步服务搭建
cover: /images/24c3bef21dbe219801f421672961ae73.jpg
date: '2024-08-19 03:59:00'
updated: '2024-08-19 05:07:00'
---

## 安装 CouchDB


使用docker进行安装


`docker run -e COUCHDB_USER=账号 -e COUCHDB_PASSWORD=密码 -d --name couchdb -p 127.0.0.1:5984:5984 --restart=always couchdb:2.3.1`


版本需要选定2.3.1，这个版本包含了 couch_peruser 配置项，更高的版本我没有找到，可能是有的。


## 配置 CouchDB


首先使用浏览器登录管理界面，地址为：https://[url]/_utils/。


使用 admin 用户登录，进入设置界面，将 couch_peruser 的 enable 项目调整为 true 。


创建_users 数据库


`curl -X PUT <http://admin>:[admin 密码]@[url]/_users \\      -H "Accept: application/json" \\      -H "Content-Type: application/json" \\`


如成功，则返回:


`{"ok":true}`


在_users 数据库中创建用户


例如创建一个用户名为 jan 密码为 apple 的用户


`curl -X PUT <http://admin>:[admin 密码]@[url]/_users/org.couchdb.user:jan \\      -H "Accept: application/json" \\      -H "Content-Type: application/json" \\      -d '{"name": "jan", "password": "apple", "roles": [], "type": "user"}'`


如成功，则返回:


`{"ok":true,"id":"org.couchdb.user:jan","rev":"xxxx"}`


同时，CouchDB 会为新建用户创立一个 userdb- 为前缀的数据库，在上例中 jan 的数据库为 userdb-6a616e 。那么 jan 在 Moon FM 的同步地址为


[http://jan](http://jan/):apple@[url]/userdb-6a616e

