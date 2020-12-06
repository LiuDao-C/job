



## Vue

### ★★★★★Vue的生命周期

````js
Vue 的生命周期指的是组件从创建到销毁的一系列的过程。通过提供的 Vue 在生命周期各个阶段的钩子函数，我们可以在 Vue 的各个生命阶段实现一些操作。
Vue 一共有8个生命阶段，分别是创建前、创建后、加载前、加载后、更新前、更新后、销毁前和销毁后，每个阶段对应了一个生命周期的钩子函数。
（1）beforeCreate 钩子函数，在实例初始化之后，在数据监听和事件配置之前触发。因此在当前阶段data、methods、computed以及watch上的数据和方法都不能被访问
（2）created 钩子函数，在实例创建完成后触发，此时可以访问 data、methods 等属性。在这里更改数据不会触发updated函数。可以做一些初始数据的获取，在当前阶段无法与Dom进行交互，如果非要想，可以通过vm.$nextTick来访问Dom。
（3）beforeMount 钩子函数，在组件被挂载到页面之前触发。在这之前，会找到对应的 template，并编译成 render 函数。而当前阶段虚拟Dom已经创建完成，即将开始渲染。在此时也可以对数据进行更改，不会触发updated。
（4）mounted 钩子函数，在组件挂载到页面之后触发。在当前阶段，真实的Dom挂载完毕，数据完成双向绑定，可以访问到Dom节点，使用$refs属性对Dom进行操作。
（5）beforeUpdate 钩子函数，在响应式数据更新时触发，发生在虚拟 DOM 重新渲染和打补丁之前，在当前阶段进行更改数据，不会造成重渲染，这个时候我们可以对可能会被移除的元素做一些操作，比如移除事件监听器。
（6）updated 钩子函数，虚拟 DOM 重新渲染和打补丁之后调用，在此期间更改状态可能会导致更新无限循环。
（7）beforeDestroy 钩子函数，在实例销毁之前调用。一般在这一步我们可以销毁定时器、解绑全局事件、解绑$on事件等。
（8）destroyed 钩子函数，在实例销毁之后调用，调用后，Vue 实例中的所有东西都会解除绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

//keep-alive的作用
keep-alive可以实现组件的缓存，当组件切换时不会对当前组件进行卸载，常用的2个属性include/exclude,允许组件有条件的进行缓存。2个生命周期 activated,deactivated 用来得知当前组件是否处于活跃状态。    LRU算法

当我们使用 keep-alive 的时候，还有两个钩子函数，分别是 activated 和 deactivated 。用 keep-alive 包裹的组件在切换时不会进行销毁，而是缓存到内存中并执行 deactivated 钩子函数，命中缓存渲染后会执行 actived 钩子函数。使用keep-alive组件包裹需要保存的组件可以在组件切换的时候保存一些组件的状态防止多次渲染

//created和mouted的区别
接口请求一般放在mounted中，但需要注意的是服务端渲染时不支持mounted，需要放到created中
````



### ★Vue组件通信

````js
//父子组件间通信
第一种方法是子组件通过 props 属性来接受父组件的数据，然后父组件在子组件上注册监听事件，子组件通过 $on绑定事件、$emit触发事件来向父组件发送数据（发布订阅）。
第二种是通过 ref 属性给子组件设置一个名字。父组件通过 $refs 组件名来获得子组件，子组件通过 $parent 获得父组件，这样也可以实现通信。
获取父子组件实例的方式, $children、 $parent
用 ref 获取实例的方式调用组件的属性或者方法
第三种是使用 provider/inject，在父组件中通过 provider 提供变量，在子组件中通过 inject 来将变量注入到组件中。不论子组件有多深，只要调用了 inject 那么就可以注入 provider 中的数据。

//兄弟组件间通信
第一种是使用 eventBus 的方法，它的本质是通过创建一个空的 Vue 实例来作为消息传递的对象，通信的组件引入这个实例，通信的组件通过在这个实例上监听和触发事件，来实现消息的传递。
Vue.prototype.$bus = new Vue，然后通过$on,$emit来监听
第二种是通过 $parent.$refs 来获取到兄弟组件，也可以进行通信。

//任意组件之间
使用 eventBus ，其实就是创建一个事件中心，相当于中转站，可以用它来传递事件和接收事件。
如果业务逻辑复杂，很多组件之间需要同时处理一些公共的数据，这个时候采用上面这一些方法可能不利于项目的维护。这个时候可以使用 vuex ，vuex 的思想就是将这一些公共的数据抽离出来，将它作为一个全局的变量来管理，然后其他组件就可以对这个公共数据进行读写操作，这样达到了解耦的目的。

//父组件调用子组件的方法
template>
  <div>
    <children ref="child1"></children>
  </div>
</template>
this.$refs.child1.childMethod(this.flag); 
````



### ★Vuex

