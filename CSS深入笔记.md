# CSS3深入笔记

来源：慕课网2019前端工程师职位 - 页面化妆师CSS3

# ch1 CSS3选择器

<img src="CSS深入笔记.assets/image-20200918204946030.png" alt="image-20200918204946030" style="zoom: 50%;" />

## 1.基本选择器

### 子元素选择器（直接后代选择器）

<img src="CSS深入笔记.assets/image-20200918204955234.png" alt="image-20200918204955234" style="zoom: 50%;" />

一个小技巧：vscode中快速生成

<img src="CSS深入笔记.assets/image-20200918211204597.png" alt="image-20200918211204597" style="zoom: 67%;" />

```html
<head>
  <style type="text/css">
    section > div{
      color: #f00;
    }
  </style>
</head>
<body>
  <section>
    <div>article外面的文字</div>
    <article>
      <div>article里面的文字</div>
    </article>
  </section>
</body>
```

<img src="CSS深入笔记.assets/image-20200918211517774.png" alt="image-20200918211517774" style="zoom:50%;" />

### 相邻兄弟选择器

<img src="CSS深入笔记.assets/image-20200918205012213.png" alt="image-20200918205012213" style="zoom:50%;" />

```html
<head>
  <style type="text/css">
    section > div + article{
      color: #f00;
    }
  </style>
</head>
<body>
  <section>
    <div>article外面的文字</div>
    <article>
      <div>article里面的文字</div>
    </article>
    <article>
      <div>article里面的文字</div>
    </article>
  </section>
</body>
```

<img src="CSS深入笔记.assets/image-20200918211851706.png" alt="image-20200918211851706" style="zoom: 50%;" />

### 通用兄弟选择器

<img src="CSS深入笔记.assets/image-20200918205027751.png" alt="image-20200918205027751" style="zoom:50%;" />

```html
<head>
  <style type="text/css">
    section > div ~ article{
      color: #f00;
    }
  </style>
</head>
<body>
  <section>
    <article>
      <div>article里面的文字</div>
    </article>
    <div>article外面的文字</div>
    <article>
      <div>article里面的文字</div>
    </article>
    <article>
      <div>article里面的文字</div>
    </article>
    <article>
      <div>article里面的文字</div>
    </article>
  </section>
</body>
```

<img src="CSS深入笔记.assets/image-20200918212109225.png" alt="image-20200918212109225" style="zoom:50%;" />

### 群组选择器

<img src="CSS深入笔记.assets/image-20200918205037379.png" alt="image-20200918205037379" style="zoom:50%;" />

```html
<head>
  <style type="text/css">
    section > article,
    section > aside,
    section > div{
      color: #ff0000;
        margin: 10px 0;
      background: #abcdef;
    }
  </style>
</head>
<body>
  <section>
    <article>article</article>
    <aside>aside</aside>
    <div>div</div>
  </section>
</body>
</html>
```

<img src="CSS深入笔记.assets/image-20200918212426350.png" alt="image-20200918212426350" style="zoom:50%;" />

## 2.属性选择器

<img src="CSS深入笔记.assets/image-20200918212647967.png" alt="image-20200918212647967" style="zoom: 67%;" />

### `Element[attribute]`

![image-20200918213319904](CSS深入笔记.assets/image-20200918213319904.png)

### `Element[attribute="value"]`

<img src="CSS深入笔记.assets/image-20200918205110619.png" alt="image-20200918205110619" style="zoom:50%;" />

```html
<style type="text/css">
    a[href="#"]{
      color: red;
    }
    a[href~="#"]{
      color: green;
    }
  </style>
</head>
<body>
  <a href="attribute.html">attribute</a>
  <a href="#">attribute</a>
  <a href="#1">attribute</a>
  <a href="#2">attribute</a>
  <a href="#3">attribute</a>
  <a href="#4">attribute</a>
</body>
```

<img src="CSS深入笔记.assets/image-20200918213758023.png" alt="image-20200918213758023" style="zoom:50%;" />

### `Element[attribute~"value"]`

<img src="CSS深入笔记.assets/image-20200918205119160.png" alt="image-20200918205119160" style="zoom:50%;" />

```html
<style type="text/css">
    a[class~="two"]{
      color: #ffff00;;
    }
  </style>
</head>
<body>
  <a href="attribute.html">attribute</a>
  <a href="#">attribute</a>
  <a class="one two" href="#">attribute</a>
  <a class="two there"href="#">attribute</a>
  <a href="#1">attribute</a>
</body>
```

<img src="CSS深入笔记.assets/image-20200918213854208.png" alt="image-20200918213854208" style="zoom:50%;" />

### `Element[attribute^="value"]`

<img src="CSS深入笔记.assets/image-20200918205124964.png" alt="image-20200918205124964" style="zoom:50%;" />

```html
<head>
  <style type="text/css">
    a[href^="#"]{
      color: green;
    }
  </style>
</head>
<body>
  <a href="attribute.html">attribute</a>
  <a href="#">attribute</a>
  <a href="#1">attribute</a>
  <a href="#2">attribute</a>
</body>
</html>
```

<img src="CSS深入笔记.assets/image-20200918214002569.png" alt="image-20200918214002569" style="zoom:50%;" />

### `Element[attribute$="value"]`

<img src="CSS深入笔记.assets/image-20200918205136085.png" alt="image-20200918205136085" style="zoom:50%;" />

```html
<style type="text/css">
    a[href$="#"]{
      color:lightblue;
    }
  </style>
</head>
<body>
  <a href="#">attribute</a>
  <a href="#1">attribute</a>
  <a href="#2">attribute</a>
  <a href="1#">attribute</a>
  <a href="2#">attribute</a>
</body>

```

<img src="CSS深入笔记.assets/image-20200918214124245.png" alt="image-20200918214124245" style="zoom:50%;" />

### `Element[attribute*="value"]`

<img src="CSS深入笔记.assets/image-20200918205155994.png" alt="image-20200918205155994" style="zoom:50%;" />

