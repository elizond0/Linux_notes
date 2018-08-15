# 7-Ubuntu软件安装

## 1. 更新Ubuntu国内镜像源

* [清华国内镜像源](https://mirrors.tuna.tsinghua.edu.cn)

* 选择ubuntu版本 => 复制代码块的内容

* 备份Ubuntu默认的源地址 sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup

* 更新源服务器列表 打开sources.list 把复制的内容全选替换

* 更新源 sudo apt-get update

## 2. Ubuntu软件操作的相关命令

* sudo apt-get update  更新源

* sudo apt-get install package 安装包

* sudo apt-get remove package 删除包

* sudo apt-cache search package 搜索软件包

* sudo apt-cache show package  获取包的相关信息，如说明、大小、版本等

* sudo apt-get install package --reinstall   重新安装包

* sudo apt-get -f install   修复安装

* sudo apt-get remove package --purge 删除包，包括配置文件等

* sudo apt-get build-dep package 安装相关的编译环境

* sudo apt-get upgrade 更新已安装的包

* sudo apt-get dist-upgrade 升级系统

* sudo apt-cache depends package 了解使用该包依赖那些包

* sudo apt-cache rdepends package 查看该包被哪些包依赖

* sudo apt-get source package  下载该包的源代码

* sudo apt-get clean && sudo apt-get autoclean 清理无用的包

* sudo apt-get check 检查是否有损坏的依赖