# Vue.js 实战

***

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

​                                                                                                                                    