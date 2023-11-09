> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_43855876/article/details/106310305)

一、Dmitry 是什么？

```
    DMitry是黑客渗透流程中进行深度信息收集的利器,它是一个由C语言编写的UNIX/(GNU)Linux命令行工具，无GUI操作界面，需掌握其常用使用参数。

```

二、Dmitry 可以用来做什么？

```
   1、进行TCP端口扫描，收集端口相关状态或其他信息

        如(可探测目标主机上打开的端口、被屏蔽的端口和关闭的端口)

   2、从Netcraft.com获取主机信息，子域名，域名中包含的邮件地址

        注：www.netcraft.com可用来查看站点服务器使用的操作系统信息

   3、收集Whois主机IP和域名等信息

        值得主要的是，Dmitry还支持将收集的信息保存在一个文件中，方便查看和再利用，很友好！

```

三、Dmitry 支持使用参数有哪些？  
![](https://img-blog.csdnimg.cn/20200524090803377.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg1NTg3Ng==,size_16,color_FFFFFF,t_70)三、英文不好的看这里  
-o 将输出保存到％host.txt 或由 - o 文件指定的文件

```
       -i 对主机的IP地址执行whois查找

```

![](https://img-blog.csdnimg.cn/20200524092943143.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg1NTg3Ng==,size_16,color_FFFFFF,t_70)  
-w 对主机的域名执行 whois 查找  
![](https://img-blog.csdnimg.cn/20200524093028543.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg1NTg3Ng==,size_16,color_FFFFFF,t_70)  
-n 在主机上检索 Netcraft.com 信息  
![](https://img-blog.csdnimg.cn/20200524091633694.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg1NTg3Ng==,size_16,color_FFFFFF,t_70)  
-s 执行搜索可能的子域  
![](https://img-blog.csdnimg.cn/20200524093418199.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg1NTg3Ng==,size_16,color_FFFFFF,t_70)  
-e 执行搜索可能的电子邮件地址

```
      -p 在主机上执行TCP端口扫描 (这个参数在测试时候时间有点长，还以为是没有效果。。。。。)

```

![](https://img-blog.csdnimg.cn/20200524092757732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg1NTg3Ng==,size_16,color_FFFFFF,t_70)  
-f 在显示输出报告过滤端口的主机上执行 TCP 端口扫描  
![](https://img-blog.csdnimg.cn/20200524093515940.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg1NTg3Ng==,size_16,color_FFFFFF,t_70)  
-b 读取从扫描端口接收的 banner  
![](https://img-blog.csdnimg.cn/20200524091538213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzg1NTg3Ng==,size_16,color_FFFFFF,t_70)  
-t 0-9 扫描 TCP 端口时设置 TTL（默认为 2）  
![](https://img-blog.csdnimg.cn/20200524091411619.png)