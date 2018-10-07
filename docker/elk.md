##docker elk
- vm.max_map_count至少需要262144，附永久修改vm.max_map_count操作以下命令更改配置：

```sh
vi /etc/sysctl.conf
   #增加以下属性后退出保存：
vm.max_map_count=262144
```

- 防火墙开放端口

```sh
firewall-cmd --zone=public --add-port=5601/tcp --permanent;firewall-cmd --reload;
```

- 如果本机内存不符合安装要求，为了保证ELK能够正常运行，加了-e参数限制使用最小内存及最大内存。

```sh
docker pull sebp/elk:642
docker run -p 4560:4560 -p 5601:5601 -p 9200:9200 -p 5044:5044 -e ES_MIN_MEM=128m -e ES_MAX_MEM=2048m -it -d --name elk sebp/elk:642

docker exec -it elk bash
#模拟一条日志
/opt/logstash/bin/logstash --path.data /tmp/logstash/data -e 'input { stdin { } } output { elasticsearch { hosts => ["localhost"] } }'
#等待 然后输入任意字符 到5601控制台建索引(logstash-*)查看
```



