
# Movie-Recommender-System
电影推荐系统
#  Movie-Recommender-System

 **电影推荐系统**


# 一：系统环境搭建

注：以下操作基于root用户，非root用户部分操作前需加sudo指令

## 1.1 工具环境：Cent OS 6.9 
   `centos69-64\CentOS-6.9-x86_64-bin-DVD1.iso`

`yum -y install gcc`安装gcc

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


**`bin/mongo`** //访问 MongoDB 服务器

```
MongoDB shell version v3.4.3
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.3
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	http://docs.mongodb.org/
Questions? Try the support group
	http://groups.google.com/group/mongodb-user
Server has startup warnings: 
2021-01-04T23:16:48.545+0800 I STORAGE  [initandlisten] 
2021-01-04T23:16:48.545+0800 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2021-01-04T23:16:48.545+0800 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2021-01-04T23:16:48.946+0800 I CONTROL  [initandlisten] 
2021-01-04T23:16:48.946+0800 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2021-01-04T23:16:48.946+0800 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2021-01-04T23:16:48.946+0800 I CONTROL  [initandlisten] ** WARNING: You are running this process as the root user, which is not recommended.
2021-01-04T23:16:48.946+0800 I CONTROL  [initandlisten] 
2021-01-04T23:16:48.947+0800 I CONTROL  [initandlisten] 
2021-01-04T23:16:48.947+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2021-01-04T23:16:48.947+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2021-01-04T23:16:48.947+0800 I CONTROL  [initandlisten] 
2021-01-04T23:16:48.947+0800 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2021-01-04T23:16:48.947+0800 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2021-01-04T23:16:48.947+0800 I CONTROL  [initandlisten] 
> 
```

**`cd /usr/local/mongodb`**

**`mongodb-linux-x86_64-rhel62-3.4.3/bin/mongod -shutdown -config  ./data/mongodb.conf`** //停止 MongoDB 服务器
```
killing process with pid: 27335
```

`ps -ef | grep mongo`显示mongo进程，证明已结束进程
```
root      27453   3723  0 23:58 pts/0    00:00:00 grep mongo
```

`wget http://download.redis.io/releases/redis-4.0.2.tar.gz` //通过 WGET 下载 REDIS 的源码

`tar -xf redis-4.0.2.tar.gz -C ~/` //解压

`cd redis-4.0.2/`进入Redis源码

`make MALLOC=libc`// 编译源代码

` make install`// 编译安装

`ll /usr/local/bin/redis-*`查看默认安装位置

```
cd ../
mkdir redis
cd ./redis-4.0.2
```

`vim ./redis.conf`// 修改配置文件内容

`set nu //显示行号`
`?daemonize //查找daemonize`

```
**daemonize yes** #136 行 #是否以后台 daemon 方式运行，默认不是后台运行
**bind 0.0.0.0** #69 行 #绑定主机 IP，默认值为 127.0.0.1，我们是跨机器运行，所以需要更改，0.0.0.0即所有网卡都能提供redis服务
logfile "/root/桌面/redis/redis.log" #171 行 #定义 log 文件位置，模式 log
信息定向到 stdout，输出到/dev/null（可选）
dir "/root/桌面/redis/" #本地数据库存放路径，默认为./，编译安装默认存在在/usr/local/bin 下（可选）
```


