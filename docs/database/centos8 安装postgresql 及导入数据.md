# centos8 安装postgresql 及导入数据
[toc]
---
## Postgresql安装
参照官网安装步骤安装
https://www.postgresql.org/download/linux/redhat/；
https://www.cnblogs.com/xingqiang/p/14147549.html；
docker安装postgresql
https://hub.docker.com/r/kartoza/postgis/tags?page=1&ordering=last_updated
postgis安装（官网教程）
https://people.planetpostgresql.org/devrim/index.php?/archives/107-Installing-PostGIS-3.1-and-PostgreSQL-13-on-CentOS-8.html
Centos-8 安装postgresql 13
https://computingforgeeks.com/install-postgresql-13-on-centos-rhel/

## 配置修改
### 修改允许远程连接
```shell
vim /var/lib/pgsql/13/data/pg_hba.conf
#Ipv4 修改成 0.0.0.0/0 trust
```
### 修改postgresql.conf
```shell
listen_address = ‘*’
```
### 修改默认端口
```shell
vim /var/lib/pgsql/13/data/postgresql.conf
Port = 54321
```
### 修改密码
重置密码方法：
1、找打pg_hba.conf配置文件，目录为: /var/lib/pgsql/13/data
2、修改IPv4行的 METHOD的 md5为trust。（这样就可以直接链接数据库，然后修改密码）
3、重启服务，修改完密码后再把值改为md5
### 使用root用户重启服务
```shell
#centos
systemctl stop postgresql-13
systemctl start postgresql-13

#debain
sudo systemctl restart postgresql
sudo systemctl enable postgresql 
```

## postgis安装
https://people.planetpostgresql.org/devrim/index.php?/archives/107-Installing-PostGIS-3.1-and-PostgreSQL-13-on-CentOS-8.html

### 插件安装及配置
如果想要使用shp2pgsql命令导入shp，则需要安装该客户端
https://andrewsarver.com/getting-started-with-postgis-import-shapefiles-shp-files/
找到对应版本的客户端，例如我安装的postgis版本是postgis31_13，则可以通过
dnf search postgis31_13-client 查找是否存在该版本对应的客户端工具，
dnf install -y postgis31_13-client  安装该工具；
使用su - postgres 切换用户到postgres下（root用户不可以使用），输入shp2pgsql命令测试是否安装成功。
### 安装pgrouting
参考http://www.cxyzjd.com/article/ceshell/97560304
查找是否存在pgrouting
yum search pgrouting 
安装pgrouting
yum install pgrouting_13
创建pgrount扩展
在数据库中执行 create extension pgrouting;
查看postgis和pgrouting安装版本
在数据库中执行 select postgis_full_version(),pgr_version();

## 数据导入
官方文档：
https://postgis.net/docs/using_postgis_dbmanagement.html#loading_geometry_data
参考：
https://docs.boundlessgeo.com/suite/1.1.0/dataadmin/pgGettingStarted/shp2pgsql.html
https://blog.crunchydata.com/blog/loading-data-into-postgis-an-overview
https://postgis.net/workshops/postgis-intro/loading_data.html
导入shp
在postgresql安装目录下的bin目录，打开命令行工具（window自带的）
shp2pgsql -I -s 4269 C:\MyData\roads\roads.shp roads | psql -U postgres -d ’数据库名’
例如：
centos操作系统时，切换到postgres账号下

```shell
centos操作系统时，切换到postgres账号下
//切换用户
su - postgres
//执行
shp2pgsql -I -s 4326 /tmp/road_hainan/road_hn.shp road_hn | psql -U postgres -d route

//-p 54321代表数据库端口,-S(大写的)代表把多线、面转成简单要素
shp2pgsql -I -s 4326 -S /tmp/road_hainan/road_hn.shp road_hn | psql -U postgres -d route -p 23451
```

或者使用root用户，定位到pg安装目录下的bin目录
/usr/pgsql-13/bin
执行以下命令
shp2pgsql -I -s 4326 /tmp/road_hainan/road_hn.shp road_hn | psql -U postgres -d route

命令中的参数含义如下：

参数    含义
-s    空间参考标识符（SRID）
-d   重新建立表，并插入数据
-a   在同一个表中增加数据
-c  建立新表，并插入数据(缺省)
-p  只创建表
-g    指定要创建的表的空间字段名称(在追加数据时有用)
-D     使用dump方式，比缺省生成sql的速度快
-G     使用类型geography
-k    保持标识符（列名，模式，属性）大小写。
-i     将所有整型都转为标准的32-bit整数
-I     在几何列上建立GIST索引
-S    生成简单几何，而非MULTI几何
-t    指定几何的维度
-w    指定输出格式为WKT
-W    输入的dbf文件编码方式
-N    指定几何为空时的操作
-n    只导入dbf文件
-T    指定表的表空间
-X    指定索引的表空间
-?    帮助

## 备份数据库
参考：
https://cloud.tencent.com/developer/article/1185186
https://www.postgresql.org/docs/12/app-pgdump.html
https://developer.aliyun.com/article/662509
1.以postgres用户身份登录：
su - postgres

2.备份数据库
文件备份到了/var/lib/pgsql 这个位置，可以修改备份路径
pg_dump --host 127.0.0.1 --port 23451 gisxz > gisxz.bak



备份
pg_dump.exe -E UTF8 -U postgres -d shygjc_20201104_shygjc -w > E:/shygjc_20201104_shygjc.bak
恢复
psql.exe -U postgres -d ehys < C:\Users\86184\Downloads\ehys\ehys.bak