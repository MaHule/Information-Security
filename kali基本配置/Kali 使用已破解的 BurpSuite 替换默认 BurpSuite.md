# Kali 使用已破解的 BurpSuite 替换默认 BurpSuite

1.首先确保你可以通过控制台输入 `burpsuite` 来启动 BurpSuite

2.然后寻找 burpsuite 这个程序的绝对路径

```
which burpsuite
```

3.创建文件夹 /opt/burpsuite

```
mkdir /opt/burpsuite
```

4.将 BurpLoader.jar 和 BurpSuite-xx.xx.jar 移动到刚才创建的路径

5.备份之前的burpsuite

```
cd /usr/bin/
mv burpsuite burpsuite.bak
```

6.创建符号链接替换之前的 burpsuite , 并链接到 /opt/burpsuite/BurpLoader.jar

```
ln -s /opt/burpsuite/BurpLoader.jar /usr/bin/burpsuite
```

7.为 BurpLoader.jar 添加可执行权限

```
chmod +x /opt/burpsuite/BurpLoader.jar
```

8.测试