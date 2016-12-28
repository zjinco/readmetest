# 准备文件：
jdk-8u51-linux-x64.rpm  
apache-tomcat-7.0.63.tar.gz  

# 安装步骤如下:
## 一、安装JDK

    java -version  
    rpm -ivh /CYMap/install/search/jdk-8u51-linux-x64.rpm  
    java -version  
    cd /usr/java  
    ls  

## 二、安装Tomcat

    rm -rf /usr/local/tomcat7  
    cd /usr  
    cd /usr/local/tomcat7  
    ls  
    cd /  
    tar zxvf /CYMap/install/search/apache-tomcat-7.0.63.tar.gz  
    mv /apache-tomcat-7.0.63 /usr/local/tomcat7  
    chmod +x /usr/local/tomcat7  

## 三、设置环境变量

修改profile

    vim /etc/profile
添加环境变量代码如下

	#jdk config
	export JAVA_HOME=/usr/java/jdk1.8.0_51
	export CALSSPATH=$JAVA_HOME/lib/*.*  
	#tomcat config
	export TOMCAT_HOME=/usr/local/tomcat7
	export CATALINA_HOME=/usr/local/tomcat7
	#path config
	export PATH=$PATH:$JAVA_HOME/bin:$TOMCAT_HOME/bin

刷新环境变量

    source /etc/profile

## 四、启动Tomcat

    sh /usr/local/tomcat7/bin/startup.sh  
在火狐浏览器中打开网址http://localhost:8080测试  

关闭Tomcat命令  

    sh /usr/local/tomcat7/bin/shutdown.sh
## 五、开机启动Tomcat脚本
### 生成脚本文件
执行指令 vi /etc/rc.d/init.d/tomcat，添加内容如下

	#!/bin/bash

	# /etc/rc.d/init.d/tomcat
	# init script for tomcat precesses
	
	# processname: tomcat
	# description: tomcat is a j2se server
	# chkconfig: 2345 86 16
	# description: Start up the Tomcat servlet engine.

	if [ -f /etc/init.d/functions ]; then
	. /etc/init.d/functions
	elif [ -f /etc/rc.d/init.d/functions ]; then
	. /etc/rc.d/init.d/functions
	else
	echo -e "\atomcat: unable to locate functions lib. Cannot continue."
	exit -1
	fi
	RETVAL=$?
	CATALINA_HOME="/usr/local/tomcat7" #tomcat安装目录
	case "$1" in
	start)
	if [ -f $CATALINA_HOME/bin/startup.sh ];
	then
	echo $"Starting Tomcat"
	$CATALINA_HOME/bin/startup.sh
	fi;;
	stop)
	if [ -f $CATALINA_HOME/bin/shutdown.sh ];
	then
	echo $"Stopping Tomcat"
	$CATALINA_HOME/bin/shutdown.sh
	fi;;
	*)
	echo $"Usage: $0 {start|stop}"
	exit 1;;
	esac
	exit $RETVAL


### 添加权限
使得脚本文件可执行  

    chmod 755 /etc/rc.d/init.d/tomcat
将其加到服务中  

    chkconfig --add /etc/rc.d/init.d/tomcat  
编辑文件  

    vim /usr/local/tomcat7/bin/catalina.sh  
文件中加入以下语句：  

	#auto startup tomcat config
	export JAVA_HOME=/usr/java/jdk1.8.0_51
	export CATALINA_HOME=/usr/local/tomcat7
	export CATALINA_BASE=/usr/local/tomcat7
	export CATALINA_TMPDIR=/usr/local/tomcat7/temp
 

## 启动tomcat服务

    service tomcat start
## 停止tomcat服务

    service tomcat stop