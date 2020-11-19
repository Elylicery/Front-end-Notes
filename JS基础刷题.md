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
   1、以 $ 开始
   2、整数部分，从个位起，满 3 个数字用 , 分隔
   3、如果为小数，则小数部分长度为 2
   4、正确的格式如：$1,023,032.03 或者 $2.03，错误的格式如：$3,432,12.12 或者 $34,344.3

```js
function isUSD(str) {
	return /^\$\d{1,3}(,\d{3})*(\.\d{2})?$/.test(str);
}
```

# 12 数组合集

**开头插入元素**

* `newArr = [item,...arr]`
* `newArr.unshift(item)`
* `[item].concat(arr)`

**末尾添加元素**

* `arr.push(item)`
* `arr.concat(item)`

**删除最后一个元素**

* `newArr = arr.slice(0,arr.length-1)`
* arr.pop()

**删除数组第一个元素**

* `arr.splice(0,1)`
* `arr.shift()`
* `arr.slice(1)`