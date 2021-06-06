

<p align='center'>
    <img src="https://badgen.net/badge/labels/8"/>
    <img src="https://badgen.net/github/issues/Sweet-KK/blog"/>
    <img src="https://badgen.net/badge/last-commit/2021-06-06 08:30:36"/>
    <img src="https://badgen.net/github/forks/Sweet-KK/blog"/>
    <img src="https://badgen.net/github/stars/Sweet-KK/blog"/>
    <img src="https://badgen.net/github/watchers/Sweet-KK/blog"/>
    <img src="https://badgen.net/github/release/Sweet-KK/blog"/>
</p>

<p align='center'>
    <a href="https://github.com/jwenjian/visitor-count-badge">
        <img src="https://visitor-badge.glitch.me/badge?page_id=sweet_kk.blog"/>
    </a>
</p>


## 置顶 :thumbsup: 
- [迁移ing](https://github.com/Sweet-KK/blog/issues/2)  <sup>0 :speech_balloon:</sup>  	 
## 最新 :new: 

#### [CentOS 7.4 从0到1部署node项目](https://github.com/Sweet-KK/blog/issues/6) <sup>0 :speech_balloon:</sup> 	 2021-06-06 08:30:07

:label: : [服务器、运维](https://github.com/Sweet-KK/blog/labels/%E6%9C%8D%E5%8A%A1%E5%99%A8%E3%80%81%E8%BF%90%E7%BB%B4)

date: 2018-04-15

一直以来对Linux都不是很熟悉，只是通过w3c之类的一些网站看过介绍，敲过几行命令，仅此而已。总是对Linux有些逃避，偶尔看到那些建站教程，一堆看不懂的命令，倍感头大。虽说自己是个前端，平时只要懂得写页面写交互就好，最多也就是写一下后台，但是最终还是会接触到网站部署的，熟悉整一个建站的流程，对前端一些优化也会有更深的认识。这不刚好领导给了个服务器我练练手，让我学学怎么部署维护网站。所以就有了此文，记录下我这个Linux小白建站全过程。



首先就是有一台服务器，然后用xshell或者别的远程连接

### 系统版本说明

```
# cat /etc/redhat-release 
CentOS Linux release 7.4.1708 (Core)
```

### 更新服务器系统以及软件，安装常用工具

一般来说，云服务器或者 `vps` 安装的系统镜像都不是最新的，所以我们连接上服务器之后，必须尽快更新服务器的系统以及软件，这样可以更好的保障我们的服务器系统安全

#### 服务器系统以及软件升级命令

```
# yum -y update
```

`CentOS` 系列的服务器系统有一个毛病，就是官方自带的源的软件比较古老，并且很多的软件都没有。因为他们的首要任务是保证服务器的稳定，而不是追求最新。但是太过于保守了，一般来说，我们会给服务器添加一个 `epel-release` 这个源。这个源里包含了例如 `nginx` 之类的我们需要的软件，使用起来比较方便。

#### 安装 epel-release

```
# yum install epel-release -y
```

通过上面的命令进行安装。确认是否安装成功，可以用下面的命令检测一下

```
# yum search nginx
```

如果搜索的结果包含下面的这行内容，就表示安装成功了，然后我们就能愉快的安装我们需要的软件了。

```
nginx.x86_64 : A high performance web server and reverse proxy server
```

我在配置的时候发现不能搜索出来，但是确实是安装上了。后来检查了一下 `/etc/yum.repos.d/epel.repo` 文件，发现里面配置不对，修改了一下就好。

主要是 `epel` 段落中的 `enabled` 值默认设置为 `0` 了，我们将值改成 `1` 就可以了。

> PS：你应该没这个问题。如果遇到了问题，可以看下这里。如果是其他问题，请自行搜索解决。

#### 安装服务器常用软件

前面我们登录上服务器之后，第一件事情就是安装 `vim` 编辑器。

安装vim详细过程:

[https://blog.csdn.net/u013233097/article/details/52355256](https://blog.csdn.net/u013233097/article/details/52355256)

但我们在工作中，可能会需要各种各样的软件，例如我经常使用的如下：

wget 下载工具

```
# yum install wget
```

统一各种格式压缩文件的工具

```
# yum install atool
```

tmux 好用的终端工具(如何使用请自行搜索)

```
# yum install tmux
```

zsh 最好用的终端

```
# yum install zsh
```

替代 top 命令的好工具

```
# yum install htop
```

git 代码版本管理工具

```
# yum install git
```

上面这些常用软件可以根据你自己的需求进行选择安装。不是都必须安装的。

什么 `zsh` 之类的配置，可以使用 `oh-my-zsh` 这个配置工具，具体搜索一下。网上教程很多。不是必须的。

### 配置 lnmp 服务器环境

好，准备工作差不多了，下面正式开始。

#### 安装 nginx

如果你是直接跳到这段看的，请确保你已经运行过下面的命令安装过 `epel-release` 。如果不是，请跳过这条命令。

```
# yum install epel-release -y
```

开始安装：

安装 nginx

```
# yum install nginx -y
```

启动 nginx

```
# systemctl start nginx
```

将 nginx 设置为开机启动

```
# systemctl enable nginx
```

好，通过上面三条命令执行之后，应该可以在浏览器中直接用服务器IP可以访问到 `nginx` 默认的首页了。

注意：安装成功后，在浏览器输入IP地址，打不开默认欢迎页面。

原因：CentOS 7版本之后对防火墙进行加强，不再使用原来的iptables，启用firewall防火墙默认不开放任何端口，所以Nginx默认的80端口也没有被放开，故而无法访问。

解决：这里，采用让firewall放开80端口，具体操作如下：

1）查看当前已经开放的端口

```
# firewall-cmd --list-ports
```

2）开启80端口

```
# firewall-cmd --zone=public --add-port=80/tcp --permanent
success
```

3）重启防火墙

```
# firewall-cmd --reload
success
```

4）查看当前已经开放的端口

```
# firewall-cmd --list-ports
80/tcp
```

此时，在浏览器直接输入IP地址访问Nginx默认欢迎页面，正常。

### 安装 php

`nginx` 安装好之后，我们就需要来安装我们的 `php` 环境了。

#### 安装 php

执行下面的命令，安装 `PHP` 已经它的常用的库

```
# yum install php php-mysql php-fpm php-gd php-imap php-ldap php-mbstring php-odbc php-pear php-xml php-xmlrpc -y
```

#### 配置 php

安装完成之后，我们需要对它进行一些配置。首先，我们打开配置文件：

```
# vim /etc/php.ini
（至于vi/vim的使用，自行查阅吧，最基本的i、esc、:wq、:q!会了就好）
```

打开文件后，我们找到 `cgi.fix_pathinfo`(大概在 763 行) 并把它的值设置为 `0`

配置好 `php.ini` 文件之后，我们来配置 `/etc/php-fpm.d/www.conf` 文件

```
# vim /etc/php-fpm.d/www.conf
```

第一处修改，将 `listen = 127.0.0.1:9000` 修改为如下：

```
listen = /var/run/php-fpm/php-fpm.sock
```

然后找到下面两行，删掉前面的 `;` 分号，取消注释。

```
listen.owner = nobody
listen.group = nobody
```

最后，我们找到下面两行

```
user = apache
group = apache
```

将 `apache` 换成 `nginx`，如下所示：

```
user = nginx
group = nginx
```

好，这样，我们就已经安装并且配置好了。下面我们可以启动了。

启动PHP

```
# systemctl start php-fpm
```

将它设置为开机启动

```
# systemctl enable php-fpm
```

### 配置 nginx 使其支持 php

好，我们在安装好 `nginx` 和 `php` 之后，他们还不能协同作战，我们需要对 `nginx` 进行一些配置才可以。

首先，我们打开 `nginx` 的配置文件

```
# vim /etc/nginx/nginx.conf
```

然后找到 `server` 这一段，添加如下内容,如图所示：

```
    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
```

另外，还需要配置默认的首页文件，添加 `index.php` ,如图所示：

```
index index.php index.html index.htm;
```



![image.png](https://upload-images.jianshu.io/upload_images/8192053-65db438957199cb3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好，经过这样的简单配置，我们的任务就已经完成了。

重启 nginx 服务

```
# systemctl restart nginx
```

### 安装 MySQL(问题很多,建议跳到下一章安装MariaDB)

从CentOS 7.0发布以来，yum源中开始使用mariadb来代替MySQL的安装。即使你输入的是yum install mysql , 显示的也是mariadb的安装内容。

![image.png](https://upload-images.jianshu.io/upload_images/8192053-16f03d0d33876cc6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入yum install mysql-server，提示yum没有可用的安装包。

![image.png](https://upload-images.jianshu.io/upload_images/8192053-102f5db29517e531.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因此，如果使用yum安装MySQL的话，就需要去下载官方指定的yum源。

网址为： [https://dev.mysql.com/downloads/repo/yum/](https://dev.mysql.com/downloads/repo/yum/)

先卸载mariadb。

检查mariadb是否已安装

```
# yum list installed | grep mariadb
mariadb-libs.x86_64                    1:5.5.56-2.el7                  @anaconda 
```

全部卸载

```
# yum -y remove mariadb*
```

下面进行MySQL的安装。

一、下载安装官方提供的yum rpm包

打开网页：[https://dev.mysql.com/downloads/repo/yum/](https://dev.mysql.com/downloads/repo/yum/)

点击Red Hat Enterprise Linux 7 / Oracle Linux 7 (Architecture Independent), RPM Package后面的download，进入新的页面点击[No thanks, just start my download.](https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm)就可以看到下载源地址了,复制下来。

进入如下目录

```
# cd /usr/local/src/
```

下载rpm包：

```
# wget https://repo.mysql.com//mysql80-community-release-el7-1.noarch.rpm
```

安装rpm包：

```
# rpm -ivh mysql80-community-release-el7-1.noarch.rpm
```

检查mysql的yum源是否安装成功：

```
# yum repolist enabled | grep "mysql.*-community.*"
mysql-connectors-community/x86_64        MySQL Connectors Community           45  
mysql-tools-community/x86_64             MySQL Tools Community                57  
mysql57-community/x86_64                 MySQL 5.7 Community Server          247
```

看到如上，表示安装成功。

再次使用yum来安装mysql-server，就不会提示没有可用软件包。

二、使用yum install mysql-server安装

```
# yum install mysql-server
```

查看版本信息：

```
# rpm -qi mysql-community-server
```

三、启动mysql-server

```
# service mysqld start
```

四、使用初始密码登陆

```
# cat /var/log/mysqld.log|grep 'A temporary password'
2018-02-11T06:47:54.773267Z 1 [Note] A temporary password is generated for root@localhost: 3=v/i;z/Y;P>
```

最后一行冒号后面的部分3=v/i;z/Y;P>就是初始密码。 
使用此密码登录MySQL:

```
# mysql -u root -p
Enter password:  
Welcome to the MySQL monitor.  Commands end with ; or \g.  
Your MySQL connection id is 3  
Server version: 5.7.21  
  
Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.  
  
Oracle is a registered trademark of Oracle Corporation and/or its  
affiliates. Other names may be trademarks of their respective  
owners.  
  
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.  
  
mysql>
```

正常登陆后发现一个问题：

```
mysql> show databases; 
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.  
```

解决方法如下：

```
mysql> alter user 'root'@'localhost' identified by 'yourpassword';  
Query OK, 0 rows affected (0.00 sec)  
  
mysql> set password=password("yourpassword");  
Query OK, 0 rows affected, 1 warning (0.00 sec)  
  
mysql> flush privileges;  
Query OK, 0 rows affected (0.00 sec)  
  
mysql> show databases;  
+

[更多>>>](https://github.com/Sweet-KK/blog/issues/6)

---


#### [干货!各种常见布局+知名网站实例分析+相关阅读推荐](https://github.com/Sweet-KK/blog/issues/5) <sup>0 :speech_balloon:</sup> 	 2021-06-06 08:27:10

:label: : [前端](https://github.com/Sweet-KK/blog/labels/%E5%89%8D%E7%AB%AF)

date: 2018-03-13

阅前必看：本文总结了各种常见的布局实现，网上搜的“史上最全布局”好像也没有这么全吧？哈哈！就当作一个知识整理吧。各位读者如果发现问题或者有什么意见，欢迎提出！——欢迎关注！欢迎Star！

[链接](https://github.com/Sweet-KK/

[更多>>>](https://github.com/Sweet-KK/blog/issues/5)

---


#### [CSS预处理器--Less语法整理](https://github.com/Sweet-KK/blog/issues/4) <sup>0 :speech_balloon:</sup> 	 2021-06-06 08:20:36

:label: : [前端](https://github.com/Sweet-KK/blog/labels/%E5%89%8D%E7%AB%AF)

date: 2017-09-30

这是本人整理的Less语法,基本上全了，至于进阶就是看你怎么组合怎么使用了。本文整理的内容都亲测有效！


## 一,变量
#### 基本使用
**Less:**
```
.@{selector} {
    width: 100px;
  

[更多>>>](https://github.com/Sweet-KK/blog/issues/4)

---


#### [对constructor和prototype的理解](https://github.com/Sweet-KK/blog/issues/3) <sup>0 :speech_balloon:</sup> 	 2021-06-06 08:16:54

:label: : [前端](https://github.com/Sweet-KK/blog/labels/%E5%89%8D%E7%AB%AF)

date: 2017-08-13

在学习js面向对象过程中，我们总是对`constructor`和`prototype`充满疑惑，这两个概念是相当重要的，深入理解这两个概念对掌握js的一些核心概念非常的重要。因此，在这里记录下鄙人见解，希望可以给读者带来一些帮助。如若有错，请大佬们不吝指正，十

[更多>>>](https://github.com/Sweet-KK/blog/issues/3)

---


#### [迁移ing](https://github.com/Sweet-KK/blog/issues/2) <sup>0 :speech_balloon:</sup> 	 2021-06-01 03:05:45

:label: : [:+1: 置顶](https://github.com/Sweet-KK/blog/labels/%3A%2B1%3A%20%E7%BD%AE%E9%A1%B6)

迁移ing


[更多>>>](https://github.com/Sweet-KK/blog/issues/2)

---


## 分类  :card_file_box: 

<details open="open">
    <summary>
        <img src="assets/wordcloud.png" title="词云, 点击展开详细分类" alt="词云， 点击展开详细分类">
        <p align="center">:cloud: 词云 :cloud: <sub>点击词云展开详细分类:point_down: </sub></p>
    </summary>


<details>
<summary>:+1: 置顶	<sup>1:newspaper:</sup></summary>

- [迁移ing](https://github.com/Sweet-KK/blog/issues/2)  <sup>0 :speech_balloon:</sup>  	 


</details>

<details>
<summary>:framed_picture: 封面	<sup>1:newspaper:</sup></summary>

- [Open the future](https://github.com/Sweet-KK/blog/issues/1)  <sup>0 :speech_balloon:</sup>  	 


</details>

<details>
<summary>java	<sup>0:newspaper:</sup></summary>



</details>

<details>
<summary>node	<sup>0:newspaper:</sup></summary>



</details>

<details>
<summary>python	<sup>0:newspaper:</sup></summary>



</details>

<details>
<summary>前端	<sup>3:newspaper:</sup></summary>

- [干货!各种常见布局+知名网站实例分析+相关阅读推荐](https://github.com/Sweet-KK/blog/issues/5)  <sup>0 :speech_balloon:</sup>  	 
- [CSS预处理器--Less语法整理](https://github.com/Sweet-KK/blog/issues/4)  <sup>0 :speech_balloon:</sup>  	 
- [对constructor和prototype的理解](https://github.com/Sweet-KK/blog/issues/3)  <sup>0 :speech_balloon:</sup>  	 


</details>

<details>
<summary>开源	<sup>0:newspaper:</sup></summary>



</details>

<details>
<summary>服务器、运维	<sup>1:newspaper:</sup></summary>

- [CentOS 7.4 从0到1部署node项目](https://github.com/Sweet-KK/blog/issues/6)  <sup>0 :speech_balloon:</sup>  	 


</details>


</details>    
