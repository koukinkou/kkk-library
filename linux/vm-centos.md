# linux 命令大全
[linux 命令大全](http://man.linuxde.net/, "linux 命令大全")

# Here Document
### 什么是Here Document
Here Document 是在Linux Shell 中的一种特殊的重定向方式，它的基本的形式如下

```sh
cmd << delimiter
  Here Document Content
delimiter
```

它的作用就是将两个 delimiter 之间的内容(Here Document Content 部分) 传递给cmd 作为输入参数。

比如在终端中输入cat <<EOF > file ，系统会提示继续进行输入，输入多行信息再输入EOF，中间输入的信息将会显示在文件里。如下：

```sh
$ cat <<EOF > /etc/yum.repos.d/kubernetes.repo
> [kubernetes]
> name=Kubernetes
> baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
> enabled=1
> gpgcheck=0
> EOF
```


