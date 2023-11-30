### github常用命令
### 多账户提交（多个ssh账号）
1. 创建ssh公钥和私钥
```shell
ssh-keygen -t rsa -C "这个地方写备注"
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\xxxx/.ssh/id_rsa): test
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in test.
Your public key has been saved in test.pub.
The key fingerprint is:
SHA256:tweBkPJ0X/gCwu9VrtMUupyS8VbxxxxxxxxpqKGKVMg 
The key's randomart image is:
+---[RSA 3072]----+
|    . =.o.. +  ..|
|   E .. o.=o*.= o|
|   .   .+oOoo . .|
|   . .     .     |
+----[SHA256]-----+

    目录: C:\Users\xxxx\.ssh
```
同样的操作在创建另外一个账号，此步骤忽略。

2. 由于使用非默认命名sshkey方式（id_rsa，id_rsa.pub)
需要将私钥加入到ssh agent中
```shell
ssh-add 私钥文件路径
###使用ssh-add -l查询都有哪些私钥添加到ssh agent中
ssh-add -l
```
![](attachments/Pasted%20image%2020231201001139.png)

==注意：==
1. 如果提示No such file or directory，且修改为全路径无效，或者提示the agent has no identities，则需要检查ssh-agent服务是否启动：
![](attachments/Pasted%20image%2020231201000830.png)
2. 如果提示Could not open a connection to your authentication agent，那么尝试以下命令后再输入以上命令
```shell
ssh-agent bash
ssh-add 私钥文件路径
```