````js
//vuex是什么
vuex是一个专为vue.js应用程序开发的状态管理模式，它采用集中式存贮管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。
//vuex的属性
vuex有5个核心属性，state，getter，mutation，action，module
state用来存储数据和状态；我们在根实例中注册了store后，可以用this.$store.state来访问，它对应vue里面的data，store存放数据的方式为响应式，vue组件可以从store中读取数据，如果数据发生变化，组件也会对应更新；
getter可以认为是store的计算属性，它的返回值会根据它的依赖被缓存起来，只有当它的依赖值发生了改变才会重新计算；对store中的数据进行过滤或者业务上的处理，然后再返回。
mutation：更改vuex的store中的状态的唯一方法是提交mutation
action：包含任意异步操作，通过提交mutation间接改变状态；
module：将store分割成模块，每个模块都具有state、mutation、action、getter甚至是嵌套子模块
//mutation 和 action 的区别
mutation是同步更新数据（内部会进行是否为异步方式更新数据的检测）
action异步操作，可以获取数据后调用mutation提交最终数据
//vuex的数据传递流程
当组件进行数据修改的时候我们需要调用dispatch来触发actions里面的方法。actions里面的每个方法都会有一个commit方法，当方法执行的时候会通过commit来触发mutations里面的方法进行数据的修改。mutations里面的每个函数都会有一个state参数，这样就可以在mutations里面进行state的数据修改，当数据修改完毕后会传导给页面。页面的数据也会发生改变.
//为什么要用vuex
由于传参的方法对于多层嵌套的组件将会非常繁琐，并且对于兄弟组件间的状态传递无能为力。我们经常会采用父子组件直接引用或者通过事件来变更和同步状态的多份拷贝。以上的这些模式非常脆弱，通常会导致代码无法维护。所以我们需要把组件的共享状态抽取出来，以一个全局单例模式管理。在这种模式下，我们的组件树构成了一个巨大的“视图”，不管在树的哪个位置，任何组件都能获取状态或者触发行为！另外，通过定义和隔离状态管理中的各种概念并强制遵守一定的规则，我们的代码将会变得更结构化且易维护。

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。每一个 Vuex 应用的核心就是 store（仓库）。“store” 基本上就是一个容器，它包含着你的应用中大部分的状态 ( state )。
（1）Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
（2）改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation。这样使得我们可以方便地跟踪每一个状态的变化。
主要包括以下几个模块:
State：定义了应用状态的数据结构，可以在这里设置默认的初始状态
Getter：允许组件从 Store 中获取数据，mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性
Mutation：是唯一更改 store 中状态的方法，且必须是同步函数
Action：用于提交 mutation，而不是直接变更状态，可以包含任意异步操作
Module：允许将单一的 Store 拆分为多个 store 且同时保存在单一的状态树中。
      
在vue组件中是通过
this.$store.dispatch('saveUserName',res.username)
来派发action的，action再通过
saveUserName(context,username){
    context.commit('saveUserName', username);
}
来提交mutation，最后mutation再通过
saveUserName(state, username) {
    state.username = username;
  },
来修改store中的数据

由于是先渲染组件，再拿到请求返回的数据，所以需要将store加到computed属性中
````



### Key

````js
vue 中 key 值的作用可以分为两种情况来考虑。
//第一种情况是 v-if 中使用 key。
由于 Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染。因此当我们使用 v-if 来实现元素切换的时候，如果切换前后含有相同类型的元素，那么这个元素就会被复用。如果是相同的 input 元素，那么切换前后用户的输入不会被清除掉，这样是不符合需求的。因此我们可以通过使用 key 来唯一的标识一个元素，这个情况下，使用 key 的元素不会被复用。这个时候 key 的作用是用来标识一个独立的元素。
//第二种情况是 v-for 中使用 key。
用 v-for 更新已渲染过的元素列表时，它默认使用“就地复用”的策略。如果数据项的顺序发生了改变，Vue 不会移动 DOM 元素来匹配数据项的顺序，而是简单复用此处的每个元素。因此通过为每个列表项提供一个 key 值，来以便 Vue 跟踪元素的身份，从而高效的实现复用。这个时候 key 的作用是为了高效的更新渲染虚拟 DOM。


Vue 中 key 的作用是：key 是为 Vue 中 vnode 的唯一标记，通过这个 key，我们的 diff 操作可以更准确、更快速 
更准确：因为带 key 就不是就地复用了，在 sameNode 函数 a.key === b.key 对比中可以避免就地复用的情况。所以会更加准确。
更快速：利用 key 的唯一性生成 map 对象来获取对应节点，比遍历方式
````



### Computed和watch、method的差异

````js
（1）computed 是计算一个新的属性，并将该属性挂载到 Vue 实例上，而 watch 是监听已经存在且已挂载到 Vue 实例上的数据，所以用 watch 同样可以监听 computed 计算属性的变化。
（2）computed 本质是一个惰性求值的观察者，具有缓存性，只有当依赖变化后，下一次访问 computed 属性，才会计算新的值。而 watch 则是当数据发生变化时都会调用回调函数进行后续操作。
（3）从使用场景上说，computed 适用一个数据被多个数据影响，而 watch 适用一个数据影响多个数据。
//用dirty属性来控制是否重新计算，dirty默认值是true，计算完之后将dirty置为false，直到依赖的数据发生变化才将dirty置为true

Computed本质是一个具备缓存的watcher，依赖的属性发生变化就会更新视图。
适用于计算比较消耗性能的计算场景。当表达式过于复杂时，在模板中放入过多逻辑会让模板难以维护，可以将复杂的逻辑放入计算属性中处理。
Watch没有缓存性，更多的是观察的作用，可以监听某些数据执行回调。当我们需要深度监听对象中的属性时，可以打开deep：true选项，这样便会对对象中的每一项进行监听。这样会带来性能问题，优化的话可以使用字符串形式监听，如果没有写到组件中，不要忘记使用unWatch手动注销哦。

相同： computed和watch都起到监听/依赖一个数据，并进行处理的作用。
不同： 它们都是vue对监听器的实现，computed主要用于对同步数据的处理，watch则主要用于观测某个值的变化去完成一段开销较大的复杂业务逻辑。能用computed的时候优先用computed，避免了多个数据影响其中某个数据时多次调用watch的尴尬情况。
````





