---

layout: post
title: "docker-compose 搭建 YOURLS 短网址服务"
date: 2023-11-13 
description: 
tag: 实用工具

---
#### 创建任意文件夹
在vps上自定义创建一个安装文件夹
#### 创建docker-compose.yml文件
创建docker-compose.yml文件，粘贴下面代码
```
version: '3.1'

services:

  yourls:
    image: yourls
    restart: always
    ports:
      - 8088:80
    environment:
      YOURLS_DB_HOST: 1.1.1.1:3307
      YOURLS_DB_PASS: 123456789
      YOURLS_SITE: http://1.1.1.1:8088
      YOURLS_USER: admin
      YOURLS_PASS: admin

  mysql:
    image: mysql:5.7
    restart: always
    ports:
      - "3307:3306"
    environment:
      MYSQL_ROOT_PASSWORD: 123456789
      MYSQL_DATABASE: yourls
```
#### 开放端口
在vps上开放8088和3307端口
#### 启动
```
docker-compose up -d
```
#### 查看网站
```
http://ip:8088/admin
```