```html
<!DOCTYPE html>
 <style type="text/css">
    a[href*="#"]{
      color:lightblue;
    }
  </style>
</head>
<body>
  <a href="attribute.html">attribute</a>
  <a href="#">attribute</a>
  <a href="#1">attribute</a>
  <a href="#2">attribute</a>
  <a href="1#">attribute</a>
  <a href="2#">attribute</a>
  <a href="1#1">attribute</a>
  <a href="2#2">attribute</a>
</body>
</html>
```

<img src="CSS深入笔记.assets/image-20200918214327856.png" alt="image-20200918214327856" style="zoom:50%;" />

### `Element[attribute|="value"]`

<img src="CSS深入笔记.assets/image-20200918214358384.png" alt="image-20200918214358384" style="zoom: 67%;" />

```html
<style type="text/css">
    a[href|="#"]{
      color:gold;
    }
  </style>
</head>
<body>
  <a href="attribute.html">attribute</a>
  <a href="#">attribute</a>
  <a href="#1">attribute</a>
  <a href="#2">attribute</a>
  <a href="1#">attribute</a>
  <a href="2#">attribute</a>
  <a href="#-1">attribute</a>
  <a href="#-2">attribute</a>
</body>
```

<img src="CSS深入笔记.assets/image-20200920161113300.png" alt="image-20200920161113300" style="zoom:50%;" />

## 3.伪类选择器

<img src="CSS深入笔记.assets/image-20200918205218832.png" alt="image-20200918205218832" style="zoom: 33%;" />

### 动态伪类

<img src="CSS深入笔记.assets/image-20200918205226902.png" alt="image-20200918205226902" style="zoom:33%;" />

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style type="text/css">
    input{
      width: 200px;
      height: 30px;
      border:5px solid #f00
    }
    input:focus{
      background: #abcdef;
    }
  </style>
</head>
<body>
  <input type="text">
```

<img src="CSS深入笔记.assets/image-20200920162305704.png" alt="image-20200920162305704" style="zoom:33%;" />

光标进入变成蓝色

<img src="CSS深入笔记.assets/image-20200920162322548.png" alt="image-20200920162322548" style="zoom:33%;" />

### UI元素状态伪类

<img src="CSS深入笔记.assets/image-20200918205232660.png" alt="image-20200918205232660" style="zoom:33%;" />

```html
 <style type="text/css">
    input{
      width: 100px;
      height: 30px;
    }
    input:enabled{
      border:1px solid red
    }
    input:disabled{
      background: #abcdef;
      border: 1px solid yellow;
    }
  </style>
</head>
<body>
  <input type="text" disabled>
  <input type="text" >
  <input type="text" >
  <input type="text" >
</body>
```

<img src="CSS深入笔记.assets/image-20200920162800060.png" alt="image-20200920162800060" style="zoom:33%;" />

### CSS3结构类

<img src="CSS深入笔记.assets/image-20200918205241193.png" alt="image-20200918205241193" style="zoom:33%;" />

#### `Element:first-child`

<img src="CSS深入笔记.assets/image-20200918205246208.png" alt="image-20200918205246208" style="zoom:33%;" />

```html
  <style type="text/css">
   section:first-child{
     color:red
   }
  </style>
</head>
<body>
  <section>
    <div>1-1</div>
    <div>1-2</div>
    <div>1-3</div>
  </section>
  <section>
    <div>2-1</div>
    <div>2-2</div>
    <div>2-3</div>
  </section>
```

![image-20200920163226082](CSS深入笔记.assets/image-20200920163226082.png)

```html
  <style type="text/css">
   div:first-child{
     color:red
   }
  </style>
</head>
<body>
  <div>0-1</div>
  <div>0-2</div>
  <div>0-3</div>
  <section>
    <div>1-1</div>
    <div>1-2</div>
    <div>1-3</div>
  </section>
  <section>
    <div>2-1</div>
    <div>2-2</div>
    <div>2-3</div>
  </section>
```

<img src="CSS深入笔记.assets/image-20200920163323688.png" alt="image-20200920163323688" style="zoom:33%;" />

配合其他选择器使用

#### `Element:last-child`

<img src="CSS深入笔记.assets/image-20200918205250833.png" alt="image-20200918205250833" style="zoom:33%;" />

```html
  <style type="text/css">
   div:last-child{
     color:red
   }
  </style>
</head>
<body>
  <div>0-1</div>
  <div>0-2</div>
  <div>0-3</div>
  <section>
    <div>1-1</div>
    <div>1-2</div>
    <div>1-3</div>
  </section>
  <section>
    <div>2-1</div>
    <div>2-2</div>
    <div>2-3</div>
  </section>
```

<img src="CSS深入笔记.assets/image-20200920163745239.png" alt="image-20200920163745239" style="zoom:33%;" />

#### `Element:nth-child(N)`

<img src="CSS深入笔记.assets/image-20200918205256781.png" alt="image-20200918205256781" style="zoom:33%;" />

```html
  <style type="text/css">
   ul>li:nth-child(3){
     background: red;
   }
  </style>
</head>
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <li>5</li>
    <li>6</li>
    <li>7</li>
    <li>8</li>
  </ul>
  <hr>
```

<img src="CSS深入笔记.assets/image-20200921150623935.png" alt="image-20200921150623935" style="zoom:33%;" />

存疑：这道题不会做

![image-20200921151826015](CSS深入笔记.assets/image-20200921151826015.png)

![image-20200921152045091](CSS深入笔记.assets/image-20200921152045091.png)

#### 参数N的选择

<img src="CSS深入笔记.assets/image-20200918205307929.png" alt="image-20200918205307929" style="zoom: 50%;" />

例如

```css
div:nth-child(2n)
div:nth-child(3n+1)
```

#### `Element:nth-last-child(N)`

<img src="CSS深入笔记.assets/image-20200918205321152.png" alt="image-20200918205321152" style="zoom:50%;" />

可以理解为：计数时，不按类型，但是显示的时候，还是要看的

#### `Element:nth-of-type(N)`

<img src="CSS深入笔记.assets/image-20200918205344909.png" alt="image-20200918205344909" style="zoom:50%;" />

可以理解为：计数时，就数特定的类型

<img src="CSS深入笔记.assets/image-20200921153736663.png" alt="image-20200921153736663" style="zoom:50%;" />

```html
<style type="text/css">
    div:nth-of-type(2){
      color: red;/*只属div的第二个*/
    }
  </style>