### 1、MVVM、MVC和MVP

````js
MVC、MVP 和 MVVM 是三种常见的软件架构设计模式，主要通过分离关注点的方式来组织代码结构，优化我们的开发效率。
比如说在使用单页应用时，往往一个路由页面对应了一个脚本文件，所有的页面逻辑都在一个脚本文件里。页面的渲染、数据的获取，对用户事件的响应所有的应用逻辑都混合在一起，这样在开发简单项目时，可能看不出什么问题，但是一旦项目变得复杂，那么整个文件就会变得冗长，混乱，这样对我们的项目开发和后期的项目维护是非常不利的。
MVC 通过分离 Model、View 和 Controller 的方式来组织代码结构。其中 View 负责页面的显示逻辑，Model 负责存储页面的业务数据，以及对相应数据的操作。并且 View 和 Model 应用了观察者模式，当 Model 层发生改变的时候它会通知有关 View 层更新页面。Controller 层是 View 层和 Model 层的纽带，它主要负责用户与应用的响应操作，当用户与页面产生交互的时候，Controller 中的事件触发器就开始工作了，通过调用 Model 层，来完成对 Model 的修改，然后 Model 层再去通知 View 层更新。

MVP 模式与 MVC 唯一不同的在于 Presenter 和 Controller。在 MVC 模式中我们使用观察者模式，来实现当 Model 层数据发生变化的时候，通知 View 层的更新。这样 View 层和 Model 层耦合在一起，当项目逻辑变得复杂的时候，可能会造成代码的混乱，并且可能会对代码的复用性造成一些问题。MVP 的模式通过使用 Presenter 来实现对 View 层和 Model 层的解耦。MVC 中的
Controller 只知道 Model 的接口，因此它没有办法控制 View 层的更新，MVP 模式中，View 层的接口暴露给了 Presenter 因此我们可以在 Presenter 中将 Model 的变化和 View 的变化绑定在一起，以此来实现 View 和 Model 的同步更新。这样就实现了对 View 和 Model 的解耦，Presenter 还包含了其他的响应逻辑。

MVVM 模式中的 VM，指的是 ViewModel，它和 MVP 的思想其实是相同的，不过它通过双向的数据绑定，将 View 和 Model 的同步更新给自动化了。当 Model 发生变化的时候，ViewModel 就会自动更新；ViewModel 变化了，View 也会更新。这样就将 Presenter 中的工作给自动化了。我了解过一点双向数据绑定的原理，比如 vue 是通过使用数据劫持和发布订阅者模式来实现的这一功
能。
````



### 2、★VUE的双向数据绑定原理

````js
vue 通过使用双向数据绑定，来实现了 View 和 Model 的同步更新。vue 的双向数据绑定主要是通过使用数据劫持和发布订阅者模式来实现的。
首先我们通过 Object.defineProperty() 方法来对 Model 数据各个属性添加访问器属性，以此来实现数据的劫持，因此当 Model 中的数据发生变化的时候，我们可以通过配置的 setter 和 getter 方法来实现对 View 层数据更新的通知。
//多层对象是通过递归来实现劫持，Vue3中是使用proxy来实现响应式数据，提升了性能，但存在兼容性问题
//如果做到内部依赖收集？每个属性都拥有自己的dep属性，存放他所依赖的watcher，当属性变化后会通知自己对应的watcher去更新
//性能优化：对象层级过深，性能会变差；不需要响应数据的内容不要放到data中，object.freeze()可以冻结数据，冻结后的数据不可以用defineProperty重新定义
//模板编译原理
先将模板转化成抽象语法树，然后优化树，最后将AST树生成代码
数据在 html 模板中一共有两种绑定情况，一种是使用 v-model 来对 value 值进行绑定，一种是作为文本绑定，在对模板引擎进行解析的过程中。如果遇到元素节点，并且属性值包含 v-model 的话，我们就从 Model 中去获取 v-model 所对应的属性的值，并赋值给元素的 value 值。然后给这个元素设置一个监听事件，当 View 中元素的数据发生变化的时候触发该事件，通知 Model 中的对应的属性的值进行更新。如果遇到了绑定的文本节点，我们使用 Model 中对应的属性的值来替换这个文本。
对于文本节点的更新，我们使用了发布订阅者模式，属性作为一个主题，我们为这个节点设置一个订阅者对象，将这个订阅者对象加入这个属性主题的订阅者列表中。当 Model 层数据发生改变的时候，Model 作为发布者向主题发出通知，主题收到通知再向它的所有订阅者推送，订阅者收到通知后更改自己的数据。

//data更新之后，vue是怎么更新视图的
Vue 实现响应式并不是数据发生变化之后 DOM 立即变化，而是异步执行DOM的更新

//Vue2.x响应式数据原理
Vue在初始化数据时，会使用Object.defineProperty重新定义data中的所有属性，当页面使用对应属性时，首先会进行依赖收集(收集当前组件的watcher)如果属性发生变化会通知相关依赖进行更新操作(发布订阅)。
//Vue3.x响应式数据原理
Vue3.x改用Proxy替代Object.defineProperty。因为Proxy可以直接监听对象和数组的变化，并且有多达13种拦截方法。并且作为新标准将受到浏览器厂商重点持续的性能优化。
//Proxy只会代理对象的第一层，Vue3怎么解决这个问题
判断当前Reflect.get的返回值是否为Object，如果是则再通过reactive方法做代理， 这样就实现了深度观测
//监测数组的时候可能触发多次get/set,如何防止触发多次
判断key是否为当前被代理对象target自身属性，也可以判断旧值与新值是否相等，只有满足以上两个条件之一时，才有可能执行trigger
````



