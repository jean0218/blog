# Vue的知识点
## vue的生命周期：
初始化：
.1 beforeCreate
created
初始化数据进行数据的观测，数据已经和data数据进行绑定
beforeMount
beforeMount阶段应该是虚拟DOM
render/template
渲染view
mounted
适合初始化数据

更新
当vue发现data中的数据发生了改变，会触发对应组件的重新渲染
beforeUpdate
可以监听到data的变化但是view层没有被重新渲染,view层的数据没有变化
updated
重新渲染vue完成

销毁
beforeDestroy
beforeDestroy钩子函数在实例销毁之前调用。在这一步，实例仍然完全可用。
destroyed
destroyed钩子函数在Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

methods:
编写事件和方法的位置

computed: 计算属性
计算属性是自动监听依赖值的变化，从而动态返回内容
watch：监听属性变化处理
监听是一个过程，在监听的值变化时，可以触发一个回调，并做一些事情,
编写组件时，当props传下新的值，需要根据新的值来操作时，写在这里
