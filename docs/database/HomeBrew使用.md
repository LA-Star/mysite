# HomeBrew使用

[TOC]

## 文章推荐

> https://sspai.com/post/56009#toc_7



## Homebrew 的几个核心概念

| 词汇        | 含义                                                         |
| :---------- | :----------------------------------------------------------- |
| formula (e) | 安装包的描述文件，formulae 为复数                            |
| cellar      | 安装好后所在的目录                                           |
| keg         | 具体某个包所在的目录，keg 是 cellar 的子目录                 |
| bottle      | 预先编译好的包，不需要现场下载编译源码，速度会快很多；官方库中的包大多都是通过 bottle 方式安装 |
| tap         | 下载源，可以类比于 Linux 下的包管理器 repository             |
| cask        | 安装 macOS native 应用的扩展，你也可以理解为有图形化界面的应用。 |
| bundle      | 描述 Homebrew 依赖的扩展                                     |



## 安装

Homebrew 依赖于Xcode Command Line Tools，所以需先执行：
```shell
xcode-select --install
```
在终端中执行：
```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
检查是否已安装成功：
```shell
~ brew -v
Homebrew 2.2.6
Homebrew/homebrew-core (git revision 6f34; last commit 2020-02-19)
```



## 使用代理

> https://www.firfor.cn/articles/2022/04/07/1649304460443.html

```shell

http_proxy=http://127.0.0.1:8889 https_proxy=http://127.0.0.1:8889 brew install python
```



## 基本用法
基于brew安装的所有软件及其依赖均会安装到目录/usr/local/Cellar

### 搜索软件

brew search [关键词]

```shell
brew search nginx
brew search nginx 
```


### 已安装的软件列表

```shell
#查看全部
brew list
#查看具体某个
brew list | grep 'nginx'
#查看某个信息
brew list nginx
```

### 查看软件信息

```shell
brew info nginx
```
### 安装软件
```shell
brew install nginx
```
### 更新brew和安装的软件包
```shell
#更新HomeBrew本身
brew update

#查看所有有更新版本的软件
brew outdated 

#更新所有软件
brew upgrade 

#更新某个软件，更新git
brew upgrade git
```
### 卸载软件包
```shell
#卸载指定
brew uninstall nginx
```
### 服务管理
诸如 Nginx、MySQL 等软件，都是有一些服务端软件在后台运行，如果你希望对这些软件进行管理，可以使用 `brew services` 命令来进行管理

- `brew services list`： 查看所有服务
- `brew services run [服务名]`: 单次运行某个服务
- `brew services start [服务名]`: 运行某个服务，并设置开机自动运行。
- `brew services stop [服务名]`：停止某个服务
- `brew services restart`：重启某个服务。



### 清理软件

Homebrew 用久了，会有一些历史版本的软件遗留在系统里，这个时候，你可以使用 `brew cleanup` 命令来清理系统中所有软件的历史版本，或者可以使用 `brew cleanup [软件名]`来清理特定软件的旧版。

>brew cleanup -n 列出需要清理的内容 
>brew cleanup 清理所有的过时软件 
>brew cleanup [FORMULA] 清理指定软件的过时包
### 查看brew配置信息
>用于查看 brew 所在环境及相关的配置情况
### 诊断问题
>诊断当前 brew 存在哪些问题，并给出解决方案
### 仓库管理
>已安装的仓库列表
>brew tap [--full] user/repo [URL] 添加仓库
>brew untap tap 移除仓库

### 设置brew镜像源
```shell
cd "$(brew --repo)"
git remote set-url origin git://mirrors.ustc.edu.cn/brew.git
```
打开 ~/.bash_profile 文件，添加下列一行
```shell
export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles
```
即时生效
```shell
source ~/.bash_profile
```