### 3、Object.defineProperty()来进行数据劫持有什么缺点

````js
有一些对属性的操作，使用这种方法无法拦截，比如说通过下标方式修改数组数据或者给对象新增属性，vue 内部通过重写函数解决了这个问题。在 Vue3.0 中已经不使用这种方式了，而是通过使用 Proxy 对对象进行代理，从而实现数据劫持。使用 Proxy 的好处是它可以完美的监听到任何方式的数据改变，唯一的缺点是兼容性的问题，因为这是 ES6 的语法。
````



### 4、Virtual DOM？

````js
我对 Virtual DOM 的理解是，
首先对我们将要插入到文档中的 DOM 树结构进行分析，使用 js 对象将其表示出来，比如一个元素对象，包含 TagName、props 和 Children 这些属性。然后我们将这个 js 对象树给保存下来，最后再将 DOM 片段插入到文档中。

当页面的状态发生改变，我们需要对页面的 DOM 的结构进行调整的时候，我们首先根据变更的状态，重新构建起一棵对象树，然后将这棵新的对象树和旧的对象树进行比较，记录下两棵树的的差异。

最后将记录的有差异的地方应用到真正的 DOM 树中去，这样视图就更新了。

我认为 Virtual DOM 这种方法对于我们需要有大量的 DOM 操作的时候，能够很好的提高我们的操作效率，通过在操作前确定需要做的最小修改，尽可能的减少 DOM 操作带来的重流和重绘的影响。其实 Virtual DOM 并不一定比我们真实的操作 DOM 要快，这种方法的目的是为了提高我们开发时的可维护性，在任意的情况下，都能保证一个尽量小的性能消耗去进行操作。

由于在浏览器中操作DOM是很昂贵的。频繁的操作DOM，会产生一定的性能问题。这就是虚拟Dom的产生原因。
Vue2的Virtual DOM借鉴了开源库snabbdom的实现。
Virtual DOM本质就是用一个原生的JS对象去描述一个DOM节点。是对真实DOM的一层抽象。(也就是源码中的VNode类，它定义在src/core/vdom/vnode.js中。)
VirtualDOM映射到真实DOM要经历VNode的create、diff、patch等阶段。
````



### 5、diff算法及其原理

````js
渲染组件时，会通过Vue.extend方法构建子组件的构造函数，并进行实例化。最终手动调用$mount()进行挂载。更新组件时会进行patchVnode()流程，核心就是diff算法。

正常Diff两个树的时间复杂度是O(n^3)，但实际情况下我们很少会进行跨层级的移动DOM，所以Vue将Diff进行了优化，从O(n^3) -> O(n)，只有当新旧children都为多个子节点时才需要用核心的Diff算法进行同层级比较。

1、先同级比较，再比较子节点
2、先判断一方有子节点，另一方没子节点的情况(如果新的children没有子节点，将旧的子节点移除)
3、比较都有子节点的情况
4、递归比较子节点

Vue2的核心Diff算法采用了双端比较的算法，同时从新旧children的两端开始进行比较，借助key值找到可复用的节点，再进行相关操作。相比React的Diff算法，同样情况下可以减少移动节点次数，减少不必要的性能损耗，更加的优雅。

Vue3.x在创建VNode时就确定其类型，以及在mount/patch的过程中采用位运算来判断一个VNode的类型，在这个基础之上再配合核心的Diff算法，使得性能上较Vue2.x有了提升。
````



### 6、谈谈你对webpack的看法

````js
我当时使用 webpack 的一个最主要原因是为了简化页面依赖的管理，并且通过将其打包为一个文件来降低页面加载时请求的资源数。

我认为 webpack 的主要原理是，它将所有的资源都看成是一个模块，并且把页面逻辑当作一个整体，通过一个给定的入口文件，webpack 从这个文件开始，找到所有的依赖文件，将各个依赖文件模块通过 loader 和 plugins 处理后，然后打包在一起，最后输出一个浏览器可识别的 JS 文件。

Webpack 具有四个核心的概念，分别是 Entry（入口）、Output（输出）、loader 和 Plugins（插件）。

Entry 是 webpack 的入口起点，它指示 webpack 应该从哪个模块开始着手，来作为其构建内部依赖图的开始。

Output 属性告诉 webpack 在哪里输出它所创建的打包文件，也可指定打包文件的名称，默认位置为 ./dist。

loader 可以理解为 webpack 的编译器，它使得 webpack 可以处理一些非 JavaScript 文件。在对 loader 进行配置的时候，test 属性，标志有哪些后缀的文件应该被处理，是一个正则表达式。use 属性，指定 test 类型的文件应该使用哪个 loader 进行预处理。常用的 loader 有 css-loader、style-loader 等。

插件可以用于执行范围更广的任务，包括打包、优化、压缩、搭建服务器等等，要使用一个插件，一般是先使用 npm 包管理器进行安装，然后在配置文件中引入，最后将其实例化后传递给 plugins 数组属性。

使用 webpack 的确能够提供我们对于项目的管理，但是它的缺点就是调试和配置起来太麻烦了。但现在 webpack4.0 的免配置一定程度上解决了这个问题。但是我感觉就是对我来说，就是一个黑盒，很多时候出现了问题，没有办法很好的定位。
````



### 7、require模块引入的查找方式

````js
当 Node 遇到 require(X) 时，按下面的顺序处理。