</head>
<body>
  <div>0-1</div>
  <section>
    <div>1-1</div>
    <div>1-2</div>
    <div>1-3</div>
  </section>
  <div>0-2</div>
  <div>0-3</div>
  <section>
    <div>2-1</div>
    <div>2-2</div>
    <div>2-3</div>
  </section>
</body>
```

<img src="CSS深入笔记.assets/image-20200921154034487.png" alt="image-20200921154034487" style="zoom:50%;" />

#### `Element:nth-last-of-typr(N)`

<img src="CSS深入笔记.assets/image-20200918210424227.png" alt="image-20200918210424227" style="zoom:50%;" />

```html
 <style type="text/css">
    div:nth-of-type(2){
      color: red;
    }
  </style>
</head>

<body>
  <div>0-1</div>
  <section>
    <div>1-1</div>
    <div>1-2</div>
    <div>1-3</div>
  </section>
  <div>0-2</div>
  <div>0-3</div>
  <section>
    <div>2-1</div>
    <div>2-2</div>
    <div>2-3</div>
  </section>
</body>

</html>
```

![image-20200921154310531](CSS深入笔记.assets/image-20200921154310531.png)

![image-20200921154328129](CSS深入笔记.assets/image-20200921154328129.png)

<img src="CSS深入笔记.assets/image-20200921154440880.png" alt="image-20200921154440880" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20200918210433751.png" alt="image-20200918210433751" style="zoom:50%;" />

<img src="CSS深入笔记.assets/image-20200918210452231.png" alt="image-20200918210452231" style="zoom:50%;" />

#### `Element:only-child`

<img src="CSS深入笔记.assets/image-20200918210535827.png" alt="image-20200918210535827" style="zoom:50%;" />

（找独生子女哈哈哈哈哈）

<img src="CSS深入笔记.assets/image-20200918210541328.png" alt="image-20200918210541328" style="zoom:50%;" />

#### `Element:empty`

<img src="CSS深入笔记.assets/image-20200918210615371.png" alt="image-20200918210615371" style="zoom:50%;" />

### 分类选择器

##### :not

<img src="CSS深入笔记.assets/image-20200918210620147.png" alt="image-20200918210620147" style="zoom:50%;" />

```html
  <style type="text/css">
  *{
    padding: 0;
    margin: 0;
    border: none;
  }
  a{
    text-decoration: none;
    display: block;
    float: left;  
    width: 100px;
    height: 30px;
  }
  nav{
    width: 800px;
    margin: 0 auto;
  }
  /*最后一个标签后面不需要*/
  nav > a:not(:last-of-type){
    border-right: 1px solid red;
  }
  </style>
</head>

<body>
  <!--写导航条-->
  <nav>
    <a href="#">first</a>
    <a href="#">second</a>
    <a href="#">third</a>
    <a href="#">fourth</a>
    <a href="#">five</a>
  </nav>
</body> 

</html>
```

![image-20200921161906342](CSS深入笔记.assets/image-20200921161906342.png)

#### CSS权重

<img src="CSS深入笔记.assets/image-20200921162125016.png" alt="image-20200921162125016" style="zoom: 50%;" />

<img src="CSS深入笔记.assets/image-20200921162200186.png" alt="image-20200921162200186" style="zoom:50%;" />

### 伪元素

<img src="CSS深入笔记.assets/image-20200921162550075.png" alt="image-20200921162550075" style="zoom:50%;" />

<img src="CSS深入笔记.assets/image-20200921162840276.png" alt="image-20200921162840276" style="zoom:50%;" />

 

<img src="CSS深入笔记.assets/image-20200921162634553.png" alt="image-20200921162634553" style="zoom:50%;" />

<img src="CSS深入笔记.assets/image-20200918210650621.png" alt="image-20200918210650621" style="zoom: 33%;" />

<img src="CSS深入笔记.assets/image-20200921163849362.png" alt="image-20200921163849362" style="zoom:80%;" />

<img src="CSS深入笔记.assets/image-20200918210658179.png" alt="image-20200918210658179" style="zoom:33%;" />

比较实用，经常用于清除浮动

在这里为了清除header的浮动，让其高度由子元素撑开

```html
<head>
  <title>Document</title>
  <style type="text/css">
  /*清除浮动*/
  header::after{
    display: block;
    content: "";
    clear: both;
  }
  header{
    background: #abcdef;
  }
  header > article{
    float: left;
    width: 40%;
    height: 30px;
    background: #ff0000;
  }
  header > aside{
    float: right;
    width: 40%;
    height: 50px;
    background: #ff0;
  }
  </style>
</head>
<body>
  <header>
    <article></article>
    <aside></aside>
  </header>
</body> 

```

![image-20200921200920010](CSS深入笔记.assets/image-20200921200920010.png)

<img src="CSS深入笔记.assets/image-20200918210707726.png" alt="image-20200918210707726" style="zoom:50%;" />

```html
<style type="text/css">
  div::selection{
    background: red;
    color: #ff0;
  }
  </style>
</head>
<body>
  <div>
    选中部分成为红底白字哈哈哈哈哈
  </div>
</html>
```

![image-20200921201141635](CSS深入笔记.assets/image-20200921201141635.png)

# ch2 CSS3边框与圆角

## 1. CSS3圆角

![image-20200927153942814](CSS深入笔记.assets/image-20200927153942814.png)

```css
border-radius:5em;/*浏览器的默认字体的5em*/
border-radius:50%;/*关于自己的百分比，宽高一样则变成一正圆形*/
```

<img src="CSS深入笔记.assets/image-20200927154328944.png" alt="image-20200927154328944" style="zoom: 67%;" />

<img src="CSS深入笔记.assets/image-20200927154356887.png" alt="image-20200927154356887" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20200927154408335.png" alt="image-20200927154408335" style="zoom:67%;" />

![image-20200927154512657](CSS深入笔记.assets/image-20200927154512657.png)

考虑兼容性

![image-20200927155509153](CSS深入笔记.assets/image-20200927155509153.png)

一个案例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>talk</title>
  <style type="text/css">
    div{
      position: relative;
      width: 500px;
      height: 300px;
      border: 1px solid black;
      border-radius:50%;
      font-size: 24px;
      font-weight: bold;
      text-align: center;
      line-height: 300px;
    }
    div:before,
    div:after{
      position: absolute;/*子绝父相*/
      content: "";
      width: 50px;
      height: 50px;
      display: block;
      border:1px solid black;
      border-radius: 50%;
    }
    div:before{
      width: 50px;
      height: 50px;
      bottom: -20px;
      right: 0;
    }
    div:after{
      width: 50px;
      height: 50px;
      bottom: -75px;
      right: -75px;
    }
  </style>
</head>
<body>
  <div>大家好，欢迎学习CSS！</div>
</body>
</html>
```

