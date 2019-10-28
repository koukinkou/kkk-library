##jsipster 准备环境
- sdkman 安装(卸载)

```sh
sudo apt install zip
apt install unzip

curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"

#卸载
tar zcvf ~/sdkman-backup_$(date +%F-%kh%M).tar.gz -C ~/ .sdkman
rm -rf ~/.sdkman
#
sdk list java
sdk install java 12.0.1-open
sdk list maven
sdk install maven 3.6.1
```

- npm 安装以及权限和源的问题

```sh
sudo apt install npm
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
vim ~/.zshrc
(export PATH=~/.npm-global/bin:$PATH)
source ~/.zshrc
npm install npm -g

npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global

#清除
npm config delete registry
npm config delete disturl
或者 
npm config edit 
找到淘宝那两行,删除
#
```

- npm 安装 jhipster

```sh
npm install -g generator-jhipster
npm install -g generator-jhipster-vuejs
```

## 启动jhipster的注册中心容器

```sh
docker container run --name registry-app -e JHIPSTER.SECURITY.AUTHENTICATION.JWT.SECRET=dkk20dldkf0209342334 -d -p 8761:8761 jhipster/jhipster-registry:v4.0.0
```

## 启动generator-jhipster容器

```sh
docker container run --name jhipster -v /opt/jhipster:/home/jhipster/app -v /opt/.m2:/home/jhipster/.m2 -p 8080:8080 -p 9000:9000 -p 3001:3001 -d -t jhipster/jhipster:v6.4.1

docker exec -it jhipster bash

mvn clean -Pdev package -DskipTests
nohup ./target/uaa-0.0.1-SNAPSHOT.war &
tail -f nohup.out
```
- 如果没有指定容器用户，容器内默认root用户挂载宿主机的jhipster，并改变盖目录用户组和用户，需改变回当前用户组和用户
```sh
sudo chown -R gjg.gjg jhipster
```

