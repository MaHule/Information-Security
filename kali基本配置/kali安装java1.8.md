# kali安装java1.8

### 1.下载

去`http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html`下载系统对应的版本



### 2.预准备工作

卸载kali自带的openjdk

```
apt-get remove open jdk-10-jdk
```



### 3.安装

解压下载的oracle JDK

```
tar zxvf jdk-8u112-linux-i586.tar.gz
```

开始复制目录，手动安装

```
mkdir -p /usr/local/java
cp -r jdk1.8.0_112 /usr/local/java/
```



配置JDK环境变量

```
gedit /etc/profile

复制下面的内容到文件的末尾：
JAVA_HOME=/usr/local/java/jdk1.8.0_112
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
export JAVA_HOME
export PATH
```

告诉JDK的位置，最后两行代码不是重复，是重复两次

```
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/local/java/jdk1.8.0_112/bin/java" 1

sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/local/java/jdk1.8.0_112/bin/javac" 1

sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/local/java/jdk1.8.0_112/bin/javaws" 1

sudo update-alternatives --install "/usr/bin/javaws" "javaws" "/usr/local/java/jdk1.8.0_112/bin/javaws" 1

```

设置新的JDK为默认，代码也是执行两次

```
sudo update-alternatives --set java /usr/local/java/jdk1.8.0_112/bin/java
sudo update-alternatives --set java /usr/local/java/jdk1.8.0_112/bin/java

sudo update-alternatives --set javac /usr/local/java/jdk1.8.0_112/bin/javac
sudo update-alternatives --set javac /usr/local/java/jdk1.8.0_112/bin/javac

sudo update-alternatives --set javaws /usr/local/java/jdk1.8.0_112/bin/javaws
sudo update-alternatives --set javaws /usr/local/java/jdk1.8.0_112/bin/javaws
```

重载profile文件

```
source /etc/profile
```



### 4.测试

```
# java -version
java version "1.8.0_181"
Java(TM) SE Runtime Environment (build 1.8.0_181-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.181-b13, mixed mode)
```

