### kali SSH的连接

> ssh默认不可以root连接

#### 1.配置SSH参数

修改sshd_config文件，命令为

```
leafpad /etc/ssh/sshd_config
```

将#PasswordAuthentication no的注释去掉，并且将NO修改为YES            //kali中默认是yes
将PermitRootLogin without-password修改为
PermitRootLogin yes
Kali 2.0使用SSH进行远程登录

#### 2.启动SSH服务

```
/etc/init.d/ssh start
或 service ssh start
```

`/etc/init.d/ssh status 或 service ssh status `   查看ssh服务是否正常运行