![image-20200927160348372](CSS深入笔记.assets/image-20200927160348372.png)

## 2. CSS3盒阴影

![image-20200927160444997](CSS深入笔记.assets/image-20200927160444997.png)

```css
    div{
      position: relative;
      width: 500px;
      height: 300px;
      margin: 0 auto;
      background: red;
      box-shadow: 50px 20px 0 0 yellow;
    }
```

<img src="CSS深入笔记.assets/image-20200927160638969.png" alt="image-20200927160638969" style="zoom:33%;" />

## 3. CSS3边界图片

![image-20200927161213120](CSS深入笔记.assets/image-20200927161213120.png)

![image-20200927161425881](CSS深入笔记.assets/image-20200927161425881.png)

![image-20200927161505623](CSS深入笔记.assets/image-20200927161505623.png)

![image-20200927161633728](CSS深入笔记.assets/image-20200927161633728.png)

![image-20200927162049777](CSS深入笔记.assets/image-20200927162049777.png)

![image-20200927162136154](CSS深入笔记.assets/image-20200927162136154.png)

# ch3 CSS3背景与渐变

## 1. 背景

![image-20200927163108158](CSS深入笔记.assets/image-20200927163108158.png)

### 背景图像区域 background-clip

![image-20200927163327036](CSS深入笔记.assets/image-20200927163327036.png)

```html
  <style type="text/css">
  *{
    margin: 0;
    padding: 0;
    border: none;
  }
  div{
    width: 800px;
    height: 400px;
    padding: 50px;
    border: 50px solid transparent;
    background: url('bg1.jpg') no-repeat center center;
    background-clip: border-box;
    /* background-clip: padding-box; */
    /* background-clip: content-box; */
  }
  span.div_border{
    position: absolute;
    top:0;
    left:0;
    width: 800px;
    height: 400px;
    padding: 50px;
    border: 50px solid rgba(255, 0, 0, 0.25);
  }
  span.div_padding{
    position: absolute;
    top:50px;
    left:50px  ;
    width: 700px;
    height: 300px;
    padding: 50px;
    border: 50px solid rgba(255, 255, 0, 0.25);
  }

  </style>
</head>
<body>
  <div></div>
  <span class="div_border"></span>
  <span class="div_padding"></span>
</body>
</html>
```

 **background-clip: border-box;**

![image-20200927164351724](CSS深入笔记.assets/image-20200927164351724.png)

**background-clip: padding-box;**

![image-20200927164427035](CSS深入笔记.assets/image-20200927164427035.png)

  **background-clip: content-box;**

![image-20200927164442408](CSS深入笔记.assets/image-20200927164442408.png)

### 背景图片定位 background-origin

![image-20200927164625311](CSS深入笔记.assets/image-20200927164625311.png)

![image-20200927164633563](CSS深入笔记.assets/image-20200927164633563.png)

![image-20200927164720343](CSS深入笔记.assets/image-20200927164720343.png)

![image-20200927164924633](CSS深入笔记.assets/image-20200927164924633.png)

![image-20200927164912484](CSS深入笔记.assets/image-20200927164912484.png)

![image-20200927164937266](CSS深入笔记.assets/image-20200927164937266.png)

![image-20200927164950524](CSS深入笔记.assets/image-20200927164950524.png)

### 背景图片大小 background-size

![image-20200927165104229](CSS深入笔记.assets/image-20200927165104229.png)

![image-20200927165157721](CSS深入笔记.assets/image-20200927165157721.png)

![image-20200927165219567](CSS深入笔记.assets/image-20200927165219567.png)

![image-20200927170320863](CSS深入笔记.assets/image-20200927170320863.png)

![image-20200927170433126](CSS深入笔记.assets/image-20200927170433126.png)

![image-20200927170645531](CSS深入笔记.assets/image-20200927170645531.png)

  ### 多重背景图像 background-image

![image-20200927170825067](CSS深入笔记.assets/image-20200927170825067.png)

### 背景属性整合 background

![image-20200927171155034](CSS深入笔记.assets/image-20200927171155034.png)

![image-20200927171454620](CSS深入笔记.assets/image-20200927171454620.png)

## 2. 渐变

![image-20200927171859603](CSS深入笔记.assets/image-20200927171859603.png)

### 线性渐变，角度，结点，透明

![image-20200927172043481](CSS深入笔记.assets/image-20200927172043481.png)

![image-20200927172310855](CSS深入笔记.assets/image-20200927172310855.png)

![image-20200927172155372](CSS深入笔记.assets/image-20200927172155372.png)

```html
  <style type="text/css">
  *{
    margin: 0;
    padding: 0;
    border: none;
  }
  div{
    width: 800px;
    height: 400px;
    background: linear-gradient(to right,red,blue);
  }

  </style>
</head>
<body>
  <div></div>
</body>
```

![image-20200927172356454](CSS深入笔记.assets/image-20200927172356454.png)

![image-20200927172406989](CSS深入笔记.assets/image-20200927172406989.png)

```css
    background: linear-gradient(to right bottom,red,blue);
```

![image-20200927172503975](CSS深入笔记.assets/image-20200927172503975.png)

```css
    background: linear-gradient(to right bottom,red,yellow,blue);
```

![image-20200927172539559](CSS深入笔记.assets/image-20200927172539559.png)

