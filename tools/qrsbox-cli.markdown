---
layout: default
title: qrsbox 和 qrsboxcli 同步工具
---


## 简介

七牛云存储提供一组同步上传的客户端工具，包括qrsbox和qrsboxcli。qrsbox 是同步上传客户端的 Windows GUI 版，而 qrsboxcli 是同步上传客户端的命令行版。
同步上传客户端可将用户本地的某目录的文件同步到七牛云存储中，支持大文件上传，支持增量同步。此外，它们能够监控目录变化，将目录中新增的文件上传至七牛云存储。
需要注意的是，这两个同步上传客户端不会同步文件的删除操作。也就是，如果被监控的目录中文件被删除，已上传至七牛云存储的文件将仍旧保留。如果用户确实需要删除该文件，可以到七牛的 [portal 后台](https://portal.qiniu.com/) 中删除。采用这种方式的目的是为了防止用户误删文件造成数据丢失。同时，另外的一个好处是，就是同步完一个文件后，本地就可以直接删除它以释放本地的磁盘空间。

关于这两个同步客户端的一些疑问，可以在[疑问简答](http://kb.qiniu.com/537ps105)中找到答案。


## 下载

qrsbox下载地址：

  - Windows: [qrsbox v0.6.0 windows_386](http://open.qiniudn.com/qrsbox-v0.6.0.zip)

qrsboxcli下载地址：

  - Mac 64bits: [qrsboxcli v2.5.20130921 darwin_amd64](http://open.qiniudn.com/devtools/v2.5.20130921/darwin_amd64/qrsboxcli)
  - Linux 64bits: [qrsboxcli v2.5.20130921 linux_amd64](http://open.qiniudn.com/devtools/v2.5.20130921/linux_amd64/qrsboxcli)
  - Linux 32bits: [qrsboxcli v2.5.20130921 linux_386](http://open.qiniudn.com/devtools/v2.5.20130921/linux_386/qrsboxcli)
  - Windows: [qrsboxcli v2.5.20130921 windows_386](http://open.qiniudn.com/devtools/v2.5.20130921/windows_386/qrsboxcli.exe)


## qrsbox使用方法

qrsbox是Windows版的同步上传客户端，拥有GUI界面，使用方便。

首先，下载qrsbox，并解压。

然后，在资源管理器中，进入解压后的文件，双击qrsbox.exe，弹出如下图所求的界面：

<div class="imgwrap"><img src="img/qrsbox-demo.png" alt="qrsbox"/></div>

其中，`access_key` 和 `secret_key` 在七牛云存储平台上申请。步骤如下：

1. [开通七牛开发者帐号](https://portal.qiniu.com/signup)
1. [登录七牛开发者自助平台，查看 Access Key 和 Secret Key](https://portal.qiniu.com/setting/key)

`同步源目录` 是本地需要上传的目录，绝对路径完整表示。这个目录中的所有内容会被同步到指定的 `bucket` 上。注意：Windows 平台上路径的表示格式为：`盘符:/目录`，比如 E 盘下的目录 data 表示为：`e:/data` 。

`空间名(bucket)` 是你在七牛云存储上希望保存数据的 Bucket 名（类似于数据库的表），这个自己选择一个合适的就可以，要求是只能由字母、数字、下划线等组成。

完成这些设置后，点击确认，qrsbox便会开始进行初始化。初始化完成后，就开始文件的同步。用户可以在qrsbox的界面上看到同步的进程，大致如下图所示：

![查看同步进程](img/qrsbox-sync.png)

随着文件同步的进行，用户可以在[开发者平台](https://portal.qiniu.com/)的空间管理中看到已上传的文件。

qrsbox启动后会常驻内存，在Windows的任务栏中显示托盘 ![托盘](img/qrsbox-icon.png) 。

如果用户需要修改同步目录，AccessKey/SecretKey，或其他参数，可以右键单击qrsbox的托盘，选择“配置”菜单项，打开配置界面，重新配置。

## qrsboxcli使用方法

qrboxcli是命令行版的同步上传客户端，可以在多种操作系统上使用，包括：Linux、OS X、Windows等。

首先，下载qrsboxcli，并解压。

然后，执行以下命令，进行初始化：

```
    qrsboxcli init <AccessKey> <SecretKey> <SyncDir> <Bucket> [<KeyPrefix>]
```

其中，`<AccessKey>` 和 `<SecretKey>` 在七牛云存储平台上申请。步骤如下：

1. [开通七牛开发者帐号](https://portal.qiniu.com/signup)
1. [登录七牛开发者自助平台，查看 Access Key 和 Secret Key](https://portal.qiniu.com/setting/key)

`<SyncDir>` 是本地的同步目录，该目录下的文件会随时同步上传值七牛云存储。

`<Bucket>` 是保存同步文件的[资源空间](http://docs.qiniu.com/api/v6/terminology.html#Bucket)名。

`<KeyPrefix>` 是文件前缀，可选。如果设置了该参数，那么上传的文件名前都会加上前缀。这个前缀主要用于在空间中区分不同上传来源的文件。

最后，用户可以使用以下命令开始文件同步：

```
    qrsboxcli sync &
```

这里使用了 `&` 符号，让同步客户端进程运行在后台。

用户可以通过已下命令查看同步过程：

```
    qrsboxcli log
```

如果用户希望改变同步的目录，只需要用新的目录路径重新运行 qrsboxcli 初始化命令，并重启同步进程，qrsboxcli会立刻将新目录的文件同步至七牛云存储。
