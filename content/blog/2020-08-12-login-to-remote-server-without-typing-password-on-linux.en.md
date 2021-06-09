---
title: Login to remote server without typing password (on Linux)
author: Ji Huang
date: '2020-08-12'
slug: login-to-remote-server-without-typing-password-on-linux
categories:
  - dry lab
tags:
  - server
keywords: []
description: ''
comment: no
toc: true
autoCollapseToc: false
postMetaInFooter: no
contentCopyright: no
reward: no
mathjax: no
mathjaxEnableSingleDollar: no
---


The method is very similar with Windows machine, but much easier, just two steps. The key is `ssh-keygen`. This works with WSL also.

Update: 2021-06-09


### 1. Generate local SSH key.

```shell
ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/td/.ssh/id_rsa): /home/td/.ssh/ID_NAME_HERE
```

### 2. Copy the key to the remote server.

```shell
ssh-copy-id -i ~/.ssh/ID_NAME_HERE.pub your@remote.server
```

### 3. Set file permissions

Set the permisson to user only, otherwise it will not work.

```shell
chmod 700 .ssh/
cd .ssh/
chmod 600 *
```

### 4. login to the remote server from terminal

```shell
ssh -i ~/.ssh/ID_NAME_HERE your@remote.server
```

You can also add the alias.

```shell
alias connect_greene="ssh -i ~/.ssh/ID_NAME_HERE your@remote.server"
```

PS: The SSH key does not work for me using the NYU-VPN. It works when connecting to NYU wifi or internet. 