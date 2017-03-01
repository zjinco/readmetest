# 使用PostgreSQL源 
## 设置代理 

    wget http://yum.postgresql.org/9.3/redhat/rhel-6-x86_64/pgdg-centos93-9.3-3.noarch.rpm
## 安装yum源

    rpm -ivh pgdg-centos93-9.3-3.noarch.rpm
## 配置文件  

    vim /etc/yum.repos.d/CentOS-Base.repo
在 [base] 和 [updates] 区域中各加入一行  

    exclude=postgresql\*  
## 加入缓存  

    yum makecache  
# 安装PostgreSQL  
## 执行安装  

    yum list postgres\*  
    yum install postgresql93-server  
注：yum安装PostgreSQL的默认路径为/usr/pgsql-9.3  
# 配置PostgreSQL参数，初始化数据库  
## 创建data文件夹  

    mkdir /usr/pgsql-9.3/data  
## 修改权限  

    chmod 777 -R /usr/pgsql-9.3/
    chgrp -R postgres /usr/pgsql-9.3/
    chown -R postgres /usr/pgsql-9.3/
## 更改用户  

    su postgres  
## 数据库初始化  

    /usr/pgsql-9.3/bin/initdb -D /usr/pgsql-9.3/data
# 使用EPEL源  
## 切换为root用户  

    su root  
## 设置代理  

    wget http://dl.fedoraproject.org/pub/epel/6/x86\_64/epel-release-6-8.noarch.rpm  
    wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm  
## 安装yum源  

    rpm -ivh epel-release-6-8.noarch.rpm  
    rpm -ivh remi-release-6.rpm  
# 安装POSTGIS  

    yum list postgis\*  
    yum install postgis2\_93.x86\_64 postgis2\_93-client.x86\_64pos postgis2\_93-devel.x86\_64 postgis2\_93-docs.x86\_64 postgis2\_93-utils.x86\_64  
# 加载POSTGIS插件  

    su postgres  
    cd /  
    /usr/pgsql-9.3/bin/pg_ctl -D /usr/pgsql-9.3/data start  
    psql  
    create database postgis;  
    \c postgis;  
    create extension postgis;  
    \q  
    cd /usr/pgsql-9.3/share/contrib/postgis-2.1/  
    psql -d postgis -f spatial_ref_sys.sql  
# 设置POSTGRESQL服务开机启动  
## 切换为root用户  

    su root  
## 进入postgresql\_start.sh存放目录  
## 拷贝postgresql\_start.sh到/etc/rc.d/init.d/  

    cp postgresql_start.sh /etc/rc.d/init.d/postgresql  
## 权限设置  

    chown root.root /etc/rc.d/init.d/postgresql  
    chmod 755 /etc/rc.d/init.d/postgresql  
    chkconfig --add postgresql
