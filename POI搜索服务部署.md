# ����
POI����������Ҫ����½ͼ�ؼ�����������ͼ�ؼ���������½ͼ��������ͼ�����ķ��������ݲ���
# ����������װ
## ϵͳȷ��
�������֧������ϵͳ

| ��� | ϵͳ���� | �汾 | ˵�� |
| --- | --- | --- | --- |
| 1 | CentOS | 6.5 |   |
## ����ȷ��

| ��� | �������� | �汾 | ��װ�ο����� |
| --- | --- | --- | --- |
| 1 | Apache��װ | 2.2 | yum install httpd |
| 2 | PostgreSQL | 9.3 | �μ�PostgreSQL��װ�ĵ� |
| 3 | postgis | 2.1 | �μ�PostgreSQL��װ�ĵ� |
| 4 | Java runtime | 1.7 | �μ�jdk��tomcat����̳� |
| 5 | Tomcat | 7 | �μ�jdk��tomcat����̳� |
| 6 | 8080�˿ڿ��� |   | ע1 |

ע1��  

    /sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT  
    /etc/rc.d/init.d/iptables save  
    service iptables restart  

# ��������װ

## Apache����

����libxxngc.so �� /usr/lib  

    cp /HDMap1.0/install/search/libxxngc.so /usr/lib/

����libpoi\_search.so��  /usr/lib64  

    cp /HDMap1.0/install/search/libpoi\_search.so /usr/lib64/

## Tomcat����

ɾ��&lt;Tomcat\_Root\_Path&gt;/webapps/ROOT �е���������  

    rm /usr/local/tomcat7/webapps/ROOT  
����MapApi.war �� &lt;Tomcat\_Root\_Path&gt;/webapps��  

    cp /HDMap1.0/install/search/MapApi.war /usr/local/tomcat7/webapps

# ���ݲ���
## ½ͼ��
���������е�dataĿ¼��ָ��Ŀ¼�У���ָ��ʹ��/HDMap1.0/searchdata/land
## ��ͼ��
�����ݵ���PostgreSQL���ݿ⡣
�������ļ���sea_poi.sql��Ϊ�������ļ��������/HDMap1.0/searchdata/sea
ִ���������

    psql -U postgres postgis < /HDMap1.0/searchdata/sea/sea_poi.sql

# �����ļ��޸�
## Tomcat����
��&lt;Tomcat\_Root\_Path&gt;/conf/server.xml�ļ�
��Host�ڵ��������������

    <Context path="/" docBase="MapApi" reloadable="true" />

��Connector�ڵ������ URIEncoding=&quot;UTF-8&quot; ����

    <Connector port="8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443" URIEncoding="UTF-8" />
  

## MapApi����
�԰�װ·��Ϊ/HDMap1.0/�����ݴ��·��Ϊ/HDMap1.0/searchdata/land/Ϊ�����޸����£�
��&lt;Tomcat\_Root\_Path&gt;/webapps/MapApi/WEB-INF/classes/META-INF/app.properties

�޸��������ݣ�

    limit.max=500 # ���ݲ�ѯ���ֵ
    limit.max.address=100 # ���ݲ�ѯ���ֵ(��ַ)
    path.log=/HDMap1.0/log/gisserver.log # ��־�ļ�  
    path.poidata=/HDMap1.0/searchdata/land/ # �����ļ�Ŀ¼
    path.poidiffdata= # �����ļ�Ŀ¼(��ʱΪ��)
    path.adddata=/HDMap1.0/searchdata/land/addr.db # ��ַ��
    path.districtdata=/HDMap1.0/searchdata/land/districtblock.db # ����������
    path.poitypedata=/HDMap1.0/searchdata/land/poitype.xml #����
    path.geometrydata=/HDMap1.0/searchdata/land/admin3.json # GeoMetry����  
    type.poimatch=2 #

# �������

**����Tomcat**
**½ͼ���Բο����ӣ�**
http://[IP]:8080/v1/poi?query=%E5%AD%A6%E6%A0%A1&amp;region=������
**��ͼ���Բο����ӣ�**  
http://[IP]:8080/v1/poi?query=%E9%94%9A%E5%9C%B0&amp;location=39.99188,116.90957&amp;radius=320000&amp;type=1