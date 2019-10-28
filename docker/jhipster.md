## 准备环境 安装sdkman

```sh
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk version
sdk list java
sdk install java xxx:
sdk install maven 3.6.2
```

```sh
docker pull jhipster/jhipster:v6.4.1
mkdir ~/jhipster
docker container run --name jhipster -v ~/jhipster:/home/jhipster/app -v ~/.m2:/home/jhipster/.m2 -p 8080:8080 -p 9000:9000 -p 3001:3001 -d -t jhipster/jhipster:v6.4.1

docker exec -it jhipster bash
```
- 如果没有指定容器用户，容器内默认root用户挂载宿主机的jhipster，并改变盖目录用户组和用户，需改变回当前用户组和用户
```sh
sudo chown -R gjg.gjg jhipster
```


- old
```sh
docker container run --name registry-app -e JHIPSTER.SECURITY.AUTHENTICATION.JWT.SECRET=dkk20dldkf0209342334 -d -p 8761:8761 jhipster/jhipster-registry:v4.0.0

docker container run --name jhipster -v /opt/jhipster:/home/jhipster/app -v /opt/.m2:/home/jhipster/.m2 -p 8080:8080 -p 9000:9000 -p 3001:3001 -d -t jhipster/jhipster:v5.1.0

mvn clean -Pdev package -DskipTests
nohup ./target/uaa-0.0.1-SNAPSHOT.war &
tail -f nohup.out
```