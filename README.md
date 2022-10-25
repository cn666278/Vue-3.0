# Vue-3.0
Vue 3.0 study note

### References
1. [Vue API Reference official website](https://vuejs.org/api)  
2. [UI](https://www.naiveui.com/zh-CN/light/components)  
3. [API](http://111.59.30.30:20002/doc)  
4. [Vue 防抖的三种处理方式](https://blog.csdn.net/qq453660983/article/details/125276335)  
5. [防抖与节流](https://blog.csdn.net/m0_48166663/article/details/121144995)  
6. [Vue生命周期函数](https://blog.csdn.net/weixin_45791692/article/details/124045505)  
7. [后盾人前端](https://doc.houdunren.com/)  
8. [Flex布局](https://juejin.cn/post/7031050931206619172)
9. [Grid布局](https://juejin.cn/post/7031366821106155556)
10. [EP6-Promise](https://blog.csdn.net/m0_46846526/article/details/119345337?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166607450616782391816539%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166607450616782391816539&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-2-119345337-null-null.142^v58^pc_search_tree,201^v3^control_1&utm_term=Promise&spm=1018.2226.3001.4187)
11. [computed-caching-vs-methods](https://cn.vuejs.org/guide/essentials/computed.html#computed-caching-vs-methods)
12. [Vue中的扩展运算符 ...](https://blog.csdn.net/weixin_44682587/article/details/113740701)
13. [原生DOM中的事件（事件流、事件委托）](https://blog.csdn.net/weixin_46163658/article/details/121869715)
14. [原生js Dom事件](https://www.jianshu.com/p/b2b8a4186195)
15. [v-model的详细讲解](https://blog.csdn.net/weixin_45215308/article/details/121618639)


## shallowReadonly()

Unlike readonly(), there is no deep conversion: only root-level properties are made readonly.  
Property values are stored and exposed as-is - this also means properties with ref values will not be automatically unwrapped.  

### Example

```js
// shallowReadonly 生成的对象保持最外层是readonly状态，嵌套对象里面的对象（bar)不是readonly
const state = shallowReadonly({
  foo: 1,
  nested: {
    bar: 2
  }
})

console.log(isReadonly(state));              // true, readOnly
console.log(isReadonly(state.foo));          // false, readOnly ???
console.log(isReadonly(state.nested));       // false, readOnly ???
console.log(isReadonly(state.nested.bar));   // false
console.log(state.nested.bar++); // works, can edit

// mutating state's own properties will fail
state.foo++

// ...but works on nested objects
isReadonly(state.nested) // false

// works
state.nested.bar++
```

## v-if 和 v-for  

警告：
同时使用 v-if 和 v-for 是不推荐的，因为这样二者的优先级不明显。

当 v-if 和 v-for 同时存在于一个元素上的时候，`v-if 会首先被执行`。

当它们同时存在于一个节点上时，v-if 比 v-for 的优先级更高。这意味着 v-if 的条件将无法访问到 v-for 作用域内定义的变量别名：
```js
<!--
 这会抛出一个错误，因为属性 todo 此时
 没有在该实例上定义
-->
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo.name }}
</li>
```
在外新包装一层 <template> 再在其上使用 v-for 可以解决这个问题 (这也更加明显易读)：
```js
<template v-for="todo in todos">
  <li v-if="!todo.isComplete">
    {{ todo.name }}
  </li>
</template>
```

## 命名方式
### 骆驼式命名法（camelCase）
  ```
又称驼峰式命名法，是电脑程式编写时的一套命名规则（惯例）。  
正如它的名称CamelCase所表示的那样，是指混合使用大小写字母来构成变量和函数的名字。  
程序员们为了自己的代码能更容易的在同行之间交流，所以多采取统一的可读性比较好的命名方式。  
骆驼式命名法就是当变量名或者函数名是由一个或者多个单词连结在一起，而构成的唯一识别字时，第一个单词以小写字母开始；  
  第二个单词开始以后的每个单词的首字母都采用大写字母。  
  例如：myFirstName、myLastName。
  ```

1. 小驼峰法
  变量一般用小驼峰法标识。

  驼峰法的意思是：除第一个单词之外，其他单词首字母大写。例如：int myStudentCount; 变量myStudentCount的第一个单词全部小写，后面的单词首字母大写。

2. 大驼峰法
  相比小驼峰法，大驼峰法（即帕斯卡命名法）把第一个单词的首字母也大写了。

  常用于类名，命名空间等。例如：public class DataBaseUser;

### 帕斯卡命名法
  ```
  帕斯卡命名法指当变量名和函式名称是由两个或两个以上单字连结在一起，而构成的唯一识别字时，用以增加变量和函式的可读性。

  命名规则：

  单字之间不以空格断开或连接号（-）、底线（_）连结，第一个单字首字母采用大写字母；后续首字母亦用大写字母，例如：FirstName、LastName。  
  每一个单字的首字母都采用大写字母的命名格式，被称为“Pascal命名法”，也有人称之为“大驼峰式命名法”（Upper Camel Case）,为驼峰式大小写的子集。

  帕斯卡命名法是在命名的时候将首字母大写，例如：public void DisplayInfo(); string UserName;二者都是采用了帕斯卡命名法。
  ```

  在C#中，以帕斯卡命名法和骆驼式命名法居多。

  C#中的编码惯例中，给公共成员变量（public）、受保护的成员变量（protect）、或内部成员变量（internal）命名时，应使用帕斯卡命名法，如score，name,Status均为有效地成员变量名；私有成员变量（private）必须以骆驼命名法命名，并以一个下划线开头。
  
 ## 总结：
  ```
  PascalCase：帕斯卡命名法，每个单词首字母大写，又名大驼峰命名法。  

  camelCase：驼峰命名法，第一个单词首字母小写，后面的每个单词首字母大写，又名小驼峰命名法。  

  kebab-case：短横线隔开命名法，每个单词首字母小写。  
  ```
  
  在vue官网上有这样的一句话：
“camelCase vs. kebab-case
HTML 属性是不区分大小写的。所以，当使用的不是字符串模版，camelCased (驼峰式) 命名的 prop 需要转换为相对应的 kebab-case (短横线隔开式) 命名： 如果你使用字符串模版，则没有这些限制。”
#### 重点在这里：
1. html特性不区分大小写：
```html
  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>prop动态=绑定</title>
    <script src="vue.js"></script>
</head>
<body>

<div id="app">
    <input type="text" v-model="message">
    <!--<child v-bind:myMEssage="message"></child>-->
    <child v-bind:mymessage="message"></child>
    <!--由于HTML的特性不识别大小写，所以“myMEssage”与“mymessage”是一样的，都解析为小写。故而下边的组件也应该是小写。-->
</div>
<script>
    Vue.component('child',{
    //此处都为小写。
        props:['mymessage'],
        template:'<p>{{mymessage}}</p>'
    });
    new Vue({
        el:'#app',
        data:{
            message:''
        }
    })
</script>
</body>
</html>
```
2. 组件中使用camelCased（驼峰式）命名，在html中应改为kebab-case（短横线）命名方式。
  ```html
  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>prop动态=绑定</title>
    <script src="vue.js"></script>
</head>
<body>

<div id="app">
    <input type="text" v-model="message">
    <child v-bind:my-message="message"></child>
    <!--此处的my-message只能是短横线命名（若为驼峰式则全部转换为小写。）-->
</div>
<script>
    Vue.component('child',{
//        props:['my-message'],
        props:['myMessage'],//props中传递的数据可以为驼峰式也可以为短横线式，他们在此处是相互转换的

        template:'<p>{{myMessage}}</p>'
        // 此处有限制，是字符串模板，{{ }}语法中不能是短横线连接方式。此处只能是驼峰命名方式。若为短横线的命名方式，则会报错。如下图：
    });
    new Vue({
        el:'#app',
        data:{
            message:''
        }
    })
</script>
</body>
</html>
```