（1）如果 X 是内置模块（比如 require('http')）
　　a. 返回该模块。
　　b. 不再继续执行。

（2）如果 X 以 "./" 或者 "/" 或者 "../" 开头
　　a. 根据 X 所在的父模块，确定 X 的绝对路径。
　　b. 将 X 当成文件，依次查找下面文件，只要其中有一个存在，就返回该文件，不再继续执行。
    X
    X.js
    X.json
    X.node

　　c. 将 X 当成目录，依次查找下面文件，只要其中有一个存在，就返回该文件，不再继续执行。
    X/package.json（main字段）
    X/index.js
    X/index.json
    X/index.node

（3）如果 X 不带路径
　　a. 根据 X 所在的父模块，确定 X 可能的安装目录。
　　b. 依次在每个目录中，将 X 当成文件名或目录名加载。

（4）抛出 "not found"
````



### 11、vue-router中的导航钩子函数

````js
（1）全局的钩子函数 beforeEach 和 afterEach
beforeEach 有三个参数，to 代表要进入的路由对象，from 代表离开的路由对象。next 是一个必须要执行的函数，如果不传参数，那就执行下一个钩子函数，如果传入 false，则终止跳转，如果传入一个路径，则导航到对应的路由，如果传入 error ，则导航终止，error 传入错误的监听函数。
（2）单个路由独享的钩子函数 beforeEnter，它是在路由配置上直接进行定义的。
（3）组件内的导航钩子主要有这三种：beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave。它们是直接在路由组件内部直接进行定义的。
````



### 12、$route和$router的区别

````js
$route 是“路由信息对象”，包括 path，params，hash，query，fullPath，matched，name 等路由信息参数。
而 $router 是“路由实例”对象包括了路由的跳转方法，钩子函数等。
````



### 13、vue常用修饰符

````js
.prevent: 提交事件不再重载页面，防止执行预设的行为；
.stop: 阻止单击事件冒泡；
.self: 当事件发生在该元素本身而不是子元素的时候会触发；
.once: 只会触发一次；
.capture: 与事件冒泡的方向相反，即事件捕获
````

​                                               

### 15、vue中mixin和mixins的区别

````js
mixin 用于全局混入，会影响到每个组件实例。
mixins 应该是我们最常使用的扩展组件的方式了。如果多个组件中有相同的业务逻辑，就可以将这些逻辑剥离出来，通过 mixins 混入代码，比如上拉下拉加载数据这种逻辑等等。另外需要注意的是 mixins 混入的钩子函数会先于组件内的钩子函数执行，并且在遇到同名选项的时候也会有选择性的进行合并

Vue.mixin   给组件每个生命周期，函数等都混入一些公共逻辑

Vue.mixin({
    beforeCreate(){
        
    }
})
````



### 17、nexttick的实现原理

````js
//nextTick是如何做到监听dom更新完毕的？
vue用异步队列的方式来控制DOM更新和nextTick回调先后执行，保证了能在dom更新后在执行回调
//nextTick实现原理
在下次 DOM 更新循环结束之后执行延迟回调。nextTick主要使用了宏任务和微任务。根据执行环境分别尝试采用
Promise
MutationObserver
setImmediate
如果以上都不行则采用setTimeout
定义了一个异步方法，多次调用nextTick会将方法存入队列中，通过这个异步方法清空当前队列。

````



### 18、★单页面和多页面的优缺点

````js
//单页面的优缺点
SPA仅在 Web 页面初始化时加载相应的 HTML、JavaScript 和 CSS。一旦页面加载完成，SPA 不会因为用户的操作而进行页面的重新加载或跳转；取而代之的是利用路由机制实现 HTML 内容的变换，UI 与用户的交互，避免页面的重新加载。
//优点:
用户体验好、快，内容的改变不需要重新加载整个页面，避免了不必要的跳转和重复渲染； 
基于上面一点, SPA对于服务器的压力较小 
前后端职责分离,架构清晰 
//缺点：
初次加载耗时多:为实现单页Web应用功能及显示效果,需要在加载页面的时候将JavaScript, CSS统一加载,部分页面按需加载;
在SEO上其有着天然的弱势。

//多页面的优缺点

````



### 19、★axios和ajax的区别

````js
ajax实现了网页的局部数据更新，axios实现了对ajax的封装
axios客户端支持防止CSRF，在每个请求都带一个从cookie中拿到的key, 根据浏览器同源策略，假冒的网站是拿不到cookie中的key的，这样，后台就可以轻松辨别出这个请求是否是用户在假冒网站上的误导输入，从而采取正确的策略。
````



### 20、★你是如何使用vue-router的，你都用到了哪些属性？

````js
在项目中主要是用vue-router来实现前端路由
<router-link> 用于导航，通过传入“to”属性来指定链接，默认会被渲染成一个<a>标签
<router-view> 路由出口，路由匹配到的组件将会渲染在这里
$router.push
path: 路径
name: 路由名称，对应router-view的名称
component: 组件名
redirect:  不渲染组件，直接重定向
children:  嵌套路由
//动态路径参数，以冒号开头，当匹配到一个路由时，参数值会被设置到this.$route.params
如/product/1，则$route.prams = {id: 1}
动态路由：  path:'/product/:id'  //动态路径参数  以冒号开头
//当使用路由参数时，例如从/product/1 导航到/product/2，原来的组件实例会被复用。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。不过，这也意味着组件的生命周期钩子不会再被调用。
路由懒加载：延迟加载或按需加载，即在需要的时候的时候进行加载
在单页应用中，如果没有应用懒加载，运用webpack打包后的文件将会异常的大，这就会造成进入首页时，需要加载的内容过多，延时过长，不利于用户体验，运用懒加载可以将页面进行划分，按需加载页面，可以分担首页所承担的加载压力，减少加载用时。
结合 Vue 的异步组件和 Webpack 的代码分割功能
````



