# VUE.JS

## Vue实例

每个 Vue.js 应用都是通过构造函数 Vue 创建一个 Vue 的根实例 启动的:

每个 Vue 实例都会代理其 data 对象里所有的属性：

    var data = { a: 1 }
    var vm = new Vue({
    data: data
    })
    vm.a === data.a // -> true
    // 设置属性也会影响到原始数据
    vm.a = 2
    data.a // -> 2
    // ... 反之亦然
    data.a = 3
    vm.a // -> 3

注意只有这些被代理的属性是响应的，也就是说值的任何改变都是触发视图的重新渲染。如果在实例创建之后添加新的属性到实例上，它不会触发视图更新。