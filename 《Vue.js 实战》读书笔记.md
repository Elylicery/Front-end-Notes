# Vue.js 实战

***

# ch2 数据绑定

## 2.1 Vue实例与数据绑定

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>示例</title>
</head>
<body>
  <div id="app">
    <input type="text" v-model="name" placeholder="用户名">
    <h1>你好，{{name}}</h1>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    var app = new Vue({
      el:'#app',
      data:{
        name:'',
      }
    })
  </script>
</body>
</html>
```

### 2.1.1. 实例与数据

上面，el用于指定一个页面中已存在的DOM元素来挂载Vue实例。

### 2.1.2 生命周期

每个Vu实例创建时，都会经历一系列的初始化过程，同时也会调用相应的生命周期钩子。

vue常用的生命周期钩子有

* created：实例创建完成后调用，此阶段完成了数据的观测等，但尚未怪哉，$el还不可用，需要初始化处理一些数据时会比较有用
* mounted ：el挂载到实例上后调用，一般我们的第一个业务逻辑会在这里
* beforeDestroy：实例销毁之前调用。主要解绑一些使用addEventListener监听的事件。

### 2.1.4 过滤器

在`{{}}`插值的尾部添加一个管道符`(|)`对数据进行过滤，常用于格式化文本

Vue实例—— 实时显示当前的时间

# ch3 计算属性

1. 实现简单文本插值。                                                                                                                               
2. 动态设置元素的样式名称class和内联样式style

>  计算属性是基于它的以来缓存的，一个计算属性所依赖的数据发生变化时，它才会重新取值，所以当遍历大数组和做大量计算时，应当使用计算属性

# ch4 v-bind 及class与style绑定

## 4.2 绑定class的几种方式

### 4.2.1 对象语法

```html
<div id="app">
    <div :class="classes">
      绑定class的几种方式
    </div>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script> 
    var app = new Vue({
      el:'#app',
      data:{
        isActive:true,
        error:null
      },
      computed:{
        classes:function(){
          return {
            active:this.isActive && !this.error,
            'text-fail':this.error && this.error.type === 'fail'
          }
        }
      }
    })
  </script>
```

### 4.2.2 数组语法

给class绑定一个数组

```html
<div id="app">
    <div :class="[activeCls,errorCls]">
      需要多个class
    </div>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script> 
    var app = new Vue({
      el:'#app',
      data:{
        activeCls:'active',
        errorCls:'error',
      }
    })
```

可以使用计算属性给元素动态设置类名

```html
 <div id="app">
    <div :class="classes">
      动态设置类名
    </div>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script> 
    var app = new Vue({
      el:'#app',
      data:{
        size:'large',
        disabled:true,
      },
      computed:{
        classes:function(){
          return [
            'btn',
            {
              ['btn-'+this.size]:this.size!== '',
              ['btn-disabled']:this.disabled
            }
          ]
        }
      }
    })
  </script>
```

## ch5 内置指令

## 5.1 基本指令

### 5.1.1. v-cloak

对于简单项目可以解决初始化慢导致页面闪。

工程化项目里通过路由去挂载不同组件，不需要v-cloak

### 5.1.2 v-once

定义它的元素或组件只渲染一次，包括元素或组件的所有子节点。首次渲染后，不再随数据的变化重新渲染，将被视为静态内容。（少用。进一步优化性能时可能用到

## 5.2 v-if v-show的选择

v-if若表达式初值为false，则一开始元素就不会被渲染，条件为真才渲染。

v-show只是简单的CSS属性切换，无论条件真或否，都会被编译。

**v-if更适合条件不经常改变的场景，因为其切换开销相对较大，v-show适用于频繁切换条件**

## 5.3 v-for

### 5.3.1 基本用法

v-for可以遍历

* 数据
* 对象的属性（没用过！？
* 还可以迭代整数

### 5.3.2 数组更新

例如，给示例的数组books添加一项

**改变原数组的情况**

使用push为数组添加一项

**不改变原数组的情况**

有些方法不会改变原数组，例如 `filter()`,`concat()`,`slice()，他们返回一个新数组。

### 5.3.3 过滤与排序

不想改变原数组，想通过一个数组的副本来做过滤或排序的显示时，可以使用计算属性来返回过滤或排序后的数组

https://blog.csdn.net/sinat_38368658/article/details/107997407

## 5.4 方法与事件

Vue提供了一个特殊变量`$event`，用于访问原生DOM事件。例如下面的实例可以阻止链接打开

```vue
  <div id="app">
    <a href="www.baidu.com" @click="handleClick('禁止打开',$event)">打开链接</a>
  </div>
  <script>
    var app = new Vue({
      el:'#app',
      methods:{
        handleClick:function(message,event){
          event.preventDefault();
          window.alert(message);
        }
      }
    });
  </script>
```

上述使用的` event.preventDefault()`也可以用Vue事件的修饰符实现。

Vue支持一下修饰符

* .stop
* .prevent
* .capture
* .self
* .once

练习题 购物车https://blog.csdn.net/Clara_G/article/details/89840364

# ch6 表单与v-model

***

## ## 6.3 修饰符

.number 将输入转换为Number类型，否则虽然你输入的是数字，但它的类型其实是string，用于数字输入框

