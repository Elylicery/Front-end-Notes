# 1. 计时器

实现一个打点计时器，要求
1、从 start 到 end（包含 start 和 end），每隔 100 毫秒 console.log 一个数字，每次数字增幅为 1
2、返回的对象中需要包含一个 cancel 方法，用于停止定时操作
3、第一个数需要立即输出

```js
function count(start, end) {
  console.log(start++);
  var myVar = setInterval(()=>{
    if(start<=end){
      console.log(start);
      start++;
    }else{
      clearInterval(myVar)
    }
  },100)

  return{
    cancel(){
      clearInterval(myVar); 
    }
  }

}
```

# 2. 函数的上下文

将函数 fn 的执行上下文改为 obj 对象

输入

```
function () {return this.greeting + ', ' + this.name + '!!!';}, {greeting: 'Hello', name: 'Rebecca'}
```

输出

```
Hello, Rebecca!!!
```

答案：

```js
function speak(fn, obj) {
  return fn.bind(obj)();
}
```

```js
function speak(fn, obj) {
	return fn.apply(obj)
}
```

```js
function speak(fn, obj) {
    return fn.call(obj);
}
```

# 3. 返回函数

实现函数 functionFunction，调用之后满足如下条件：
1、返回值为一个函数 f
2、调用返回的函数 f，返回值为按照调用顺序的参数拼接，拼接字符为英文逗号加一个空格，即 ', '
3、所有函数的参数数量为 1，且均为 String 类型

输入

```
functionFunction('Hello')('world')
```

输出

```
Hello, world
```

```js
function functionFunction(str) {
    var f = function(x){
        return str+', '+x
    }
	return f
}
```

# 4. 使用闭包

实现函数 makeClosures，调用之后满足如下条件：
1、返回一个函数数组 result，长度与 arr 相同
2、运行 result 中第 i 个函数，即 result[i]()，结果与 fn(arr[i]) 相同

输入

```
[1, 2, 3], function (x) { 
	return x * x; 
}
```

输出

```
4
```

```js
function makeClosures(arr, fn) {
  var result = [];
  for(let i=0;i<arr.length;i++){
    result[i]=function(){
      return fn(arr[i])
    }
  }
  return result;
}
```

# 5. 二次封装函数

已知函数 fn 执行需要 3 个参数。请实现函数 partial，调用之后满足如下条件：
1、返回一个函数 result，该函数接受一个参数
2、执行 result(str3) ，返回的结果与 fn(str1, str2, str3) 一致

**例1**

输入

```js
var sayIt = function(greeting, name, punctuation) {     return greeting + ', ' + name + (punctuation || '!'); };  partial(sayIt, 'Hello', 'Ellie')('!!!');
```

输出

```
Hello, Ellie!!!
```

```js
function partial(fn, str1, str2) {
  var res = function(s){
    return res(str1,str2,s);
  }
  return res;
}
```

**例2**

实现函数 partialUsingArguments，调用之后满足如下条件：
1、返回一个函数 result
2、调用 result 之后，返回的结果与调用函数 fn 的结果一致
3、fn 的调用参数为 partialUsingArguments 的第一个参数之后的全部参数以及 result 的调用参数

```js
function partialUsingArguments(fn) {
  let [fn,...argu] = [...arguments];
  var result = function(){
     return fn.apply(this,argu.concat([...arguments]));
  };
  return result;
}

```

```js
function partialUsingArguments(fn) {
    let args = Array.prototype.slice.call(arguments, 1)
    return function result() {
        return fn.apply(this, args.concat([].slice.call(arguments)))
    }
}
```

# 6. 使用apply调用函数

实现函数 callIt，调用之后满足如下条件
1、返回的结果为调用 fn 之后的结果
2、fn 的调用参数为 callIt 的第一个参数之后的全部参数

```js
function callIt(fn) {
  let [f,...argu] = [...arguments]
  return fn.apply(this,argu)
}
```

