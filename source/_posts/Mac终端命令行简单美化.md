---
title: Mac终端命令行简单美化
date: 2018-05-09 18:24:32
tags: Mac
categories: 其他
---
天天对着Mac终端命令行,看不下去了，网上有很多美化的教程，就是太复杂了，所以我选了简单的方法和效果又不错的。话不多说开始配置。

## 操作步骤
1. 先找到用户下的 **.bash_profile** 文件，如果没有这个文件就新建一个,打开命令行按照下面执行。
```bash
$ cd ~/
$ vim .bash_profile
```
2. 把下面两行加到 **.bash_profile** 文件内。
```
export CLICOLOR=1 # 打开颜色区分显示
export LSCOLORS=Gxfxcxdxbxegedabagacad  # 颜色显示的格式
```

3. 再使修改后的 **.bash_profile** 文件生效,使用 source命令 使配置的环境变量生效。
```bash
$ source .bash_profile
```

4. 然后在打开 Terminal 的设置窗口 ，做右上角： 终端 -> 偏好设置 -> 文本 -> 显示ANSI颜色（启用），然后重新打开 Terminal 就能看到效果了。

## 额外提醒⚠️
如果重新开机发现，npm 安装的包的命令都找不到了，那可能就是 Mac 环境变量没有加载到 npm 的环境变量配置。
因为Mac系统的环境变量，加载顺序为：
1. /etc/profile     （系统级）
2. /etc/paths       （系统级）
3. ~/.bash_profile  （用户级）
4. ~/.bash_login    （用户级）
5. ~/.profile       （用户级）
6. ~/.bashrc        （打开命令行自动载入）

其中 **/etc/profile** 和 **/etc/paths**是系统级别的，系统启动就会加载，后面几个是当前用户级的环境变量。后面3个按照从前往后的顺序读取，如果 **~/.bash_profile** 文件存在，则后面的几个文件就会被忽略不读了，如果 **~/.bash_profile** 文件不存在，才会以此类推读取后面的文件。**~/.bashrc没有上述规则，它是bash shell打开的时候载入的**。

### 解决npm安装包命令找不到的办法
1. 直接把 **export PATH=~/.npm-global/bin:$PATH** 加到 **~/.bash_profile** 文件最上面：
```
export PATH=~/.npm-global/bin:$PATH
export CLICOLOR=1 # 打开颜色区分显示
export LSCOLORS=Gxfxcxdxbxegedabagacad  # 颜色显示的格式
```

2. 记得执行 **source .bash_profile** 就是让环境变量立即生效
```bash
$ source ~/.bash_profile
```

## 效果图
修改前：
<img src="https://s2.ax1x.com/2019/07/03/Ztha4J.png" width="100%"/>

修改后：
<img src="https://s2.ax1x.com/2019/07/03/ZthUN4.png" width="100%"/>
