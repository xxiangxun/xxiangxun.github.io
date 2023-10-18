---

layout: post
title: "一个反向代理神器——Nginx Proxy Manager"
date: 2023-10-18 
description: 
tag: 实用工具

---

#### 安装 Nginx Proxy Manager
创建安装目录
```
sudo -i

mkdir -p /root/data/docker_data/npm

cd /root/data/docker_data/npm
```
这边我们直接用 docker 的方式安装。
```
vim docker-compose.yml
```
英文输入法下，按 i	
```
version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'  # 保持默认即可，不建议修改左侧的80
      - '81:81'  # 冒号左边可以改成自己服务器未被占用的端口
      - '443:443' # 保持默认即可，不建议修改左侧的443
    volumes:
      - ./data:/data # 冒号左边可以改路径，现在是表示把数据存放在在当前文件夹下的 data 文件夹中
      - ./letsencrypt:/etc/letsencrypt  # 冒号左边可以改路径，现在是表示把数据存放在在当前文件夹下的 letsencrypt 文件夹中
```
按一下 esc，然后 :wq 保存退出

查看端口是否被占用（以 81 为例），输入：
```
lsof -i:81  #查看 81 端口是否被占用，如果被占用，重新自定义一个端口
```
如果端口没有被占用（被占用了就修改一下端口，比如改成 82，注意 docker 命令行里和防火墙都要改）
#### 运行并访问 Nginx Proxy Manager
```
cd /root/data/docker_data/npm   # 来到 dockercompose 文件所在的文件夹下

docker-compose up -d 
```
理论上我们就可以输入 http://ip:81 访问了。
默认登陆名和密码：
```
Email:    admin@example.com
Password: changeme
```
#### 更新 Nginx Proxy Manager
```
cd /root/data/docker_data/npm

docker-compose down 

cp -r /root/data/docker_data/npm /root/data/docker_data/npm.archive  # 万事先备份，以防万一

docker-compose pull

docker-compose up -d    # 请不要使用 docker-compose stop 来停止容器，因为这么做需要额外的时间等待容器停止；docker-compose up -d 直接升级容器时会自动停止并立刻重建新的容器，完全没有必要浪费那些时间。

docker image prune  # prune 命令用来删除不再使用的 docker 对象。删除所有未被 tag 标记和未被容器使用的镜像
```
#### 卸载 Nginx Proxy Manager
```
cd /root/data/docker_data/npm

docker-compose down 

rm -rf /root/data/docker_data/npm  # 完全删除映射到本地的数据
```
----
来源：https://iwanlab.com/nginx-proxy-manager/
