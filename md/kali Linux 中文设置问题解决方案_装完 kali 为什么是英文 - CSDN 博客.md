> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_35722690/article/details/122875715)

kali linux 2020 版 虚拟机文件默认为英语状态，怎样设置中文

1、如果没有将 kali 的更新源切换成国内的源，先将更新源切换成国内的

2、在 root 用户下直接输入 “[dpkg](https://so.csdn.net/so/search?q=dpkg&spm=1001.2101.3001.7020)-reconfigure locales” 命令，如果是 kali 普通用户则输入 “sudo dpkg-reconfigure locales” 然后输入密码 kali

![](https://img-blog.csdnimg.cn/2e79c1a682fc44c88f72b0b6afd8eaaa.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfMzU3MjI2OTA=,size_20,color_FFFFFF,t_70,g_se,x_16)

3、先找到 en_US.UTF-8，空格键取消它，然后再找到 zh_CN.UTF-8，空格键选中

![](https://img-blog.csdnimg.cn/3a32467e0dfb44b49599586f20fc13f1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAcXFfMzU3MjI2OTA=,size_20,color_FFFFFF,t_70,g_se,x_16)

4、单击确定后，下一界面，选择 zh_CN.UTF-8，确定

5、然后 reboot 重启生效

6、重启后会出现乱码，再次打开终端，输入 “apt-get install [ttf](https://so.csdn.net/so/search?q=ttf&spm=1001.2101.3001.7020)-wqy-microhei ttf-wqy-zenhei xfonts-wqy” 下载中文字体

7、输入 reboot 重启，重启后就完成了 kali 的中文配置