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

##ubuntu 安装 zip

```sh
apt-get install zip

zip -r -q xxx.zip /opt/xxx
```
 
 
#ubuntu 设置时区

```sh
tzselect
Asia China beijing Yes
sudo cp /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
```

##ubuntu 同步时间

```sh
sudo apt install ntpdate
sudo ntpdate cn.pool.ntp.org
```

##使用代理会出现语言编码的问题
- root 用户执行后重启
```sh
echo 'LANGUAGE="en_US.UTF-8"' >> /etc/default/locale
echo 'LC_ALL="en_US.UTF-8"' >> /etc/default/locale
```

##设置代理
- 
```sh
alias setproxy='export http_proxy=socks4a://192.168.10.137:3213 && export https_proxy=$http_proxy'
alias unsetproxy='unset http_proxy && unset https_proxy'
```