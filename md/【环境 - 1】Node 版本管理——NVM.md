> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/646970780)

一、概述
----

nvm（Node Version Manager）是 [Node.js](https://link.zhihu.com/?target=https%3A//nodejs.org/en/) 的版本管理器，可以让我们轻松地在不同的 Node.js 版本之间进行切换。

官网：

[nvm-sh/nvm: Node Version Manager - POSIX-compliant bash script to manage multiple active node.js versions (github.com)](https://link.zhihu.com/?target=https%3A//github.com/nvm-sh/nvm)

nvm-windows：

[https://github.com/coreybutler/nvm-windows](https://link.zhihu.com/?target=https%3A//github.com/coreybutler/nvm-windows)

中文网

[nvm 文档手册 - nvm 是一个 nodejs 版本管理工具 - nvm 中文网](https://link.zhihu.com/?target=https%3A//nvm.uihtm.com/)

二. 使用（安装在后面）
------------

```
#切换node的版本
$ nvm use 16
Now using node v16.9.1 (npm v7.21.1)
$ node -v
v16.9.1
$ nvm use 14
Now using node v14.18.0 (npm v6.14.15)
$ node -v
v14.18.0
# 安装指定版本，latest 是最新版本
$ nvm install 12
Now using node v12.22.6 (npm v6.14.5)
# 查看当前node的版本
$ node -v
v12.22.6

```

### 1. 查看当前使用的 Node.js 版本

```
nvm current   // v20.5.0

```

### 2. 查看所有 Node.js 版本，* 表示当前使用的版本

```
nvm ls


    20.5.0
  * 14.21.0 (Currently using 64-bit executable)

```

### 3. 查看可安装列表

```
nvm list available

|   CURRENT    |     LTS      |  OLD STABLE  | OLD UNSTABLE |
|--------------|--------------|--------------|--------------|
|    20.5.0    |   18.17.0    |   0.12.18    |   0.11.16    |
|    20.4.0    |   18.16.1    |   0.12.17    |   0.11.15    |
|    20.3.1    |   18.16.0    |   0.12.16    |   0.11.14    |
|    20.3.0    |   18.15.0    |   0.12.15    |   0.11.13    |
|    20.2.0    |   18.14.2    |   0.12.14    |   0.11.12    |
|    20.1.0    |   18.14.1    |   0.12.13    |   0.11.11    |
|    20.0.0    |   18.14.0    |   0.12.12    |   0.11.10    |
|    19.9.0    |   18.13.0    |   0.12.11    |    0.11.9    |
|    19.8.1    |   18.12.1    |   0.12.10    |    0.11.8    |
|    19.8.0    |   18.12.0    |    0.12.9    |    0.11.7    |
|    19.7.0    |   16.20.1    |    0.12.8    |    0.11.6    |
|    19.6.1    |   16.20.0    |    0.12.7    |    0.11.5    |
|    19.6.0    |   16.19.1    |    0.12.6    |    0.11.4    |
|    19.5.0    |   16.19.0    |    0.12.5    |    0.11.3    |
|    19.4.0    |   16.18.1    |    0.12.4    |    0.11.2    |
|    19.3.0    |   16.18.0    |    0.12.3    |    0.11.1    |
|    19.2.0    |   16.17.1    |    0.12.2    |    0.11.0    |
|    19.1.0    |   16.17.0    |    0.12.1    |    0.9.12    |
|    19.0.1    |   16.16.0    |    0.12.0    |    0.9.11    |
|    19.0.0    |   16.15.1    |   0.10.48    |    0.9.10    |

This is a partial list. For a complete list, visit https://nodejs.org/en/download/releases

```

### 4. 升级 / 安装 Node.js 版本

Node.js 的版本管理方法有很多种，比如手动下载最新版，使用 Node 版本管理工具等。而使用 NVM 管理 Node.js 版本则非常方便。使用如下的命令升级或安装 Node.js 的版本：

```
nvm install 10
nvm install 10.0.0
nvm install latest

```

### 5. 切换 Node.js 版本命令

```
nvm use 8.0.0
nvm use 8

```

### 6. 离线安装

有时候，我们可能需要在没有网络的情况下安装 Node.js 版本。这时候，我们可以先下载 Node.js 的**二进制**安装包然后通过 NVM 进行安装。以下是 NVM 离线安装 Node.js 版本的命令：

```
nvm install /path/to/binary

```

其中，/path/to/binary 表示你要安装的二进制文件路径。

例如，我们已经将 Node.js 的 8.0.0 二进制文件放置在了 “/opt/nodejs/node-v8.0.0-linux-x64.tar.xz” 这个路径下，那么可以执行以下命令进行安装：

```
nvm install /opt/nodejs/node-v8.0.0-linux-x64.tar.xz

```

执行该命令后，NVM 就会自动解压、安装 Node.js 8.0.0，并将其添加到已安装列表中。

### 7. 命令列表说明

```
nvm命令提示
nvm arch：显示node是运行在32位还是64位。
nvm install <version> [arch] ：安装node， version是特定版本也可以是最新稳定版本latest。可选参数arch指定安装32位还是64位版本，默认是系统位数。可以添加--insecure绕过远程服务器的SSL。
nvm list [available] ：显示已安装的列表。可选参数available，显示可安装的所有版本。list可简化为ls。
nvm on ：开启node.js版本管理。
nvm off ：关闭node.js版本管理。
nvm proxy [url] ：设置下载代理。不加可选参数url，显示当前代理。将url设置为none则移除代理。
nvm node_mirror [url] ：设置node镜像。默认是https://nodejs.org/dist/。如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。
nvm npm_mirror [url] ：设置npm镜像。https://github.com/npm/cli/archive/。如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。
nvm uninstall <version> ：卸载指定版本node。
nvm use [version] [arch] ：使用制定版本node。可指定32/64位。
nvm root [path] ：设置存储不同版本node的目录。如果未设置，默认使用当前目录。
nvm version ：显示nvm版本。version可简化为v。


```

三、Window 安装
-----------

官网下载地址：[Releases · coreybutler/nvm-windows](https://link.zhihu.com/?target=https%3A//github.com/coreybutler/nvm-windows/releases)

![](https://pic4.zhimg.com/v2-cbf41dabc4f973d47e3f2582da6644b7_r.jpg)

我下载了`nvm-steup.exe`，下载后双击运行选择安装位置。

![](https://pic4.zhimg.com/v2-127ad97c160da27be377be55c9f8f383_r.jpg)![](https://pic1.zhimg.com/v2-4ab21c4e6813078acce7f76b79404f24_r.jpg)

> 在这里创建一个软连接，并添加到 path，相当于配置了环境变量。  
> 选用默认的或者自己已安装 node 的目录也是可以的。会有弹框，一直点是就可以了

![](https://pic1.zhimg.com/v2-3cc01de78c49f7ce4677f1a66dc799b8_r.jpg)

选择是。相当于电脑已经安装了 node.js，是否让 nvm 管理起来，选择是之后就会复制一份到自己的安装目录中。省去了再下载一次。

最后，Finish 就安装好了。可以通过前面的使用命令进行使用了。

四、Linux 安装
----------

### 1. 下载安装包

`wget https://github.com/nvm-sh/nvm/archive/refs/tags/v0.39.0.tar.gz`

版本号可改，去看官网版本列表，官网地址在最前面。

```
[root@centos1 nvm]# wget https://github.com/nvm-sh/nvm/archive/refs/tags/v0.39.0.tar.gz
--2023-07-31 10:06:14--  https://github.com/nvm-sh/nvm/archive/refs/tags/v0.39.0.tar.gz
正在解析主机 github.com (github.com)... 20.205.243.166
正在连接 github.com (github.com)|20.205.243.166|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 302 Found
位置：https://codeload.github.com/nvm-sh/nvm/tar.gz/refs/tags/v0.39.0 [跟随至新的 URL]
--2023-07-31 10:06:15--  https://codeload.github.com/nvm-sh/nvm/tar.gz/refs/tags/v0.39.0
正在解析主机 codeload.github.com (codeload.github.com)... 20.205.243.165
正在连接 codeload.github.com (codeload.github.com)|20.205.243.165|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：未指定 [application/x-gzip]
正在保存至: “v0.39.0.tar.gz”

    [  <=>                                                                                               ] 154,875      444KB/s 用时 0.3s   

2023-07-31 10:06:16 (444 KB/s) - “v0.39.0.tar.gz” 已保存 [154875]

[root@centos1 nvm]# ll
总用量 152
-rw-r--r--. 1 root root 154875 7月  31 10:06 v0.39.0.tar.gz
[root@centos1 nvm]# 

```

2. 解压

```
mkdir -p /root/.nvm
tar -zxvf v0.39.0.tar.gz -C /root/.nvm
ls /root/.nvm/

```

> 注意解压文件名，看看下载下来是啥名就改成啥名，有的是`nvm-0.39.0.tar.gz`

3. 配置环境

（1）打开`vim ~/.bashrc`

（2）在~/.bashrc 的末尾，添加如下语句：

```
export NVM_DIR="$HOME/.nvm/nvm-0.39.0"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
# This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  
# This loads nvm bash_completion
# nodejs下载更换淘宝镜像
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node

```

> 这个注意一下 NVM_DIR，确定这个目录下能找到 nvm.sh，有的解压之后可能文件名会变，去看一下`ls /root/.nvm/`

（3）使配置生效

```
source ~/.bashrc

```

4. 查看和使用`nvm -v`

五、nvm 切换国内镜像（如果下载 node.js 慢的话）
------------------------------

如果下载 node 过慢或者安装失败，请更换国内镜像源, 在 nvm 的安装路径下，找到 settings.txt，设置 node_mirro 与 npm_mirror 为国内镜像地址。下载就飞快了~~

```
root: D:\nvm
path: D:\nodejs
nvm npm_mirror https://npmmirror.com/mirrors/npm/
nvm node_mirror https://npmmirror.com/mirrors/node/
或者：
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/


```

### 命令行切换 (注意：请切换国内镜像后再安装 node 版本，否则会很慢)

### 阿里云镜像

```
nvm npm_mirror https://npmmirror.com/mirrors/npm/
nvm node_mirror https://npmmirror.com/mirrors/node/

```

### 腾讯云镜像

```
nvm npm_mirror http://mirrors.cloud.tencent.com/npm/
nvm node_mirror http://mirrors.cloud.tencent.com/nodejs-release/

```

打开链接查看可以 node 版本：[https://registry.npmmirror.com/binary.html?path=node/](https://link.zhihu.com/?target=https%3A//registry.npmmirror.com/binary.html%3Fpath%3Dnode/)