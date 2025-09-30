# CentOS 8 上安装 MySQL 8.0
## 安装
以 root 或者其他有 sudo 权限的用户身份，通过使用 CentOS 包管理器来安装 MySQL 8.0 服务器：
```shell
sudo dnf install @mysql

#@mysql模块会安装 MySQL 和所有依赖安装包。
#一旦安装完成，启动 MySQL 服务并且启用开机启动功能，运行下面的命令：
sudo systemctl enable --now mysqld

#检查 MySQL 服务器是否正在运行，输入：
sudo systemctl status mysqld
# ● mysqld.service - MySQL 8.0 database server
#   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
#   Active: active (running) since Thu 2019-10-17 22:09:39 UTC; 15s ago
#   ...
```
## 保护 MySQL
运行mysql_secure_installation脚本，执行一些安全相关的操作，并且设置 MySQL root 用户密码：
```shell
sudo mysql_secure_installation
```
你将会被问到配置VALIDATE PASSWORD PLUGIN,这个插件是用来测试 MySQL 用户的密码强度和提高安全性的。有三个密码安全级别，弱，中等，强。如果你不想设置密码验证插件，请直接按Enter回车。
在下一个被提示的地方，你会被问到给 MySQL root 用户设置密码。一旦你操作完成，脚本将会要求你移除匿名用户，限制 root 用户访问本地机器，移除 test 测试数据库。你对于所有的问题都应该回到”Y“（yes）。
为了通过终端命令行与 MySQL 数据库交互，使用已经安装的 MySQL 客户端工具。测试 root 用户访问，输入：
```shell
mysql -u root -p
#当被提示的时候，输入 root 用户密码，MySQL shell 将会展示如下：
#Welcome to the MySQL monitor.  Commands end with ; or \g.
#Your MySQL connection id is 12
#Server version: 8.0.17 Source distribution
```
就这些，你已经安装并且保护了在你的 CentOS 8 服务器上的 MySQL 8.0，你可以使用它了。

## 用户验证
CentOS 8 源仓库中的 MySQL 8.0 被设置采用古老的 mysql_native_password用户验证插件，因为 CentOS 8 上的一些客户端工具和库不兼容caching_sha2_password这个 标准 MySQL 8.0 默认采用的方法。
mysql_native_password在大部分设置中都没问题。如果你想将默认的用户验证插件修改为更快更安全的caching_sha2_password，打开下面的配置文件：
```shell
sudo vim /etc/my.cnf.d/mysql-default-authentication-plugin.cnf
#将默认的default_authentication_plugin修改为caching_sha2_password:
#[mysqld]
#default_authentication_plugin=caching_sha2_password
#关闭并且保存文件，同时重启 MySQL 服务器，使修改生效：
sudo systemctl restart mysqld
```
## 配置远程登录

接着继续执行mysql语句，将将root用户的host字段设为'%'：
```shell
use mysql;
update user set host='%' where user='root';
flush privileges;
#设置完成后输入exit退出mysql，回到终端shell界面，接着开启系统防火墙的3306端口：
sudo firewall-cmd --add-port=3306/tcp --permanent
sudo firewall-cmd --reload
```

## 修改默认端口
//定位
cd /etc/my.cnf.d
编辑文件
vim mysql-server.cnf
//添加端口配置
[mysqld]
#省略其他配置
……….
 #端口配置
port = 60333
参考：
https://cloud.tencent.com/developer/article/1626795
https://www.cnblogs.com/kasnti/p/11929030.html