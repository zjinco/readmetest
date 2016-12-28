# 概述
POI搜索服务主要包括陆图关键字搜索，海图关键字搜索，陆图热区，海图热区的服务与数据部署。
# 依赖环境安装
## 系统确认
服务可以支持以下系统

| 序号 | 系统名称 | 版本 | 说明 |
| --- | --- | --- | --- |
| 1 | CentOS | 6.5 |   |
## 服务确认

| 序号 | 依赖服务 | 版本 | 安装参考命令 |
| --- | --- | --- | --- |
| 1 | Apache安装 | 2.2 | yum install httpd |
| 2 | PostgreSQL | 9.3 | 参见PostgreSQL安装文档 |
| 3 | postgis | 2.1 | 参见PostgreSQL安装文档 |
| 4 | Java runtime | 1.7 | 参见jdk与tomcat部署教程 |
| 5 | Tomcat | 7 | 参见jdk与tomcat部署教程 |
| 6 | 8080端口开启 |   | 注1 |

注1：  

    /sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT  
    /etc/rc.d/init.d/iptables save  
    service iptables restart  

# 搜索服务安装

## Apache服务

拷贝libxxngc.so 到 /usr/lib  

    cp /HDMap1.0/install/search/libxxngc.so /usr/lib/

拷贝libpoi\_search.so到  /usr/lib64  

    cp /HDMap1.0/install/search/libpoi\_search.so /usr/lib64/

## Tomcat服务

删除&lt;Tomcat\_Root\_Path&gt;/webapps/ROOT 中的所有内容  

    rm /usr/local/tomcat7/webapps/ROOT  
拷贝MapApi.war 到 &lt;Tomcat\_Root\_Path&gt;/webapps中  

    cp /HDMap1.0/install/search/MapApi.war /usr/local/tomcat7/webapps

# 数据部署
## 陆图：
拷贝附件中的data目录到指定目录中，本指南使用/HDMap1.0/searchdata/land
## 海图：
将数据导入PostgreSQL数据库。
以数据文件【sea_poi.sql】为例，该文件存放在在/HDMap1.0/searchdata/sea
执行以下命令：

    psql -U postgres postgis < /HDMap1.0/searchdata/sea/sea_poi.sql

# 配置文件修改
## Tomcat配置
打开&lt;Tomcat\_Root\_Path&gt;/conf/server.xml文件
在Host节点内添加下面内容

    <Context path="/" docBase="MapApi" reloadable="true" />

在Connector节点中添加 URIEncoding=&quot;UTF-8&quot; 属性

    <Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" URIEncoding="UTF-8" />
  

## MapApi配置
以安装路径为/HDMap1.0/，数据存放路径为/HDMap1.0/searchdata/land/为例，修改如下：
打开&lt;Tomcat\_Root\_Path&gt;/webapps/MapApi/WEB-INF/classes/META-INF/app.properties

修改以下内容：

    limit.max=500 # 数据查询最大值
    limit.max.address=100 # 数据查询最大值(地址)
    path.log=/HDMap1.0/log/gisserver.log # 日志文件  
    path.poidata=/HDMap1.0/searchdata/land/ # 数据文件目录
    path.poidiffdata= # 差异文件目录(暂时为空)
    path.adddata=/HDMap1.0/searchdata/land/addr.db # 地址库
    path.districtdata=/HDMap1.0/searchdata/land/districtblock.db # 行政区划块
    path.poitypedata=/HDMap1.0/searchdata/land/poitype.xml #类型
    path.geometrydata=/HDMap1.0/searchdata/land/admin3.json # GeoMetry数据  
    type.poimatch=2 #

# 部署测试

**重启Tomcat**
**陆图测试参考链接：**
http://[IP]:8080/v1/poi?query=%E5%AD%A6%E6%A0%A1&amp;region=北京市
**海图测试参考链接：**  
http://[IP]:8080/v1/poi?query=%E9%94%9A%E5%9C%B0&amp;location=39.99188,116.90957&amp;radius=320000&amp;type=1