### 21、★Vue和React的区别，包括原理和写法？

````js
//数据是不是可变的
react整体是函数式的思想，把组件设计成纯组件，状态和逻辑通过参数传入，所以在react中，是单向数据流，推崇结合immutable来实现数据不可变。
而vue的思想是响应式的，也就是基于是数据可变的，通过对每一个属性建立Watcher来监听，当属性变化的时候，响应式的更新对应的虚拟dom。
//通过js来操作一切，还是用各自的处理方式
react的思路是all in js，通过js来生成html，所以设计了jsx，还有通过js来操作css，社区的styled-component、jss等
vue是把html，css，js组合到一起，用各自的处理方式，vue有单文件组件，可以把html、css、js写到一个文件中，html提供了模板引擎来处理。
//类式的组件写法，还是声明式的写法
react是类式的写法，api很少
而vue是声明式的写法，通过传入各种options，api和参数都很多。所以react结合typescript更容易一起写，vue稍微复杂
//什么功能内置，什么交给社区去做
react做的事情很少，很多都交给社区去做，vue很多东西都是内置的，写起来确实方便一些
//模板渲染方式不同
React是在组件JS代码中，通过原生JS实现模板中的常见语法，比如插值，条件，循环等，都是通过JS语法实现的
Vue是在和组件JS代码分离的单独的模板中，通过指令来实现的，比如条件语句就需要 v-if 来实现
````



### 22、★你是如何使用vuex的，vuex中的store中的数据是如何获取和赋值的？

````js
我使用vuex主要是为了管理用户信息、权限控制等全局状态量
将数据存入vuex中（在要存入数据的页面写）
this.$store.commit("print/setPrint", {  //print 表示vuex的文件名
       ID: this.ID,
       BrandID: 402
});
将数据从vuex中取出来使用（在要使用的页面写）
import { mapState, mapActions } from "vuex";
    computed: {
        ...mapState({
             print:state=>state.print.all
        })
}
在用到的地方直接写入以下代码即可：
this.CreateID = this.print.ID;
this.GoodsID = this.print.BrandID;
````



### 23、★Vue如何实现多页面应用？

````js
//用nust.js

//修改webpack配置
在webpack的entry中添加多个入口
````



### 24、./和../和@的区别

````js
/表示相对路径，具体代表当前目录下的同级目录，遵从的是从后往前找文件

@/的意思：
表示的是相对路径(当然这也是简写啦)，因为这个在根目录/build/webpack.base.conf.js文件中@是配置的，
比如我的配置文件中@就代表src目录，遵从的是从前往后找，比如’@/components/login’ 就表示的是src/components/login文件
````



### 25、★Axios

````js
axios 是什么
1. Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。前端最流行的 ajax 请求库，
2. react/vue 官方都推荐使用 axios 发 ajax 请求

axios 特点
1. 基于 promise 的异步 ajax 请求库，支持promise所有的API
2. 浏览器端/node 端都可以使用，浏览器中创建XMLHttpRequests，在node环境使用http对象发送ajax请求。
3. 支持请求／响应拦截器
4. 支持请求取消
5. 可以转换请求数据和响应数据，并对响应回来的内容自动转换成 JSON类型的数据
6. 批量发送多个请求
7. 安全性更高，客户端支持防御 XSRF，就是让你的每个请求都带一个从cookie中拿到的key, 根据浏览器同源策略，假冒的网站是拿不到你cookie中得key的，这样，后台就可以轻松辨别出这个请求是否是用户在假冒网站上的误导输入，从而采取正确的策略。

if (!window.navigator.online) { // 断网处理
   // todo: jump to offline page
   return -1;

//请求拦截
axios.interceptors.request.use

//响应拦截
axios.interceptors.response.use(function(response) {
  let res = response.data;
  if(res.status == 0) {
    return res.data;
  } else if(res.status == 10) {
      window.location.href = '/#/login';
      return Promise.reject(res);
  } else {
    //alert(res.msg);
    Message.warning(res.msg);
    return Promise.reject(res);
  }
},(error)=>{
  return Promise.reject(error);
});
    
//执行多个并发请求
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    // 两个请求现在都执行完成
  }));
axios.spread(): 用来指定接收所有成功数据的回调函数的方法
    
//移除拦截器
const myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
````



### 26、★v-if和v-show有什么区别？

````js
v-if 是真正的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建；也是短路的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 的 “display” 属性进行切换。
所以，v-if 适用于在运行时很少改变条件，不需要频繁切换条件的场景；v-show 则适用于需要非常频繁切换条件的场景。

//为什么v-for和v-if不能连用
v-for会比v-if的优先级高一些，如果连用的话会把v-if添加到每个元素上，造成性能问题
````



### 27、Vue如何检测数组变化？

````js
考虑到性能原因，数组没有用defineProperty对数组的每一项元素进行劫持，而是选择重写数组的push、shift、pop、splice、unshift、sort、reverse等方法，通过更改数组的原型方法来手动进行数组更新，会对数组的每一项进行观测；
在Vue中修改数组的索引和长度是无法监控到的，需要通过以上7种变异方法修改数组才会触发数组对应的watcher进行更新，数组中如果是对象类型也会进行递归劫持；
如果想通过更改索引来更新数据，可以通过Vue.$set()来进行处理，内部用的是splice方法；
````



