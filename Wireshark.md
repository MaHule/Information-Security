# Wireshark

### 1.认识界面

### 2.适用表格





```
ip.addr==180.97.34.134  #选择IP为180.97.34.134的数据包
搜索支持and or 和not
tcp.port==80       #选择端口号为80的数据包
!tcp               #选择非tcp的数据包
frame.len<=150     #选择数据长度小于150的数据包
tcp[13]==0x18      
```



### 3.图形显示

1. IO图表
2. TCP流时间：往返时间
3. 流量图

将连接可视化，将一段时间的浏览显示出来，一般以列的方式将主机之间的连接显示出来，并将数据组织到一起



### 4.高级特性



### 5.命令行模式

> wireshark命令行模式tshark

1. 命令行网络分析的优势

tshark可以引入脚本文件



2. 命令行对破获文件进行调优分析



3. 命令行工具的常用命令

```
tshark -r 文件       #使用tshark读取文件
```

```
tshark -Y 限制条件
```









4. 命令行工具与第三方辅助工具的使用方法



