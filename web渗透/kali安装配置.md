---
style: summer
---
# kali安装及配置
## 1.更新安装源
修改sources.list文件
leafpad /etc/apt/sources.list
选择下面的适合自己的源

```
#kali官方源
deb http://http.kali.org/kali kali-rolling main non-free contrib
#中科大的源
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb http://mirrors.ustc.edu.cn/kali kali-rolling main contrib non-free
deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main contrib non-free
deb http://mirrors.ustc.edu.cn/kali-security kali-current/updates main contrib non-free
deb-src http://mirrors.ustc.edu.cn/kali-security kali-current/updates main contrib non-free
#阿里云源
deb http://mirrors.aliyun.com/kali sana main non-free contrib
deb http://mirrors.aliyun.com/kali-security/ sana/updates main contrib non-free
deb-src http://mirrors.aliyun.com/kali-security/ sana/updates main contrib non-free
```
然后安装并更新

```
apt-get update
apt-get upgrade
apt-get dist-upgrade //更新这个系统，系统有新版本是更新
```

## 2.安装中文输入法（以下任选一种）
```
apt-get install fcitx-table-wbpy ttf-wqy-microhei ttf-wqy-zenhei  //拼音五笔
apt-get install ibus ibus-pinyin    //经典的ibus
apt-get install fcitx fcitx-googlepinyin   //fcitxpyin
```
*安装完，注销重新登录彩可以使用*

## 3.浏览器
1.汉化火狐浏览器

```
apt-get install iceweasel-l10n-zh-cn
```
2.flash player 安装

```
apt-get install flashplugin-nonfree update-flashplugin-nonfree --install
```
## 4.添加普通用户

```
#useradd -m username /自定义名字
#passwd username /回车后设置用户密码，输入两次
#usermod -a -G sudo username /加入sudo组，sudo可理解为临时root权限
#chsh -s /bin/bash username /设置用户默认外壳为bash
#reboot /重启
```

## 5.安装WPS
下载[安装包](http://wps-community.org/download.html)
解决libpng12-0依赖，到[debian packages](https://packages.debian.org/wheezy/libpng12-0)下载依赖包
字体缺失问题解决，[附件下载]（ https://pan.baidu.com/s/1e-vP2fyurDy0g5ahR_aPbQ ）密码: 275e