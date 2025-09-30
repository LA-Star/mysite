[toc]
# Linux设置开机自启脚本

## 1、开机执行脚本

新建一个.sh为后缀的脚本，如geoserver_auto.sh，在脚本中填入内容

```shel
#!/bin/sh

#add for chkconfig
#chkconfig: 2345 70 30
#description: start geoserver
#processname: geoserver

#以下内容为要自启的脚本，先定位到脚本所在目录，然后使用sh命令执行脚本
cd /usr/local/geoserver-2.17.5/bin/
sh startup.sh
```

把脚本放到/etc/rc.d/init.d目录下，使用chkconfig命令添加启动脚本

```she
#添加脚本
chkconfig --add geoserver_auto.sh   

#启动脚本
chkconfig geoserver_auto.sh on

#赋予脚本权限
chmod +x  geoserver_auto.sh

#命令重启
reboot 
```

> 参考：
>
> https://www.jianshu.com/p/7bd61fc1de4b

## 2、使用systemctl添加自定义服务

进入到/lib/systemd/system/目录，创建nginx.service文件，并编辑。

```bash
cd /lib/systemd/system/

vim nginx.service

```
文件内容：
```bash
[Unit]
Description=nginx service
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/nginx/sbin/nginx
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

>  Description:描述服务
>  After:描述服务类别
>  [Service]服务运行参数的设置
>  Type=forking是后台运行的形式
>  ExecStart为服务的具体运行命令
>  ExecReload为重启命令
>  ExecStop为停止命令
>  PrivateTmp=True表示给服务分配独立的临时空间
>  注意：[Service]的启动、重启、停止命令全部要求使用绝对路径
>  [Install]运行级别下服务安装的相关设置，可设置为多用户，即系统运行级别为3
>  保存退出。

服务的启动/停止/刷新配置文件/查看状态

```bash
#加入开机自启动
systemctl enable nginx.service

#取消开机自启动
systemctl disable nginx.service

#启动nginx服务
systemctl start nginx.service　

#停止服务
systemctl stop nginx.service　 

 #重新启动服务
systemctl restart nginx.service　

#查看所有已启动的服务
systemctl list-units --type=service  

#查看服务当前状态
systemctl status nginx.service  
```



## 3、Windows开机自启

>https://blog.csdn.net/lwpkjio/article/details/85129507
>
>https://blog.csdn.net/w20101310/article/details/123717964



## 4、pm2开机自启nodejs服务

>https://juejin.cn/s/pm2%20%E5%BC%80%E6%9C%BA%E8%87%AA%E5%90%AF%E5%8A%A8