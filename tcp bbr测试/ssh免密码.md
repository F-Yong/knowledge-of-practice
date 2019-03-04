<font color=red size = 5>客户端Client想要ssh免密码登录服务器端Server</font>


1. Client端生成密匙文件：  
```
ssh-keygen -t rsa
```
相关操作按确定，如果之前存在会问：“是否重新创建”。文件会在~/.ssh/文件夹中


2. 拷贝文件到Server端  
```
scp /home/UserName/.ssh/id_rsa.pub UserName@ServerIp:~/.ssh
```

3. 把公钥授权到Key中
把公匙写在authorized_keys中
```
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

如果Server端没有"~/.ssh/authorized_keys"文件
```
cp  id_rsa.pub  authorized_keys
```