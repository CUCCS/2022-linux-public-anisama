### 2022-linux-public-anisama
---
#### 实验三
完成度自评：85
#### 实验环境
* MacOS
* VirtualBox 6.1
* Ubuntu 20.04

#### 实验要求
* 根据Systemd 入门教程：命令篇完成相关Systemd基本操作
* 根据Systemd 入门教程：实战篇完成相关操作
* 自查清单

#### 实验内容
#### 一、Systemd入门教程：命令篇

* `systemd_analyze`命令用于查看启动耗时
```bash
# 查看启动耗时
$ systemd-analyze                                                                                   
# 查看每个服务的启动耗时
$ systemd-analyze blame

# 显示瀑布状的启动过程流
$ systemd-analyze critical-chain

# 显示指定服务的启动流
$ systemd-analyze critical-chain atd.service
```
[![asciicast](https://asciinema.org/a/jBVws8YDmXgFSvaEm1E996pQD.svg)](https://asciinema.org/a/jBVws8YDmXgFSvaEm1E996pQD)

* `hostnamectl`命令用于查看当前主机的信息
```bash
# 显示当前主机的信息
$ hostnamectl

# 设置主机名。
$ sudo hostnamectl set-hostname rhel7
```
[![asciicast](https://asciinema.org/a/97NWY1efPHF0bcdUsGTRmbpE4.svg)](https://asciinema.org/a/97NWY1efPHF0bcdUsGTRmbpE4)

* `localectl`命令用于查看本地化设置 
```bash
# 查看本地化设置
$ localectl

# 设置本地化参数。
$ sudo localectl set-locale LANG=en_GB.utf8
$ sudo localectl set-keymap en_GB
```
[![asciicast](https://asciinema.org/a/ItmTonwfem0NlxFnOrWDW7GUT.svg)](https://asciinema.org/a/ItmTonwfem0NlxFnOrWDW7GUT)

* `timedatectl`命令用于查看当前时区设置
```bash
# 查看当前时区设置
$ timedatectl

# 显示所有可用的时区
$ timedatectl list-timezones                                                                                   
# 设置当前时区
$ sudo timedatectl set-timezone America/New_York
$ sudo timedatectl set-time YYYY-MM-DD
$ sudo timedatectl set-time HH:MM:SS
```
[![asciicast](https://asciinema.org/a/4lMpHo8FV9SdFyH2Je9VWTIep.svg)](https://asciinema.org/a/4lMpHo8FV9SdFyH2Je9VWTIep)

* `loginctl`命令用于查看当前登录的用户
```bash
# 列出当前session
$ loginctl list-sessions

# 列出当前登录用户
$ loginctl list-users

# 列出显示指定用户的信息
$ loginctl show-user ruanyf
```
[![asciicast](https://asciinema.org/a/bVTqpe4FKDzHKwlmyLbPDip7g.svg)](https://asciinema.org/a/bVTqpe4FKDzHKwlmyLbPDip7g)

* `systemctl list-units`命令可以查看当前系统的所有 Unit
```bash
# 列出正在运行的 Unit
$ systemctl list-units

# 列出所有Unit，包括没有找到配置文件的或者启动失败的
$ systemctl list-units --all

# 列出所有没有运行的 Unit
$ systemctl list-units --all --state=inactive

# 列出所有加载失败的 Unit
$ systemctl list-units --failed

# 列出所有正在运行的、类型为 service 的 Unit
$ systemctl list-units --type=service
```
[![asciicast](https://asciinema.org/a/AY7AVKonzchu1RQxU78HD7pZz.svg)](https://asciinema.org/a/AY7AVKonzchu1RQxU78HD7pZz)


* `systemctl status`命令用于查看系统状态和单个 Unit 的状态
```bash
# 显示系统状态
$ systemctl status

# 显示单个 Unit 的状态
$ sysystemctl status bluetooth.service

# 显示远程主机的某个 Unit 的状态
$ systemctl -H root@rhel7.example.com status httpd.service
```
此外`systemctl`还提供了三个查询状态的简单方法，主要供脚本内部的判断语句使用
```bash
#显示某个 Unit 是否正在运行
$ systemctl is-active application.service

# 显示某个 Unit 是否处于启动失败状态
$ systemctl is-failed application.service

# 显示某个 Unit 服务是否建立了启动链接
$ systemctl is-enabled application.service
```
[![asciicast](https://asciinema.org/a/7ln3zNuktHIsSAv1TnCcGS5Aw.svg)](https://asciinema.org/a/7ln3zNuktHIsSAv1TnCcGS5Aw)

* 对于用户来说，最常用的是下面这些命令，用于启动和停止 Unit（主要是 service）
```bash
# 立即启动一个服务
$ sudo systemctl start apache.service

# 立即停止一个服务
$ sudo systemctl stop apache.service

# 重启一个服务
$ sudo systemctl restart apache.service

# 杀死一个服务的所有子进程
$ sudo systemctl kill apache.service

# 重新加载一个服务的配置文件
$ sudo systemctl reload apache.service

# 重载所有修改过的配置文件
$ sudo systemctl daemon-reload

# 显示某个 Unit 的所有底层参数
$ systemctl show httpd.service

# 显示某个 Unit 的指定属性的值
$ systemctl show -p CPUShares httpd.service

# 设置某个 Unit 的指定属性
$ sudo systemctl set-property httpd.service CPUShares=500
```
[![asciicast](https://asciinema.org/a/eHkEhrm2tP6QUvFGeQnSBRajD.svg)](https://asciinema.org/a/eHkEhrm2tP6QUvFGeQnSBRajD)
[![asciicast](https://asciinema.org/a/xcevX5e5JtuVSTby16PC4Zyej.svg)](https://asciinema.org/a/xcevX5e5JtuVSTby16PC4Zyej)

* `systemctl list-dependencies`命令列出一个 Unit 的所有依赖
```bash
$ systemctl list-dependencies nginx.service
#如果要展开 Target，就需要使用--all参数。
$ systemctl list-dependencies --all nginx.service
```
[![asciicast](https://asciinema.org/a/CAuUCrlPXotUE9NO4ZfRYND3S.svg)](https://asciinema.org/a/CAuUCrlPXotUE9NO4ZfRYND3S)

* 每一个 Unit 都有一个配置文件，告诉 Systemd 怎么启动这个 Unit 。

Systemd 默认从目录`/etc/systemd/system/`读取配置文件。但是，里面存放的大部分文件都是符号链接，指向目录`/usr/lib/systemd/system/`，真正的配置文件存放在那个目录。

`systemctl enable`命令用于在上面两个目录之间，建立符号链接关系
```bash
$ sudo systemctl enable clamd@scan.service
# 等同于
$ sudo ln -s '/usr/lib/systemd/system/clamd@scan.service' '/etc/systemd/system/multi-user.target.wants/clamd@scan.service'
```
如果配置文件里面设置了开机启动，`systemctl enable`命令相当于激活开机启动。

与之对应的，`systemctl disable`命令用于在两个目录之间，撤销符号链接关系，相当于撤销开机启动
```bash
$ sudo systemctl disable clamd@scan.service
```
配置文件的后缀名，就是该 Unit 的种类，比如`sshd.socket`。如果省略，Systemd 默认后缀名为`.service`，所以sshd会被理解成`sshd.service`

* `systemctl list-unit-files`命令用于列出所有配置文件
```bash
# 列出所有配置文件
$ systemctl list-unit-files

# 列出指定类型的配置文件
$ systemctl list-unit-files --type=service
```
[![asciicast](https://asciinema.org/a/LwMQ9Ql6ZLlm6djUQ2iMpqWeb.svg)](https://asciinema.org/a/LwMQ9Ql6ZLlm6djUQ2iMpqWeb)

从配置文件的状态无法看出，该 Unit 是否正在运行。这必须执行前面提到的`systemctl status`  命令
```bash
$ systemctl status bluetooth.service
$ sudo systemctl daemon-reload
$ sudo systemctl restart httpd.service
#一旦修改配置文件，就要让 SystemD 重新加载配置文件，然后重新启动，否则修改不会生效。
```

* `systemctl cat`命令可以查看配置文件的内容
```
$ systemctl cat atd.service

[Unit]
Description=ATD daemon

[Service]
Type=forking
ExecStart=/usr/bin/atd

[Install]
WantedBy=multi-user.target
```
[![asciicast](https://asciinema.org/a/o29XECDbbXw4f0hyUJZGEPNp5.svg)](https://asciinema.org/a/o29XECDbbXw4f0hyUJZGEPNp5)

配置文件的区块共三个。[Unit]区块通常是配置文件的第一个区块，用来定义 Unit 的元数据，以及配置与其他 Unit 的关系。[Install]通常是配置文件的最后一个区块，用来定义如何启动，以及是否开机启动。[Service]区块用来 Service 的配置，只有 Service 类型的 Unit 才有这个区块。
Unit 配置文件的完整字段清单，请参考官方文档

* 启动计算机的时候，需要启动大量的 Unit。如果每一次启动，都要一一写明本次启动需要哪些 Unit，显然非常不方便。Systemd 的解决方案就是 Target。 简单说，Target 就是一个 Unit 组，包含许多相关的 Unit 。启动某个 Target 的时候，Systemd 就会启动里面所有的 Unit。
```bash
# 查看当前系统的所有 Target
$ systemctl list-unit-files --type=target

# 查看一个 Target 包含的所有 Unit
$ systemctl list-dependencies multi-user.target

# 查看启动时的默认 Target
$ systemctl get-default

# 设置启动时的默认 Target
$ sudo systemctl set-default multi-user.target

# 切换 Target 时，默认不关闭前一个 Target 启动的进程，
# systemctl isolate 命令改变这种行为，
# 关闭前一个 Target 里面所有不属于后一个 Target 的进程
$ sudo systemctl isolate multi-user.target
```
Target 与 传统 RunLevel 的对应关系如下:

>Traditional runlevel      New target name     Symbolically linked to...
Runlevel 0           |    runlevel0.target -> poweroff.target
Runlevel 1           |    runlevel1.target -> rescue.target
Runlevel 2           |    runlevel2.target -> multi-user.target
Runlevel 3           |    runlevel3.target -> multi-user.target
Runlevel 4           |    runlevel4.target -> multi-user.target
Runlevel 5           |    runlevel5.target -> graphical.target
Runlevel 6           |    runlevel6.target -> reboot.target

它与init进程的主要差别如下:

>1.默认的 RunLevel（在/etc/inittab文件设置）现在被默认的 Target 取代，位置是/etc/systemd/system/default.target，通常符号链接到graphical.target（图形界面）或者multi-user.target（多用户命令行）。
2.启动脚本的位置，以前是/etc/init.d目录，符号链接到不同的 RunLevel 目录 （比如/etc/rc3.d、/etc/rc5.d等），现在则存放在/lib/systemd/system和/etc/systemd/system目录。
3.配置文件的位置，以前init进程的配置文件是/etc/inittab，各种服务的配置文件存放在/etc/sysconfig目录。现在的配置文件主要存放在/lib/systemd目录，在/etc/systemd目录里面的修改可以覆盖原始设置。

[![asciicast](https://asciinema.org/a/SZR2DOCba0tQ8355vRu4GeAZX.svg)](https://asciinema.org/a/SZR2DOCba0tQ8355vRu4GeAZX)

* 日志管理
>ystemd 统一管理所有 Unit 的启动日志。带来的好处就是，可以只用`journalctl`一个命令，查看所有日志（内核日志和应用日志）。日志的配置文件是`/etc/systemd/journald.conf`。

>`journalctl`功能强大，用法非常多。
```bash
# 查看所有日志（默认情况下 ，只保存本次启动的日志）
$ sudo journalctl

# 查看内核日志（不显示应用日志）
$ sudo journalctl -k

# 查看系统本次启动的日志
$ sudo journalctl -b
$ sudo journalctl -b -0

# 查看上一次启动的日志（需更改设置）
$ sudo journalctl -b -1

# 查看指定时间的日志
$ sudo journalctl --since="2012-10-30 18:17:16"
$ sudo journalctl --since "20 min ago"
$ sudo journalctl --since yesterday
$ sudo journalctl --since "2015-01-10" --until "2015-01-11 03:00"
$ sudo journalctl --since 09:00 --until "1 hour ago"

# 显示尾部的最新10行日志
$ sudo journalctl -n

# 显示尾部指定行数的日志
$ sudo journalctl -n 20

# 实时滚动显示最新日志
$ sudo journalctl -f

# 查看指定服务的日志
$ sudo journalctl /usr/lib/systemd/systemd

# 查看指定进程的日志
$ sudo journalctl _PID=1

# 查看某个路径的脚本的日志
$ sudo journalctl /usr/bin/bash

# 查看指定用户的日志
$ sudo journalctl _UID=33 --since today

# 查看某个 Unit 的日志
$ sudo journalctl -u nginx.service
$ sudo journalctl -u nginx.service --since today

# 实时滚动显示某个 Unit 的最新日志
$ sudo journalctl -u nginx.service -f

# 合并显示多个 Unit 的日志
$ journalctl -u nginx.service -u php-fpm.service --since today

# 查看指定优先级（及其以上级别）的日志，共有8级
# 0: emerg
# 1: alert
# 2: crit
# 3: err
# 4: warning
# 5: notice
# 6: info
# 7: debug
$ sudo journalctl -p err -b

# 日志默认分页输出，--no-pager 改为正常的标准输出
$ sudo journalctl --no-pager

# 以 JSON 格式（单行）输出
$ sudo journalctl -b -u nginx.service -o json

# 以 JSON 格式（多行）输出，可读性更好
$ sudo journalctl -b -u nginx.serviceqq
 -o json-pretty

# 显示日志占据的硬盘空间
$ sudo journalctl --disk-usage

# 指定日志文件占据的最大空间
$ sudo journalctl --vacuum-size=1G

# 指定日志文件保存多久
$ sudo journalctl --vacuum-time=1years
```
[![asciicast](https://asciinema.org/a/jCCPoYrE5LrymLRSM85Iz9ZbM.svg)](https://asciinema.org/a/jCCPoYrE5LrymLRSM85Iz9ZbM)

#### 二、Systemd入门教程：实战篇
[![asciicast](https://asciinema.org/a/4yJDF7jZ0qDERPh8PMbI761I5.svg)](https://asciinema.org/a/4yJDF7jZ0qDERPh8PMbI761I5)
#### 三、自查清单
1.如何添加一个用户并使其具备sudo执行程序的权限
```bash
# 添加用户
sudo adduser cuc1
#添加sudo权限
sudo usermod -G sudo -a cuc1
```

2.如何将一个用户添加到一个用户组
```bash
usermod -a -G <groupname><username>
```

3.如何查看当前系统的分区表和文件系统详细信息
`sodu fdisk -l`

4.如何实现开机自动挂载Virtualbox的共享目录分区
挂载，指的是将设备文件中的顶级目录连接到 Linux 根目录下的某一目录（最好是空目录），使得访问此目录就等同于访问设备文件.
>在宿主机创建一个共享文件夹，让外部的顶级目录可以与其连接

5.基于LVM的分区如何实现动态扩容和缩减容量
```bash
lvextend -L +<容量> <目录>    #扩容
lvreduce -L -<容量> <目录>    #减容
```

6.如何通过Systemd设置实现在网络连通时运行一个指定脚本，在网络断开时运行另一个脚本
```bash
cd /etc/systemd/system
systemctl cat systemd-networkd.service
systemctl daemon-reload
sudo systemctl stop systemd-networkd.service
sudo systemctl start systemd-networkd.service
sudo systemctl stop systemd-networkd.socket
```

7.如何通过systemd设置实现一个脚本在任何情况下被杀死之后会立即重新启动？实现杀不死？
>修改服务配置文件Service
修改配置参数：
StartLimitIntervalSec=0
Restart=always
RestartSec=1
重新加载配置文件
sudo systemctl daemon-reload


#### 参考资料
[Systemd 入门教程：命令篇](https://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html)

[Systemd 入门教程：实战篇](http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html)

[什么是挂载](http://c.biancheng.net/view/2859.html)

[Ubuntu创建用户给予sudo权限](https://blog.csdn.net/wowocpp/article/details/100115112)

