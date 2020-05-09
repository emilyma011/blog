
[TOC]

# 环境变量
## 查看环境变量
* export
* echo $PATH
## 设置环境变量
### 临时设置
* export /tmp/testPath:$PATH
在终端关闭 后就会消失。ss
### 长久设置
* ~/.bashrc
强烈推荐
    ```shell
    vi ~/.bashrc
    PATH=$PATH:/usr/local/MATLAB/R2013a/bin 
    export PATH
    source ~/.bashrc
    ```
* /etc/profile
修改 /etc/profile 文件，在文件末尾加上如下两行代码 
PATH=$PATH:/usr/local/MATLAB/R2013a/bin 
export PATH

最后执行命令 source /etc/profile 或执行点命令 ./profile 使其修改生效。
备注：发现并为生效，且重启后回复原有

* ~/.profile
未找到


# 文件操作
## 压缩解压

* 压缩： tar -cf all.tar *.jpg
* 解压：tar -xvf file.tar //解压 tar包

## 远程拷贝scp

>scp local_file remote_username@remote_ip:remote_folder 

# 安装卸载软件
## Debian系列，使用apt-get 
* 更新apt-get：apt-get update
* 搜索软件包xxx：apt-cache search xxx
* 获取包xxx的信息：apt-cache show xxx
* 安装xxx软件包：apt-get install xxx
* 移除xxx软件包：apt-get remove xxx
* 删除包xxx，包括删除配置文件等:apt-get remove package -- purge
* 更新所有deb软件包：apt-get upgrade
* 更新xxx软件包：apt-get upgrade xxx 

## CentOS 安装
* yum

# 1. 关机重启命令
重启命令：
1、reboot
2、shutdown -r now 立刻重启(root用户使用)
3、shutdown -r 10 过10分钟自动重启(root用户使用) 
4、shutdown -r 20:35 在时间为20:35时候重启(root用户使用)
如果是通过shutdown命令设置重启的话，可以用shutdown -c命令取消重启

关机命令：
1、halt   立刻关机
2、poweroff  立刻关机
3、shutdown -h now 立刻关机(root用户使用)
4、shutdown -h 10 10分钟后自动关机
如果是通过shutdown命令设置关机的话，可以用shutdown -c命令取消重启

# 账户操作
## 修改密码
>passwd
passwd命令加一个用户名，表示修改这个用户的密码；passwd命令后面不加用户名，则表示修改当前登录用户的密码（就是你执行passwd命令时的那个用户）。

# 2. less 查看文件

>less 在查看之前不会加载整个文件。可以尝试使用 less 和 vi 打开一个很大的文件，你就会看到它们之间在速度上的区别。在 less 中导航命令类似于 vi。

## 2.1. 1 搜索：
当使用命令 less file-name 打开一个文件后，可以使用下面的方式在文件中搜索。搜索时整个文本中匹配的部分会被高亮显示。
* 向前搜索：
>/ - 使用一个模式进行搜索，并定位到下一个匹配的文本
n - 向前查找下一个匹配的文本
N - 向后查找前一个匹配的文本
* 向后搜索
>? - 使用模式进行搜索，并定位到前一个匹配的文本
n - 向后查找下一个匹配的文本
N - 向前查找前一个匹配的文本


2 全屏导航
ctrl + F - 向前移动一屏
ctrl + B - 向后移动一屏
ctrl + D - 向前移动半屏
ctrl + U - 向后移动半屏
3 单行导航
j - 向前移动一行
k - 向后移动一行
4 其它导航
G - 移动到最后一行
g - 移动到第一行
q / ZZ - 退出 less 命令
5 其它有用的命令
v - 使用配置的编辑器编辑当前文件
h - 显示 less 的帮助文档
&pattern - 仅显示匹配模式的行，而不是整个文件
6 标记导航
当使用 less 查看大文件时，可以在任何一个位置作标记，可以通过命令导航到标有特定标记的文本位置。
ma - 使用 a 标记文本的当前位置
'a - 导航到标记 a 处
7 浏览多个文件
方式一，传递多个参数给 less，就能浏览多个文件。
less file1 file2
方式二，正在浏览一个文件时，使用 :e 打开另一个文件。
less file1
:e file2
当打开多个文件时，使用如下命令在多个文件之间切换
:n - 浏览下一个文件
:p - 浏览前一个文件

# 3. 分屏
## 3.1. 命令行终端使用tmux分屏
使用tmux分屏（既可以左右分屏，也可以上下分屏）
（1）安装工具

在ubuntu系统中使用sudo apt-get install tmux安装tmux工具

（2）使用工具

1，输入命令tmux使用工具

2，上下分屏：ctrl + b  再按 "

3，左右分屏：ctrl + b  再按 %

4，切换屏幕：ctrl + b  再按o

5，关闭一个终端：ctrl + b  再按x
promotionpromotion
6，上下分屏与左右分屏切换： ctrl + b  再按空格键

https://blog.csdn.net/xiaochonghao/article/details/69397564


# CentOS
* 查看防火墙状态

    systemctl status firewalld

    ```
    [root@centos7g local]# systemctl status firewalld
    ● firewalld.service - firewalld - dynamic firewall daemon
    Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
    Active: inactive (dead)
        Docs: man:firewalld(1)
    ```


* 打开防火墙

    systemctl start firewalld

* 查看防火墙状态
    ```
    [root@centos7g local]# systemctl status firewalld
    ● firewalld.service - firewalld - dynamic firewall daemon
    Loaded: loaded (/usr/lib/systemd/system/firewalld.service; disabled; vendor preset: enabled)
    Active: active (running) since 六 2020-05-09 10:16:09 CST; 6s ago
        Docs: man:firewalld(1)
    Main PID: 97440 (firewalld)
    CGroup: /system.slice/firewalld.service
            └─97440 /usr/bin/python -Es /usr/sbin/firewalld --nofork --nopid

    5月 09 10:16:08 centos7g systemd[1]: Starting firewalld - dynamic firewall.....
    5月 09 10:16:09 centos7g systemd[1]: Started firewalld - dynamic firewall ...n.
    Hint: Some lines were ellipsized, use -l to show in full.
    ```

* 关闭防火墙
    systemctl stop firewalld


* 开启端口
    firewall-cmd --zone=public --add-port=9092/tcp --permanent

    ```
    [root@centos7g local]# firewall-cmd --zone=public --add-port=9092/tcp --permanent
    success
    ```
