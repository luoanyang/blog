---
title: Linux一些使用
date: 2019-07-22 21:10:18
tags: linux
categories: 后端
---
## 常用命令行
1. ls (查看当前路径下的文件，dir 命令也有同样的效果)
2. ls -l (查看当前路径下的文件，显示更多的信息)
3. ls-a (查看当前路径下的文件，显示隐藏文件)
4. cd 目录名 (切换路径)
5. mkdir 目录名 (创建文件夹)
6. cp 要复制的文件 复制到的目录/复制后的文件名称 (复制文件)
7. cp -R 要复制的目录 复制到的目录 (复制目录)
8. pwd (查看当前目录的完整链接)
9. rm 删除的文件名 (删除文件)
10. rm -r 删除的目录名 (删除目录)

## 重要的命令行
### 行编辑器 vi/vim
### 网络命令 ifconfig 、ip命令
  - ss  (获取socket统计信息)
  - netstat  (命令用于显示各种网络相关信息)
  - lsof (查找谁在使用文件系统)

### 命令行下载命令 curl、wget
### 远程复制 scp
从本地复制到远程:
复制文件： **scp local_file remote_username@remote_ip:remote_folder**
```
scp /home/space/music/1.mp3 root@www.runoob.com:/home/root/others/music 
```
复制目录：**scp -r local_folder remote_username@remote_ip:remote_folder**
```
scp -r /home/space/music/ root@www.runoob.com:/home/root/others/
```
### 服务管理命令 systemctl
- systemctl enable [服务名] 启用服务
- systemctl disable [服务名] 禁用服务

### wget -c url 断点续传

### 压缩和解压 （tar）

**参数**
- x：解压
- t：查看内容
- u：更新原压缩包里的文件
- c：建立压缩档案
- r：向压缩文件末位追加文件
- z：有gzip属性的
- Z：有compress属性的
- j：有bz2属性的
- O：将文件解压到标准输出
- v：显示所有过程细节

**压缩和解压**
- tar -cf packmd.tar *.md
  此命令将所有md文件达成一个名为packmd.tar的包,-c产生新包，-f指定包名。
- tar -uf packmd.tar readme.md
此命令为更新packmd包里原有的readme.md文件 ，-u为更新操作
- tar -tf packmd.tar
此命令将列出packmd.tar包中所有文件，-t 列出文件
- tar -rf packmd.tar push.md
此命令将把push.md文件追加到packmd.tar包里面, -r添加文件
- tar -xf packmd.tar
此命令是解开packmd.tar包中所有文件，-x 解压
- tar -czvf pack.tar.gz dir 将dir文件夹打包成pack.tar后，用gzip压缩，生成一个gzip压缩过的包 并显示整个打包压缩过程
- tar -cjvf pack.tar.bz2 dir 将dir文件夹打包成pack.tar后，用bz2压缩，生成一个bz2压缩过的包 并显示整个打包压缩过程
- tar -cZvf pack.tar.Z dir 将dir文件夹打包成pack.tar后，用compress压缩，生成一个uncompress压缩过的包 并显示整个打包压缩过程

**解压不同格式的文件**
- tar -xvf pack.tar 解压tar包
- tar -xzvf pack.tar.gz 解压tar.gz
- tar -xjvf pack.tar.bz2 解压tar.bz2
- tar -xZvf pack.tar.Z 解压tar.Z
- tar -xzvf pack.tar.gz -C /etc/nginx 压缩到指定目录 -C指定目录


## 常用的终端命令
1. ctrl+c 结束正在运行的程序【ping、telnet等】
2. ctrl+d 结束输入或退出shell
3. ctrl+s 暂停屏幕输出
4. ctrl+q 恢复屏幕输出
5. ctrl+l 清屏，等同于Clear
6. ctrl+a/ctrl+e 快速移动光标到行首/行尾

## 进程管理相关命令
### top (命令实时显示进程的状态)
**字段说明：**
- PID: 进程描述符
- USER： 进程的拥有者
- PRI：进程的优先级
- NI： nice level
- SIZE: 进程拥有的内存（包括code segment + data segment + stack segment）
- RSS: 物理内存使用
- VIRT（virtul memory usage）: 进程需要的虚拟内存大小
- RES(resident memory usage)： 常驻内存
- SHARE: 和其他进程共享的物理内存空间
- STAT：进程的状态，有 S=sleeping，R=running，T=stopped or traced，D=interruptible sleep（不可中断的睡眠状态），Z=zombie。
- %CPU： CPU使用率
- %MEM： 物理内存的使用
- TIME： 进程占用的总共cpu时间
- COMMAND：进程的命令

### ps 命令 用于显示当前进程 (process) 的状态。 ps [options] 
**options字段说明：**
- A 列出所有的行程
- w 显示加宽可以显示较多的资讯
- au 显示较详细的资讯
- aux 显示所有包含其他使用者的行程

### ps -ef |grep ngin
- ps命令将某个进程显示出来
- grep命令是查找
- 中间的|是管道命令 是指ps命令与grep同时执行
- PS是LINUX下最常用的也是非常强大的进程查看命令
- grep命令是查找，是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。
- grep全称是Global Regular Expression Print，表示全局正则表达式版本，它的使用权限是所有用户。

### kill、pkill 命令
- kill [pid] 杀死进程
- kill -9 [pid] 强制杀死进程
- pkill [pid|程序名]

### w [-fhlsuV] [用户名称] 命令用于显示目前登入系统的用户信息。
- f 　开启或关闭显示用户从何处登入系统。
- h 　不显示各栏位的标题信息列。
- l 　使用详细格式列表，此为预设值。
- s 　使用简洁格式列表，不显示用户登入时间，终端机阶段作业和程序所耗费的CPU时间。
- u 　忽略执行程序的名称，以及该程序耗费CPU时间的信息。
- V 　显示版本信息。

## linux免密登陆配置
[配置参考地址](https://blog.csdn.net/pengjunlee/article/details/80919833)

1. 生成密钥对
ssh-keygen -t rsa -C "写入密钥里面的名字" -f "生成文件的名字_rsa" 。
2. 上传配置公钥
上传公钥到服务器对应账号的home路径下的.ssh/中，（ssh-copy-id -i "公钥文件名" 用户名@服务器ip或域名）。配置公钥文件访问权限为600。
3. 配置本地私钥
把第一步生成的私钥复制到你的home目录下的 .ssh/中，配置你的私钥文件访问权限为600，chmod 600 你的私钥文件名。
4. 免密登陆功能的本地配置文件 
编辑自己home目录的.ssh/ 路径下的config文件，配置config文件的访问权限为 644。
5. ssh [生成的密钥文件]  ，就不用输入密码就连接到服务器了。