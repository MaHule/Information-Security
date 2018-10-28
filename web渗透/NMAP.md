# NMAP

> 最强大大扫描器

参数介绍：

```
# nmap
Nmap 7.70 ( https://nmap.org )
Usage: nmap [Scan Type(s)] [Options] {target specification}
TARGET SPECIFICATION:                                 #发现目标
  Can pass hostnames, IP addresses, networks, etc.
  Ex: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.0-255.1-254    #实例
  -iL <inputfilename>: Input from list of hosts/networks         #指定IP地址列表
  -iR <num hosts>: Choose random targets                         #随机选择目标
  --exclude <host1[,host2][,host3],...>: Exclude hosts/networks  #过滤某些IP，不对其扫描
  --excludefile <exclude_file>: Exclude list from file      #过滤整个文件的IP,不对其进行扫描
HOST DISCOVERY:
  -sL: List Scan - simply list targets to scan              #列出要扫描的IP
  -sn: Ping Scan - disable port scan                        #不做端口扫描
  -Pn: Treat all hosts as online -- skip host discovery     #防止防火墙拒绝扫描，继续后面端口扫描
  -PS/PA/PU/PY[portlist]: TCP SYN/ACK, UDP or SCTP discovery to given ports
  -PE/PP/PM: ICMP echo, timestamp, and netmask request discovery probes  
  -PO[protocol list]: IP Protocol Ping
  -n/-R: Never do DNS resolution/Always resolve [default: sometimes]  #不做DNS解析/-R做反向解析
  --dns-servers <serv1[,serv2],...>: Specify custom DNS servers    #调用指定服务器
  --system-dns: Use OS's DNS resolver
  --traceroute: Trace hop path to each host                     #路由路径
SCAN TECHNIQUES:
  -sS/sT/sA/sW/sM: TCP SYN/Connect()/ACK/Window/Maimon scans
  -sU: UDP Scan
  -sN/sF/sX: TCP Null, FIN, and Xmas scans
  --scanflags <flags>: Customize TCP scan flags
  -sI <zombie host[:probeport]>: Idle scan                 #僵尸扫描
  -sY/sZ: SCTP INIT/COOKIE-ECHO scans
  -sO: IP protocol scan
  -b <FTP relay host>: FTP bounce scan
PORT SPECIFICATION AND SCAN ORDER:
  -p <port ranges>: Only scan specified ports              #指定端口扫描
    Ex: -p22; -p1-65535; -p U:53,111,137,T:21-25,80,139,8080,S:9
  --exclude-ports <port ranges>: Exclude the specified ports from scanning     #指定端口范围
  -F: Fast mode - Scan fewer ports than the default scan
  -r: Scan ports consecutively - don't randomize
  --top-ports <number>: Scan <number> most common ports
  --port-ratio <ratio>: Scan ports more common than <ratio>
SERVICE/VERSION DETECTION:
  -sV: Probe open ports to determine service/version info
  --version-intensity <level>: Set from 0 (light) to 9 (try all probes)
  --version-light: Limit to most likely probes (intensity 2)           #扫描强度
  --version-all: Try every single probe (intensity 9)
  --version-trace: Show detailed version scan activity (for debugging)
SCRIPT SCAN:
  -sC: equivalent to --script=default
  --script=<Lua scripts>: <Lua scripts> is a comma separated list of
           directories, script-files or script-categories
  --script-args=<n1=v1,[n2=v2,...]>: provide arguments to scripts
  --script-args-file=filename: provide NSE script args in a file
  --script-trace: Show all data sent and received
  --script-updatedb: Update the script database.
  --script-help=<Lua scripts>: Show help about scripts.
           <Lua scripts> is a comma-separated list of script-files or
           script-categories.
OS DETECTION:
  -O: Enable OS detection
  --osscan-limit: Limit OS detection to promising targets
  --osscan-guess: Guess OS more aggressively
TIMING AND PERFORMANCE:
  Options which take <time> are in seconds, or append 'ms' (milliseconds),
  's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m).
  -T<0-5>: Set timing template (higher is faster)
  --min-hostgroup/max-hostgroup <size>: Parallel host scan group sizes
  --min-parallelism/max-parallelism <numprobes>: Probe parallelization
  --min-rtt-timeout/max-rtt-timeout/initial-rtt-timeout <time>: Specifies
      probe round trip time.
  --max-retries <tries>: Caps number of port scan probe retransmissions.
  --host-timeout <time>: Give up on target after this long
  --scan-delay/--max-scan-delay <time>: Adjust delay between probes
  --min-rate <number>: Send packets no slower than <number> per second
  --max-rate <number>: Send packets no faster than <number> per second
FIREWALL/IDS EVASION AND SPOOFING:
  -f; --mtu <val>: fragment packets (optionally w/given MTU)
  -D <decoy1,decoy2[,ME],...>: Cloak a scan with decoys
  -S <IP_Address>: Spoof source address
  -e <iface>: Use specified interface
  -g/--source-port <portnum>: Use given port number
  --proxies <url1,[url2],...>: Relay connections through HTTP/SOCKS4 proxies
  --data <hex string>: Append a custom payload to sent packets
  --data-string <string>: Append a custom ASCII string to sent packets
  --data-length <num>: Append random data to sent packets
  --ip-options <options>: Send packets with specified ip options
  --ttl <val>: Set IP time-to-live field
  --spoof-mac <mac address/prefix/vendor name>: Spoof your MAC address   #欺骗mac地址
  --badsum: Send packets with a bogus TCP/UDP/SCTP checksum              #差错检测
OUTPUT:
  -oN/-oX/-oS/-oG <file>: Output scan in normal, XML, s|<rIpt kIddi3,    #选择输出格式
     and Grepable format, respectively, to the given filename.
  -oA <basename>: Output in the three major formats at once
  -v: Increase verbosity level (use -vv or more for greater effect)
  -d: Increase debugging level (use -dd or more for greater effect)
  --reason: Display the reason a port is in a particular state
  --open: Only show open (or possibly open) ports
  --packet-trace: Show all packets sent and received
  --iflist: Print host interfaces and routes (for debugging)
  --append-output: Append to rather than clobber specified output files
  --resume <filename>: Resume an aborted scan
  --stylesheet <path/URL>: XSL stylesheet to transform XML output to HTML
  --webxml: Reference stylesheet from Nmap.Org for more portable XML
  --no-stylesheet: Prevent associating of XSL stylesheet w/XML output
MISC:
  -6: Enable IPv6 scanning
  -A: Enable OS detection, version detection, script scanning, and traceroute
  --datadir <dirname>: Specify custom Nmap data file location
  --send-eth/--send-ip: Send using raw ethernet frames or IP packets
  --privileged: Assume that the user is fully privileged
  --unprivileged: Assume the user lacks raw socket privileges
  -V: Print version number
  -h: Print this help summary page.
EXAMPLES:
  nmap -v -A scanme.nmap.org
  nmap -v -sn 192.168.0.0/16 10.0.0.0/8
  nmap -v -iR 10000 -Pn -p 80
SEE THE MAN PAGE (https://nmap.org/book/man.html) FOR MORE OPTIONS AND EXAMPLES
```



