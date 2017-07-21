# 设置aliyun的源

### 禁用

```sh
$ cat <<EOF > /etc/yum.repos.d/kubernetes.repo
> [kubernetes]
> name=Kubernetes
> baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
> enabled=1
> gpgcheck=0
> EOF
```

### 禁用SELINUX：

```sh
$ setenforce 0					# 不重启Linux服务器关闭SeLinux
$ vim /etc/selinux/config		# 打开SeLinux配置文件

SELINUX=disabled				# 修改SELINUX的值
```

### 安装kubelet和docker
```sh
$ yum install -y docker kubelet kubeadm kubectl kubernetes-cni
$ systemctl enable docker
$ systemctl start docker
$ systemctl enable kubelet

===================================================================================================================
 Package                      架构           版本                                         源                  大小
===================================================================================================================
正在安装:
 docker                       x86_64         2:1.12.6-32.git88a4867.el7.centos            extras              14 M
 kubeadm                      x86_64         1.7.1-0                                      kubernetes         8.6 M
 kubectl                      x86_64         1.7.1-0                                      kubernetes         8.9 M
 kubelet                      x86_64         1.7.1-0                                      kubernetes          16 M
 kubernetes-cni               x86_64         0.5.1-0                                      kubernetes         7.4 M
为依赖而安装:
 container-selinux            noarch         2:2.19-2.1.el7                               extras              28 k
 docker-client                x86_64         2:1.12.6-32.git88a4867.el7.centos            extras             3.2 M
 docker-common                x86_64         2:1.12.6-32.git88a4867.el7.centos            extras              77 k
 oci-register-machine         x86_64         1:0-3.11.gitdd0daef.el7                      extras             1.0 M
 oci-systemd-hook             x86_64         1:0.1.7-4.gite533efa.el7                     extras              30 k
 skopeo-containers            x86_64         1:0.1.20-2.el7                               extras             7.8 k
 socat                        x86_64         1.7.2.2-5.el7                                base               255 k
```

### 配置kubelet的源为aliyun(此aliyun源为网友从google的源搬过来的)
```sh
$ cat > /etc/systemd/system/kubelet.service.d/20-pod-infra-image.conf <<EOF
> [Service]
> Environment="KUBELET_EXTRA_ARGS=--pod-infra-container-image=registry.cn-hangzhou.aliyuncs.com/centos-bz/pause-amd64:3.0"
> EOF

$ systemctl daemon-reload
$ systemctl restart kubelet
$ reboot
```

# 配置内核
### 所有机器运行  (???不明白什么意思 并且没有如下配置文件 可能没有安装iptables 如果没装,是否需要安装呢?)
**用以上3个选项阻止桥接流量获得通过主机iptables规则，Netfilter是默认情况下启用了桥梁，如果不阻止会导致严重的混乱**
[问题说明](https://bugzilla.redhat.com/show_bug.cgi?id=512206)

net.bridge.bridge-nf-call-ip6tables = 0  
net.bridge.bridge-nf-call-iptables = 0  
net.bridge.bridge-nf-call-arptables = 0 

```sh
$ cat <<EOF > /etc/sysctl.conf
> net.bridge.bridge-nf-call-ip6tables = 1
> net.bridge.bridge-nf-call-iptables = 1
> EOF

$ sysctl -p		# -p：从配置文件"/etc/sysctl.conf"加载内核参数设置；
```


