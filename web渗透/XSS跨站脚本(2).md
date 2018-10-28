# XSS（跨站脚本）

### 应用实例：键盘记录器

> 反射型漏洞利用

> 原则上：只要XSS漏洞存在，可以编写任何功能的Js脚本提交

键盘记录器：被记录下来的数据会发送到攻击者指定的URL上





















### XSS利用工具

> 专门针对XSS漏洞，使用Python编写

- 有图形化界面和字符界面，xsser --gtk：打开图形化界面   （界面不友好，不建议使用）

![XSS5](img/XSS/XSS5.png)

- 可以绕过服务器端输入筛选   【xss存在及其普遍】

​           1.编码：10进制/16进制

​           2.函数：uniecape()

**简单的使用语法：**

```
# xsser -u "http://10.252.119.85/dvwa/vulnerabilities/" -g "xss_r/?name=" --cookie="security=low; PHPSESSID=8e08720dc358fe4b7979c75b773abdcf" -s -v --reverse-check
```

![XSS6](img/XSS/XSS6.png)

```
Get:将对应参数和页面写进-g参数中；Post：使用-p； -s：统计请求数； -v：显示详细信息； 
--reverse-check：禁止提交hash值方式验证（此方法可能误判）
--heuristic   探测服务器，检查被过滤的字符（会发送大量请求）

```















### XSS源码分析

1. low

```

<?php
if(!array_key_exists ("name", $_GET) || $_GET['name'] == NULL || $_GET['name'] == ''){
 $isempty = true;
} 
else {        
 echo '<pre>';
 echo 'Hello ' . $_GET['name'];
 echo '</pre>';    
}
?> 

#$_Get[]:直接回显输入的数据，不做任何过滤
```

2. medium

```
 <?php

if(!array_key_exists ("name", $_GET) || $_GET['name'] == NULL || $_GET['name'] == ''){

 $isempty = true;

} else {

 echo '<pre>';
 echo 'Hello ' . str_replace('<script>', '', $_GET['name']);
 echo '</pre>'; 

}
?> 


#使用    str_replace(‘<script>’,‘’)过滤，将<script>替换为空，可使用复写<script>跳过
```



3.high

```
 <?php
    
if(!array_key_exists ("name", $_GET) || $_GET['name'] == NULL || $_GET['name'] == ''){
    
 $isempty = true;
        
} else {
    
 echo '<pre>';
 echo 'Hello ' . htmlspecialchars($_GET['name']);
 echo '</pre>';
        
}
?> 

使用 htmlspecialchars($_Get['name']) 过滤，将服务器输出的html代码在转化为十六进制
```







