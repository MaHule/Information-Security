# CSRF

XSS和CSRF从信任角度的区别：

XSS：利用用户对站点的信任

CSRF：利用站点对已经身份认证用户的信任（默认情况下站点不信任客户端）

**结合社工在身份认证会话过程中实现攻击**

场景：

1、修改账号密码、个人信息（email、收货地址）

2、发送伪造的业务请求（网银、购物、投票）

3、关注他人社交账号、推送博文

4、在用户非自愿、不知情情况下提交请求



**属于业务逻辑漏洞**（所有请求都是正常的请求）

对**关键操作（支付、提交订单等）**缺少确认机制（如验证码）

自动扫描程序无法发现此类漏洞



**漏洞利用条件**

**被害用户已经完成身份认证（即已登录）**

新请求的提交（重要性的）不需要重新身份验证

攻击者必须了解Web Application请求的参数构造

诱使用户触发攻击的指令（社工）



**Burpsuite CSRF PoC generator**

Post/Get方法



**CSRF漏洞演示：**

![CSRF](img/CSRF/CSRF1.png)

构建CSRF页面csrf.html

```
<a href="http://10.252.119.85/dvwa/vulnerabilities/csrf/?password_new=123&password_conf=123&Change=Change">csrf</a>
```

![CSRF2](img/CSRF/CSRF2.png)











**代码审计：**

*low:*

```
<?php
                
    if (isset($_GET['Change'])) {
    
        // Turn requests into variables
        $pass_new = $_GET['password_new'];
        $pass_conf = $_GET['password_conf'];


        if (($pass_new == $pass_conf)){
            $pass_new = mysql_real_escape_string($pass_new);
            $pass_new = md5($pass_new);

            $insert="UPDATE `users` SET password = '$pass_new' WHERE user = 'admin';";
            $result=mysql_query($insert) or die('<pre>' . mysql_error() . '</pre>' );
                        
            echo "<pre> Password Changed </pre>";        
            mysql_close();
        }
    
        else{        
            echo "<pre> Passwords did not match. </pre>";            
        }

    }
?>
```

*medium:*(需要输入元密码)





```
<?php
            
    if (isset($_GET['Change'])) {
    
        // Checks the http referer header
        if ( eregi ( "127.0.0.1", $_SERVER['HTTP_REFERER'] ) ){  //判断来源，限制接受本机IP的包【可将其替换或在referer中包含127.0.0.1】
    
            // Turn requests into variables
            $pass_new = $_GET['password_new'];
            $pass_conf = $_GET['password_conf'];

            if ($pass_new == $pass_conf){
                $pass_new = mysql_real_escape_string($pass_new);
                $pass_new = md5($pass_new);

                $insert="UPDATE `users` SET password = '$pass_new' WHERE user = 'admin';";
                $result=mysql_query($insert) or die('<pre>' . mysql_error() . '</pre>' );
                        
                echo "<pre> Password Changed </pre>";        
                mysql_close();
            }
    
            else{        
                echo "<pre> Passwords did not match. </pre>";            
            }    

        }
        
    }
?>
```

*high:*

```
<?php
            
    if (isset($_GET['Change'])) {
    
        // Turn requests into variables
        $pass_curr = $_GET['password_current'];
        $pass_new = $_GET['password_new'];
        $pass_conf = $_GET['password_conf'];

        // Sanitise current password input
        $pass_curr = stripslashes( $pass_curr );
        $pass_curr = mysql_real_escape_string( $pass_curr );
        $pass_curr = md5( $pass_curr );
        
        // Check that the current password is correct
        $qry = "SELECT password FROM `users` WHERE user='admin' AND password='$pass_curr';";
        $result = mysql_query($qry) or die('<pre>' . mysql_error() . '</pre>' );

        if (($pass_new == $pass_conf) && ( $result && mysql_num_rows( $result ) == 1 )){
            $pass_new = mysql_real_escape_string($pass_new);
            $pass_new = md5($pass_new);

            $insert="UPDATE `users` SET password = '$pass_new' WHERE user = 'admin';";
            $result=mysql_query($insert) or die('<pre>' . mysql_error() . '</pre>' );
                        
            echo "<pre> Password Changed </pre>";        
            mysql_close();
        }   
        else{        
            echo "<pre> Passwords did not match or current password incorrect. </pre>";          
        }
    }
?>
```



**自动扫描程序的检测方法【代码安全，确认机制角度】**

在请求和响应过程中检查是否存在[anti-CSRF token](http://www.2cto.com/article/201511/449909.html)名

检查服务器是否验证anti-CSRF token的名值

检查token中可编辑的字符串

检查referrer头是否可以伪造

 

**对策** 

Captcha

[anti-CSRF token](http://www.ibm.com/developerworks/cn/web/1102_niugang_csrf/)

Referrer头　　【可绕过机率较大】

降低会话超时时间 

