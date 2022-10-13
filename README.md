# Vue-3.0
Vue 3.0 study note

[Vue API Reference official website](https://vuejs.org/api) 

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
