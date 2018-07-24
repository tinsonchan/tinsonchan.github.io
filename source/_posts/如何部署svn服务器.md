---
title: 如何部署svn服务器
comments: false
categories:
  - 技术
date: 2018-07-24 21:55:47
tags:
  - svn
---

时间过的好快呀，一眨眼又是一个月过去了，距离上次写日志已经是一个月了。最近在研究svn服务器的东西，主要是为了让多人协助的时候，可以很好的做一个权限控制。虽然git也是挺好用的，但是权限控制还是不够，毕竟站在公司角度，还是希望这方面可以做的比较好一点。所以最近在自己购买的搬瓦工的机器上实验了一下，还是可以的，呵呵呵。现在有一种强迫症，就是一个东西开始了，就必须强迫自己完成，不能再像之前那样做到一半就放弃了。
<!--more-->
下面开始备注一下部署svn的一个过程。

### 准备工作 ###
1. 购买一台云机器，当然Windows也是可以的，如果用于内网就完全没问题。本人的机器是搬瓦工上面的，主要是够便宜，买了做vpn用的，想不到居然还可以用来做这个实验。
2. 安装系统，这里主要是用centos

    [root@host ~]# uname -a
	Linux host.localdomain 2.6.32-642.el6.i686 #1 SMP Tue May 10 16:13:51 UTC 2016 i686 i686 i386 GNU/Linux
3. 重启服务器，准备开始工作了

### 安装apache ###
    [root@host ~]#yum install httpd httpd-devel
	[root@host ~]#service httpd start
	[root@host ~]#chkconfig httpd on

	//修改apache端口
	[root@host ~]#vi /etc/httpd/conf/httpd.conf
	//用 "/ServerName"搜索，可以快速定位
	//增加这一行内容ServerName localhost:80
	
	//打开防火墙的80端口
	[root@host ~]#vi /etc/sysconfig/iptables
	//增加一行"-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT"
	[root@host ~]#service iptables restart

到这里，apache已经安全成功了
可以打开网址 http://ip看看有没有能够打开apache的默认页面

### 安装svn服务 ###
安装svn服务 

    [root@host ~]#yum install mod_dav_svn subversion

漫长的等待安装，然后重启apache

	[root@host ~]#service httpd restart

查看是否安装svn模块

	[root@host ~]#ls /etc/httpd/modules/ | grep svn

查看svn版本

	[root@host ~]#svn --version

创建svn主目录

	[root@host ~]#mkdir /data/svn/
	[root@host ~]#cd /etc/httpd/conf.d

在这个目录下，有一个subversion.conf的配置文件，需要增加以下内容
	
	[root@host ~]#vi subversion.conf
	<Location /svn/>
	DAV svn
	SVNListParentPath on
	SVNParentPath /data/svn
	AuthType Basic
	AuthName "Subversion repositories"
	AuthUserFile /data/svn/passwd.http
	AuthzSVNAccessFile /data/svn/authz
	Require valid-user
	</Location>
	RedirectMatch ^(/data/svn)$ $1/

创建/data/svn/passwd.http和/data/svn/authz
    [root@host ~]#touch /svn/passwd.http
	[root@host ~]# touch /svn/authz

重启apache

	[root@host ~]# service httpd restart

### 安装jsvnadmin ###
svnadmin是一个java版本的svn管理可视化工具，方便我们多个版本控制

	https://code.google.com/p/jsvnadmin/

现在最新版本是3.0.5版本，可以去下载

### 安装Mysql ###
是的，需要数据库，jsvnadmin支持多个数据库，不过习惯用mysql，就继续用了
	
查看该操作系统上是否已经安装了mysql数据库，

	[root@host ~]#rpm -qa | grep MySQL
	
	[root@host ~]# installmysql-server mysql mysql-devel
	[root@host ~]# service mysqld start ##启动数据库

查看MySQL是否设置开机启动

	[root@host ~]# chkconfig --list| grep mysqld 
	mysqld          0:off   1:off  2:off   3:off   4:off  5:off   6:off

设置开启启动
	
	[root@host ~]# chkconfig mysqld on

因为整个库管理是用网页打开的，所以需要打开防火墙端口

	[root@host ~]#vi /etc/sysconfig/iptables
	//增加一行"-A INPUT -p tcp -m state --state NEW -m tcp --dport 3060 -j ACCEPT"
	//重启防火墙
	[root@host ~]#service iptables restart

设置MySQL数据库root用户的密码：
	
	[root@host ~]# mysqladmin -uroot password 'root'

登陆数据库

	[root@host ~]# mysql -u root -p

授权远程登陆

	grant all privileges  on *.* to root@'%' identified by "root";
	flush privileges;

创建需要的数据库以及导入数据

	mysql> create database svnadmin;
    mysql> use svnadmin;
	mysql> source /svnadmin/db/mysql5.sql //svnadmin自带的数据库脚本呢
	mysql> source /svnadmin/db/en/lang/en.sql

### 安装jdk ###  
查看存在的jdk的版本
	
	[root@host ~]#yum search java|grep jdk
	[root@host ~]#yum install java-1.7.0-openjdk

设置环境变量

	[root@host ~]#vi /etc/profile
	#set java environment
	JAVA_HOME=/usr/java/jdk1.7.0_79
	JRE_HOME=/usr/java/jdk1.7.0_79/jre
	CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
	PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
	export JAVA_HOME JRE_HOME CLASS_PATH PATH

让环境变量生效
	[root@host ~]#source /etc/profile

### 使用Tomcat部署svnadmin ###
下载tomcat的包

	[root@host ~]#wget http://mirrors.hust.edu.cn/apache/tomcat/tomcat-7/v7.0.90/bin/apache-tomcat-7.0.90.tar.gz

解压tomcat包	

	[root@host ~]#tar -zxvf apache-tomcat-7.0.90.tar.gz
	[root@host ~]# mv apache-tomcat-7.0.90 svnadmin-tomcat

	[root@host ~]#cd /root/svnadmin-tomcat/webapps
	[root@host ~]#rm -rf *

把svnadmin.war上传到webapps

	[root@host ~]# cd /root/svnadmin-tomcat/webapps
	[root@host ~]# unzip svnadmin.war -d svnadmin
	[root@host ~]# cd svnadmin/WEB-INF
	[root@host ~]# vi jdbc.properties

修改配置内容

	db=MySQL
	#MySQL
	MySQL.jdbc.driver=com.mysql.jdbc.Driver
	MySQL.jdbc.url=jdbc:mysql://127.0.0.1:3306/svnadmin?characterEncoding=utf-8
	MySQL.jdbc.username=root
	MySQL.jdbc.password=root

启动svnadmin-tomcat
	
	[root@host ~]# /root/svnadmin-tomcat/bin/startup.sh

### 最后 ###
在浏览器中打开地址就可以了
	
	http://ip:9000/svnadmin/

注意：在项目管理中，url路径是不需要带上端口的，我在这里纠结了好久好久，也是醉了

	项目：MiniGame  类型：http(多库)
	路径:/data/svn/MiniGame
    URl:http://ip/svn/MiniGame

----------
追求卓越 成功就会在不经意间追上你