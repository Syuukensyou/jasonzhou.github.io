---

layout: article
keys: 20190413
tags: Mac Github SSH
title: Mac generate Github SSH key
---

# 1、生成本地SSH

```shell
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~
╰─➤  ssh-keygen -t rsa -b 4096 -C "1459606798@qq.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/jasonzhou/.ssh/id_rsa):
/Users/jasonzhou/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

建议不要修改名字，后面会用到相应的名字的。

# 2、观望官网

```shell
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~
╰─➤  eval "$(ssh-agent -s)"
Agent pid 1405
```

# 3、Mac用户特有的一步--创建config文件

```shell
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~/.ssh
╰─➤  vim config                                                   
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~/.ssh
╰─➤  ll
total 24
-rw-------  1 jasonzhou  staff   3.3K Apr 13 22:13 id_rsa
-rw-r--r--  1 jasonzhou  staff   743B Apr 13 22:13 id_rsa.pub
-rw-r--r--  1 jasonzhou  staff    75B Apr 13 22:23 config
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~/.ssh
╰─➤  cat config
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa
```

`vim`创建新的文件，在`config`文件中填入下列内容：（这一步会用到相应的名字id_rsa）

```shell
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa
```

添加：

```shell
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~/.ssh
╰─➤  ssh-add -K ~/.ssh/id_rsa
Identity added: /Users/jasonzhou/.ssh/id_rsa (1459606798@qq.com)
```

# 4、拷贝

```shell
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~
╰─➤  pbcopy < ~/.ssh/id_rsa.pub
```

# 5、添加SSH到github

这个参考官网，

```settings —> SSH and GPG keys —> New SSH key —> 填写title（随意）和复制到key —> Add SSH key```

# 6、测试

```shell
╭─jasonzhou@SyuuKensyoudeMacBook-Pro.local ~
╰─➤  ssh -T git@github.com
The authenticity of host 'github.com (52.74.223.119)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com,52.74.223.119' (RSA) to the list of known hosts.
Hi Syuukensyou! You've successfully authenticated, but GitHub does not provide shell access.
```

皆大欢喜！！！

# 7、push遇到问题

问题：
```shell
➜  syuukensyou.github.io git:(master) ✗ git push origin master
Username for 'https://github.com': syuukensyou
Password for 'https://syuukensyou@github.com':
remote: Invalid username or password.
fatal: Authentication failed for 'https://github.com/Syuukensyou/syuukensyou.github.io.git/'
```

解决方法：
[使用二次认证的可以尝试这种提交方法](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line)

参考：

[为官网打call](https://help.github.com/en/articles/connecting-to-github-with-ssh)