![image-20200927172658420](CSS深入笔记.assets/image-20200927172658420.png)

![image-20200927172759443](CSS深入笔记.assets/image-20200927172759443.png)

```css
background: linear-gradient(-45deg,red,yellow）
```

<img src="CSS深入笔记.assets/image-20200927172938426.png" alt="image-20200927172938426" style="zoom:33%;" />

![image-20200927173033838](CSS深入笔记.assets/image-20200927173033838.png)

```css
    background: linear-gradient(90deg,red 10%,orange 15%,yellow,green,blue,purple );/*最后一个元素不写值，默认是100%；如果第一个不写值，默认是0%*/
```

![image-20200927173403952](CSS深入笔记.assets/image-20200927173403952.png)

```css
 background: linear-gradient(90deg,rgba(255,0,0,0),rgba(255,0,0,1));
```

![image-20200927173643250](CSS深入笔记.assets/image-20200927173643250.png)

### 重复渐变

![image-20200927180948933](CSS深入笔记.assets/image-20200927180948933.png)

```css
background: repeating-linear-gradient(90deg,red 0%,blue 10%,red 20%);
```

![image-20200927181112619](CSS深入笔记.assets/image-20200927181112619.png)

### 径向渐变

![image-20200927181330876](CSS深入笔记.assets/image-20200927181330876.png)

![image-20200928093623347](CSS深入笔记.assets/image-20200928093623347.png)

![image-20200928093809701](CSS深入笔记.assets/image-20200928093809701.png)

![image-20200928093915850](CSS深入笔记.assets/image-20200928093915850.png)

![image-20200928094719195](CSS深入笔记.assets/image-20200928094719195.png)

![image-20200928095306044](CSS深入笔记.assets/image-20200928095306044.png)

![image-20200928100031471](CSS深入笔记.assets/image-20200928100031471.png)

**一个简单应用**

<img src="CSS深入笔记.assets/image-20201002220844839.png" alt="image-20201002220844839" style="zoom:33%;" />

```html
  <style type="text/css">
  *{
    margin: 0;
    padding: 0;
    
    border: none;
  }
  div{
    width: 600px;
    height: 600px;
    background-color: #abcdef;
    background-image: linear-gradient(45deg,red 25%,transparent 25%),
                linear-gradient(-45deg,red 25%,transparent 25%),
                linear-gradient(45deg,transparent 75%,red 75%),
                linear-gradient(-45deg,transparent 75%,red 75%);
    background-size: 100px 100px;
  }
  </style>
</head>
<body>
```

<img src="CSS深入笔记.assets/image-20201002221009963.png" alt="image-20201002221009963" style="zoom:50%;" />

# ch4 CSS3文本与字体

## 1.文本阴影

![image-20201002221839031](CSS深入笔记.assets/image-20201002221839031.png)

 h-shadow 水平偏移 v-shadow 竖直偏移 blur 模糊举例 color颜色

```html
<style type="text/css">
h1{
  text-shadow: 15px 25px 2px red;
}
  </style>
</head>
<body>
  <h1>text-shadow</h1>
</body>
</html>
```

![image-20201002225214338](CSS深入笔记.assets/image-20201002225214338.png)

**text-outline属性**

规定文本轮廓

**语法**

> text-outline:thickness blur color;

thickness 宽度值

## 2.换行

![image-20201002221859938](CSS深入笔记.assets/image-20201002221859938.png)

normal 使用浏览器默认规则

break-all 所有换行的地方就换行

keep-all 半角空格或连字符的地方换行

<img src="CSS深入笔记.assets/image-20201002225255248.png" alt="image-20201002225255248" style="zoom:50%;" />

<img src="CSS深入笔记.assets/image-20201002225317517.png" alt="image-20201002225317517" style="zoom:50%;" />

<img src="CSS深入笔记.assets/image-20201002221912474.png" alt="image-20201002221912474" style="zoom:50%;" />

只针对英文，针对中日韩文之类的不起作用

![image-20201002225404327](CSS深入笔记.assets/image-20201002225404327.png)

## 3. 新文本属性

![image-20201002221933124](CSS深入笔记.assets/image-20201002221933124.png)

<img src="CSS深入笔记.assets/image-20201002225444889.png" alt="image-20201002225444889" style="zoom:50%;" />

<img src="CSS深入笔记.assets/image-20201002225509949.png" alt="image-20201002225509949" style="zoom:50%;" />

<img src="CSS深入笔记.assets/image-20201002225554608.png" alt="image-20201002225554608" style="zoom:50%;" />

inherit是继承

![image-20201002221945758](CSS深入笔记.assets/image-20201002221945758.png)

![image-20201002221956352](CSS深入笔记.assets/image-20201002221956352.png)

<img src="CSS深入笔记.assets/image-20201002225632313.png" alt="image-20201002225632313" style="zoom:50%;" />

<img src="CSS深入笔记.assets/image-20201002225643095.png" alt="image-20201002225643095" style="zoom:50%;" />

<img src="CSS深入笔记.assets/image-20201002225658694.png" alt="image-20201002225658694" style="zoom:50%;" />

![image-20201002222005340](CSS深入笔记.assets/image-20201002222005340.png)

clip 修建文本  | ellipsis 修剪（带上一个省略号）|  string 自己写一个符号代替隐藏的内容

<img src="CSS深入笔记.assets/image-20201002225716099.png" alt="image-20201002225716099" style="zoom:50%;" />

**注意：** 

* `text-overflow`要起作用一定要 加上`overflow:hidden`属性

## 4. CSS3字体

之前使用的是web安全字体（windows自带的），如果电脑上没有安装这个字体。浏览器无法从系统文件中找到这个字体。

font-face让网站开发不再局限于安全字体。

<img src="CSS深入笔记.assets/image-20201002222120029.png" alt="image-20201002222120029" style="zoom: 33%;" />

### @font-face的语法规则

<img src="CSS深入笔记.assets/image-20201002222238451.png" alt="image-20201002222238451" style="zoom: 50%;" />

source 字体存放路径，可多个；

font-family和src必选，其他可选

### @font-face的取值说明

<img src="CSS深入笔记.assets/image-20201002222246825.png" alt="image-20201002222246825" style="zoom: 50%;" />