```js
function callIt(fn) {
    return fn.apply(this,Array.prototype.slice.call(arguments,1));
}
```

```js
function callIt(fn) {
    const arr = [];
    for (let i = 1; i < arguments.length; i++) {
        arr.push(arguments[i])
    }
    return fn.apply(null, arr);
}
```

# 7. 柯里化

已知 fn 为一个预定义函数，实现函数 curryIt，调用之后满足如下条件：
1、返回一个函数 a，a 的 length 属性值为 1（即显式声明 a 接收一个参数）
2、调用 a 之后，返回一个函数 b, b 的 length 属性值为 1
3、调用 b 之后，返回一个函数 c, c 的 length 属性值为 1
4、调用 c 之后，返回的结果与调用 fn 的返回值一致
5、fn 的参数依次为函数 a, b, c 的调用参数

输入

```
var fn = function (a, b, c) {return a + b + c}; curryIt(fn)(1)(2)(3);
```

输出

```
6
```

```js
function curryIt(fn) {
  return function a(xa){
    return function b(xb){
      return function c(xc){
        return fn.call(this,xa,xb,xc)
        //return fcn.apply(this,[xa,xb,xc])
      }
    }
  }
}
```

# 8. 乘法

【备注】：压根不会

求 a 和 b 相乘的值，a 和 b 可能是小数，需要注意结果的精度问题

输入

```
3, 0.0001
```

输出

```
0.0003
```

**思路1**

先将小数用10的n次方转化成整数，乘完的结果在除以之前乘了多少个10

```js

function multiply(a, b) {
    a = a.toString();
    b = b.toString();
    //看a和b乘多少能得到一个整数
    var aLen = a.substring(a.indexOf(".")+1).length;
    var bLen = a.substring(a.indexOf(".")+1).length;
    return (a * Math.pow(10,aLen)) * (b * Math.pow(10,bLen)) / Math.pow(10,aLen+bLen);
}

```

**思路2**

计算两小数的小数点位数，然后相加得到保留的小数点位数，用tofixed函数即可

```js

function multiply(a, b) {
  var aDec = a.toString().split('.')[1] || "";
  var bDec = b.toString().split('.')[1] || "";
  var fix = aDec.length + bDec.length;
  return (a*b).toFixed(fix)
}

```

# 9 批量改变对象的属性

**题目描述**

给定一个构造函数 constructor，请完成 alterObjects 方法，将 constructor 的所有实例的 greeting 属性指向给定的 greeting 变量。

**输入**

```
var C = function(name) {this.name = name; return this;}; 
var obj1 = new C('Rebecca'); 
alterObjects(C, 'What\'s up'); obj1.greeting;
```

**输出**

```
What's up
```

```js
function alterObjects(constructor, greeting) {
  return constructor.prototype.greeting = greeting
}

var C = function(name) {this.name = name; return this;}; var obj1 = new C('Rebecca'); 
alterObjects(C, 'What\'s up'); 
console.log(obj1.greeting);
```

# 10. 属性遍历

**题目描述**

找出对象 obj 不在原型链上的属性(注意这题测试例子的冒号后面也有一个空格~)
1、返回数组，格式为 key: value
2、结果数组不要求顺序

**输入**

```
var C = function() {this.foo = 'bar'; this.baz = 'bim';}; 
C.prototype.bop = 'bip'; 
iterate(new C());
```

**输出**

```
["foo: bar", "baz: bim"]
```

```js
function iterate(obj) {
var ownPropertyArr = [];
for(var key in obj)
    {
        if(obj.hasOwnProperty(key))
            {
                ownPropertyArr.push(key+": "+obj[key]);
            }
    }
    return ownPropertyArr;
}

var C = function() {this.foo = 'bar'; this.baz = 'bim';}; 
C.prototype.bop = 'bip'; 
var res = iterate(new C())
console.log(res)
```

**题目要求找不在原型链上的属性，即返回实例属性，**有3种正确方法（按提交运行速度由快到慢排列）：

