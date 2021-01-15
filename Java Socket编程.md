本节主要内容：

socket编程，使用java实现服务器和客户端之间的相互通信。

# 第一章 网络基础知识

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127165016869.png" alt="image-20191127165016869" style="zoom:80%;" />

## TCP/IP协议

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127165108133.png" alt="image-20191127165108133" style="zoom:80%;" />

在传输层 ：TCP/IP

应用层：HTTP、FTP、SMTP、Telnet

## IP地址

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127165256278.png" alt="image-20191127165256278" style="zoom:80%;" />



IP地址格式：数字型，如：192.168.0.1

## 端口

![image-20191127165626215](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127165626215.png)

>  常用端口号：http:80  ftp:12  telnet:23

## java中的网络支持

 <img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127165748527.png" alt="image-20191127165748527" style="zoom:80%;" />

# 第二章 java中网络相关API的应用

## InetAddress类

![image-20191127171420054](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191207141823787.png)

```java
/*
 * InetAddress类
 */
public class Test01 {

    public static void main(String[] args) throws UnknownHostException {
        //获取本机的InetAddress实例
        InetAddress address = InetAddress.getLocalHost();
        System.out.println("计算名："+address.getHostName());
        System.out.println("IP地址："+address.getHostAddress());
        byte[] bytes = address.getAddress();//获取字节数组形式的IP地址
        System.out.println("字节数组形式的IP:"+ Arrays.toString(bytes));
        System.out.println(address);//直接输出InetAddress对象
        //根据机器名获取InetAddress实例
        InetAddress address2 = InetAddress.getByName("LAPTOP-QQ7UOKQV");
        System.out.println("计算机名："+address2.getHostName());
        System.out.println("IP地址："+address2.getHostAddress());
        
        //根据ip地址获取iNetaddress实例
        InetAddress address3 = InetAddress.getByName("192.168.1.151");
        System.out.println("计算机名："+address3.getHostName());
        System.out.println("IP地址："+address3.getHostAddress());
    }
}
```

## URL类

![image-20191127171337781](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127171337781.png)

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127171329278.png" alt="image-20191127171329278" style="zoom:50%;" />

```java
import java.net.MalformedURLException;
import java.net.URL;

public class Test02 {
    public static void main(String[] args) {
        try {
            //创建一个URL实例
            URL imooc = new URL("http://www.imooc.com");
            //有已存在的url实例创建一个新的url
            URL url = new URL(imooc,"/index.html?username=tom#test");//?后面表示参数，#后面表示瞄点
            System.out.println("协议："+url.getProtocol());
            System.out.println("主机: "+url.getHost());
            //未指定端口号，则使用默认的端口号,此时getport（）方法返回值为-1
            System.out.println("端口: "+url.getPort());
            System.out.println("文件路径："+url.getPath());
            System.out.println("文件名："+url.getFile());
            System.out.println("相对路径: "+url.getRef());
            System.out.println("查询字符串："+url.getQuery());
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
    }
}

```

![image-20191127172417621](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127172417621.png)

```java
/*
 * 使用url读取页面内容
 */
public class Test03 {
    public static void main(String[] args) {
        //创建一个url实例
        try {
            URL url = new URL("http://www.baidu.com");
            //通过URL的openStream方法获取URL对象所表示的资源的字节输入流
            InputStream is = url.openStream();
            //将字节输入流转换为字符输入流
            InputStreamReader isr = new InputStreamReader(is,"utf-8");
            //为字符输入流添加缓冲
            BufferedReader br = new BufferedReader(isr);
            //读取输入流,读取数据
            String data = br.readLine();
            while (data!=null){
                System.out.println(data);
                data = br.readLine();
            }
            // 关闭相应资源
            br.close();
            isr.close();
            is.close();
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

# 第三章 通过socket实现TCP编程

## Socket简介

![image-20191127195001486](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127195001486.png)

![image-20191127195104422](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127195104422.png)

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127195158183.png" alt="image-20191127195158183" style="zoom:67%;" />

>  具体方法从api文档里面看

## 基于TCP的Socket通信——服务端

以登录界面为例子

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127195552883.png" alt="image-20191127195552883" style="zoom:75%;" />

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127195629220.png" alt="image-20191127195629220" style="zoom:80%;" />

![image-20191127195703959](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127195703959.png)

```java
/*
 * 基于TCP协议的Socket通信,实现用户登录
 * 服务器端
 */
