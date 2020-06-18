#  Rocket.Chat Notes

组件名称：Rocket.Chat 
安装文档：https://docs.rocket.chat/installation/manual-installation/ubuntu/
配置文档: 
支持平台： Debian家族 | RHEL家族 | Windows |macOS   

责任人：zxc

## 概要



## 环境要求

* 程序语言： Meteor  由 JavaScript 和 CoffeeSript 编写
* 应用服务器：无
* 数据库： 
* 依赖组件：Mongodb NodeJS 
* 其他：

## 安装说明

本项目通过官网下载tar包解压安装。
Manual install:
  Rocket.Chat 3.0.0
  OS: Ubuntu 18.04 LTS, Ubuntu 19.04 and Ubuntu 20.04(Latest)
  Mongodb 4.0.9
  NodeJS 12.14.0

下面基于不同的安装平台，分别进行安装说明。

### CentOS  

```shell


    





```

### Ubuntu 

```shell
# Install dependencys packages: build-essential mongodb-org nodejs graphicsmagick
  sudo apt-get -y update
  sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
  echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
  sudo apt-get -y update && sudo apt-get install -y curl && curl -sL https://deb.nodesource.com/setup_12.x | sudo bash -
# graphicsmagick: 图像处理系统
  sudo apt-get install -y build-essential mongodb-org nodejs graphicsmagick  
# npm: Node.js package manager
  sudo apt-get install -y npm                # Only for Ubuntu 19.04 install npm
  sudo npm install -g inherits n && sudo n 12.14.0
    
# Install Rocket.Chat
  curl -L https://releases.rocket.chat/latest/download -o /tmp/rocket.chat.tgz
  tar -xzf /tmp/rocket.chat.tgz -C /tmp
  cd /tmp/bundle/programs/server && npm install
  sudo mv /tmp/bundle /opt/Rocket.Chat

# Configure the Rocket.Chat service
  sudo useradd -M rocketchat && sudo usermod -L rocketchat
  sudo chown -R rocketchat:rocketchat /opt/Rocket.Chat
  cat << EOF |sudo tee -a /lib/systemd/system/rocketchat.service
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
EOF

# Set  environmental : /lib/systemd/system/rocketchat.service
  MONGO_URL=mongodb://localhost:27017/rocketchat?replicaSet=rs01
  MONGO_OPLOG_URL=mongodb://localhost:27017/local?replicaSet=rs01
  ROOT_URL=http://your-host-name.com-as-accessed-from-internet:3000
  PORT=3000

# Configure the MongoDB : /etc/mongod.conf
  sudo sed -i "s/^#  engine:/  engine: mmapv1/"  /etc/mongod.conf
  sudo sed -i "s/^#replication:/replication:\n  replSetName: rs01/" /etc/mongod.conf
  sudo systemctl enable mongod && sudo systemctl start mongod
  mongo --eval "printjson(rs.initiate())"

# Start & Enable rocketchat
  sudo systemctl enable rocketchat && sudo systemctl start rocketchat

```
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

无

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