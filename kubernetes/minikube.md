## ubuntu 18.04 安装minikukbe
- 更新包

```sh
sudo apt-get update
sudo apt-get install apt-transport-https
sudo apt-get upgrade
```

- 安装Virtual Box（用来跑minikube）

```sh
sudo apt install virtualbox virtualbox-ext-pack
```

- 下载minikube 验证版本

```sh
wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube-linux-amd64
sudo mv minikube-linux-amd64 /usr/local/bin/minikube

minikube version
```

- 在安装Ubuntu上安装 kubectl 验证版本 启动

```sh
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -

echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt update
sudo apt -y install kubectl

kubectl version -o json

minikube start
```

- 基本操作

```sh
# 检查集群信息
kubectl cluster-info

# 进入minikube所在的虚拟机
minikube ssh

# 停止minikube
minikube stop

# 删除一个本地的k8s集群：
minikube delete
```