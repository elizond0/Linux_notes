# 文件的基本管理和XFS文件系统备份恢复

## 1.Linux系统目录结构

* 在WIN中，它是多根的，每一个盘符就是一个根节点；Linux只有一个根目录 / ,使用tree命令查看linux目录结构，这个命令默认是没有安装的，需要手工安装一下
1. mount /dev/sr0  /media/
2. rpm -ivh /media/Packages/tree-1.6.0-10.el7.x86_64.rpm
3. tree  /tmp/

* 根下的目录作用说明：
1. / --> 处于linux系统树形结构的最顶端，它是linux文件系统的入口，所有的目录、文件、设备都在 / 之下
2. /bin --> bin是Binary的缩写。常用的二进制命令目录。比如 ls、cp、mkdir、cut等；和/usr/bin类似，一些用户级gnu工具
3. /boot --> 存放的系统启动相关的文件，例如：kernel.grub(引导装载程序)
4. /dev --> dev是Device的缩写。设备文件目录，比如声卡、磁盘……在Linux中一切都被看做文件。终端设备、磁盘等等都被看做文件。设备文件: /dev/sda,/dev/sda1,/dev/tty1,/dev/tty2,/dev/pts/1, /dev/zero, /dev/null, /dev/cdrom
5. /etc --> 常用系统及二进制安装包配置文件默认路径和服务器启动命令目录。passwd 用户信息文件；shadow  用户密码文件；group 存储用户组信息；fstab 系统开机启动自动挂载分区列表；hosts 设定用户自己的IP与主机名对应的信息
6. /home --> 普通用户的家目录默认存放目录
7. /lib --> 库文件存放目录,函数库目录
8. /mnt和/media --> 一般用来临时挂载存储设备的挂载目录，比如有cdrom、U盘等目录，在CENTOS7中会挂载到/run下面
9. /opt --> 表示的是可选择的意思，有些软件包也会被安装在这里
10. /proc --> 操作系统运行时，进程（正在运行中的程序）信息及内核信息（比如cpu、硬盘分区、内存信息等）存放在这里。/proc目录是伪装的文件系统proc的挂载目录，proc并不是真正的文件系统。因此，这个目录是一个虚拟的目录，它是系统内存的映射，可以通过直接访问这个目录来获取系统信息。也就是说，这个目录的内容不在硬盘上而是在内存里。查看CPU信息 cat /proc/cpuinfo
11. /sys --> 系统目录，存放硬件信息的相关文件
12. /run --> 运行目录，存放的是系统运行时的数据，比如进程的PID文件
13. /srv --> 服务目录，存放的是我们本地服务的相关文件
14. /sbin --> 大多数涉及系统管理的命令都存放在该目录中，它是超级权限用户root的可执行命令存放地，普通用户无权限执行这个目录下的命令，凡是目录sbin中包含的命令都是root权限才能执行的
15. /tmp --> 该目录用于存放临时文件，有时用户运行程序的时候，会产生一些临时文件。/tmp就是用来存放临时文件的。/var/tmp目录和该目录的作用是相似的,不能存放重要数据，它的权限比较特殊>sticky位表示这个目录里的文件只能被owner和root删除
16. /var --> 系统运行和软件运行时产生的日志信息，该目录的内容是经常变动的，存放的是一些变化的文件。比如/var下有/var/log目录用来存放系统日志的目录，还有mail、/var/spool/cron
17. /usr --> 存放应用程序和文件，/usr/bin 普通用户使用的应用程序;/usr/sbin 管理员使用的应用程序;/usr/lib 库文件Glibc(32位);/usr/lib64 库文件Glibc
18. /usr/lib 库文件Glibc(32位)和/usr/lib64 库文件Glibc --> 目录里存放着系统最基本的动态链接共享库，包含许多被/bin/和/sbin/中的程序使用的库文件，目录/usr/lib/中含有更多用于用户程序的库文件。作用类似于windows里的DLL文件，几乎所有的应用程序都需要用到这些共享库。lib***.a是静态库在编译时被加载到二进制文件中，lib***.so是动态库在运行时加载到进程的内存空间中。

## 2.文件的管理-创建/修改/移动/删除

* touch常用来创建空文件,如果文件存在，则修改这个文件的时间，文件的三种时间(最近访问atime,最近更改mtime,最近改动ctime)
1. touch file1 file2 创建多个文件
2. touch file{6..20} 创建file6到file20的文件
3. touch -d "20181019 21:30" test.txt 创建一个带虚假创建时间的文件

* vim和echo重定向创建一个新文件
1. vim 2222.txt
2. echo  aaa > 333.txt 追加aaa内容到333.txt

