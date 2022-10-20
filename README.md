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

### v-if 和 v-for  

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
