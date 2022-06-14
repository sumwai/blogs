---
title: "Alacritty SSH 连接问题"
date: 2022-06-13T22:32:36+08:00
draft: true
---

## 问题描述

在使用`Alacritty`终端连接SSH服务器后，如果使用`clear`/`ctrl`+`l`会出现以下报错
```text
'alacritty': unknown terminal type.
```

## 解决方案

### 1. 使用环境变量

将以下内容写入到`.bashrc`/`.zshrc`

```bash
alias ssh='TERM=xterm-256color ssh'
```

### 2. 将alacritty信息保存到服务端

在服务器使用以下命令

```bash
curl -sSL https://raw.githubusercontent.com/alacritty/alacritty/master/extra/alacritty.info | tic -x -
```

> Tips: 如果是国内的服务器，可能上面的命令下载不了，可以使用ghproxy下载

```bash
curl -sSL https://ghproxy.com/https://raw.githubusercontent.com/alacritty/alacritty/master/extra/alacritty.info | tic -x -
```