### @font-face的字体格式

<img src="CSS深入笔记.assets/image-20201002222322410.png" alt="image-20201002222322410" style="zoom: 50%;" />

* RAW 写入式，支持就支持，不支持就不支持
* 可以用在手机端，移动端兼容性不同要注意

<img src="CSS深入笔记.assets/image-20201002222346070.png" alt="image-20201002222346070" style="zoom:50%;" />

<img src="CSS深入笔记.assets/image-20201002222406731.png" alt="image-20201002222406731" style="zoom:50%;" />

* web字体中的最佳格式
* 压缩版本，字体版本变小，提高网页的访问速度
* **居然不兼容手机端！**

<img src="CSS深入笔记.assets/image-20201002222418122.png" alt="image-20201002222418122" style="zoom:50%;" />

* IE专用（IE4+）

<img src="CSS深入笔记.assets/image-20201002222426545.png" alt="image-20201002222426545" style="zoom:50%;" />

* 其实就是Svg图片

### @font-face字体的应用

<img src="CSS深入笔记.assets/image-20201002222439075.png" alt="image-20201002222439075" style="zoom: 50%;" />

<img src="CSS深入笔记.assets/image-20201002222448673.png" alt="image-20201002222448673" style="zoom: 50%;" />

![image-20201004163233280](CSS深入笔记.assets/image-20201004163233280.png)

```html
<!DOCTYPE html>
<html lang="en">

<head>
<meta charset="UTF-8">
<title>CSS字体</title>
<style type="text/css">
@font-face {
    font-family: 'myfont';
    src: url('font/myFont.eot');
    src: url('font/myFont.eot?#iefix') format('embedded-opentype'),
         url('font/myFont.ttf') format('truetype'),
         url('font/myFont.woff') format('woff'),
         url('font/myFont.svg#myFont') format('svg');
}
h1 {
    font-family: 'myfont';
}
</style>
</head>

<body>
<h1>将梦想悬挂于枝头，在夏季的丰盛中饱满，绽放；为梦想披星戴月，刷新生命的温度。那些因梦想蜕变了的灵魂，历经时光坎坷，痛苦挣扎，依然在枝头放歌，温暖了生命如花的影子。前世，储蓄梦想；今生，演绎铿锵。</h1>
</body>

</html>

```

![image-20201004163246778](CSS深入笔记.assets/image-20201004163246778.png)

## 5. CSS3获取特殊字体

![image-20201002222500418](CSS深入笔记.assets/image-20201002222500418.png)

这个网址帮助我们获取特殊字体。

例如上传ttf格式，选择压缩版本，然后download your kit

# ch5 CSS3转换

**重点！！**

<img src="CSS深入笔记.assets/image-20201004163602390.png" alt="image-20201004163602390" style="zoom: 67%;" />

## 1. transform简介

<img src="CSS深入笔记.assets/image-20201004163623517.png" alt="image-20201004163623517" style="zoom:67%;" />



## 2. transform的2D转换

<img src="CSS深入笔记.assets/image-20201004163632038.png" alt="image-20201004163632038" style="zoom:67%;" />

### rotate() 旋转

<img src="CSS深入笔记.assets/image-20201004163651686.png" alt="image-20201004163651686" style="zoom:67%;" />

```css
<style type="text/css">
div { width: 1500px; height: 250px; background: #abcdef; margin: auto; }
div > img {
	transform: rotate(45deg);
}
</style>
</head>
<body>
<div><img src="images/sprite.jpg"></div>
```

<img src="CSS深入笔记.assets/image-20201004181336423.png" alt="image-20201004181336423" style="zoom:50%;" />

### translate() 平移

<img src="CSS深入笔记.assets/image-20201004163715718.png" alt="image-20201004163715718" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20201004163727214.png" alt="image-20201004163727214" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20201004163733498.png" alt="image-20201004163733498" style="zoom:67%;" />

![image-20201004170552116](CSS深入笔记.assets/image-20201004170552116.png)

```html
<style type="text/css">
div { width: 1500px; height: 250px; background: #abcdef; margin: auto; }
div > img {
	transform: translateY(-20%);
}
</style>
</head>
<body>
<div><img src="images/sprite.jpg"></div>
```

说明坐标原点与父容器的上边缘重合

![image-20201004181428879](CSS深入笔记.assets/image-20201004181428879.png)

<img src="CSS深入笔记.assets/image-20201004163741533.png" alt="image-20201004163741533" style="zoom: 67%;" />

### scale() 缩放

<img src="CSS深入笔记.assets/image-20201004163813300.png" alt="image-20201004163813300" style="zoom:67%;" />

原点线在中间

```html
<style type="text/css">
div { width: 1500px; height: 250px; background: #abcdef; margin: auto; }
div > img {
	transform: scale(0.5, 0.8 );
}
</style>
</head>
<body>
<div><img src="images/sprite.jpg"></div>
```

![image-20201004181500992](CSS深入笔记.assets/image-20201004181500992.png)

<img src="CSS深入笔记.assets/image-20201004163820077.png" alt="image-20201004163820077" style="zoom: 67%;" />

<img src="CSS深入笔记.assets/image-20201004163825777.png" alt="image-20201004163825777" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20201004163835900.png" alt="image-20201004163835900" style="zoom:67%;" />

一个参数是 **等比缩放**

### skew() 扭曲或斜切

<img src="CSS深入笔记.assets/image-20201004163851655.png" alt="image-20201004163851655" style="zoom: 67%;" />

```html
<style type="text/css">
div { width: 1500px; height: 250px; background: #abcdef; margin: auto; }
div > img {
	transform: skew(10deg, 30deg);
}
</style>
</head>
<body>
<div><img src="images/sprite.jpg"></div>
```

![image-20201004181549311](CSS深入笔记.assets/image-20201004181549311.png)

<img src="CSS深入笔记.assets/image-20201004163900240.png" alt="image-20201004163900240" style="zoom: 67%;" />

<img src="CSS深入笔记.assets/image-20201004163907256.png" alt="image-20201004163907256" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20201004163914391.png" alt="image-20201004163914391" style="zoom: 60%;" />

