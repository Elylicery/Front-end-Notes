

# linux学习-问题解决手册

2020-3-2  
开始阅读《鸟哥的linux私房菜》
学习环境是：VM虚拟机+CentOS7.1



[toc]



# 【安装centos】

## 1.CentOS 7如何安装图形界面

**方法一**：图形界面安装，在安装的时候【软件选择】选择安装【GNOME桌面】，centos默认安装是最小安装

**方法二**：命令安装

```
yum groupinstall 'X Window System'
```

在过程中输入确认，等待安装完成（出现com*plete！）

输入命令 进入图形界面

```
startx
```



## 2. Linux终端里面如何显示上一屏的内容

* 执行命令的时候在**后面加个”|more”**，就如同我使用”dpkg –l|more”，这样就可以用回车一点点的查看内容了。这个方法有个弊端，就是只能一直向下翻页，无法向上查看内容。

* 同方法1，只是在执行命令的时候，**在后面加上”|less”**，这个要比more好用的多，它可以用”PageUp”和”PageDown”按键上下翻页(也可以使用space向下翻页)，也可以用上下方向键一点点查看。退出按q。

* 可以将**输出重定向**。例如可以执行命令”dpkg–l >a.c”，这样你就会发现在当前目录多了一个a.c文件，然后查看该文件，就是你所想要显示的内容。所谓的重定向也很好理解：就是将本来应该在终端上显示的内容显示在了文件里面。
  

# 【网络问题】
## 1.centos无法ping通百度

***

`ping: www.baicu.com`

1. 将网络模式设置**为桥接模式**

2. 在宿主机上查看所需要的信息

![image-20200306210158912](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20200306210158912.png)

我们这里需要**IPv4地址**、**子网掩码**、**默认网关**三个数据

3. 编辑配置文件

可能最后的名字不一样，如果不一样就自己换一下

```
vi /etc/sysconfig/network-scripts/ifcfg-ens33
```

```
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
#设置静态ip
BOOTPROTO="static"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="1f3f4a9a-858d-47f6-b1f5-287ac6457216"
DEVICE="ens33"
ONBOOT="yes"
#输入你想设置的ip地址
IPADDR="192.168.1.100"
PREFIX="24"
#设置网关
GATEWAY="192.168.1.1"
#子网掩码
NETMASK=255.255.255.0
DNS1="8.8.8.8"
IPV6_PRIVACY="no"
 
```

说明：

**IPADDR**的值要与步骤2中的**IPv4地址**处于同一网段；

**GATEWAY**为步骤2中的**默认网关****；**

**NETMASK**为步骤2中的**子网掩码**

4. 重启网络

   ```
   systemctl restart network.service
   ```

5. 这个时候应该能ping通百度

   ```
   ping www.baidu.com
   ```

6. 如果还是不行就继续

   ```
   ll /etc/sysconfig/
    
   查看是否有static-routes文件，如果有则：
    
   vi /etc/sysconfig/static-routes
    
   添加下面的数据，保存退出
    
   any net default gw 192.168.0.1
   ```

   **如果重装过vmware：**

   **【记得删除之前的注册表信息！！！】**







# 【Vim】

## 1.使用vi 修改文件，保存时提示readonly

***

先从当前用户切换到root用户 su - root，然后重新修改即可

```

```

