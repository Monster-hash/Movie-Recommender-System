
# Movie-Recommender-System
电影推荐系统
#  Movie-Recommender-System

 **电影推荐系统**


# 一：系统环境搭建

## 1.1 工具环境：Cent OS 6.9 
   `centos69-64\CentOS-6.9-x86_64-bin-DVD1.iso`

## 1.2 MongoDB环境配置：
**`wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel62-3.4.3.tgz`**
//通过wget下载Linux版本的MongoDB （**mongodb-linux-x86_64-rhel62-3.4.3**）

**`tar -xf`** // 将压缩包解压到指定目录

**`mv mongodb-linux-x86_64-rhel62-3.4.3 /usr/local/mongodb`**
 // 将解压后的文件移动到最终的安装目录

**`mkdir /usr/local/mongodb/data/`** //在安装目录下创建 data 文件夹用于存放数据和日志

**`mkdir /usr/local/mongodb/data/db/`**  //在 data 文件夹下创建 db 文件夹，用于存放数据

**`mkdir /usr/local/mongodb/data/logs/`**  //在 data 文件夹下创建 logs 文件夹，用于存放日志

**`touch/usr/local/mongodb/data/logs/mongodb.log`**  //在 logs 文件夹下创建 mongodb.log 文件

**`touch /usr/local/mongodb/data/mongodb.conf`**  //在 data 文件夹下创建 mongodb.conf 配置文件

**`cd /usr/local/mongodb`** //进入mongodb文件

**`vim ./data/mongodb.conf`** // 编辑mongodb.conf文件
```
**#端口号 
port = 27017 
#数据目录 
dbpath = /usr/local/mongodb/data/db 
#日志目录 
logpath = /usr/local/mongodb/data/logs/mongodb.log 
#设置后台运行 
fork = true 
#日志输出方式 
logappend = true 
#开启认证 
#auth = true**
```

**`cd /usr/local/mongodb`** //进入mongodb文件

**`mongodb-linux-x86_64-rhel62-3.4.3/bin/mongod -config ./data/mongodb.conf`** //启动 MongoDB 服务器

```
当出现
about to fork child process, waiting until server is ready for connections.
forked process: 27335
child process started successfully, parent exiting 时,证明成功
```

**`ps -ef | grep mongo`** //显示mongo进程，mongodb安装完成
```
root      27335      1  0 23:16 ?        00:00:03 mongodb-linux-x86_64-rhel62-3.4.3/bin/mongod -config ./data/mongodb.conf
root      27397   3723  0 23:26 pts/0    00:00:00 grep mongo
```
**`cd mongodb-linux-x86_64-rhel62-3.4.3/`** //进入mongodb-linux-x86_64-rhel62-3.4.3