## 3.transform的3D转换

<img src="CSS深入笔记.assets/image-20201004163931081.png" alt="image-20201004163931081" style="zoom:50%;" />

### rotate3d()

<img src="CSS深入笔记.assets/image-20201004163953914.png" alt="image-20201004163953914" style="zoom: 67%;" />

```html
<style type="text/css">
div { width: 1500px; height: 250px; background: #abcdef; margin: auto; }
div > img {
	transform: rotate3d(1, 1, 1, 45deg);
}
</style>
</head>
<body>
<div><img src="images/sprite.jpg"></div>
```

![image-20201004181637135](CSS深入笔记.assets/image-20201004181637135.png)

<img src="CSS深入笔记.assets/image-20201004164003309.png" alt="image-20201004164003309" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20201004164010168.png" alt="image-20201004164010168" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20201004164021530.png" alt="image-20201004164021530" style="zoom:67%;" />

### translate3d()

<img src="CSS深入笔记.assets/image-20201004164054065.png" alt="image-20201004164054065" style="zoom:67%;" />

>  translateZ在页面上看不出来，更多用于 **遮罩**

<img src="CSS深入笔记.assets/image-20201004164101040.png" alt="image-20201004164101040" style="zoom:67%;" />

```html
<style type="text/css">
div { width: 1500px; height: 250px; background: #abcdef; margin: auto; }
div > img {
	transform: translate3d(200px, 200px, 200px);
}
</style>
</head>
<body>
<div><img src="images/sprite.jpg"></div>
```

![image-20201004181702478](CSS深入笔记.assets/image-20201004181702478.png)

### scale3d()

<img src="CSS深入笔记.assets/image-20201004164110114.png" alt="image-20201004164110114" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20201004164117898.png" alt="image-20201004164117898" style="zoom:67%;" />

```html
<style type="text/css">
div { width: 1500px; height: 250px; background: #abcdef; margin: auto; }
div > img {
	transform: scale3d(.5, .5, .5);
}
</style>
</head>
<body>
<div><img src="images/sprite.jpg"></div>
```

<img src="CSS深入笔记.assets/image-20201004181727824.png" alt="image-20201004181727824" style="zoom:50%;" />

## 4.transform与坐标系统

<img src="CSS深入笔记.assets/image-20201004164131354.png" alt="image-20201004164131354" style="zoom:67%;" />

坐标系统的引入，是用于元素的平移，选择，缩放等等

```css
transform-origin:left top;/*使用左上角作为基准点*/
transform-origin:25% top;/*靠近左上角一定距离的基准点*/
```

## 5. CSS3矩阵

<img src="CSS深入笔记.assets/image-20201004164138932.png" alt="image-20201004164138932" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20201004164146982.png" alt="image-20201004164146982" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20201004164202795.png" alt="image-20201004164202795" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20201004164214193.png" alt="image-20201004164214193" style="zoom:67%;" />

```html
<style type="text/css">
div { width: 1500px; height: 250px; background: #abcdef; margin: 100px auto; }
div:nth-child(1) > img {
	transform: matrix(.5, 0, 0, .5, 0, 0);
}
div:nth-child(2) > img {
	transform: scale(.5, .5);
}
</style>
</head>
<body>
<div><img src="images/sprite.jpg"></div>
<div><img src="images/sprite.jpg"></div>
```

![image-20201004181811206](CSS深入笔记.assets/image-20201004181811206.png)

<img src="CSS深入笔记.assets/image-20201004164236549.png" alt="image-20201004164236549" style="zoom: 67%;" />

<img src="CSS深入笔记.assets/image-20201004164246387.png" alt="image-20201004164246387" style="zoom:67%;" />

## 6. 扩展属性

<img src="CSS深入笔记.assets/image-20201004164302672.png" alt="image-20201004164302672" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20201004164334486.png" alt="image-20201004164334486" style="zoom:67%;" />

<img src="CSS深入笔记.assets/image-20201004164341887.png" alt="image-20201004164341887" style="zoom: 67%;" />

<img src="CSS深入笔记.assets/image-20201004164355072.png" alt="image-20201004164355072" style="zoom:67%;" />

# ch6 CSS过渡

## 1. 过渡概念

<img src="CSS深入笔记.assets/image-20201004202900814.png" alt="image-20201004202900814" style="zoom: 33%;" />

> CSS3过渡：一个元素，它的属性从一个值变化到另一个值

## 2. transition属性

### transition-property

 指向相关属性

<img src="CSS深入笔记.assets/image-20201004205134442.png" alt="image-20201004205134442" style="zoom:33%;" />

```css
transition-property:color;
transition-property:opacity
```

> 如果不写transition-property属性，表示all所有属性支持过渡

### transition-duration

要是想看到过渡的整个过程？那就让过渡的速度变慢 — duration

<img src="CSS深入笔记.assets/image-20201004205228983.png" alt="image-20201004205228983" style="zoom:33%;" />

### transition-timing-function

**只能使用一个属性值**

<img src="CSS深入笔记.assets/image-20201004205305739.png" alt="image-20201004205305739" style="zoom:33%;" />

<img src="CSS深入笔记.assets/image-20201004205323825.png" alt="image-20201004205323825" style="zoom:33%;" />

ease ease-in 结束比较生硬

<img src="CSS深入笔记.assets/image-20201004205357259.png" alt="image-20201004205357259" style="zoom:33%;" />

<img src="CSS深入笔记.assets/image-20201004205413780.png" alt="image-20201004205413780" style="zoom: 50%;" />

### transition-delay

**设置 `transition-delay`后，等待time时候后执行**

<img src="CSS深入笔记.assets/image-20201004211811560.png" alt="image-20201004211811560" style="zoom:33%;" />

### transition复合属性

**总结：CSS3 transition属性**

- **transition-property 指定属性名**
- **transition-duration 过渡时间**
- **transition-timing-function 过渡方法**
- **transition-delay 延迟**

复合属性，注意顺序一定不能错！

<img src="CSS深入笔记.assets/image-20201004211839992.png" alt="image-20201004211839992" style="zoom:50%;" />

# ch7 CSS3动画

