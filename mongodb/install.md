# 安装环境

centos 7 <br>
mongodb 3.4.6

## 下载 解压缩

```sh
$ cd /usr/local
$ wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-3.4.6.tgz
$ tar -xvf mongodb-linux-x86_64-rhel70-3.4.6.tgz
$ mv mongodb-linux-x86_64-rhel70-3.4.6.tgz mongodb
```

## 配置环境变量 生效
```sh
$ vim /etc/profile
$ source /etc/profile

$ vim /etc/profile
$ source /etc/profile
```
___
export MONGODB_HOME=/usr/local/mongodb
export PATH=$MONGODB_HOME/bin:$PATH
___

## 查看mongodb版本信息
```sh
$ mongod -v

2017-07-27T10:15:04.059+0800 I CONTROL  [initandlisten] MongoDB starting : pid=3557 port=27017 dbpath=/data/db 64-bit host=localhost.localdomain
2017-07-27T10:15:04.059+0800 I CONTROL  [initandlisten] db version v3.4.6
2017-07-27T10:15:04.059+0800 I CONTROL  [initandlisten] git version: c55eb86ef46ee7aede3b1e2a5d184a7df4bfb5b5
2017-07-27T10:15:04.059+0800 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1e-fips 11 Feb 2013
2017-07-27T10:15:04.059+0800 I CONTROL  [initandlisten] allocator: tcmalloc
2017-07-27T10:15:04.059+0800 I CONTROL  [initandlisten] modules: none
2017-07-27T10:15:04.059+0800 I CONTROL  [initandlisten] build environment:
2017-07-27T10:15:04.059+0800 I CONTROL  [initandlisten]     distmod: rhel70
2017-07-27T10:15:04.059+0800 I CONTROL  [initandlisten]     distarch: x86_64
2017-07-27T10:15:04.059+0800 I CONTROL  [initandlisten]     target_arch: x86_64
2017-07-27T10:15:04.059+0800 I CONTROL  [initandlisten] options: { systemLog: { verbosity: 1 } }
2017-07-27T10:15:04.059+0800 D -        [initandlisten] User Assertion: 29:Data directory /data/db not found. src/mongo/db/service_context_d.cpp 98
2017-07-27T10:15:04.059+0800 I STORAGE  [initandlisten] exception in initAndListen: 29 Data directory /data/db not found., terminating
2017-07-27T10:15:04.059+0800 I NETWORK  [initandlisten] shutdown: going to close listening sockets...
2017-07-27T10:15:04.059+0800 I NETWORK  [initandlisten] shutdown: going to flush diaglog...
2017-07-27T10:15:04.059+0800 I CONTROL  [initandlisten] now exiting
2017-07-27T10:15:04.059+0800 I CONTROL  [initandlisten] shutting down with code:100
```

- 恭喜你安装成功了! 下面进行配置, 启动 


# 启动

## 创建数据库目录

### MongoDB需要自建数据库文件夹
```sh
$ mkdir -p /data/mongodb
$ mkdir -p /data/mongodb/log
$ touch /data/mongodb/log/mongodb.log
```

### 添加配置文件
```sh
$ vim /etc/mongodb.conf

mongodb的参数说明：
--dbpath 数据库路径(数据文件)
--logpath 日志文件路径
--master 指定为主机器
--slave 指定为从机器
--source 指定主机器的IP地址
--pologSize 指定日志文件大小不超过64M.因为resync是非常操作量大且耗时，最好通过设置一个足够大的oplogSize来避免resync(默认的 oplog大小是空闲磁盘大小的5%)。
--logappend 日志文件末尾添加
--port 启用端口号
--fork 在后台运行
--only 指定只复制哪一个数据库
--slavedelay 指从复制检测的时间间隔
--auth 是否需要验证权限登录(用户名和密码)

注：mongodb配置文件里面的参数很多，定制特定的需求，请参考官方文档
```
### 配置文件内容:
___
dbpath=/data/mongodb
logpath=/data/mongodb/log/mongodb.log
logappend=true
port=27017
fork=true


**auth = true # 先关闭, 创建好用户在启动
___

### 通过配置文件启动
```sh
$ mongod -f /etc/mongodb.conf

about to fork child process, waiting until server is ready for connections.
forked process: 2814
child process started successfully, parent exiting
```
- 出现successfully表示启动成功了

### 进入 MongoDB后台管理 Shell
```sh
$ cd /usr/local/mongodb/bin
$ ./mongo

> use test
> switched to db test
> db.createUser(
    {
        user: "test",
        pwd: "test",
        roles: [ { role: "readWrite", db: "test" } ]
    }
 )
> exit

$ mongod -f /etc/mongodb.conf  --shutdown

$ vim /etc/mongodb.conf
放开auth=true

$ mongod -f /etc/mongodb.conf
```










