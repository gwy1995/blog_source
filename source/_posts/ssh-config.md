---
title: 服务器上用ssh连接多个git账号
tags: 实用技巧
date: 2018-10-22 17:10:56
---


有时候需要git连接同一网站多个账号的项目。这时候需要配置config。
首先,生成多个公钥和私钥。
```
~/.ssh/
├── authorized_keys
├── config
├── id_rsa_1
├── id_rsa_2.pub
├── id_rsa_2
├── id_rsa_2.pub
└── known_hosts
```
<!-- more -->
然后分别将`id_rsa_1`分配到`user_1`,`id_rsa_2`分配到`user_2`
然后关键在于配置`~/.ssh/config`
```
# github账号user_1
Host host1
	HostName github.com
	User user_1
	IdentityFile ~/.ssh/id_rsa_1

# github账号user_2
Host host2
	HostName github.com
	User user_2
	IdentityFile ~/.ssh/id_rsa_2

# gitlab账号user_1
Host host3
	HostName gitlab.com
	User user_1
	IdentityFile ~/.ssh/id_rsa_1

# gitlab账号user_2
Host host4
	HostName gitlab.com
	User user_2
	IdentityFile ~/.ssh/id_rsa_2
```

这样配置就完成了，可以通过`ssh -T`测试
```
ssh -T git@host1
ssh -T git@host2
ssh -T git@host3
ssh -T git@host4
```
`git clone`或者`git remote add`时候需要修改对应的地址，否则会失败。
`git clone git@host1:user_1/project1.git`