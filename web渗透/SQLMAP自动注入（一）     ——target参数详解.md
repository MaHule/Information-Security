# SQLMAP自动注入（一）     ——target参数详解

> **[SqlMap:]** 开源sql自动化注入工具，可以用来检测和利用sql注入漏洞【动态页面get/post参数，cookie，Http头。 】它由Python语言开发而来，因此运行需要安装python环境。其功能完善，有强大的引擎，适用几乎所有数据库，可自动进行数据榨取，也**可对检测与利用的自动化处理（数据库指纹、访问底层文件系统、执行操作系统命令），还可以做XSS漏洞检测** 。

*提示：* sqlmap只是用来检测和利用sql注入点的，并不能扫描出网站有哪些漏洞使用前需要先扫描出注入点



#### 五种漏洞检测技术

1. 基于布尔的盲注测试
2. 基于时间的盲注检测【若是该语句可以在服务器中运行，则按指定时间返回，若不能执行，返回错误或正常页面】`and (select * from (select(sleep(20)))a)--+`
3. 基于错误的检测
4. 基于Union联合查询的检测（*前提：*使用于通过for/while循环直接输出联合查询结果，即SQL语句在循环内，否则只会显示第一项结果）
5. 基于堆叠查询的检测

> > 通过分号（；）堆叠多个查询语句
> >
> > 使用非select的数据修改，删除的操作

  

#### 支持的数据库管理系统 DBMS

`MySQL、Oracle、PostgreSQL、Microsoft SQL Server, Microsoft Access，IBM DB2， SQLite，Firebird， Sybase ， SAP MaxDB `





#### SQL其它特性

1. 数据库直接连接`-d`（不通过SQL注入，指定身份认证信息【前提有数据库管理系统的相应权限】，IP，端口）
2. 与Burpsuite，Google结合使用，支持正则表达式限定测试目标
3. Get，Post，Cookie，Referer，User-Agent（随机或指定）

> cookie过期后自动处理Set-Cookie头，更新Cookie信息【不用担心扫描过程中，cookie过期】

4. 限速：最大编发，延迟发送
5. 支持Basic，Digeset，NTLM，CA身份认证
6. 数据库版本，用户，权限，hash枚举和字典爆破，暴力破解表列名称【可使用内建字典，也可使用自定义字典】
7. 文件上传下载，UDP，启动并执行存储过程，操作系统命令执行，访问windows注册表
8. 与W3af，metasploit集成结合使用 ，基于数据库服务进程提权和上传执行后门



#### SQLMAP

##### 1.功能详情





