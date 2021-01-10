
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

## 1.3 Redis（单节点）环境配置：

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
daemonize yes #136 行 #是否以后台 daemon 方式运行，默认不是后台运行

bind 0.0.0.0 #69 行 #绑定主机 IP，默认值为 127.0.0.1，我们是跨机器运行，所以需要更改，0.0.0.0即所有网卡都能提供redis服务

logfile "/root/桌面/redis/redis.log" #171 行 #定义 log 文件位置，模式 log

dir "/root/桌面/redis/" #本地数据库存放路径，默认为./，编译安装默认存在在/usr/local/bin 下（可选）
```

`cp ./redis-4.0.2/redis.conf ./redis`copy到redis目录下

```
[root@localhost 桌面]# redis-server ./redis/redis.conf  // 启动 Redis 服务器

[root@localhost 桌面]# ls

redis  redis-4.0.2  tree-1.8.0

[root@localhost 桌面]# cd redis

[root@localhost redis]# ls

redis.conf  redis.log

[root@localhost redis]# cat redis.log

32783:C 05 Jan 15:08:11.961 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
32783:C 05 Jan 15:08:11.961 # Redis version=4.0.2, bits=64, commit=00000000, modified=0, pid=32783, just started
32783:C 05 Jan 15:08:11.961 # Configuration loaded
32784:M 05 Jan 15:08:11.962 * Increased maximum number of open files to 10032 (it was originally set to 1024).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 4.0.2 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 32784
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

32784:M 05 Jan 15:08:11.963 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
32784:M 05 Jan 15:08:11.963 # Server initialized
32784:M 05 Jan 15:08:11.963 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
32784:M 05 Jan 15:08:11.963 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
32784:M 05 Jan 15:08:11.963 * Ready to accept connections
```
           
`[root@localhost redis]# redis-cli` //连接 Redis 服务器
```

127.0.0.1:6379> set a b `//测试
OK
127.0.0.1:6379> get a
"b"
127.0.0.1:6379> exit//退出

```

`[root@localhost redis]# redis-cli shutdown` // 停止 Redis 服务器

## 1.4 ElasticSearch（单节点）环境配置：

`wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.6.2.tar.gz`

```
效果如下：
[root@localhost 桌面]# ls
elasticsearch-5.6.2.tar.gz  redis  redis-4.0.2  tree-1.8.0
[root@localhost 桌面]# tar -xf elasticsearch-5.6.2.tar.gz 
[root@localhost 桌面]# ls
elasticsearch-5.6.2  elasticsearch-5.6.2.tar.gz  redis  redis-4.0.2  tree-1.8.0
[root@localhost 桌面]# mv elasticsearch-5.6.2 elasticsearch
[root@localhost 桌面]# ls
elasticsearch  elasticsearch-5.6.2.tar.gz  redis  redis-4.0.2  tree-1.8.0
[root@localhost 桌面]# cd elasticsearch/
[root@localhost elasticsearch]# ls
benchmarks             core               LICENSE.txt  README.textile
bootstrap.memory_lock  dev-tools          migrate.sh   rest-api-spec
build.gradle           distribution       modules      settings.gradle
buildSrc               docs               NOTICE.txt   test
client                 GRADLE.CHEATSHEET  plugins      TESTING.asciidoc
CONTRIBUTING.md        gradle.properties  qa           Vagrantfile
[root@localhost elasticsearch]# 
```

**`vim /etc/security/limits.conf`** // 修改文件数配置，在文件末尾添加如下配置
```
\* soft nofile 65536 
\* hard nofile 131072 
\* soft nproc 2048 
\* hard nproc 4096 
 // 修改* soft nproc 1024 为 * soft nproc 2048
```

**`vim /etc/security/limits.d/90-nproc.conf`** 
\* soft nproc 2048 #将该条目修改成 2048

**`vim /etc/sysctl.conf`**
// 在文件末尾添加：vm.max_map_count=655360

**`sysctl -p`** //配置linux的参数，配置完成后，执行**sudo sysctl -p**，使配置生效
```
[root@localhost elasticsearch]#  sysctl -p
net.ipv4.ip_forward = 0
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.default.accept_source_route = 0
kernel.sysrq = 0
kernel.core_uses_pid = 1
net.ipv4.tcp_syncookies = 1
kernel.msgmnb = 65536
kernel.msgmax = 65536
kernel.shmmax = 68719476736
kernel.shmall = 4294967296
vm.max_map_count = 655360
```
`tar -xf  `/ 解压 ElasticSearch 到安装目录

`cd elasticsearch/` // 进入 ElasticSearch 安装目录

`mkdir elasticsearch/data/ ` //创建 ElasticSearch 数据文件夹 data 

`mkdir elasticsearch-5.6.2/logs/` // 创建 ElasticSearch 日志文件夹 logs 

新建用户es，因为elasticsearch不能在root用户下进行

```
cluster.name: es-cluster #设置集群的名称
node.name: es-node #修改当前节点的名称
path.data: /home/es/elasticsearch/elasticsearch/data #修改数据路径
path.logs: /home/es/elasticsearch/elasticsearch/logs #修改日志路径
bootstrap.memory_lock: false #设置 ES 节点允许内存交换
bootstrap.system_call_filter: false #禁用系统调用过滤器
network.host: localhost #设置当前主机名称
discovery.zen.ping.unicast.hosts: ["localhost"] #设置集群的主机列表,若此时有多个集群应在此列出，此时仅为单节点故只列本机
```

虚拟机设置开启主机访问

- 在虚拟机网络中设置**虚拟网络编辑器**把主机IP地址加入vment8的NAT设置中

- 此时便可在主机中的**elasticsearch head**中查看集群



