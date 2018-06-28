##docker 准备
- ubuntu 设置 root 密码

```
sudo passwd root
```

- ubuntu server 1804 固定ip

```sh
vim /etc/netplan/50-cloud-init.yaml

network:
    ethernets:
        enp0s3:
            addresses: [192.168.0.62/24]
            gateway4: 192.168.0.1
            nameservers:
                addresses: [114.114.114.114,8.8.8.8]
            optional: true
    version: 2
    
netplan apply

```



##docker 记录 参数 命令
- 先记录简单的 以后熟悉后增加更丰富的内容
- 感觉不用chmod也行，用这个参数试试 --privileged=true (or -privileged) Docker将拥有访问主机所有设备的权限
- --privileged=true (or -privileged) Docker将拥有访问主机所有设备的权限
- -p 设置端口
- -d 守护进程
- -v 挂载目录
- docker exec -it [containId] /bin/bash 进入容器 如果不是自己搭建的镜像，比如从官方pull的镜像 基本是简化版本 有些命令需要额外安装
- docker images 查看所有镜像
- docker ps -a 查看所有容器
- docker rmi [imageId] 删除镜像 没容器时删
- docker start [containId] 启动容器
- docker stop [containId] 停容器 默认10秒 可以使用 --time=20 设置停容器最大时间
- docker rm [containId] 删容器

- apt-get install procps 使用ps -ef|grep tomcat 需要单独安装

##ubuntu 安装 oracle－jdk 
```sh
add-apt-repository ppa:webupd8team/java
apt-get update
apt-get install oracle-java8-installer
java -version
apt-get install oracle-java8-set-default
cat /etc/profile.d/jdk.sh
source /etc/profile
echo $JAVA_HOME
```

##docker jenkins 
```sh
mkdir jenkins
chmod 777 jenkins
docker pull jenkins/jenkins:lts

```


##docker tomcat
- 挂载的意义就是目录同步，容器外什么样，容器内对应的目录也就是什么样。
- 挂载日志目录后，容器内的日志可以在重启外查看，配合日志监控等方式更佳
- 挂载应用目录后，可以拷贝war到应用目录下，重启容器后生效，也可以配置动态加载等

```sh
mkdir -p tomcat/webapps tomcat/logs
docker pull tomcat:8.5.31
docker run --name docker0tomcat --privileged=true -p 8080:8080 -v $PWD/tomcat/logs:/usr/local/tomcat/logs -v $PWD/tomcat/webapps:/usr/local/tomcat/webapps -d tomcat:8.5.31
```