* mkdir创建目录
1. mkdir dir2 dir3 /home/dir4
2. mkdir -p /tmp/a/b/c  #在创建一个目录的时候，如果这个目录的上一级不存在的话，要加参数-p

* rm 可以删除一个目录中的一个或多个文件或目录，对于链接文件，只是删除整个链接文件，而原文件保持不变的
1. -f  强制删除，没有提示
2. -r  删除目录
3. rm -rf  (慎用,一定要在删除以前确定一下所在目录，防止误删除重要数据)

* cp 复制文件
1. 命令：cp  源文件/目录   目录文件/目录 # cp /etc/passwd /opt/   #复制文件
2. 选项：-R/r：递归处理，将指定目录下的所有文件与子目录一并处理 # cp -r /boot/grub /opt/  #复制目录

* mv 移动文件/重命名 mv b.txt dir1/a.txt   #在移动文件的时候支持改名操作

## 3.查看文件

* cat查看文件内容
1. 一次显示整个文件的内容 cat /etc/passwd
2. cat可查看多个文件的内容 cat 1.txt 2.txt > new.txt 可以实现合并多个文件内容

* more以分页形式显示文件内容，按下回车刷新一行，按下空格刷新一屏，输入q键退出 more /etc/passwd

* less与more类似，但支持前后翻滚和翻页

* head用于显示文件的开头的内容。在默认情况下，head命令显示文件的头10行内容
1. 语法:head(选项)文件名  # head /etc/passwd
2. 参数: -n 显示从文件头开始的行数 # head -n 3 /etc/passwd #显示前3行

* tail用于显示文件中的尾部内容。默认在屏幕上显示指定文件的末尾10行
1. 语法:tail (选项)文件名   
2. -n 显示文件尾部多少行的内容(n为数字)  tail -n 3 /var/log/secure  #查看最后3行记录
3. -f 动态显示数据（不关闭）,常用来查看日志   tail -f /var/log/secure   #在一个终端执行此命令动态查看文件内容
4. ssh root@192.168.1.80在另一个终端远程登录Linux，登录成功后，在前一个终端可以动态看到登陆成功的日志

* grep文件内容搜索命令
1. 语法：grep "关键字" xxx.txt(搜索的文档) 单词匹配
2. -n 可以显示行号
3. -v 显示不包含关键字的内容
4. “^关键字$” '^'和'$'的作用和正则中的用法相同，匹配行首和行尾

* find在指定路径里找文件
1. 语法：find [路径] [选项(-name文件名/-size大小/-perm权限)] [值]
2. find ./ -name test.sh  =>  查找当前目录下所有全名为test.sh的文件
3. find ./ -name '*.sh'  =>  查找当前目录下所有后缀为.sh的文件，可以使用正则表达式
4. find /tmp -size 2M  =>  查找/tmp目录下等于2M的文件
5. find /tmp -size +2M -size -5M  =>  查找/tmp目录下大于2M且小于5M的文件
6. find /tmp -perm 777  =>  查找/tmp目录下权限为777的文件

## 4.xfs文件系统的备份和恢复

### 4.1 xfs文件系统简介

* centos7选择xfs格式作为默认文件系统，而且不再使用以前的ext，仍然支持ext4，xfs专为大数据产生，每个单个文件系统最大可以支持8eb，单个文件可以支持16tb，不仅数据量大，而且扩展性高。还可以通过xfsdump，xfsrestore来备份和恢复。XFS提供了 xfsdump 和 xfsrestore 工具协助备份XFS文件系统中的数据。xfsdump 按inode顺序备份一个XFS文件系统。

* 与传统的UNIX文件系统不同，XFS不需要在备份前被卸载；对使用中的XFS文件系统做备份就可以保证镜像的一致性。XFS的备份和恢复的过程是可以被中断然后继续的，无须冻结文件系统。xfsdump 甚至提供了高性能的多线程备份操作——它把一次dump拆分成多个数据流，每个数据流可以被发往不同的目的地

* xfsdump的备份级别有两种: 0 - 完全备份 ; 1~9 : 增量备份
1. 完全备份：每次都把指定的备份目录完整的复制一遍，不管目录下的文件有没有变化；
2. 增量备份：每次将之前（第一次、第二次、直到前一次）做过备份之后有变化的文件进行备份；
3. 差异备份：每次都将第一次完整备份以来有变化的文件进行备份。

### 4.2 准备备份环境

