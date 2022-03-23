# TypeScript入门指南

# ch1 课程介绍

![image-20210508153147943](TypeScript入门指南.assets/image-20210508153147943.png)

![image-20210508153646078](TypeScript入门指南.assets/image-20210508153646078.png)

# ch2 了解TypeScript 工作流

## 2-1 什么是 TypeScript

![image-20210508153906209](TypeScript入门指南.assets/image-20210508153906209.png)

注：ts无法再浏览器中运行，需要编译器将ts转换（翻译）成JS。

**Typing强类型**

* 规范我们的代码
* 使我们代码编译阶段就能及时发现错误
* 在原生js的基础上加上了一层类型定义

**为什么要使用Typescript**

![image-20210508154649689](TypeScript入门指南.assets/image-20210508154649689.png)

* 类型推演与类型匹配
* 开发编译时报错
* 极大程度的避免了低级错误
* 支持Javascript最新特性（包含ES6\7\8)

改后的代码：

![image-20210508161943980](TypeScript入门指南.assets/image-20210508161943980.png)

##  2-2 开发环境配置

官网：https://www.typescriptlang.org/

安装：`npm install -g typescript`

打开terminal快捷键： ctrl + `

```bash
#编译ts：
tsc main.ts
#运行js：
node main.js
```

##  2-3 typescript工作流

![image-20210508170310886](TypeScript入门指南.assets/image-20210508170310886.png)

* ES(ECMAScript):ECMA International负责维护版本
* ES3、ES5：支持目前所有主流浏览器
* ES6 = ES2015：目前浏览器不支持，但未来一定会支持，ES2016（ES7），ES2017（ES8）

新建一个ts项目

```bash
npm init
# lite-server用于开发的一个轻量级服务器（自动部署在localhost3000上）
npm install --save-dev lite-server
npm start
```

注意：package.json中的lite-server设置

```json
{
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start":"lite-server"
  },
  "devDependencies": {
    "lite-server": "^2.6.1"
  }
}
```



# 第3章 TypeScript 基础

- [3-1 变量声明(05:23)](https://www.imooc.com/video/23204)
- [3-2 TypeScript类型简介(02:08)](https://www.imooc.com/video/23205)
- [3-3 数字、布尔、与字符串(07:40)](https://www.imooc.com/video/23206)
- [3-4 数组(Array)和元组(Tupple)(08:27)](https://www.imooc.com/video/23207)
- [3-5 联合(Union)与字面量(Literal)类型(07:50)](https://www.imooc.com/video/23208)
- [3-6 枚举类型 Enum(03:39)](https://www.imooc.com/video/23209)
- [3-7 Any 与 unknow(05:22)](https://www.imooc.com/video/23210)
- [3-8 void、undefined 与 Never-(06:22)](https://www.imooc.com/video/23211)
- [3-9 类型适配 Type Assertions(03:44)](https://www.imooc.com/video/23212)
- [3-10 函数类型(03:57)](https://www.imooc.com/video/23213)

# 第4章 TypeScript 面对对象

- [4-1 object对象类型(05:52)](https://www.imooc.com/video/23214)
- [4-2 Interface 接口(05:43)](https://www.imooc.com/video/23215)
- [4-3 class 类(11:37)](https://www.imooc.com/video/23216)
- [4-4 Access Modifier 访问修饰符(13:25)](https://www.imooc.com/video/23217)
- [4-5 Module 模块(03:06)](https://www.imooc.com/video/23218)
- [4-6 Generics 泛型(07:30)](https://www.imooc.com/video/23219)
- [4-7 课程总结(04:06)
    ](https://www.imooc.com/video/23221)