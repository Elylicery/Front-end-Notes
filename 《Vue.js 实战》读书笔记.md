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

## 





