1. **Object.keys 方法**（156 ms）

返回**可枚举**的实例属性的数组。

(即Object.keys 只收集自身属性名，不继承自原型链上的属性）

```js
function iterate(obj) {
    return Object.keys(obj).map(function(key) {
        return key + ": " + obj[key];
    });
}
```

2. **for-in 和 hasOwnProperty 方法**（171 ms）

前者用于遍历所有属性，后者用于判断是否为实例属性。

```js
function iterate(obj) {
    const res = [];
    for (var prop in obj) {
        if (obj.hasOwnProperty(prop)) {
            res.push(prop + ": " + obj[prop]);
        }
    }
    return res;
}
```

3. **Object.getOwnPropertyNames 方法**（209 ms）

用法跟1.一样，区别在于返回的是**所有实例属性（包括不可枚举的）。**

```js
function iterate(obj) {
    return Object.getOwnPropertyNames(obj).map(function(key) {
        return key + ": " + obj[key];
    });
} 
```



# 11. 正则表达式合集

1. 给定字符串 str，**检查其是否包含数字**，包含返回 true，否则返回 false

```js
function containsNumber(str) {
  const numreg = /\d/g;
  return numreg.test(str)
}
```

2. 给定字符串 str，**检查其是否包含连续重复的字母（a-zA-Z）**，包含返回 true，否则返回 false

```js
function containsRepeatingLetter(str) {
    return /([a-zA-Z])\1/.test(str);
}
```

【注意】在正则表达式中，利用()进行分组，使用斜杠加数字表示引用，\1就是引用第一个分组，\2就是引用第二个分组。将[a-zA-Z]做为一个分组，然后引用，就可以判断是否有连续重复的字母。

3. 给定字符串 str，**检查其是否以元音字母结尾**
   1、元音字母包括 a，e，i，o，u，以及对应的大写
   2、包含返回 true，否则返回 false

```js
function endsWithVowel(str) {
    return numreg = /[aeiou]$/gi.test(str)
}
```

【注意】$ 表示结尾字符 i表示忽略大小写

4. 给定字符串 str，检查其**是否包含连续的任意3个数字**
   1、如果包含，返回最先出现的 3 个数字的字符串
   2、如果不包含，返回 false

```js
function captureThreeNumbers(str) {
     //声明一个数组保存匹配的字符串结果
  var arr = str.match(/\d{3}/);
     //如果arr存在目标结果，则返回第一个元素，即最早出现的目标结果
     if(arr)
         return arr[0];
     else return false;
 }
```

【备注】连续的三个任意数字用正则表达式表示为/\d{3}/。

5. 给定字符串 str，检查其**是否符合如下格式**
   1、XXX-XXX-XXXX
   2、其中 X 为 Number 类型

```js
function matchesPattern(str) {
    var pattern = /^\d{3}-\d{3}-\d{4}$/;
    return pattern.test(str)
}
```

【备注】^ 表示开头 $ 表示结尾

6. 给定字符串 str，检查其**是否符合美元书写格式**
   1、以 `$` 开始
   2、整数部分，从个位起，满 3 个数字用 , 分隔
   3、如果为小数，则小数部分长度为 2
   4、正确的格式如：$1,023,032.03 或者 $2.03，错误的格式如：$3,432,12.12 或者 $34,344.3

```js
function isUSD(str) {
	return /^\$\d{1,3}(,\d{3})*(\.\d{2})?$/.test(str);
}
```

7. 判断是否**合格邮箱**

   ```js
   function isAvailableEmail(sEmail) {
     var emailReg = /^([A-Za-z0-9_\-\.])+\@([A-Za-z0-9_\-\.])+\.([A-Za-z]{2,4})$/;
     return emailReg.test(sEmail);
   }
   ```

8. **颜色字符串转换**

   将 rgb 颜色字符串转换为十六进制的形式，如 rgb(255, 255, 255) 转为 #ffffff

   1、rgb 中每个 , 后面的空格数量不固定

   2、十六进制表达式使用六位小写字母

   3、如果输入不符合 rgb 格式，返回原始输入

   

   输入：

   ```js
   'rgb(255, 255, 255)'
   ```

   输出：

   ```js
   #ffffff
   ```

   答案：
   
   ```js
   function rgb2hex(sRGB) {
     var hexRGB = sRGB.replace(/^rgb\((\d+)\s*\,\s*(\d+)\s*\,\s*(\d+)\)$/g,function(a,r,g,b){
       return '#' + hex(r) + hex(g) + hex(b);
     });
     return hexRGB;
   }
   function hex(n){
     var num = Number(n);
     return num<16 ? '0'+num.toString(16):num.toString(16)
   }
   ```
   
   注意：
   
   * 代表正则匹配的整个字符串, r ,g, b代表红绿蓝三个通道, 分别是正则中的三个括号匹配的字符串. 通常用的$0, $1, $2, $3, 个人习惯喜欢起好记的别名
   * 颜色值使用函数RGB记法，即rgb(color)的时候，其整数范围就是0~255
   
9. **字符串转为驼峰格式**

   css 中经常有类似 background-image 这种通过 - 连接的字符，通过 javascript 设置样式的时候需要将这种样式转换成 backgroundImage 驼峰格式，请完成此转换功能

   1. 以 - 为分隔符，将第二个起的非空单词首字母转为大写
   2. webkit-border-image 转换后的结果为 webkitBorderImage

   输入

   ```js
   'font-size'
   ```

   输出

   ```js
   fontSize
   ```

   常规答案

   ```js
   function cssStyle2DomStyle(sName) {
     let arr = [...sName]
     if(arr[0]==='-') arr = arr.slice(1)
     for(let i=0;i<sName.length;i++){
       if(arr[i]==='-'){
         arr.splice(i,1);
         arr[i] = arr[i].toUpperCase();
       }
     }
     return arr.join('')
   }
   ```

   利用正则

   解法1：

   ```
   function cssStyle2DomStyle(sName) {
     return sName.replace(/-([a-z])/g,function(t,m,i){
       return i?m.toUpperCase():m;
     });
   }
   ```

   * 前一位有-的字符替换为大写【-([a-z])】
   * replace第二个参数为函数时：函数的第一个参数是匹配模式的字符 【t】；接下来的参数是与模式中的子表达式匹配的字符，可以有0个或多个这样的参数。【m】；接下来的参数是一个整数，代表匹配在被替换字符中出现的位置【i】；最后一个参数是被替换字符本身【这里没有用到


# 13 获取url参数

获取 url 中的参数

1. 指定参数名称，返回该参数的值 或者 空字符串
2. 不指定参数名称，返回全部的参数对象 或者 {}
3.  如果存在多个同名参数，则返回数组

输入

```
http://www.nowcoder.com?key=1&key=2&key=3&test=4#hehe key
```

输出

```
[1, 2, 3]
```

**答案1 使用正则**

```js
function getUrlParam(sUrl,sKey){
	var result = {};
	sUrl.replace(/\??(\w+)=(\w+)&?/g,function(a,k,v){
		if(result[k] !== void 0){
			var t = result[k];
			result[k] = [].concat(t,v);
		}else{
			result[k] = v;
		}
	});
	if(sKey === void 0){
		return result;
	}else{
		return result[sKey] || '';
	}
}

var res = getUrlParam("http://www.nowcoder.com?key=1&key=2&key=3&test=4#hehe","key");
console.log(res);
```

1. replace()的第一个参数是一个正则表达式，第二个参数是一个回调函数，每匹配到一个符合正则表达式的结果就回调一次。参数a代表该次匹配到的字符串，k代表该次匹配到的字符串中符合第一个分组的部分，v代表该次匹配到的字符串中符合第二个分组的部分
2. 当concat()的参数是具体的值时，意味着将参数连接到调用concat()方法的数组上。result原本是一个空的对象，当回调函数第一次执行时，向该对象添加了一个key-value对，但此时的value是一个字符串"1"。因此，在回调函数第二次执行时，要向一个空数组添加字符串"1"和"2"，这也是为什么要用[].concat(k,v)。
3. 正则表达式的开头的\?是不能省略的，否则.com?key=1匹配不到。

**答案2 我的代码**

利用string的substr和indexof切割字符串的思路

```js
function getUrlParam(sUrl, sKey) {
  var start = sUrl.indexOf("?");
  var end = sUrl.indexOf("#") || sUrl.length-1;
  var subUrl = sUrl.slice(start+1,end);
  //console.log(subUrl);
  
  var keysArr = subUrl.split("&");
  var map = {};
  keysArr.forEach(item => {
    let [key,value=''] = item.split('=');
    let hasMapKey = Reflect.has(map,key)
    if(hasMapKey){
        map[key].push(value)
    }else{
        map[key] = [value]
    }
  });

  //未指定key
  if(sKey === undefined || sKey === ""){
    return map;
  }

  let res = map[sKey] || [''];
  return res.length === 1 ?res[0]:res;
}
```

# 14 dom节点查找

（完全不会）

题目：查找两个节点的最近的一个共同父节点，可以包括节点自身

输入描述:

```
oNode1 和 oNode2 在同一文档中，且不会为相同的节点
```

**思路**

js中的 contains方法，用于判断某个节点是不是另一个节点的后代

**代码1**

不用去递归也不用管谁包含谁，只要随便找一个节点，直到某个祖先节点（或自己）包含另一个节点就行了。
oNode.contains(oNode)是等于true的

```js
function commonParentNode(oNode1, oNode2) {
  for(;oNode1;oNode1=oNode1.parentNode){
    if(oNode1.contains(oNode2)){
      return oNode1;
    }
  }
}
```

**代码2 递归查找**

验证一个节点是否包含另一个节点就好了吧，不必验证两次，反正如果是被包含的关系，往上爬的时候会遇到另一个节点的。

```js
function commonParentNode(oNode1, oNode2) {
  if(oNode1.contains(oNode2)){
    return oNode1;
  }else{
    return commonParentNode(oNode1.parentNode,oNode2)
  }
}
```

# 15 根据包名，在指定空间中创建对象

根据包名，在指定空间中创建对象

输入描述:

```
namespace({a: {test: 1, b: 2}}, 'a.b.c.d')
```

输出描述:

```
{a: {test: 1, b: {c: {d: {}}}}}
```

（完全不会）

```js
function namespace(oNamespace, sPackage) {
  var arr = sPackage.split('.');
  var res = oNamespace;	// 保留对原始对象的引用

  for(var i = 0, len = arr.length; i < len; i++) {
    if(arr[i] in oNamespace) {	// 空间名在对象中
      if(typeof oNamespace[arr[i]] !== "object") {	// 为原始值	
        oNamespace[arr[i]] = {};    // 将此属性设为空对象			
      }	
    } else {	// 空间名不在对象中，建立此空间名属性，赋值为空
      oNamespace[arr[i]] = {};
    } 
    oNamespace = oNamespace[arr[i]];	// 将指针指向下一个空间名属性。
  }
  return res;
}

```

# 16 数组去重

（没做对）

为 Array 对象添加一个去除重复项的方法

输入

```
[false, true, undefined, null, NaN, 0, 1, {}, {}, 'a', 'a', NaN]
```

输出

```
[false, true, undefined, null, NaN, 0, 1, {}, {}, 'a']
```

```js
Array.prototype.uniq = function () {
  return Array.from(new Set(this))
}

let input = [false, true, undefined, null, NaN, 0, 1, {}, {}, 'a', 'a', NaN];
let res = input.uniq();
console.log(res);
```

**总结：数组去重的几种方法**

1. **利用reduce**

   ```js
   let arr = [1,2,1,3]
   
   let newarr = arr.reduce(function(prev,cur){
     prev.indexOf(cur) == -1 && prev.push(cur);
     return prev
   },[])
   console.log(newarr);//[1, 2, 3]
   ```

2. **利用set的成员唯一性**

   ```js
   let arr = [1,2,3,4,2,3];
   let s = new Set(arr);
   console.log(s);
   ```

# 17 时间格式化输出

按所给的时间格式输出指定的时间

格式说明
对于 2014.09.05 13:14:20

* yyyy: 年份，2014  yy: 年份，14
* MM: 月份，补满两位，09  M: 月份, 9
* dd: 日期，补满两位，05 d: 日期, 5
* HH: 24制小时，补满两位，13  H: 24制小时，13
* hh: 12制小时，补满两位，01 h: 12制小时，1
* mm: 分钟，补满两位，14 m: 分钟，14
* ss: 秒，补满两位，20 s: 秒，20
* w: 星期，为 ['日', '一', '二', '三', '四', '五', '六'] 中的某一个，本 demo 结果为 五

输入

```
formatDate(new Date(1409894060000), 'yyyy-MM-dd HH:mm:ss 星期w')
```

输出

```
2014-09-05 13:14:20 星期五
```

```js
function formatDate(t,str){
  var obj = {
    yyyy:t.getFullYear(),
    yy:(""+ t.getFullYear()).slice(-2),
    M:t.getMonth()+1,
    MM:("0"+ (t.getMonth()+1)).slice(-2),
    d:t.getDate(),
    dd:("0" + t.getDate()).slice(-2),
    H:t.getHours(),
    HH:("0" + t.getHours()).slice(-2),
    h:t.getHours() % 12,
    hh:("0"+t.getHours() % 12).slice(-2),
    m:t.getMinutes(),
    mm:("0" + t.getMinutes()).slice(-2),
    s:t.getSeconds(),
    ss:("0" + t.getSeconds()).slice(-2),
    w:['日', '一', '二', '三', '四', '五', '六'][t.getDay()]
  };
  return str.replace(/([a-z]+)/ig,function($1){return obj[$1]})
}

let res = formatDate(new Date(1409894060000), 'yyyy-MM-dd HH:mm:ss 星期w');
console.log(res);
```

备注

* $1, $2, ... $99 表示字符串中与regexp中的第1到第99个子表达式相匹配的文本
* `+`表示匹配元字符1次或多次
* `i` 忽略大小写

# 18 获取字符串长度

如果第二个参数 bUnicode255For1 === true，则所有字符长度为 1
否则如果字符 Unicode 编码 > 255 则长度为 2

输入

```
'hello world, 牛客', false
```

输出

```
17
```

做出来了，主要是看下uncideo编码

```js
function strLength(s, bUnicode255For1) {
  if(bUnicode255For1) return s.length;

  let num = 0;
  for(let i=0;i<s.length;i++){
    if(s.charCodeAt(i) > 255){
      num += 2;
    }else{
      num +=1;
    }
  }
  return num;
}
```

**总结**

1、charAt（）：把字符串分成每一个字符，从左往右提取指定位置的字符。

```html
var str = '天气';
alert( str.charAt(1) );            //气
```

2、**charCodeAt （）**：在第一个的基础上，返回的是字符的unicode编码。

```html
var str = '天气';
alert( str.charCodeAt(0) );        //22825
```

3、**String.fromCharCode（）**：通过编码值在unicode编码库中查找出对应的字符。

```html
alert( String.fromCharCode(22825, 27668) );            //天气
```

4、当两个字符串进行大小比较时，比的是第一个字符的unicode编码的大小：

```html
alert( 'abbbbb' > 'b' );                //unicode编码中a<b，所以是false；
alert( '10000' > '2' );                 //unicode编码中1<2，所以是false；
```