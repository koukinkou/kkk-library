# virtualbox clone

##### 重复使用已安装好系统的vdi

**备注：当路径中有空格时，要对路径加上双引号**

方法1：

因为VirtualBox不允许注册重复的uuid，而每个vdi文件都有一个唯一的uuid。

所以要想拷贝一份vdi文件再次在VBOX中注册，简单的复制是不行的。

此时我们需要用到命令VBoxManage clonehd，这个命令在克隆vdi文件时会给新文件设置一个uuid【注：要运行这个命令，先打开命令提示行，并进入到virtual box的安装目录】。

- VBoxManage clonehd

示例如下：
D:\Program Files\Oracle\VirtualBox>VBoxManage clonehd "E:\VirtualBox\Ubuntu 12.04\Ubuntu 12.04.vdi" "E:\VirtualBox\Ubuntu 12.04\Ubuntu_12.04.vdi"

结果如下：
0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
Clone hard disk created in format 'VDI'. UUID: cf70d484-a3f0-4a87-953b-d1c8ec602c59

方法2：VBoxManage internalcommands sethduuid

手动复制vdi后，就会有两个相同uuid的vdi文件，可以使用命令修改其中一个vdi文件的uuid。

- VBoxManage internalcommands sethduuid

示例如下：
D:\Program Files\Oracle\VirtualBox>VBoxManage internalcommands sethduuid "E:\VirtualBox\Ubuntu 12.04.vdi"  
 
结果如下：
UUID changed to: 3b5f507c-dda7-409c-a2ef-ee075435558d

# 更新默认源
- 首先备份系统自带yum源配置文件/etc/yum.repos.d/CentOS-Base.repo
- 进入yum源配置文件所在的文件夹
- 下载aliyun的yum源配置文件到上面那个文件夹内
- 运行yum makecache生成缓存
- 这时候再更新系统就会看到以下mirrors.aliyun.com信息

```sh
$ mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
$ cd /etc/yum.repos.d/
$ wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
$ yum makecache
$ yum -y update

已加载插件：fastestmirror, refresh-packagekit, security
设置更新进程
Loading mirror speeds from cached hostfile
* base: mirrors.aliyun.com
* extras: mirrors.aliyun.com
* updates: mirrors.aliyun.com
```

# 为安装vm 增强功能的一些准备
安装增强功能前  需要重启  因为内核已更新

```sh
 $ yum erase libreoffice\*
 $ yum update
 $ yum install kernel-devel gcc 			
 $ ln -s /usr/src/kernels/3.10.0-327.22.2.el7.x86_64 /usr/src/linux	# 注意：3.10.0-327.22.2.el7.x86_64是内核的版本号，需要根据自己情况输入。
```


# 防火墙
centos 7.x 自带firewall防火墙

```sh
$ systemctl stop firewalld.service 					 
$ systemctl disable firewalld.service 				 
$ firewall-cmd [--permanent] --add-port=27017/tcp 	#（如果需要永久开放加上--permanent参数）
```



