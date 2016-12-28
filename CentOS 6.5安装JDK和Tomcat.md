# ׼���ļ���
jdk-8u51-linux-x64.rpm  
apache-tomcat-7.0.63.tar.gz  

# ��װ��������:
## һ����װJDK

    java -version  
    rpm -ivh /CYMap/install/search/jdk-8u51-linux-x64.rpm  
    java -version  
    cd /usr/java  
    ls  

## ������װTomcat

    rm -rf /usr/local/tomcat7  
    cd /usr  
    cd /usr/local/tomcat7  
    ls  
    cd /  
    tar zxvf /CYMap/install/search/apache-tomcat-7.0.63.tar.gz  
    mv /apache-tomcat-7.0.63 /usr/local/tomcat7  
    chmod +x /usr/local/tomcat7  

## �������û�������

�޸�profile

    vim /etc/profile
��ӻ���������������

	#jdk config
	export JAVA_HOME=/usr/java/jdk1.8.0_51
	export CALSSPATH=$JAVA_HOME/lib/*.*  
	#tomcat config
	export TOMCAT_HOME=/usr/local/tomcat7
	export CATALINA_HOME=/usr/local/tomcat7
	#path config
	export PATH=$PATH:$JAVA_HOME/bin:$TOMCAT_HOME/bin

ˢ�»�������

    source /etc/profile

## �ġ�����Tomcat

    sh /usr/local/tomcat7/bin/startup.sh  
�ڻ��������д���ַhttp://localhost:8080����  

�ر�Tomcat����  

    sh /usr/local/tomcat7/bin/shutdown.sh
## �塢��������Tomcat�ű�
### ���ɽű��ļ�
ִ��ָ�� vi /etc/rc.d/init.d/tomcat�������������

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
	CATALINA_HOME="/usr/local/tomcat7" #tomcat��װĿ¼
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


### ���Ȩ��
ʹ�ýű��ļ���ִ��  

    chmod 755 /etc/rc.d/init.d/tomcat
����ӵ�������  

    chkconfig --add /etc/rc.d/init.d/tomcat  
�༭�ļ�  

    vim /usr/local/tomcat7/bin/catalina.sh  
�ļ��м���������䣺  

	#auto startup tomcat config
	export JAVA_HOME=/usr/java/jdk1.8.0_51
	export CATALINA_HOME=/usr/local/tomcat7
	export CATALINA_BASE=/usr/local/tomcat7
	export CATALINA_TMPDIR=/usr/local/tomcat7/temp
 

## ����tomcat����

    service tomcat start
## ֹͣtomcat����

    service tomcat stop