### 28、为什么Vue要采用异步渲染？

````js
如果不采用异步更新，那么每次更新数据都会对当前组件进行重新渲染，所以为了性能考虑，Vue会在本轮数据更新后，再去异步更新视图。
````



### 29、Watch中的deep：true是如何实现的

````js
vm.$watch('msg',()=>{})   //监听值，值变化则调用后面的函数
当用户指定了watch中的deep属性为true时，如果当前监控的值是数组类型，会对对象中的每一项进行求值，此时会将当前watcher存入到对应属性的依赖中，这样数组中对象发生变化时也会通知数据更新
````



### 30、★Webpack

````js
//webpack打包方式
1、命令行输入：webpack {entry file} {destination for bundled file}   入口文件路径和打包文件的存放路径
2、配合配置文件进行打包：在根目录下新建一个名为webpack.config.js的文件，配置好之后在终端里运行webpack即可

//webpack实现多入口打包
多入口，创建多个html-webpack-plugin实例


上线以后路径发生变化，需要配置webpack，vue-cli较少出现，因为已经默认配置好
webpack.base.config.js和webpack.pro.config.js的publicPath路径不同
在打包时需要使用相对路径来处理静态资源，更改build中资源发布路径配置（config/index.js, build对象）：

module.exports = {
    entry: "./runoob1.js",
    output: {
        path: __dirname,
        filename: "bundle.js"
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: "style-loader!css-loader" }
        ]
    }
};
有配置文件中只需要输入webpack即可打包，没有配置文件需要输入webpack + 入口文件路径 + 输出文件路径
````



### 31、★如果用户没有登录，需要跳转到登录界面，这个怎么实现？axios拦截器能否获取到当前用户正处于哪个页面？如果有多个请求，如何避免重复跳转或者重复弹出未登录的提示？

````js
//router.beforeEach
用路由守卫判断用户用户登录状态,未登录则跳转到登录页面，已登录则正常进行页面跳转

1、路由拦截
首先在定义路由的时候就需要多添加一个自定义字段requireAuth，用于判断该路由的访问是否需要登录。如果用户已经登录，则顺利进入路由， 否则就进入登录页面。
2、拦截器
要想统一处理所有http请求和响应，就得用上 axios 的拦截器。通过配置http response inteceptor，当后端接口返回401 Unauthorized（未授权），让用户重新登录。
````



### 32、组件中的data为什么是一个函数？

````js
同一个组件被复用多次，会创建多个实例。这些实例用的是同一个构造函数，如果data是一个对象的话，那么所有组件都共享了同一个对象。为了保证组件的数据独立性要求每个组件必须通过data函数返回一个对象作为组件的状态。
````



### 33、Vue中事件绑定的原理，包含原生事件绑定和组件事件绑定

````js
原生dom事件的绑定，采用的是addEventListener实现
组件绑定事件采用的是$on方法
````



### 34、v-model中的实现原理及如何自定义v-model

````js
<el-checkbox :value="" @input=""></el-checkbox>
<el-checkbox v-model="check"></el-checkbox>

v-model可以看成是value+input方法的语法糖，前提情况是没有重新定义prop和event属性
原生的v-model会根据标签的不同生成不同的事件和属性，
如input checkbox标签中的v-model编译后的value就是checked，事件就是change
````



### 35、解决页面刷新后vuex的store数据丢失的问题

````js
项目中使用的方法
mounted() {
    // 在页面刷新重新发请求获取username和count，重新保存state
    // 解决了页面刷新后vuex的store数据丢失的问题
    // 因为vuex的store数据是保存在运行内存的，当页面刷新后内存会被清除，所以数据会丢失
    if(this.$cookie.get('userId')){
      this.getUser();
      this.getCartCount();
    }
  },
  methods: {
    getUser(){
      this.axios.get('/user').then((res={})=>{
      this.$store.dispatch('saveUserName',res.username);
      })
    },
    getCartCount(){
      this.axios.get('/carts/products/sum').then((res=0)=>{
      this.$store.dispatch('saveCartCount',res);
      })
    }
  }

其他方法：监听页面的unload事件，将store中的数据保存到localstorage中，当页面刷新完毕后，从localstorage中取出数据赋值给store
````



### 36、v-html会导致哪些问题？

````js
1、可能会导致XSS攻击，类似于innerHTML
2、v-html会替换掉标签内部的子元素
````



### 37、父子组件生命周期调用顺序

````js
组件的调用顺序都是先父后子，渲染完成的顺序是先子后父
组件的销毁操作是先父后子，销毁完成的顺序是先子后父
//加载渲染过程
父beforeCreate -> 父created -> 父beforeMount -> 子beforeCreate -> 子created -> 子beforeMount -> 子mounted -> 父mountd
//子组件更新过程
父beforeUpdate -> 子beforeUpdate -> 子updated -> 父updated
//父组件更新过程
父beforeUpdate -> 父updated
//销毁过程
父beforeDestroy -> 子beforeDestroy -> 子Destroyed -> 父Destroyed
````



### 38、异步组件

````js
如果组件功能多，打包结果会很大，可以采用异步的方式来加载组件。
主要依赖import()这个语法，可以实现文件的分割加载。
````



### 39、作用域插槽

````js
//插槽
创建组件虚拟节点时，会将组件的儿子的虚拟节点保存起来，当初始化组件时，通过插槽属性将儿子进行分类
{a:[vnode],b[vnode]}
渲染组件时会拿对应的slot属性的节点进行替换操作。（插槽的作用域为父组件）

