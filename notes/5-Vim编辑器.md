# 5-Vim编辑器

## 1. vim主要模式介绍

* 查询命令安装位置：rpm -qf `which vim` ； rpm -qf `which vi`。vim是vi的增加版，最明显的区别就是vim可以语法加亮，它完全兼容vi

* 主要模式有4种
1. 命令模式  默认
2. 扩展命令模式 ':'进入，'enter'退出
3. 编辑模式，文件底部会出现Insert字样 'i'进入，'esc'退出
4. 虚拟编辑模式 ，文件底部会出现Visual字样 'v'切换进出
