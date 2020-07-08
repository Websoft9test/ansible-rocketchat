#  Rocket.Chat Notes
 
文档：[安装](https://docs.rocket.chat/installation/manual-installation/ubuntu/) | [配置](https://docs.rocket.chat/installation/manual-installation/ubuntu/)  
平台： Debian家族 | RHEL家族 | Windows | macOS等数十种（[详情](https://github.com/RocketChat/Rocket.Chat#deployment)）

责任人：zxc

## 概要

Rocket.Chat 是一个开源在线聊天软件。

## 环境要求

* 程序语言：JavaScript/Node.js, Meteor框架
* 应用服务器：Nginx
* 数据库：MongoDB
* 依赖组件： 
* 其他：

## 安装说明

官方提供手动下载安装和[Ansible自动化安装]()两种选项。经过评估，我们采用手工安装方式

经过研究，CentOS和Ubuntu安装没有差异，故下面仅列出安装的步骤概要以及特别需要注意的地方：



本项目通过官网下载tar包解压安装。
Manual install:
  Rocket.Chat 3.0.0
  OS: Ubuntu 18.04 LTS, Ubuntu 19.04 and Ubuntu 20.04(Latest)
  Mongodb 4.0.9
  NodeJS 12.14.0

下面基于不同的安装平台，分别进行安装说明。


## 路径

* 程序路径： /opt/Rocket.Chat
* 日志路径：  
* 配置文件路径：
* 其他...

## 配置

无

## 账号密码


### 数据库密码

如果有数据库  
无

* 数据库安装方式：
* 账号密码：

### 后台账号

如果有后台账号
无

* 登录地址 
* 账号密码   
* 密码修改方案：最好是有命令行修改密码的方案

## 服务

本项目安装后自动生成：

备注：本项目安装后无服务,通过拷贝官网服务文件

服务文件位置： /lib/systemd/system/rocketchat.service
[Unit]
Description=The Rocket.Chat server
After=network.target remote-fs.target nss-lookup.target nginx.target mongod.target
[Service]
ExecStart=/usr/local/bin/node /opt/Rocket.Chat/main.js
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=rocketchat
User=rocketchat
Environment=MONGO_URL=mongodb://localhost:27017/rocketchat?replicaSet=rs01 MONGO_OPLOG_URL=mongodb://localhost:27017/local?replicaSet=rs01 ROOT_URL=http://localhost:3000/ PORT=3000
[Install]
WantedBy=multi-user.target

```

```

## 环境变量

  MONGO_URL=mongodb://localhost:27017/rocketchat?replicaSet=rs01
  MONGO_OPLOG_URL=mongodb://localhost:27017/local?replicaSet=rs01
  ROOT_URL=http://your-host-name.com-as-accessed-from-internet:3000
  PORT=3000

## 版本号

通过如下的命令获取主要组件的版本号: 

# Check  Rocket.Chat version


## 常见问题

#### 有没有管理控制台？
有 通过ip:3000访问web gui

#### 本项目需要开启哪些端口？
mongod 27017
node   3000


#### 有没有CLI工具？
rocketchatctl

## 日志
* Update & Upgrade Rocket.Chat:   https://docs.rocket.chat/installation/manual-installation/rocketchatctl#environment
rocketchatctl check-updates
Current update available for RocketChat server: from 3.0.3 to 3.2.1
rocketchatctl update
rocketchatctl upgrade-rockectchatctl



* 2020-06-完成CentOS安装研究