public class Server {

    public static void main(String[] args) {
        try {
            //1.创建一个服务器端socket，即ServerSocket，指定绑定的端口并监听此端口
            ServerSocket serverSocket = new ServerSocket(8888);
            //2.调用accept()方法开始监听，等待客户端的连接
            System.out.println("****服务器即将启动，等待客户端的连接***");
            Socket socket = serverSocket.accept();
            //3.获取输入流，用来读取客户端发送的信息
            InputStream is = socket.getInputStream();//字节输入流
            InputStreamReader isr = new InputStreamReader(is);//将字节流转换为字符流
            BufferedReader br = new BufferedReader(isr);//为输入流添加缓冲
            String info = null;
            while((info = br.readLine())!=null){
                //循环读取客户端的信息
                System.out.println("我是服务器，客户端说："+info);
            }socket.shutdownInput();//关闭输入流
            //4.关闭资源
            br.close();
            isr.close();
            is.close();
            socket.close();
            serverSocket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 基于TCP的Socket通信——客户端

![image-20191127195737841](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127195737841.png)

```java
/*
 * 客户端
 */
public class Client {

    public static void main(String[] args) {
        try {
            //1.创建客户端Socket，指定服务器地址和端口
            Socket socket = new Socket("localhost", 8888);
            //2.获取输出流，向服务器端发出信息
            OutputStream os = socket.getOutputStream();//字节输出流
            PrintWriter pw = new PrintWriter(os);//将输出流包装为打印流
            pw.write("用户名:admin;密码:123");
            pw.flush();
            socket.shutdownOutput();//关闭输出流
            //3.关闭资源
            pw.close();
            os.close();
            socket.close();
        }catch (UnknownHostException e){
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

![image-20191127202852468](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127202852468.png)

## 完善用户登陆之服务器响应客户端

![image-20191127202331767](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127202331767.png)

服务器端修改

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;
/*
 * 基于TCP协议的Socket通信,实现用户登录
 * 服务器端
 */
public class Server {

    public static void main(String[] args) {
        try {
            //1.创建一个服务器端socket，即ServerSocket，指定绑定的端口并监听此端口
            ServerSocket serverSocket = new ServerSocket(8888);
            //2.调用accept()方法开始监听，等待客户端的连接
            System.out.println("****服务器即将启动，等待客户端的连接***");
            Socket socket = serverSocket.accept();
            //3.获取输入流，用来读取客户端发送的信息
            InputStream is = socket.getInputStream();//字节输入流
            InputStreamReader isr = new InputStreamReader(is);//将字节流转换为字符流
            BufferedReader br = new BufferedReader(isr);//为输入流添加缓冲
            String info = null;
            while((info = br.readLine())!=null){
                //循环读取客户端的信息
                System.out.println("我是服务器，客户端说："+info);
            }socket.shutdownInput();//关闭输入流
            
            //4.获取输出流，用来响应客户端的请求
            OutputStream os = socket.getOutputStream();
            PrintWriter pw = new PrintWriter(os);//包装为打印流
            pw.write("欢迎您！");
            pw.flush();//调用flush()方法将缓冲输出

            //5.关闭资源
            pw.close();
            os.close();
            br.close();
            isr.close();
            is.close();
            socket.close();
            serverSocket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

客户端修改

```java
import java.io.*;
import java.net.Socket;
import java.net.UnknownHostException;

/*
 * 客户端
 */
public class Client {

    public static void main(String[] args) {
        try {
            //1.创建客户端Socket，指定服务器地址和端口
            Socket socket = new Socket("localhost", 8888);
            //2.获取输出流，向服务器端发出信息
            OutputStream os = socket.getOutputStream();//字节输出流
            PrintWriter pw = new PrintWriter(os);//将输出流包装为打印流
            pw.write("用户名:admin;密码:123");
            pw.flush();
            socket.shutdownOutput();//关闭输出流

            //3.获取输入流，并读取服务端的响应信息
            InputStream is = socket.getInputStream();
            BufferedReader br = new BufferedReader(new InputStreamReader(is));//包装为字符流并添加缓冲
            String info = null;
            while ((info = br.readLine())!=null){
                System.out.println("我是客户端，服务端说:"+info);
            }
            //4.关闭资源
            br.close();
            is.close();
            pw.close();
            os.close();
            socket.close();
        }catch (UnknownHostException e){
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127202922402.png" alt="image-20191127202922402" style="zoom: 80%;" />

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127202927637.png" alt="image-20191127202927637" style="zoom:80%;" />

再来复习一下这个过程

![image-20191127203030846](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127203030846.png)

## 使用多线程实现客户端的通信

实现服务器与多客户端之间的通信

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127203237666.png" alt="image-20191127203237666" style="zoom: 67%;" />

![image-20191127203326486](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127203326486.png)

我们新创建一个服务器线程处理类SeverThread

```java
/*
 * 服务器线程处理类
 */
import java.io.*;
import java.net.Socket;

public class ServerThread  extends Thread{
    //和本线程相关的socket
    Socket socket = null;

    public ServerThread(Socket socket){
        this.socket = socket;
    }

    //线程执行的操作，响应客户端的请求
    public void run(){
        InputStream is = null ;
        InputStreamReader isr = null;
        BufferedReader br = null;
        OutputStream os = null;
        PrintWriter pw = null;
        try {
            //获取输入流，并读取客户端信息
            is = socket.getInputStream();
            isr = new InputStreamReader(is);//将字节流转换为字符流
            br = new BufferedReader(isr);//为输入流添加缓冲
            String info = null;
            while((info = br.readLine())!=null){
                //循环读取客户端的信息
                System.out.println("我是服务器，客户端说："+info);
            }socket.shutdownInput();//关闭输入流
            
            //获取输出流，用来响应客户端的请求
            os = socket.getOutputStream();
            pw = new PrintWriter(os);//包装为打印流
            pw.write("欢迎您！");
            pw.flush();//调用flush()方法将缓冲输出
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //5.关闭资源
            try {
                if(pw!=null) pw.close();
                if(os!=null) os.close();
                if(br!=null) br.close();
                if(isr!=null) isr.close();
                if(is!=null) is.close();
                if (socket!=null) socket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}

```

修改服务端程序

```java
import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

/*
 * 基于TCP协议的Socket通信,实现用户登录
 * 服务器端
 */
public class Server {

    public static void main(String[] args) {
        try {
            //创建一个服务器端socket，即ServerSocket，指定绑定的端口并监听此端口
            ServerSocket serverSocket = new ServerSocket(8888);
            Socket socket = null;
            //定义变量，记录客户端的数量
            int count = 0;
            System.out.println("****服务器即将启动，等待客户端的连接***");
            //循环监听等待客户端连接
            while (true){
                //调用accept()方法开始监听，等待客户端的连接
                socket = serverSocket.accept();
                //创建一个新的线程
                ServerThread serverThread = new ServerThread(socket);
                //启动线程
                serverThread.start();
                count++;//统计客户端的数量
                System.out.println("客户顿的数量："+count);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

我们先启动server，启动client，再启动一次client

可以看到，两次启动client，其输出均为

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127204858134.png" alt="image-20191127204858134" style="zoom:80%;" />

server的输出为

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127204927373.png" alt="image-20191127204927373" style="zoom:80%;" />

并且服务端一直在循环监听

![image-20191127204950165](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127204950165.png)

这就实现了多客户端的同时登陆

这时我们继续完善程序，获取客户端的地址和主机名

```java
while (true){
                //调用accept()方法开始监听，等待客户端的连接
                socket = serverSocket.accept();
                //创建一个新的线程
                ServerThread serverThread = new ServerThread(socket);
                //启动线程
                serverThread.start();
                count++;//统计客户端的数量
                //--------
                System.out.println("客户顿的数量："+count);
                InetAddress address = socket.getInetAddress();
                System.out.println("当前客户端的IP "+address.getHostAddress());
            }
```

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127205251029.png" alt="image-20191127205251029" style="zoom:80%;" />

# 第四章 通过socket实现UDP编程

## DatagramPcket

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127210944378.png" alt="image-20191127210944378" style="zoom:80%;" />

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127211141278.png" alt="image-20191127211141278" style="zoom:80%;" />

## 编程实现基于UDP的Socket——服务端

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127211221046.png" alt="image-20191127211221046" style="zoom:80%;" />

```java
package com.sxc;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

/*
 * 服务器端，实现基于UDP的用户登录
 */
public class UDPServer {

    public static void main(String[] args) throws IOException {
        //1.创建服务器端DatagramSocket，指定端口
        DatagramSocket socket = new DatagramSocket(8800);
        //2.创建数据包，用于接收客户端发送的数据
        byte[] data = new byte[1024];
        DatagramPacket packet = new DatagramPacket(data,data.length);
        //3.接收客户端发送的数据
        System.out.println("----服务器端已经启动，等待客户端发送数据---");
        socket.receive(packet);//此方法在接收到数据报之前会一直阻塞
        //4.读取数据
        String info = new String(data,0,packet.getLength());
        System.out.println("我是服务器，客户端说："+info);
        
        /*
         * 向客户端响应数据
         */
        //1.定义客户端的地址，端口号，数据
        InetAddress address = packet.getAddress();
        int port = packet.getPort();
        byte[] data2 ="欢迎您！".getBytes();
        //2.创建数据报，包含响应的数据信息
        DatagramPacket packet2 = new DatagramPacket(data2,data2.length,address,port);
        //3.响应客户端
        socket.send(packet2);
        //4.关闭资源
        socket.close();
    }
}

```

## 编程实现基于UDP的Socket——客户端

![image-20191127211252911](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127211252911.png)

```java
package com.sxc;

/*
 * 客户端
 */
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

public class UDPClient {
    public static void main(String[] args) throws IOException {
        /*
         * 向服务器端发送数据
         */
        //1.定义服务器的地址，端口号，数据
        InetAddress address = InetAddress.getByName("localhost");
        int port = 8800;
        byte[] data = "用户名；admin；密码：123".getBytes();
        //2.创建数据报，包含发送的数据信息
        DatagramPacket packet = new DatagramPacket(data,data.length,address,port);
        //3.创建DatagranSocket对象
        DatagramSocket socket = new DatagramSocket();
        //4.向服务器端发送数据报
        socket.send(packet);
        
        /*
         * 接收服务器端响应的数据
         */
        //1.创建数据报，用于接收服务器端响应的数据
        byte[] data2 = new byte[1024];
        DatagramPacket packet2 = new DatagramPacket(data2,data2.length);
        //2.接收服务器端相应的数据
        socket.receive(packet2);
        //3.读取数据
        String reply = new String(data2,0,packet2.getLength());
        System.out.println("我是客户端，服务器说："+reply);
        //4.关闭资源
        socket.close();
    }
}

```

运行服务端，之后运行客户端

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127213403205.png" alt="image-20191127213403205" style="zoom:80%;" />

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127213411518.png" alt="image-20191127213411518" style="zoom:80%;" />

![image-20191127213454132](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127213454132.png)

如何实现服务器和多客户端之间的通信？使用多线程

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127213524965.png" alt="image-20191127213524965" style="zoom:50%;" />

# 第五章 socket总结

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127213605313.png" alt="image-20191127213605313" style="zoom: 50%;" />

其他注意的点 

![image-20191127213643397](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127213643397.png)

![image-20191127213707401](C:\Users\SXC\Desktop\3D建模\image-20191127213707401.png)

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127213731261.png" alt="image-20191127213731261" style="zoom:67%;" />

刚才的案例中我们使用的都是字符串数组传输数据，实际应用中我们会使用对象（用户封装成一个user对象）。这个时候要使用对象的传输。

![image-20191127213907093](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127213907093.png)

<img src="C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127213917673.png" alt="image-20191127213917673" style="zoom:80%;" />

刚才我们服务端和客户端传递的是字符串，如果我们在两端中间要传递文件呢。（涉及到文件读入操作）

![image-20191127213958728](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127213958728.png)

![image-20191127214016628](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127214016628.png)

# 第六章 综合练习

## ![image-20191127214052565](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127214052565.png)

![image-20191127214112640](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127214112640.png)

![image-20191127214131612](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127214131612.png)

![image-20191127214142580](C:\Users\SXC\AppData\Roaming\Typora\typora-user-images\image-20191127214142580.png)

参考实现：https://blog.csdn.net/su20145104009/article/details/52843735