# github-quickstart

[TOC]

## 参考链接

- `https://blog.csdn.net/qq_36667170/article/details/79085301`
- `https://blog.csdn.net/qq_36667170/article/details/79094257`

## 设置用户

- 给你的电脑设置一个用户，等你上传的时候，告诉远程仓库是谁上传的。

- 用户名，邮箱

  ```
  git config --global user.name "Dubhe"
  git config --global user.email "xxyy@163.com"
  
  # 查看配置信息
  git config --global --list
  ```

****

## SSH Keys 配置

- 检查一下电脑上有无.ssh文件夹

```
>>> ~/.ssh

# 有
bash: /c/Users/…/.ssh: Is a directory
# 没有
bash: /c/Users/…/.ssh: No such file or directory
```

- 再检查一下`.ssh`文件夹下有无`id_rsa`和`id_rsa.pub`两个秘钥文件
- 如果没有

```
>>> ssh-keygen -t rsa -C "邮箱"

Generating public/private rsa key pair.
Enter file in which to save the key (....../.ssh/id_rsa):

# 这是让你输入一个文件名，用于保存刚才生成的 SSH key 代码。为了避免麻烦，不用输入，直接回车，那么就会默认生成id_rsa和id_rsa.pub两个秘钥文件。

Enter passphrase (empty for no passphrase):
Enter same passphrase again:

# 让你输入密码，如果你设置了密码，那在你使用ssh传输文件的时候，你就要输入这个密码。为了避免麻烦，建议不用设置，直接回车。再输入一次密码，就跟我们注册账号时候设置密码需要设置两次一样。上一步没设置密码，这里直接回车就可以了。到这里你的秘钥就设置好了，你会收到这段代码提示：

Your identification has been saved in /c/Users/86133/.ssh/id_rsa.
Your public key has been saved in /c/Users/86133/.ssh/id_rsa.pub.
```

- 看到这个说明成功了

![](C:\Users\86133\Desktop\github-quickstart\img\1.png)

- 添加 SSH Key 到 github
  - 右上角头像 — Settings — SSH and GPG keys — new SSH key 
  - title 随便，key 找到上面 `.ssh` 文件夹下的 `id_rsa.pub`，复制粘贴
  - 会收到建立成功的邮件
- 测试  SSH key

```
>>> ssh -T git@github.com

The authenticity of host 'github.com (10.16.81.215)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? 

>>> yes

Warning: Permanently added 'github.com,10.16.81.215' (RSA) to the list of known hosts.（没设置密码就会有这个警告）

Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
```

****

## 仓库

### 初始化本地仓库

- 可以是空的，你建好了之后再写代码。里边也可以有东西，直接建就好。
- 在该文件夹下执行`git init`

- 执行之后该文件夹下会出现`.git`的隐藏文件

### 新建远程仓库

- 右上角 + — new repository
- 初始化远程仓库的时候 `Initialize this repository with a README` 不要选（麻烦），到时候自己加

### 建立连接

- 仓库的主人才能使用SSH连接，如果你不是仓库主人，只是某个项目的成员，那你只能使用HTTPS连接。

- 在本地仓库打开gitbash `git remote add + 自己取个名字 + 连接地址`

```
>>> git remote add origin git@github.com:xxxxxx
```

- 查看连接情况

```
>>> git reomte -v
```

- 删除连接

```
>>> git remote remove 名字
```

### 文件上传

- `git add` 将**修改的文件**添加暂存区，也就是将要提交的文件的信息添加到索引库中。

```
# 提交所有变化
git add -A
```

-  `git commit` 命令将索引的当前内容与描述更改的用户和日志消息一起存储在新的提交中。可以简单的理解为给你刚才add的东西加一个备注，你上传到远程仓库之后，修改的文件后边会显示这个备注。

```
>>> git commit -m "添加了readme"
```

![](C:\Users\86133\Desktop\github-quickstart\img\4.png)

- `git push` 把文件推到远程仓库

```
git push -u 仓库名称 分支

>>> git push -u origin master
```

![](C:\Users\86133\Desktop\github-quickstart\img\2.png)

- 我们第一次推送master分支时，加上 `–u`参数才会把本地的master分支和远程的master分支关联起来，就是告诉远程仓库的master分支，我的本地仓库和是对着它的。
- 只有第一次推的时候需要加上`-u`，以后的推送只输入 `git push 名称 分支`
- `git push 名称 分支 -f` 强制推送，如果你某次推送失败，git bash报错，你懒得处理错误，你就可以用这个。但是有风险，因为报错90%是因为你本地仓库和远程仓库数据发生冲突，使用这个会直接用本地数据覆盖掉远程数据，可能损失数据。

- `git log` 提交记录
- `git commit --amend -m "修改的内容"` 修改commit的内容

```
git add -A
git commit -m "修改的内容"
git push 名称 分支
```

github不管你做错了啥，他都会给你保存的，就是即使你改了，你的错误记录永远存在！但是使用git commit --amend，你可以悄悄地修改你的错误commit注释。

github你可以理解为差额备份，就是你本地提交上去之后，它备份起来。你本地修改了，它会对你修改的部分继续备份。**也就是说在你这次修改之前，本地仓库应该和远程仓库一模一样。**

但是我刚才强行修改了**上次**的commit注释信息。**现在**本地仓库里的信息和远程仓库的不一样，它就会报错说数据冲突。

可以通过强制推送来解决（**慎用，前提是我们知道本地数据和远程数据的差额在哪！！！**）

### 文件下载

- 上边push报错，我自己知道数据差在哪里，所以使用了强制推送。但是在团队合作中，push报错，那铁定是你队友修改了远程仓库，如果你再强制上传，那你就是毁了你队友的代码。所以如何保证在你修改之前，自己的文件跟远程仓库一致。
- `git pull 仓库名称`

![](C:\Users\86133\Desktop\github-quickstart\img\3.png)

****

## README.md

### 添加图片

- 在README.md文件中添加图片，需要使用项目的绝对路径

```
https://raw.githubusercontent.com/用户名/项目名称/master/图片文件夹/xxx.png
```

比如

```
https://raw.githubusercontent.com/tianxinliao/chunks_extract/master/img/result_1.png
```



