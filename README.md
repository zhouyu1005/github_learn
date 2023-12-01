## github常用命令
#### 多账户提交（多个ssh账号）
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
3. 将私钥添加到ssh agent后，需要配置config文件（没有自己创建，在.ssh目录下），用于使用提交到不同的github账户中。
**config文件中内容：**
```
Host 账户1.github.com
Hostname github.com
IdentityFile ~/.ssh/账户1秘钥

Host 账户2.github.com
Hostname github.com
identityFile ~/.ssh/账户2秘钥
```

**测试：**
```
$ git remote -v
origin  git@zy.github.com:账户1/github_learn.git (fetch)
origin  git@zy.github.com:账户1/github_learn.git (push)
origin1 git@sm.github.com:账户2/switch.git (fetch)
origin1 git@sm.github.com:账户2/switch.git (push)
$ git push origin1 master
```

可以直接提交到不同账号的仓库中。
**注意:**
1. 如果提示No such file or directory，且修改为全路径无效，或者提示the agent has no identities，则需要检查ssh-agent服务是否启动：
![](attachments/Pasted%20image%2020231201000830.png)
2. 如果提示Could not open a connection to your authentication agent，那么尝试以下命令后再输入以上命令
```shell
ssh-agent bash
ssh-add 私钥文件路径
```

### github基本概念
1. git简介
**Git 仓库分为两种：**
- 本地仓库：开发人员自己电脑上的Git仓库
- 远程仓库：远程服务器上的 Git 仓库
**操作：**
- commit：提交，将本地文件和版本信息保存到本地仓库
- push：推送，将本地仓库文件和版本信息上传到远程仓库
- pull：拉取，将远程仓库文件和版本信息下载到本地仓库
![](attachments/Pasted%20image%2020231201015156.png)
2. 工作区、版本库、暂存区
- 工作区：包含.git文件夹的目录就是工作区，也称为工作目录，主要用于存放开发的代码(包含.git文件夹的文件夹）
- 暂存区：.git文件夹中有很多文件，其中有一个index文件就是暂存区，也可以叫做stage。暂存区是一个临时保存修改文件的地方
- 版本库：前面看到的.git隐藏文件夹就是版本库，版本库中存储了很多配置信息、日志信息和文件版本信息等（.git文件夹）
![](attachments/Pasted%20image%2020231201015332.png)
![](attachments/Pasted%20image%2020231201020822.png)
3. git工作区中文件状态
Git工作区中的文件存在两种状态：
-   untracked 未跟踪（未被纳入版本控制）
-   tracked 已跟踪（被纳入版本控制）
    -   1）Unmodified 未修改状态
    -   2）Modified 已修改状态
    -   3）Staged 已暂存状态
```
###可以使用git status 查看

$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        attachments/Pasted image 20231201014314.png
        attachments/Pasted image 20231201015156.png
        attachments/Pasted image 20231201015332.png

no changes added to commit (use "git add" and/or "git commit -a")

```
4. Git的commit、repository、branch概念
提交commit：一次提交就是一个版本，就有一次记录。每次当写错代码的时候可以回滚到以前的版本。
仓库 repository：每一次的提交的代码都会保存在这。
分支 branch：相当于一个项目中不同的人写不同的部分，每个人就代表着自己的一个分支，最后合成就是完整的。又或者说是把当前工作从开发主线上分离开来。
5. github提交过程
![](attachments/Pasted%20image%2020231201014314.png)
6. 回退到某次提交
6.1 查看某次提交时间对应的ID
```
$ git log
commit 71618a23398900ef81db10b93573053b05f0a52a (HEAD -> master, origin/master)
Author: zhouyu1005 <zhouyu1005@hotmail.com>
Date:   Fri Dec 1 01:15:42 2023 +0800

    osbidian2markdown_image

commit d392f16b36c3ecba2f3fc8c094af83243ca058b1
Author: zhouyu1005 <zhouyu1005@hotmail.com>
Date:   Fri Dec 1 00:37:26 2023 +0800

    显示图片

commit 411ad55412da193beafca34519460598710ddcb4
Author: zhouyu1005 <zhouyu1005@hotmail.com>
Date:   Fri Dec 1 00:16:59 2023 +0800

    提交

commit e61f5dfae38bed6a28e5b20e668c0dc4c52e4a06
Author: zhouyu1005 <zhouyu1005@hotmail.com>
Date:   Thu Nov 30 15:07:09 2023 +0800

    testtest

```
6.2回退
commit-id 那长串数字
```
git reset --hard commit-id
```
7. 分支操作 Branch
```
git branch                  查看所有本地分支
git branch -r                查看所有远程分支
git branch -a               查看所有本地分支和远程分支
git branch <name>    创建分支（只是在本地仓库有这个分支）
git checkout <name>  切换分支
git push <shortName> <name>        把分支推送到远程仓库
git merge <name>        合并分支
```
8. 暂存 git stash

[git stash](git%20stash.md)