<img src="CSS深入笔记.assets/image-20201004220358906.png" alt="image-20201004220358906" style="zoom: 33%;" />

## 1. 介绍

<img src="CSS深入笔记.assets/image-20201004220417108.png" alt="image-20201004220417108" style="zoom: 33%;" />

<img src="CSS深入笔记.assets/image-20201004220430008.png" alt="image-20201004220430008" style="zoom:33%;" />

> **注意：手机设备的浏览器在使用CSS3动画时，要加上webkit前缀**

## 2. annimation属性详解

### animation-name

<img src="CSS深入笔记.assets/image-20201004220446394.png" alt="image-20201004220446394" style="zoom: 33%;" />

```css
div>.inner {
      background-image: url(images/circle_inner.png);
      -webkit-animation-name: circle_inner;
      animation-name: circle_inner;
    }
    
    @keyframes circle_inner {
      from {
        transform: rotateX(0deg);
      }

      to {
        transform: rotateX(360deg);
      }
    }
    
<body>
  <div>
    <div class="inner"></div>
  </div>
</body>
```



### animation-duration

<img src="CSS深入笔记.assets/image-20201004220551982.png" alt="image-20201004220551982" style="zoom:33%;" />

**注意：**

* 对于数值0.5s，可以写成.5s
* W3C推荐这种去掉0的写法

### animation-timing-function

<img src="CSS深入笔记.assets/image-20201004220617519.png" alt="image-20201004220617519" style="zoom: 33%;" />

### animation-delay

<img src="CSS深入笔记.assets/image-20201004220638632.png" alt="image-20201004220638632" style="zoom:33%;" />

###  animation-iteration-count

<img src="CSS深入笔记.assets/image-20201004220723493.png" alt="image-20201004220723493" style="zoom:33%;" />

### animation-direction

<img src="CSS深入笔记.assets/image-20201004220755748.png" alt="image-20201004220755748" style="zoom:33%;" />

注意：

* alternate /  alternate-reverse 需要**配合循环属性才能实现效果**

### animation-fill-mode

<img src="CSS深入笔记.assets/image-20201004220818323.png" alt="image-20201004220818323" style="zoom:33%;" />

### animation-play-state

<img src="CSS深入笔记.assets/image-20201004220836565.png" alt="image-20201004220836565" style="zoom:33%;" />

使用`animation-play-state: paused`将动画暂停：

```css
    div>.inner {
      background-image: url(images/circle_inner.png);
      -webkit-animation-name: circle_inner;
      animation-name: circle_inner;
      -webkit-animation-duration: 10s;
      animation-duration: 10s;
      -webkit-animation-timing-function: linear;
      animation-timing-function: linear;
      -webkit-animation-delay: 2s;
      animation-delay: 2s;
      -webkit-animation-iteration-count: infinite;
      animation-iteration-count: infinite;
      -webkit-animation-direction: alternate;
      animation-direction: alternate;
      -webkit-animation-fill-mode: forwards;
      animation-fill-mode: forwards;
      /*暂停动画*/
      -webkit-animation-play-state: paused;
      animation-play-state: paused;
    }
```

```css
/*当鼠标移到上面的时候开始动画*/    
div:hover>div {
      cursor: pointer;
      -webkit-animation-play-state: running;
      animation-play-state: running;
    }
```

### animatio复合属性

<img src="CSS深入笔记.assets/image-20201004220900191.png" alt="image-20201004220900191" style="zoom:33%;" />

```css
    div>.inner {
      background-image: url(images/circle_inner.png);
      -webkit-animation: circle_inner linear 10s infinite;
      animation: circle_inner linear 10s 2s infinite;
    }
```

* 在animation里 name 和duration是必需的，其他的智能识别字段
* 所以使用delay时要同时定义duration和delay（因为先识别duration）
* 上述例子：duration：10s  delay：2s

## 3.关键帧 Keyframes

<img src="CSS深入笔记.assets/image-20201004220925830.png" alt="image-20201004220925830" style="zoom:33%;" />

<img src="CSS深入笔记.assets/image-20201004220938491.png" alt="image-20201004220938491" style="zoom:33%;" />

> animation-name: animationname; 定义好的动画

这个效果就是先慢后快再慢的转动

```css
	div>.inner {
      background-image: url(images/circle_inner.png);
      -webkit-animation: circle_inner linear 10s infinite;
      animation: circle_inner linear 10s infinite;
    }
   
   @keyframes circle_inner {
      from {
        transform: rotateX(0deg);
      }
      25% {
        transform: rotateX(45deg);
      }
      75% {
        transform: rotateX(315deg);
      }
      to {
        transform: rotateX(360deg);
      }
    }
```

注意:

* animation 写在 -webkit-animation后面
* 通用标准格式写在后面，浏览器先读webkit-animation，再读animation，如果标准格式兼容，就以标准格式渲染，如果不兼容，以wekit格式渲染。
* 尽量使用通用方法

## 4.动画性能优化

> **优化动画的一些要点：**
>
> 1. **position-fixed替代background-attachment**
> 2. **带图片的元素放在伪元素中**
> 3. **巧用will-change**

<img src="CSS深入笔记.assets/image-20201004220954967.png" alt="image-20201004220954967" style="zoom:33%;" />

<img src="CSS深入笔记.assets/image-20201004221006526.png" alt="image-20201004221006526" style="zoom:33%;" />

* 实际上是使用骗取浏览器的方式来使用GPU加速
*  <img src="CSS深入笔记.assets/image-20201005101929052.png" alt="image-20201005101929052" style="zoom: 50%;" />
* 也有一定的代价：增加层叠元素，占用RAM和GPU存储空间
* 如果有个机制：一会让要触发3D引擎，浏览器提前准备一下以便备用 —— 这个CSS属性就是 will-change

### will-change

<img src="CSS深入笔记.assets/image-20201004221049581.png" alt="image-20201004221049581" style="zoom:33%;" />

```css
will-change:transform
will-change:left/top/margin/等等 （这个不推荐）
```

<img src="CSS深入笔记.assets/image-20201004221033027.png" alt="image-20201004221033027" style="zoom:33%;" />