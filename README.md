# Vue-3.0
Vue 3.0 study note

[The story of Evan You and Vue](https://baijiahao.baidu.com/s?id=1730409379485988644&wfr=spider&for=pc)  
```
"Make epic shit" -- Evan You  
```

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

### customRef
创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制  
需求: 使用 customRef 实现 debounce(防抖) 的示例    
  
```typescript
<template>
    <h2> App </h2>
    <input v-model = "keyword" placeholder = "搜索关键字" />
    <p>{{ keyword }}</p>
</template>

<script lang = "ts">
/*
customRef:
  创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制

需求: 
  使用 customRef 实现 debounce 的示例
*/

import { ref, customRef } from 'vue'

export default {
    setup() {
        const keyword = useDebouncedRef('', 500)
        console.log(keyword)
        return {
            keyword
        }
    },
}

/* 
实现hook防抖的函数
*/
// value 传入的数据，将来数据的类型不确定，所以使用泛型。 delay:放抖的时间间隔，默认为200毫秒(ms)
function useDebouncedRef<T>(value: T, delay = 200) {
    let timeoutId: number
    return customRef((track, trigger) => {
        return {
            // get:返回数据
            get() {
                // 告诉Vue追踪数据
                track()
                return value
            },
            // set:设置数据
            set(newValue: T) {
                // 清除计时
                clearTimeout(timeoutId)
                // 开启定时器
                timeoutId = setTimeout(() => {
                    value = newValue
                    // 告诉Vue去触发界面更新
                    trigger()
                }, delay)
            }
        }
    })
}

</script>
```

## Vue 中的 `h函数`
[h函数1](https://blog.csdn.net/qq_45494634/article/details/117019105)  
[h函数2](https://blog.csdn.net/weixin_47450807/article/details/122933658)  
[h函数3](https://zhuanlan.zhihu.com/p/407905035)  
[h函数常用方法以及说明](https://www.jb51.net/article/259768.htm)
  
h函数接收三个参数。  
第一个参数:，可以为一个html标签，一个组件，一个异步组件，或者是一个函数式组件。  
第二个参数：{ Object } Props，与attributes和props,以及事件对应的对象，我们可以在模板中使用，如果没有需要传入的属性，可以设置为null。  
第三个参数(optional)：{String | Object |Array}可以是字符串Text文本或者是h函数构建的对象再者可以是有插槽的对象。
  
 
"h"函数的第1个参数是"标签名",   
第2个是"属性", 在这个例子里可以理解为html的所有属性,  
第3个是"内容". "内容"不仅仅可以是字符串, 还可以是"VNode"或2者混合： 
  
```typescript
  <script>
import { defineComponent, h } from "vue";
export default defineComponent({
  render() {
    const props = { style: { color: "red" } };
    const small = h("small", "副标题");
    return h("h2", props, ["123456789", small]);
  },
});
</script>
```
  
  
```typescript
  render() {
    return h(
      "div",
      {
        class: "app",
      },
      [
        // 这里this是可以取到setup中的返回值的 
        h("h2", null, `当前计数: ${this.counter}`),
        h("button", {onclick:() => this.counter++}, "+1"),
        h("button", {onclick:() => this.counter--}, "-1"),
      ]
    );
  },
```

## ES6学习：解构赋值
  
[解构赋值](https://www.jianshu.com/p/3f7afae3a9be)

## 扩展运算符
我们先看下代码，在以往，我们给函数传不确定参数数量时，是通过arguments来获取的
```typescript
function sum() {
  console.log(arguments) // { '0': 1, '1': 2, '2': 3, '3': 4, '4': 5, '5': 6 }
                         // 我们可以看出，arguments不是一个数组，而是一个伪数组
  let total = 0
  let { length } = arguments
  for(let i = 0;i < length;i++){
    total += arguments[i]
  }
  return total
}

console.log(sum(1,2,3,4,5,6)) // 21
```
                                
接下来我们用扩展运算符看看
```typescript
function sum(...args){ // 使用...扩展运算符
    console.log(args) // [ 1, 2, 3, 4, 5, 6 ] args是一个数组
    return eval(args.join('+'))
}

console.log(sum(1,2,3,4,5,6)) // 21
```
                                
得到的args是一个数组，直接对数组进行操作会比对伪数组进行操作更加方便，还有一些注意点需要注意
```typescript
// 正确的写法 扩展运算符只能放在最后一个参数
function sum(a,b,...args){
    console.log(a) // 1
    console.log(b) // 2
    console.log(args) // [ 3, 4, 5, 6 ]
}

sum(1,2,3,4,5,6)

// 错误的写法 扩展运算符只能放在最后一个参数
function sum(...args,a,b){
    // 报错
}

sum(1,2,3,4,5,6)
```
我们可以对比下扩展运算符的方便之处
```typescript
// 以往我们是这样拼接数组的
let arr1 = [1,2,3]
let arr2 = [4,5,6]
let arr3 = arr1.concat(arr2)
console.log(arr3) // [ 1, 2, 3, 4, 5, 6 ]

// 现在我们用扩展运算符看看
let arr1 = [1,2,3]
let arr2 = [4,5,6]
let arr3 = [...arr1,...arr2]
console.log(arr3) // [ 1, 2, 3, 4, 5, 6 ]
```
```typescript
// 以往我们这样来取数组中最大的值
function max(...args){
    return Math.max.apply(null,args)
}
console.log(max(1,2,3,4,5,6)) // 6

// 现在我们用扩展运算符看看
function max(...args){
    return Math.max(...args) // 把args [1,2,3,4,5,6]展开为1,2,3,4,5,6
}
console.log(max(1,2,3,4,5,6)) // 6
```
```typescript
// 扩展运算符可以把argument转为数组
function max(){
    console.log(arguments) // { '0': 1, '1': 2, '2': 3, '3': 4, '4': 5, '5': 6 }
    let arr = [...arguments]
    console.log(arr) // [1,2,3,4,5,6]
}

max(1,2,3,4,5,6)

// 但是扩展运算符不能把伪数组转为数组（除了有迭代器iterator的伪数组，如arguments）
let likeArr = { "0":1,"1":2,"length":2 }
let arr = [...likeArr] // 报错 TypeError: likeArr is not iterable

// 但是可以用Array.from把伪数组转为数组
let likeArr = { "0":1,"1":2,"length":2 }
let arr = Array.from(likeArr)
console.log(arr) // [1,2]
```

对象也可以使用扩展运算符
```typescript
// 以往我们这样合并对象
let name = { name:"邵威儒" }
let age = { age:28 }
let person = {}
Object.assign(person,name,age)
console.log(person) // { name: '邵威儒', age: 28 }

// 使用扩展运算符
let name = { name:"邵威儒" }
let age = { age:28 }
let person = {...name,...age}
console.log(person) // { name: '邵威儒', age: 28 }
```

## 组件事件
此章节假设你已经看过了组件基础。若你还不了解组件是什么，请先阅读该章节。
[emit事件的使用-子组件给父组件传递参数](https://www.jianshu.com/p/43ac90bc7fa3)

### 触发与监听事件#
在组件的模板表达式中，可以直接使用 $emit 方法触发自定义事件 (例如：在 v-on 的处理函数中)：

```vue
<!-- MyComponent -->
<button @click="$emit('someEvent')">click me</button>
```
 
  父组件可以通过 v-on (缩写为 @) 来监听事件： 
  
```vue
<MyComponent @some-event="callback" />
```
  同样，组件的事件监听器也支持 .once 修饰符：

```vue
<MyComponent @some-event.once="callback" />
```
  像组件与 prop 一样，事件的名字也提供了自动的格式转换。注意这里我们触发了一个以 camelCase 形式命名的事件，但在父组件中可以使用 kebab-case 形式来监听。   
  与 prop 大小写格式一样，在模板中我们也推荐使用 kebab-case 形式来编写监听器。  
  
#### TIP
```
和原生 DOM 事件不一样，组件触发的事件没有冒泡机制。你只能监听直接子组件触发的事件。  
平级组件或是跨越多层嵌套的组件间通信，应使用一个外部的事件总线，或是使用一个全局状态管理方案。
```
### 事件参数#
有时候我们会需要在触发事件时附带一个特定的值。举例来说，我们想要 <BlogPost> 组件来管理文本会缩放得多大。在这个场景下，我们可以给 $emit 提供一个额外的参数：

```vue
<button @click="$emit('increaseBy', 1)">
  Increase by 1
</button>
```
然后我们在父组件中监听事件，我们可以先简单写一个内联的箭头函数作为监听器，此函数会接收到事件附带的参数：

```vue
<MyButton @increase-by="(n) => count += n" />
```
或者，也可以用一个组件方法来作为事件处理函数：

```vue
<MyButton @increase-by="increaseCount" />
```
该方法也会接收到事件所传递的参数：

```typescript
function increaseCount(n) {
  count.value += n
}
```
#### TIP
```vue
所有传入 $emit() 的额外参数都会被直接传向监听器。举例来说，$emit('foo', 1, 2, 3) 触发后，监听器函数将会收到这三个参数值。
```
