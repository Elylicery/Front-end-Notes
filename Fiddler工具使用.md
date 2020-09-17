# Fiddler工具使用

# 1.Fiddler介绍

## 1-1 Fiddler工作环境

Fiddler的功能

* 监控HTTP/HTTPS的流量
* 查看截获请求的内容
* 伪造请求，不仅可以伪造服务器返回的请求，也可以伪造请求发送给服务器
* 测试网站的性能
* 解密HTTPS的web会话

## 1-2 Fiddler工作原理

<img src="Fiddler工具使用.assets/image-20200917154257181.png" alt="image-20200917154257181" style="zoom:50%;" />

客户端和服务期间创建了一个代理服务器

<img src="Fiddler工具使用.assets/image-20200917154450148.png" alt="image-20200917154450148" style="zoom: 50%;" />

通过修改浏览器的代理服务器的地址，fiddler就可以截获浏览器发送的所有请求

当使用Fiddler时，有两种模式可以选择 

<img src="Fiddler工具使用.assets/image-20200917154602427.png" alt="image-20200917154602427" style="zoom:33%;" />

<img src="Fiddler工具使用.assets/image-20200917154720893.png" alt="image-20200917154720893" style="zoom: 50%;" />

> **缓冲模式，可控制最后的服务器响应**

<img src="Fiddler工具使用.assets/image-20200917154734945.png" alt="image-20200917154734945" style="zoom:50%;" />

> **流模式，更接近于浏览器本身真实的行为**

## 1-3 Fiddler常见使用场景

<img src="Fiddler工具使用.assets/image-20200917155202937.png" alt="image-20200917155202937" style="zoom:33%;" />

**开发环境host配置**

通常情况下，配置host需改系统文件很不方便；在多个开发环境下切换很低效。

Fiddler提供了相对高效的host配置方法。

**前后端接口调试**

通常情况下，调试前后端接口需要真实的环境，一大堆假数据，写Javascript代码。

Fiddler只需一个UI界面进行配置即可

**线上bugfix**

Fiddler可将发布文件代理到本地，快速定位线上bug

**性能分析和优化**

Fiddler会提供请求的实际图，清晰明了网站需优化的部分。

# 2. Fiddler界面操作介绍

## 2-1 Fiddler工具条常用功能



![image-20200917155831520](Fiddler工具使用.assets/image-20200917155831520.png)

![image-20200917155851529](Fiddler工具使用.assets/image-20200917155851529.png)

![image-20200917160036805](Fiddler工具使用.assets/image-20200917160036805.png)

![image-20200917160311959](Fiddler工具使用.assets/image-20200917160311959.png)

![image-20200917161308080](Fiddler工具使用.assets/image-20200917161308080.png)

![image-20200917161703639](Fiddler工具使用.assets/image-20200917161703639.png)

## 2-2 Fiddler状态栏操作

<img src="Fiddler工具使用.assets/image-20200917170427655.png" alt="image-20200917170427655" style="zoom:25%;" />

<img src="Fiddler工具使用.assets/image-20200917171416221.png" alt="image-20200917171416221" style="zoom: 33%;" />

<img src="Fiddler工具使用.assets/image-20200917171609081.png" alt="image-20200917171609081" style="zoom:33%;" />

## 2-3 Fiddler监控面板的使用

![image-20200917171855882](Fiddler工具使用.assets/image-20200917171855882.png)

![image-20200917172027241](Fiddler工具使用.assets/image-20200917172027241.png)

![image-20200917172338712](Fiddler工具使用.assets/image-20200917172338712.png)

![image-20200917172653039](Fiddler工具使用.assets/image-20200917172653039.png)

有点复杂，后续继续看