Nmap脚本：

```
/usr/share/nmap/scripts# less script.db | wc -l      #查看脚本数量
588



```





# Nmap

> Network Mapper,linux下网络扫描和嗅探工具包

**基本功能：**

- 扫描主机端口，嗅探所提供的服务
- 探索一组主机是否在线
- 推断主机所用操作系统，到达主机经过的路由，系统已开放端口的人家版本



**netstat部分参数解释：**

| 命令          | 作用                                               |
| ------------- | -------------------------------------------------- |
| -l（listen）  | 列出listen（监听）的服务                           |
| -t（tcp）     | 列出tcp相关的服务                                  |
| -n（numeric） | 直接显示ip地址以及端口，不解析为服务器名字或主机名 |
| -p（pid）     | 显示出socket所属的进程以及进程名字                 |
| --inet        | 显示ipv4相关协议的监听                             |

```
# netstat -lntp --inet                                   #查看ipv4端口上的tcp监听
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:1080            0.0.0.0:*               LISTEN      1685/ss-qt5         
```

```
# netstat -lntp --inet | grep -v 127.0.0.1              #过滤监控127.0.0.1的端口
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:1080            0.0.0.0:*               LISTEN      1685/ss-qt5         

```

 #### 扫描Tcp端口

```
# nmap 10.252.119.85 -p1-65535           #扫描1到65535所有在监听的端口
Starting Nmap 7.70 ( https://nmap.org ) at 2018-08-26 09:53 CST
Nmap scan report for 10.252.119.85
Host is up (0.000078s latency).
Not shown: 65505 closed ports
PORT      STATE SERVICE
21/tcp    open  ftp
22/tcp    open  ssh
23/tcp    open  telnet
25/tcp    open  smtp
53/tcp    open  domain
80/tcp    open  http
111/tcp   open  rpcbind
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
512/tcp   open  exec
513/tcp   open  login
514/tcp   open  shell
1099/tcp  open  rmiregistry
1524/tcp  open  ingreslock
2049/tcp  open  nfs
2121/tcp  open  ccproxy-ftp
3306/tcp  open  mysql
3632/tcp  open  distccd
5432/tcp  open  postgresql
5900/tcp  open  vnc
6000/tcp  open  X11
6667/tcp  open  irc
6697/tcp  open  ircs-u
8009/tcp  open  ajp13
8180/tcp  open  unknown
8787/tcp  open  msgsrvr
42655/tcp open  unknown
49518/tcp open  unknown
55149/tcp open  unknown
57042/tcp open  unknown
MAC Address: 08:00:27:C7:43:D8 (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 0.93 seconds



```

**注：** -p参数，如果不指定要扫描的端口，Nmap默认扫描从1到1024再加上nmap-services列出的端口

nmap-services是一个包含大约2200个著名的服务的数据库，Nmap通过查询该数据库可以报告那些端口可能对应于什么服务器，但不一定正确。所以正确扫描一个机器开放端口的方法是上面命令。

```
# nmap 10.252.119.85 -p20-200,7777,8888      #扫描一个IP的多个端口
Starting Nmap 7.70 ( https://nmap.org ) at 2018-08-26 09:56 CST
Nmap scan report for 10.252.119.85
Host is up (0.00063s latency).
Not shown: 175 closed ports
PORT    STATE SERVICE
21/tcp  open  ftp
22/tcp  open  ssh
23/tcp  open  telnet
25/tcp  open  smtp
53/tcp  open  domain
80/tcp  open  http
111/tcp open  rpcbind
139/tcp open  netbios-ssn
MAC Address: 08:00:27:C7:43:D8 (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 0.30 seconds
```



#### 是扫描UDP端口

| -sU  | 表式UDP scan，udp端口扫描                                |
| ---- | -------------------------------------------------------- |
| -Pn  | 不对端口进行Ping探测（不判断主机是否在线，直接扫描端口） |

```

```



