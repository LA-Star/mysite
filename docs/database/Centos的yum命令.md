# yum命令
yum命令

>Yum（全称为Yellow dog Updater, Modified）是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器。 基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装。
>
>RPM是”Redhat Package Manager”的缩写，根据名字也能猜到这是Redhat公司开发出来的。RPM 是以一种数据库记录的方式来将你所需要的套件安装到你的Linux 主机的一套管理程序。

语法：yum [command] [package]，command代表支持的命令 ，package代表要操作的包
```
#安装软件，以安装Nginx为例。更多操作可使用 yum -h命令查看
yum install nginx

yum install https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

- 1.使用YUM查找软件包 
命令：yum search 

- 2.列出所有可安装的软件包 
命令：yum list 

- 3.列出所有可更新的软件包 
命令：yum list updates 

- 4.列出所有已安装的软件包 
命令：yum list installed 

- 5.列出所有已安装但不在 Yum Repository 内的软件包 
命令：yum list extras 

- 6.列出所指定的软件包 
命令：yum list 

- 7.使用YUM获取软件包信息 
命令：yum info 

- 8.列出所有软件包的信息 
命令：yum info 

- 9.列出所有可更新的软件包信息 
命令：yum info updates 

- 10.列出所有已安装的软件包信息 
命令：yum info installed 

- 11.列出所有已安装但不在 Yum Repository 内的软件包信息 
命令：yum info extras 

- 12.列出软件包提供哪些文件 
命令：yum provides

- 13.卸载已安装的软件
命令：yum remove 软件名称