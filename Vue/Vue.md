# MVVM实现原理

Angular 脏值检测
Vue 数据劫持 + 发布订阅模式

Object.defineProperty 
获取值时会调用get方法，设置值时会调用set方法

vue不能新增不存在的属性，因为不存在的属性没有get和set方法
vue具有深度响应，因为每次赋值一个新对象时会给这个新对象增加数据劫持