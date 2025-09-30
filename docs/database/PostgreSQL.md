# PostgreSQL常用
[toc]

### 重启服务

postgresql的服务管理必须切换到postgres 用户。pg_ctl 用于启动、停止、重启 PostgreSQL 后端服务。

> https://developer.aliyun.com/article/564330
>
> https://www.cnblogs.com/jackyyou/p/5685502.html

```shell
#启动服务
sudo -u postgres /Library/PostgreSQL/13/bin/pg_ctl -D /Library/PostgreSQL/13/data start

#停止服务
sudo -u postgres /Library/PostgreSQL/13/bin/pg_ctl -D /Library/PostgreSQL/13/data stop
```

查看所有 postgres 进程：终端并输入：

```shell
ps aux | grep postgres
```

### 配置postgresql的pg_hba.conf

默认路径/Library/PostgreSQL/13/data/，但是mac这个data进不去需要权限，这个权限不是root用户权限，而是postgres用户才有进这个文件夹的权限，需要输入下面这个命令
sudo -u postgres bash切换postgres用户然后可以进去vim  pg_hba.conf

> https://blog.csdn.net/m0_56203837/article/details/120854368

### 启用扩展插件

```shell
#创建扩展插件
create postgis;
create extension pgrouting;
create extension postgis_topology;

#查看数据库版本
select postgis_full_version(),pgr_version();

#查询数据坐标系信息
select st_srid(geom) from road_hn;
```

### 数据库备份与还原
```sql
# pg_dump命令参数详情
# http://postgres.cn/docs/9.6/app-pgdump.html
# windows 环境为pg_dump.exe,linux为pg_dump
#备份
pg_dump.exe -E UTF8 -U postgres -d test -w > C:/test.bak
#恢复
#psql 也可以恢复pg_dump备份的sql文件
psql.exe -U postgres -d test < C:/test.bak
```

### 备份数据表

```shell
#以管理员身份运行命令行，定位到\Program Files\PostgreSQL\12\bin> 目录。
#备份表结构：
pg_dump -E UTF8 -U postgres -s -t building xsqsz > d:\building.sql
#备份表结构和数据
pg_dump -E UTF8 -U postgres -t building xsqsz > d:\building1.sql
#备份多张表
pg_dump -E UTF8 -U postgres -t table1 -t table2  xsqsz > d:\building1.sql
```

### 导入shp数据

```shell
# public.gh1为表的名称
# mp-dg 为数据库名称
shp2pgsql -s 4326 -I "C:/mapdata/online0622/gh.shp" public.gh1 | psql -h localhost -p 23451 -d mp-dg -U postgres -W

#windows环境以管理员身份运行命令行，定位到\Program Files\PostgreSQL\12\bin> 目录。
shp2pgsql.exe -s 4326 -I "C:/Users/21/Desktop/hdq/hdq_cj.shp" public.hdqcj | psql -h 192.168.240.243 -p 5433 -d szsqsz -U postgres -W
```

### 表插入数据

```sql
#从一张表插入另一张表数据
insert into test(geom,id,name) select geom,id,name from region;

#更新数据
update sys_gis_space t1 set geom = t2.geom from region t2 where t1.code = t2.code;
```

### 表的索引与序列

```sql
#查看所有索引
select *  from pg_class where relkind='S'; 

#创建序列
CREATE SEQUENCE public.test_gid_seq INCREMENT 1 MINVALUE 1 MAXVALUE 9999999 START 1 CACHE 1;
ALTER TABLE public.test_gid_seq OWNER TO postgres;

#修改表序列索引
alter sequence gh_id_seq start 1;
```

### 自动生成主键为uuid格式

安装 uuid_generate_v4() 扩展函数：create extension "uuid-ossp"。检验函数：select uuid_generate_v4()。

创建表结构格式：

```sql
CREATE TABLE "public"."car" (
  "id" UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  "pipe_id" varchar(50) COLLATE "pg_catalog"."default",
  "car_id" varchar(8) COLLATE "pg_catalog"."default"
);
```

创建自增id

> https://blog.csdn.net/weixin_42845682/article/details/107111996



### 断开数据库连接

```shell
select pg_terminate_backend(pid)
from pg_stat_activity
where pid <> pg_backend_pid()
and datname = 'zhsqsz'

--断开数据库所有连接
SELECT pg_terminate_backend(pg_stat_activity.pid)
FROM pg_stat_activity
WHERE datname='syd' AND pid<>pg_backend_pid();
```

