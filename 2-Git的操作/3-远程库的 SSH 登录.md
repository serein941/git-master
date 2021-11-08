# 远程库的 SSH 登录

在 Windows 10 系统中，`凭据管理器`为我们记录了 GitHub 的 Windows 凭据，再次从终端经过 GitHub 写数据时，可自动保持登录状态。但其他 OS 不一定有这样的功能，因此不便于频繁地提交版本。

为此，可以使用 SSH 登录的方式访问远程库。

不同于 SSH 登录，使用 HTTPS 的方式可以在多个 GitHub 帐号间管理仓库。

![img](https://cdn.nlark.com/yuque/0/2021/png/932482/1635768846750-a89b54dd-b346-44a3-90c5-ff6bdc63e66f.png)

#   在本地 `Home` 目录生成 GitHub 公/私钥

使用命令`ssh-keygen -t <KeyType> -C <Annotation|AccountEmailAddress>`在本地 Home 目录生成 GitHub 公/私钥。

使用生成过程提供的默认参数即可。

```plain
33891@LeapingQuantum MINGW64 ~
$ ssh-keygen -t rsa -C 3389167252@qq.com
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/33891/.ssh/id_rsa):
Created directory '/c/Users/33891/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/33891/.ssh/id_rsa
Your public key has been saved in /c/Users/33891/.ssh/id_rsa.pub
The key fingerprint is:
<...>
The key's randomart image is:
<...>
```

#   查看 `.ssh` 目录下的文件

`.ssh`目录下生成了`id_rsa`和`id_rsa.pub`共 2 个文件。

使用`cat`命令查看并复制`id_rsa.pub`（公钥）的内容：

```plain
33891@LeapingQuantum MINGW64 ~
$ cd ~/.ssh/

33891@LeapingQuantum MINGW64 ~/.ssh
$ ll
total 5
-rw-r--r-- 1 33891 197609 2602 11月  1 20:38 id_rsa
-rw-r--r-- 1 33891 197609  571 11月  1 20:38 id_rsa.pub

33891@LeapingQuantum MINGW64 ~/.ssh
$ cat id_rsa.pub
<...>
```

#   在 GitHub 配置该 SSH 公钥

登录 GitHub 账户后，进入`Settings`中的`SSH and GPG keys`项执行配置：

![img](https://cdn.nlark.com/yuque/0/2021/png/932482/1635771609450-948aa2fb-1ff9-41e9-8df7-51fcdfa9527b.png)

![img](https://cdn.nlark.com/yuque/0/2021/png/932482/1635771748463-0de268e5-954f-4efc-a0a5-b793098b4383.png)　![img](https://cdn.nlark.com/yuque/0/2021/png/932482/1635771848484-557078b2-6ae6-49c8-aa6e-328ca0dbb747.png)

#   更改在本地的远程库地址

使用`git remote set-url <Alias4RemoteRepositoryAddress> <RemoteRepositoryAddress>`命令，将远程库地址由 HTTPS 方式切换为 SSH 方式：

```plain
33891@LeapingQuantum MINGW64 /d/GitWorkSpace/MyFirstProject_GitHub (master)
$ git remote -v
origin  https://github.com/TullyMonster/MyFirstProject_GitHub.git (fetch)
origin  https://github.com/TullyMonster/MyFirstProject_GitHub.git (push)

33891@LeapingQuantum MINGW64 /d/GitWorkSpace/MyFirstProject_GitHub (master)
$ git remote set-url origin git@github.com:TullyMonster/MyFirstProject_GitHub.git

33891@LeapingQuantum MINGW64 /d/GitWorkSpace/MyFirstProject_GitHub (master)
$ git remote -v
origin  git@github.com:TullyMonster/MyFirstProject_GitHub.git (fetch)
origin  git@github.com:TullyMonster/MyFirstProject_GitHub.git (push)
```

随后，即可使用 SSH 的方式免密读写 GitHub 的内容。