.trim 过滤首尾空格

# ch7 组件

***

## 7.1 组件与复用

注意，Vue组件的模板在某些情况下会受到HTML的限制，比如`<table>`内规定只允许是`<tr>,<td><th>`等这些表格元素，所以在`<table>`内直接使用组件是无效的。在这种情况下，可以使用**is**属性来挂载组件

```js
  <div id="app">
    <table>
      <tbody is="my-component"></tbody>
    </table>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    Vue.component('my-component',{
      template:'<div>这里是我自定义组件的内容</div>'
    })

    var app = new Vue({
      el:'#app',
    });
    
  </script>
```

常见的限制元素还有ul ol select

【注】如果使用的是字符串模板，是不受限制的，比如vue单文件用法

## 7.2 使用props传递数据

### 7.2.1 基本用法

父组件中包含子组件，父组件要正向地向子组件传递数据或参数，这个过程就是通过props实现。

在组件中，使用选项props声明需要从父级接受的数据，props的值可以是两种，字符串数组或对象。

以数组用法为例，构造一个数组，接受一个来自父级的数据mesage，并把它在组件模板中渲染。

```html
  <div id="app">
    <my-component message="来自父组件的数据" warning-text="提示信息"></my-component>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    Vue.component('my-component',{
      props:['message','warningText'],
      template:'<div>{{message}} | {{warningText}}</div>'
    })
    var app = new Vue({
      el:'#app',
    });
  </script>
```

![image-20200814172940373](《Vue.js 实战》读书笔记.assets/image-20200814172940373.png)

当传递的数据不是写死，而是来自父级的动态数据，可以使用v-bind来动态绑定props的值，当父组件的数据发生变化时，也会传递给子组件

```html
  <div id="app">
    <input type="text" v-model="parentMessage">
    <my-component :message="parentMessage"></my-component>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    Vue.component('my-component',{
      props:['message'],
      template:'<div>来自父组件的消息:{{message}}</div>'
    })
    var app = new Vue({
      el:'#app',
      data:{
        parentMessage:''
      }
    });
  </script>
```

【注意】：如果要传递数字，布尔值，数组，对象。如果不使用v-bind，传递的仅仅是字符串

```html
  <div id="app">
    <!--未使用v-bind 传递的时字符串-->
    <my-component message="[1,2,3]"></my-component>
    <!--使用v-bind 传递的才是数组-->
    <my-component :message="[1,2,3]"></my-component>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    Vue.component('my-component',{
      props:['message'],
      template:'<div>{{message.length}}</div>'
    })
    var app = new Vue({
      el:'#app',
    });
  </script>
```

![image-20200814210925375](《Vue.js 实战》读书笔记.assets/image-20200814210925375.png)

### 7.2.2 单向数据流

props传递数据是单向，对于需要改变props的情况

**第一种是父组件传初始值，子组件将其作为初始值保存起来，在自己的作用域下可以随意修改和使用**

这种情况可以在组件data内再声明一个数据，引用父组件的prop

```html
  <div id="app">
    <my-component :init-count="1"></my-component>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    Vue.component('my-component',{
      props:['initCount'],
      template:'<div>{{count}}</div>',
      data(){
        return{
          count:this.initCount
        }
      }
    })
    var app = new Vue({
      el:'#app',
    });
  </script>
```

![image-20200814214054591](《Vue.js 实战》读书笔记.assets/image-20200814214054591.png)

组件中声明了数据count，它在组件初始化事会获取来自父组件的initCount，之后就与之无关了，指用维护count。这样就可以避免直接操作initCount

**另一种情况是prop作为需要被转变的原始值传入，这种情况使用计算属性就可以**

```html
  <div id="app">
    <my-component :width="100"></my-component>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    Vue.component('my-component',{
      props:['width'],
      template:'<div :style="style">子组件内容</div>',
      computed:{
        style:function(){
          return {
            width:this.width+'px'
          }
        }
      }
    })
    var app = new Vue({
      el:'#app',
    });
  </script>
```

![image-20200814214409130](《Vue.js 实战》读书笔记.assets/image-20200814214409130.png)

【注意】JS中对象和数组是引用类型，指向同一个内存空间，所以props是对象和数组时，在子组件内改变是会影响父组件的。

### 7.2.3 数据验证

上面的例子props的值都是数组，除了数组外，可以是对象，当prop需要验证时，就需要对象写法

一般当你的组件提供给别人使用时，推荐都进行数据验证。比如某个数据必须是数字类型，如果传入字符串，就会在控制台弹出警告

```js
Vue.component('my-component',{
      props:{
        //必须是数字类型
        propA:Number,
        //必须是字符串或数字类型
        propB:[String,Number],
        //布尔值,如果没有定义,默认值就是true
        propC:{
          type:Boolean,
          default:true
        },
        //数字，而且是必传
        propD:{
          type:Number,
          required:true
        },
        //如果是数组或对象，默认值必须是一个函数来返回
        propE:{
          type:Array,
          default:function(){
            return [];
          }
        },
        //自定义一个验证函数
        propF:{
          validator:function(value){
            return val>10;
          }
        }
      }
    });
```

当prop验证失败时，开发版本下会在控制台抛出一条警告