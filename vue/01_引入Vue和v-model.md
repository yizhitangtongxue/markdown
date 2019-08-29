##### 使用单个Vue.js只需要用script标签引入即可

##### 创建vm实例：
```html
<div id="app">
    <input type="text" v-model="username">
    <p>{{username}}</p>
</div>
```
```js
const vm = new Vue({
    el:"#app"
    data:{
        username:"myName"
    },
})

// 1、vm代表的是MVVM中的VM,即Vue实例对象,vm中包含数据绑定和Dom监听
// 2、new Vue({}) 这是一个回调函数,用来生成一个vm实例
// 3、el: 对应一个选择器,表示Vue管理的区域
// 4、data:{} ,MVVM中的M,也就是model,数据
// 5、html代表MVVM中的V,view,视图
// 6、{{}} 这是大括号表达式，用来显示内容
```

##### 使用v-model双向数据绑定
1、上面的input元素中使用了v-model指令，
vue会监听视图的改变(dom监听)，一旦元素值发生变化，
则通过vm将数据保存到data(model)中，
同时使用了{{}}模版语法(大括号表达式)的值也会发生变化，这就是
Vue中的双向数据绑定

##### {{}} 大括号表达式内可以写Js