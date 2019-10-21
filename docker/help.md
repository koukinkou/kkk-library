##docker 准备

## docker 安装
- 由于 apt 源使用 HTTPS 以确保软件下载过程中不被篡改。因此，我们首先需要添加使用 HTTPS 传输的软件包以及 CA 证书。

```
$ sudo apt-get update

$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
- 鉴于国内网络问题，强烈建议使用国内源，官方源请在注释中查看。为了确认所下载软件包的合法性，需要添加软件源的 GPG 密钥。

```
$ curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -

# 官方源
# $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
- 然后，我们需要向 source.list 中添加 Docker 软件源

```
$ sudo add-apt-repository \
    "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu \
    $(lsb_release -cs) \
    stable"

# 官方源
# $ sudo add-apt-repository \
#    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
#    $(lsb_release -cs) \
#    stable"
```
- 安装 Docker CE 更新 apt 软件包缓存，并安装 docker-ce：
- 将当前用户加入到docker组
```
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io

$ sudo groupadd docker
$ sudo usermod -aG docker $USER
```
- 启动docker ce

```
$ sudo systemctl enable docker
$ sudo systemctl start docker
```
- 镜像加速 请在 /etc/docker/daemon.json 中写入如下内容（如果文件不存在请新建该文件）之后重新启动服务。

```
{
  "registry-mirrors": [
    "https://registry.docker-cn.com"
  ]
}
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

##docker 记录 参数 命令
- 先记录简单的 以后熟悉后增加更丰富的内容

- --privileged=true (or -privileged) Docker将拥有访问主机所有设备的权限 该参数慎用
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
- --privileged=true参数慎用 还是chmod目录的方式安全些

```sh
mkdir -p tomcat/webapps tomcat/logs
docker pull tomcat:8.5.31
docker run --name docker0tomcat --privileged=true -p 8080:8080 -v $PWD/tomcat/logs:/usr/local/tomcat/logs -v $PWD/tomcat/webapps:/usr/local/tomcat/webapps -d tomcat:8.5.31
```

##docker mysql 
```sh
mkdir /opt/mysql
cd /opt/mysql
mkdir conf data logs
docker pull mysql:5.6

docker run -p 32900:3306 --name uaa-mysql -v $PWD/conf:/etc/mysql/conf.d -v $PWD/logs:/logs -v $PWD/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=1234 -d mysql:5.6
```