//作用域插槽
作用域插槽在解析的时候，不会作为组件的孩子节点。会解析成函数，当子组件渲染时，会调用此函数进行渲染（插槽的作用域为子组件）
````



### 40、Vue中常见性能优化

````js
//编码优化
1、尽量减少data中的数据，因为data中的数据都会增加getter和setter，会收集对应的watcher
2、在使用v-for给每项元素绑定事件时使用事件代理
3、SPA页面采用keep-alive缓存组件
4、拆分组件（提高复用性、增加代码的可维护性，减少不必要的渲染）
5、v-if当值为false时内部指令不会执行，具有阻断功能，很多情况下使用v-if代替v-show
6、key保证唯一性（默认vue会采用就地复用策略）
7、Object.freeze冻结数据
8、合理使用路由懒加载、异步组件
9、尽量采用runtime运行时版本
10、数据持久化的问题（防抖、节流）
11、V-if和v-for不能连用

//Vue加载性能优化
1、第三方模块按需导入(babel-plugin-component)
2、滚动到可视区域动态加载
3、图片懒加载

//用户体验
1、app-skeleton 骨架屏
2、app-shell app壳
3、pwa

//SEO优化
1、预渲染插件 prerender-spa-plugin
2、服务端渲染SSR

//打包优化
1、使用CDN的方式加载第三方模块
2、多线程打包happypack
3、splitChunks 抽离公共文件
4、sourceMap 优化

//缓存、压缩
1、客户端缓存、服务端缓存
2、服务端gzip压缩
````



### 41、Vue3.0改进

````js
Vue3采用了TypeScript来编写
支持 Composition API
//解决了mixin的缺陷，保证代码逻辑的耦合性
Vue3中响应式数据原理改成proxy
vdom的对比算法更新，只更新vdom的绑定了动态数据的部分
````



### 42、Vue-Router中导航守卫有哪些？

````js
//完整的导航解析流程
1、导航被触发
2、在失活的组件里调用离开守卫
3、调用全局的beforeEach守卫
4、在重用的组件里调用beforeRouteUpdate 守卫（2.2+）
5、在路由配置里调用beforeEnter
6、解析异步路由组件
7、在被激活的组件里调用 beforeRouteEnter
8、调用全局的 beforeResolve 守卫（2.5+）
9、导航被确认
10、调用全局的 afterEach 钩子
11、触发 DOM 更新
12、用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数
````



### 43、Vue模板编译原理

````js
简单说，Vue的编译过程就是将template转化为render函数的过程。会经历以下阶段：
1、生成AST树
2、优化
3、codegen
首先解析模版，生成AST语法树(一种用JavaScript对象的形式来描述整个模板)。
使用大量的正则表达式对模板进行解析，遇到标签、文本的时候都会执行对应的钩子进行相关处理。
Vue的数据是响应式的，但其实模板中并不是所有的数据都是响应式的。有一些数据首次渲染后就不会再变化，对应的DOM也不会变化。那么优化过程就是深度遍历AST树，按照相关条件对树节点进行标记。这些被标记的节点(静态节点)我们就可以跳过对它们的比对，对运行时的模板起到很大的优化作用。
编译的最后一步是将优化后的AST树转换为可执行的代码。
````



### 43、SSR

````js
SSR就是服务端渲染，也就是将Vue在客户端把标签渲染成HTML的工作放在服务端完成，然后再把html直接返回给客户端。
SSR有着更好的SEO、并且首屏加载速度更快等优点。不过它也有一些缺点，比如我们的开发条件会受到限制，服务器端渲染只支持beforeCreate和created两个钩子，当我们需要一些外部扩展库时需要特殊处理，服务端渲染应用程序也需要处于Node.js的运行环境。还有就是服务器会有更大的负载需求。
````



### v-cloak

````js
可以使用 v-cloak 指令设置样式，这些样式会在 Vue 实例编译结束时，从绑定的 HTML 元素上被移除。
使用 v-cloak 指令可以解决页面加载时页面闪烁的问题
````



### 动态组件

````js
让多个组件使用同一个挂载点，并动态切换，这就是动态组件。
通过使用保留的 <component> 元素，动态地绑定到它的 is 特性，可以实现动态组件
````



### Vue自定义指令

````js
通过 Vue.directive( id, [definition] ) 方式注册全局指令，第一个参数为自定义指令名称（指令名称不需要加 v- 前缀，默认是自动加上前缀的，使用指令的时候一定要加上前缀），第二个参数可以是对象数据，也可以是一个指令函数。
Vue.directive("focus", {
        inserted: function(el){
            el.focus();
        }
    })

通过在Vue实例中添加 directives 对象数据注册局部自定义指令
new Vue({
        el: "#app",
        directives: {
            focus2: {
                inserted: function(el){
                    el.focus();
                }
            }
        }
    })

一个指令定义对象可以提供如下几个钩子函数 (均为可选)：
bind：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
inserted：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
update：所在组件的 VNode 更新时调用。
componentUpdated：指令所在组件的 VNode 及其子 VNode 全部更新后调用。
unbind：只调用一次，指令与元素解绑时调用。

指令钩子函数会被传入以下参数:
el: 指令所绑定的元素，可以用来直接操作 DOM，就是放置指令的那个元素。
binding: 一个对象，里面包含了几个属性，这里不多展开说明，官方文档上都有很详细的描述。
vnode：Vue 编译生成的虚拟节点。
oldVnode：上一个虚拟节点，仅在 update 和 componentUpdated 钩子中可用。
````