* 对新添加的硬盘进行格式化并挂载的步骤：
1. 虚拟机设置中使用默认配置添加硬盘
2. 指定分区的设备：# fdisk /dev/sdb
3. 依次输入以下命令：命令(输入 m 获取帮助)：n (创建一个新的分区) => Select (default p): p (主要分区) => 扇区： +1G (指定分区大小) => 命令(输入 m 获取帮助)：p (打印分区表) => 命令(输入 m 获取帮助)：w (保存；配置)
4. 查看新增的硬盘分区(/dev/sdb1)：# ls /dev/sdb*
5. 格式化分区：# mkfs.xfs /dev/sdb1
6. 挂载分区：# mkdir /sdb1 ; # mount /dev/sdb1 /sdb1
7. 准备测试文件：# cd /sdb1/ => # cp /etc/passwd ./ => # mkdir test => # touch test/a => # tree /sdb1/

### 4.3 备份

* 备份整个分区 (类似是虚拟机的快照，服务器被黑后，进行快速恢复)
1. 语法：xfsdump -f 备份存放位置 要备份路径或设备文件 (注意：备份的路径这里不能写成/sdb1/。  可以是/dev/sdb1 或/sdb1)
2. please enter label for this dump session -> dump_sdb1  (指定备份会话标签)
3. please enter label for media in drive 0  -> sdb1  (指定设备标签，就是对要备份的设备做一个描述)

* 指定备份时免交互操作，方便后期做定时备份
1. xfsdump -f /opt/dump_passwd /sdb1 -L dump_passwd -M media1
2. -L ：xfsdump 记录每次备份的 session 标头，这里可以填写针对此文件系统的简易说明
3. -M ：xfsdump 可以纪录储存媒体的标头，这里可以填写此媒体的简易说明

* 指定只备份分区中某个目录
1. xfsdump -f /opt/dump_grub2 -s grub2/grub.cfg /boot -L dump_grub2 -M boot-sda1
2. 参数：-s 文件路径  (只对指定的文件进行备份，-s指定时，路径写的是相对路径，-s可以是文件或目录)

* 查看备份信息与内容 : xfsdump  -I（字母大写i） 可以在/var/lib/xfsdump/inventory目录下看到生成的档案信息

### 4.4 恢复

* 语法：xfsrestore -f 指定恢复文件的位置 指定存放恢复后的文件的路径
1. xfsrestore -f /opt/dump_sdb1  /sdb1
2. xfsdump 不支持没有挂载的文件系统备份！所以只能备份已挂载的
3. xfsdump 必须使用 root 的权限才能操作 (涉及文件系统的关系)
4. xfsdump 只能备份 XFS 文件系统
5. xfsdump 备份下来的数据 (档案或储存媒体) 只能让 xfsrestore 解析
6. xfsdump 是透过文件系统的 UUID 来分辨各个备份档的，因此不能备份两个具有相同 UUID 的文件系统

### 4.5 增量备份文件系统

* 增量备份步骤：
1. 首先要进行全备 xfsdump -f /opt/test-full /sdb1 -L test-full -M media0
2. 进行文件新增，然后第一次增量备份 xfsdump -l 1 -f /opt/test-back1 /sdb1 -L test-bak1 -M media0
3. 再次增加内容，然后进行level2的增量备 xfsdump -l 2 -f /opt/test-back2 /sdb1 -L test-bak2 -M media0

* 增量备份恢复步骤：
1. 先恢复完全备份
2. 情况1: 恢复最后一次增量备份（如果两次增量备份都是1级的，所以只需要恢复最后一个增量就可以了。
3. 情况2：如果做的是第一次是1级备，第二次是2级备，那么在恢复的时候就需要先恢复完全备份，然后是1级备，最后是2级备）
4. 总之就是必须按照备份级别 0~9 依次恢复 ，相同等级的备份级别只需要恢复最后一次即可

## 5.文件归档管理

* tar打包与解包，操作之后文件不会被删除，并且不会被压缩
1. 打包：tar -cvf test.tar *.py  =>  把所有py后缀的文件打包至test.tar文件
2. 解包：tar -xvf test.tar  =>  解包test.tar
3. -c 生成档案文件，创建打包文件
4. -v 列出归档/解档的详细过程，显示进度
5. -f 指定档案文件名称，f后面一定是.tar文件，所以必须放选项之后
6. -t 列出档案中包含的文件
7. -x 解开档案文件
8. -z 压缩

* gz模式压缩与解压(体积最小)
1. 压缩：tar -zcvf test.tar.gz *.py  =>  把所有py后缀的文件打包并压缩至test.tar文件
2. 解压：tar -zxvf test.tar.gz  =>  解压test.tar
3. 解压至目录：tar -zxvf test.tar.gz -C ./test

* bz2模式压缩与解压
1. 压缩：tar -jcvf test.tar.bz2 *.py
2. 解压：tar -jxvf test.tar.bz2

* zip模式压缩与解压
1. 压缩：zip test.zip *.py
2. 解压：unzip test.zip
3. 解压至目录：unzip -d ./test test.zip

* which查找命令路径 which [命令]