##docker rabbitmq

- 由于账号guest具有所有的操作权限，并且又是默认账号，
- 出于安全因素的考虑，guest用户只能通过localhost登陆使用，并建议修改guest用户的密码以及新建其他账号管理使用rabbitmq。
- 这里我们以创建个admin帐号，密码admin为例，创建一个账号并支持远程ip访问。
- 启动好后 访问宿主机ip:15672 进入web管理端
- 5672是应用访问端口

```sh
docker pull rabbitmq:management
docker run -d --hostname my-rabbit --name rabbit -p 15672:15672 -p 5672:5672 -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin rabbitmq:management
```
