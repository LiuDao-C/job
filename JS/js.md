## JavaScript

### 1、深克隆和浅克隆

````js
let obj = {
    a: 100,
    b: [10, 20, 30],
    c: {
        x: 10
    },
    d: /^\d+$/
};

let arr = [10, [100, 200], {
    x: 10,
    y: 20
}];

// let obj2 = {
// 	...obj
// };

// let obj2 = {};
// for (let key in obj) {
// 	if (!obj.hasOwnProperty(key)) break;
// 	obj2[key] = obj[key];
// }

// let obj2 = JSON.parse(JSON.stringify(obj)); //=>弊端?

//=>深克隆
function deepClone(obj) {
    if (typeof obj !== "object") return obj;
    if (obj instanceof RegExp) return new RegExp(obj);
    if (obj instanceof Date) return new Date(obj);
    let cloneObj = new obj.constructor;
    for (let key in obj) {
        if (obj.hasOwnProperty(key)) {
            cloneObj[key] = deepClone(obj[key]);
        }
    }
    return cloneObj;
}
````



### 2、JS数据类型

````js
基本数据类型：undefined,Null,Boolean,Number,String,BigInt,Symbol
//Symbol代表创建后独一无二且不可变的数据类型，可以解决可能出现的全局变量冲突的问题
复杂数据类型指的是Object类型，所有其他的如Array、Date等数据类型都可以理解为Object类型的子类。
两种类型的主要区别是它们的存储位置不同，基本数据类型的值直接保存在栈中，而复杂数据类型的值保存在堆中，通过使用在栈中保存对应的指针来获取堆的值
````



### 3、内部属性[[class]]

````js
所有typeof返回值为“object”的对象（如数组）都包含一个内部属性[[class]]，这个属性无法直接访问，一般通过Object.prototype.toString()来查看。

Object.prototype.toString.call( [1,2,3] );
// "[object Array]"
````



### 4、JS内置对象

````js
JS中的内置对象主要指的是在程序执行前就存在全局作用域里的由js定义的一些全局值属性、函数和用来实例化其他对象的构造函数对象。一般我们经常用到的如全局变量值NaN、undefined，全局函数如parseInt()、parseFloat()，用来实例化对象的构造函数如Date、Object等，还有提供数学计算的单体内置对象如Math对象。
````



### 5、undefined、null和undeclared

````js
undefined表示在作用域中声明但还没有赋值的变量。undefined在js中不是一个保留字，这意味这我们可以使用undefined来作为变量名，这样做是非常危险的，它会影响我们对undefined值的判断，但是我们可以通过一些方法获得安全的undefined值，如void 0，typeof会返回“undefined"。

undeclared表示还没有在作用域中声明过的变量，对于undeclared变量的引用，浏览器会报引用错误，如ReferenceError: b is not defined。我们可以使用typeof的安全防范机制来避免这一错误，因为对于undeclared变量，typeof会返回“undefined"。

null的含义是空对象，一般用于赋值给一些可能会返回对象的变量作为初始化。typeof会返回"object”。
使用==对undefined和null进行比较时会返回true，使用===则会返回false。
````



### 6、写JS的基本规范

````js
在平常项目开发中，我们遵守一些这样的基本规范，比如说：
//一个函数作用域中所有的变量声明应该尽量提到函数首部，用一个var声明，不允许出现两个连续的var声明，声明时如果变量没有值，应该给该变量赋值对应类型的初始值，便于他人阅读代码时，能够一目了然的知道变量对应的类型值。
//代码中出现地址、时间等字符串时需要使用常量代替。
//在进行比较的时候吧，尽量使用'===', '!=='代替'==', '!='。
//不要在内置对象的原型上添加方法，如 Array, Date。
//switch 语句必须带有 default 分支。
//for 循环和if语句必须使用大括号。
````



### 7、JS原型、原型链

````js
在js中我们是使用构造函数来新建一个对象的，每一个构造函数的内部都有一个 prototype 属性值，这个属性值是一个对象，包含了可以由该构造函数的所有实例共享的属性和方法。
当我们使用构造函数新建一个对象后，在这个对象的内部将包含一个指针，这个指针指向构造函数的prototype 属性对应的值，在ES5中这个指针被称为对象的原型。
现在浏览器中都实现了 _proto_ 属性来让我们访问这个属性，但是最好不要使用这个属性，因为它不是规范中规定的属性。ES5中新增了一个 Object.getPrototypeOf() 方法，我们可以通过这个方法来获取对象的原型。

当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么它就会原型对象里找，而这个原型对象又会有自己的原型，于是就这样一直找下去，这就是原型链的概念。原型链的尽头一般来说都是 Object.protype，这也是新建的对象为什么能够使用toString()等方法的原因。

特点：
JS对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。所以当我们修改原型时，与之相关的对象也会随之改变。

获取原型的方法：
p._proto_
p.constructor.prototype
Object.getPrototypeOf(p)
````



### 8、NaN相关

````js
typeof NaN 会返回“number", NaN是唯一一个非自反的值，即 NaN === NaN 为false
函数isNaN接收参数后，会尝试将这个参数转换为数值，任何不能被转换为数值的值都会返回true，因此非数字值传入也会返回true
函数Number.isNaN会首先判断传入参数是否为数字，如果是数字再继续判断是否为NaN，这种方法对于NaN的判断更为准确。
````



### 9、实现数组的随机排序

````js
// （1）使用数组 sort 方法对数组元素随机排序，让 Math.random() 出来的数与 0.5 比较，如果大于就返回 1 交换位置，如果小于就返回 -1，不交换位置。
function randomSort(a, b) {
  return Math.random() > 0.5 ? -1 : 1;
}
//  缺点：每个元素被派到新数组的位置不是随机的，原因是 sort() 方法是依次比较的。

// （2）随机从原数组抽取一个元素，加入到新数组
function randomSort(arr) {
  var result = [];
  while (arr.length > 0) {
    var randomIndex = Math.floor(Math.random() * arr.length);
    result.push(arr[randomIndex]);
    arr.splice(randomIndex, 1);
  }
  return result;
}

// （3）随机交换数组内的元素（洗牌算法类似）
function randomSort(arr) {
  var index,
    randomIndex,
    temp,
    len = arr.length;
  for (index = 0; index < len; index++) {
    randomIndex = Math.floor(Math.random() * (len - index)) + index;
    temp = arr[index];
    arr[index] = arr[randomIndex];
    arr[randomIndex] = temp;
  }
  return arr;
}

// （4）第3个方法用es6实现
function randomSort(array) {
  let length = array.length;
  if (!Array.isArray(array) || length <= 1) return;
  for (let index = 0; index < length - 1; index++) {
    let randomIndex = Math.floor(Math.random() * (length - index)) + index;
    [array[index], array[randomIndex]] = [array[randomIndex], array[index]];
  }
  return array;
}
````



### 10、JS创建对象的方式

````js
我们一般使用字面量的形式直接创建对象，但是这种创建方式对于创建大量相似对象的时候，会产生大量的重复代码。js和一般的面向对象的语言不同，在ES6之前它没有类的概念，但我们可以使用函数来进行模拟，从而产生出可复用的对象创建方式，我了解到的方式有这么几种:
//第一种是工厂模式，工厂模式的主要工作原理是用函数来封装创建对象的细节，通过调用函数来达到复用的目的。但是用这种方式创建对象没法知道对象的类型是什么，它只是简单的封装了复用代码，没有建立起对象和类型间的关系。
//第二种是构造函数模式。js中每一个函数都可以作为构造函数，只要一个函数是通过new来调用的，我们就把它称为构造函数。执行构造函数首先会创建一个对象，接着将对象的原型指向构造函数的prototype属性，再将执行上下文中的this指向这个对象，最后再执行整个函数，如果返回值不是对象，则返回新建的对象。因为this的值指向了新建的对象，因此我们可以使用this给对象赋值。构造函数模式相对于工厂模式的有点是，所创建的对象和构造函数建立起了联系，因此我们可以通过原型来识别对象的类型。但是构造函数模式有一个缺点就是创建了不必要的函数对象，因为在js中，函数也是一个对象，因此如果对象属性中包含函数的话，那么每次我们都会新建一个函数对象，浪费了不必要的内存空间，因为函数是所有的实例都可以通用的。
//第三种是原型模式，因为每一个函数都有一个prototype属性，这个属性是一个对象，它包含了通过构造函数创建的所有实例都能够共享的属性和方法。因此我们可以使用原型对象来添加公用属性和方法，从而实现代码的复用。这种方式相对于构造函数模式来说，解决了函数对象的复用问题。但是这种模式也存在一些问题，一个是没有办法通过传入参数来初始化值，另一个是如果存在一个引用类型如Array这样的值，那么所有的实例将共享一个对象，一个实例对引用类型值的改变会影响所有的实例。
//第四种是组合使用构造函数模式和原型模式，这是创建自定义类型的最常见方式。因为构造函数模式和原型模式分开使用都存在一些问题，因此我们可以组合使用这两种模式，通过构造函数来定义实例属性，通过原型对象来定义共享属性和实现函数方法的复用。这种方法很好的解决了两种模式单独使用时的缺点，但是有一点不足的是，由于使用了两种不同的模式，对于代码的封装性不够好。
//第五种是动态原型模式，这一种模式将原型方法赋值的创建过程移动到了构造函数的内部，通过对属性是否存在的判断，可以实现仅在第一次调用函数时对原型对象赋值的效果。这一种方式很好的对上面的组合模式进行了封装。
//第六种是寄生构造函数模式，这一种模式和工厂模式的实现基本相同，我对这个模式的理解是，它主要是基于一个已有的类型，在实例化时对实例化的对象进行扩展。这样既不用修改原来的构造函数，也达到了扩展对象的目的。它的一个缺点和工厂模式一样，无法实现对象的识别，也就是不能依赖instanceof操作符来确定对象的类型。
嗯，我目前了解到的就是这几种方式。
````



### 11、JS继承的实现方式

````js
我了解的js中实现继承的方式有：
//以原型链的方式来实现继承，就是让子类的原型指向父类的实例，但是这种实现方式存在的缺点是，在包含有引用类型的数据时，会被所有的实例对象所共享，容易造成修改的混乱，而且在创建子类型的时候不能向超类型传递参数。
//借用构造函数实现继承，这种方式是通过在子类型的函数中调用父类型的构造函数来实现的，这种方法解决了不能向超类型传递参数的问题，但是它有一个问题就是无法实现函数方法的复用，并且子类型也没有办法访问到父类型原型定义的方法。
//第三种是组合继承，组合继承是将原型链和借用构造函数组合起来使用的一种方式。通过借用构造函数的方式来实现类型的属性的继承，通过将子类型的原型设置为父类型的实例来实现方法的继承。这种方式解决了之前两种模式单独使用时的问题，但是由于我们是以父类型的实例来作为子类型的原型，所以调用了两次父类型的构造函数，造成了子类型的原型中多了很多不必要的属性。
//第四种方式是原型式继承，原型式继承的主要思路就是基于已有的对象来创建新的对象，实现的原理是向函数中传入一个对象，然后返回一个以这个对象为原型的对象。这种继承的思路主要不是为了实现创造一种新的类型，只是对某个对象实现一种简单继承，ES5中定义的Object.create()方法就是原型式继承的实现。缺点与原型链方式相同。
//第五种方式是寄生式继承，寄生式继承的思路是创建一个用于封装继承过程的函数，通过传入一个对象，然后复制一个对象的副本，然后对对象进行扩展，最后返回这个对象。这个扩展的过程就可以理解是一种继承。这种继承的优点就是对一个简单对象实现继承，如果这个对象不是我们的自定义类型时。缺点是没有办法实现函数的复用。
//第六种方式是寄生式组合继承，组合继承的缺点就是使用父类型的实例做为子类型的原型，导致添加了不必要的原型属性。寄生式组合继承的方式是使用父类型的原型的副本来作为子类型的原型，这样就避免了创建不必要的属性。
function Child(name) {
    Parent.call(this, name);
}
Child.prototype = Object.create(Parent.prototype);
Child.prototype.constructor = Child;
````



### 12、JS的作用域链

````js
作用域链的作用是保证对执行环境有权访问的所有变量和函数的有序访问，通过作用域链，我们可以访问到外层环境的变量和函数。
作用域链本质上是一个指向变量对象的指针列表。变量对象是一个包含了执行环境中所有变量和函数的对象。作用域链的前端始终都是当前执行上下文的变量对象。全局执行上下文的变量对象（也就是全局对象）始终是作用域链的最后一个对象。
当我们查找一个变量时，如果当前执行环境中没有找到，我们可以沿着作用域链向后查找。
````



### 13、谈谈this对象的理解

````js
this是执行上下文中的一个属性，它指向最后一次调用这个方法的对象。在实际开发中，this的指向可以通过4种调用模式来判断。
//第一种是函数调用模式，当一个函数不是一个对象的属性，而是直接作为函数来调用时，this会指向全局对象。
//第二种时方法调用模式，如果一个函数是作为一个对象的方法来调用时，this会指向这个对象。
//第三种是构造器调用模式，如果一个函数用new调用时，在函数执行前会新创建一个对象，this会指向这个新创建的对象。
//第四种是apply、call和bind调用模式，这三个方法都可以显式的指定调用函数的this指向。其中apply方法接收两个参数：一个是this绑定的对象，一个是参数数组。call方法接收的参数一个是this绑定的对象，其余的参数是传入函数执行的参数。也就是说，在使用call（）方法时，传递给函数的参数必须逐个列举出来。bind方法通过传入一个对象，返回一个把this绑定为传入对象的新函数。这个函数的this指向除了使用new时会被改变，其他情况下都不会改变。
这4种模式，使用构造器调用模式的优先级最高，然后是apply、call和bind调用模式，然后是方法调用模式，最后是函数调用模式。
````



### 14、apply、call和bind的实现

````js
call 函数的实现步骤：
1.判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
2.判断传入上下文对象是否存在，如果不存在，则设置为 window 。
3.处理传入的参数，截取第一个参数后的所有参数。
4.将函数作为上下文对象的一个属性。
5.使用上下文对象来调用这个方法，并保存返回结果。
6.删除刚才新增的属性。
7.返回结果。
Function.prototype.myCall = function(context) {
  // 判断调用对象
  if (typeof this !== "function") {
    console.error("type error");
  }

  // 获取参数
  let args = [...arguments].slice(1),
    result = null;

  // 判断 context 是否传入，如果未传入则设置为 window
  context = context || window;

  // 将调用函数设为对象的方法
  context.fn = this;

  // 调用函数
  result = context.fn(...args);

  // 将属性删除
  delete context.fn;

  return result;
};

apply 函数的实现步骤：
1.判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
2.判断传入上下文对象是否存在，如果不存在，则设置为 window 。
3.将函数作为上下文对象的一个属性。
4.判断参数值是否传入
4.使用上下文对象来调用这个方法，并保存返回结果。
5.删除刚才新增的属性
6.返回结果
Function.prototype.myApply = function(context) {
  // 判断调用对象是否为函数
  if (typeof this !== "function") {
    throw new TypeError("Error");
  }

  let result = null;

  // 判断 context 是否存在，如果未传入则为 window
  context = context || window;

  // 将函数设为对象的方法
  context.fn = this;

  // 调用方法
  if (arguments[1]) {
    result = context.fn(...arguments[1]);
  } else {
    result = context.fn();
  }

  // 将属性删除
  delete context.fn;

  return result;
};

bind 函数的实现步骤：
1.判断调用对象是否为函数，即使我们是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
2.保存当前函数的引用，获取其余传入参数值。
3.创建一个函数返回
4.函数内部使用 apply 来绑定函数调用，需要判断函数作为构造函数的情况，这个时候需要传入当前函数的 this 给 apply 调用，其余情况都传入指定的上下文对象。
Function.prototype.myBind = function(context) {
  // 判断调用对象是否为函数
  if (typeof this !== "function") {
    throw new TypeError("Error");
  }

  // 获取参数
  var args = [...arguments].slice(1),
    fn = this;

  return function Fn() {
    // 根据调用方式，传入不同绑定值
    return fn.apply(
      this instanceof Fn ? this : context,
      args.concat(...arguments)
    );
  };
};
````



### 15、手写一个通用的事件侦听器函数

````js
const EventUtils = {
    //绑定事件需要兼容dom0、dom2和IE方式
    //添加事件
    addEvent: function(element, type, handler) {
        if (element.addEventListener) {
            element.addEventListener(type, handler, false);
        } else if (element.attachEvent) {
            element.attachEvent("on" + type, handler);
        } else {
            element["on" + type] = handler;
        }
    },
    
    //移除事件
    removeEvent: function(element, type, handler) {
        if (element.removeEventListener) {
            element.removeEventListener(type, handler, false);
        } else if (element.detachEvent) {
            element.detachEvent("on" + type, handler);
        } else {
            element["on" + type] = null;
        }
    },
    
    //获取事件目标
    getTarget: function(event) {
        return event.target || event.srcElement;
    },
    
    //获取event对象的引用，获取事件的所有信息，确保随时能使用event
    getEvent: function(event) {
        return event || window.event;
    },
    
    //阻止事件（主要是事件冒泡，因为IE不支持事件捕获）
    stopPropogation: function(event) {
        if (event.stopPropogation) {
            event.stopPropogation();
        } else {
            event.cancelBubble = true;
        }
    },
    
    //取消事件的默认行为
    preventDefault: function(event) {
        if (event.preventDefault) {
            event.preventDefault();
        } else {
            event.returnValue = false;
        }
    }
};
````



### 16、事件委托

````js
事件委托本质上是利用了浏览器事件冒泡的机制。因为事件在冒泡过程中会上传到父节点，并且父节点可以通过事件对象获取到目标节点，因此我们可以把子节点的监听函数定义在父节点上，由父节点的监听函数统一处理多个子元素的事件，这种方式称为事件委托/代理。
事件委托让我们不用为每一个子元素都绑定一个监听事件，这样就减少了内存上的消耗，通过事件委托我们还可以实现事件的动态绑定，比如说新增了一个节点，我们可以不用为它单独添加一个监听事件，它所发生的事件可以交给它的父节点的监听函数来处理。
````



### 17、★闭包，使用闭包时有哪些要注意的地方

````js
闭包是指有权访问另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，里面的函数可以访问到外面函数的局部变量。
闭包有两个常用的用途。
//第一个用途是创建私有变量。通过使用闭包，我们可以通过在外部调用闭包函数，从而在外部访问到函数内部的变量。
//第二个用途是使已经运行结束的函数上下文中的变量对象继续留在内存中，因为闭包函数保留了这个变量对象的引用，所以这个变量对象不会被回收。
由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
闭包会在父函数外部，改变父函数内部变量的值。所以如果你把父函数当作对象使用，把闭包当作它的公用方法，把内部变量当作它的私有属性，这时一定要小心，不要随便改变父函数内部变量的值。
````



### 18、'use strict'

````js
use strict 指的是严格运行模式，这种模式对js的使用添加了一些限制。比如说禁止this指向全局对象，还有禁止使用with语句等。设立严格模式的目的主要是为了消除代码使用中的一些不安全的使用方式，也是为了消除js语法本身的一些不合理的地方，以此来减少一些运行时的怪异的行为。同时使用严格运行模式也能够提高编译的效率，从而提高代码的运行速度。我认为严格模式代表了js一种更合理、更安全、更严谨的发展方向。
````



### 19、判断对象是否属于类的方法

````js
//使用instanceof运算符来判断构造函数的prototype属性是否出现在对象的原型链中的任何位置。
function myInstanceof(left, right) {
    let proto = Object.getPrototypeOf(left), //获取对象的原型
        prototype = right.prototype; //获取构造函数的prototype对象
    //判断构造函数的prototype对象是否在对象的原型链上
    while(true) {
        if (!proto)
            return false;
        if (proto === prototype)
            return true;
        proto = Object.getPrototypeOf(proto);
    }
}
//通过对象的constructor属性来判断，对象的constructor属性会指向该对象的构造函数，但是这种方式不是很安全，因为constructor属性可以被改写。
//如果需要判断的是某个内置的引用类型的话，可以使用Object.prototype.toString()方法来打印对象的[[class]]属性来进行判断。
````



### 20、new操作符的实现过程

````js
//首先创建了一个新的空对象
//设置原型，将对象的原型设置为函数的prototype对象
//让函数的this指向这个对象，执行构造函数的代码（为这个对象添加属性）
//判断函数的返回值类型，如果是值类型就返回创建的对象。如果是引用类型就返回这个引用类型的对象。
function myNew() {
    let newObject = null,
        constructor = Array.prototype.shift.call(arguments),
        result = null;
    //参数判断
    if (typeof constructor !== 'function') {
        console.error("type error");
        return;
    }
    //创建一个原型为构造函数的rpototype对象的对象
    newObject = Object.create(constructor.prototype);
    //将this指向新建对象并执行构造函数代码
    result = constructor.apply(newObject, arguments);
    //判断返回对象的类型
    let flag = result 
    && (typeof result === "object") || typeof result == "function");
    //判断返回结果
    return flag ? result : newObject;
}
//使用方法
myNew(构造函数,初始化参数);
````



### 21、JSON

````js
JSON是一种基于文本的轻量级的数据交换格式，它可以被任何的编程语言读取和作为数据格式进行传递。
在项目开发中，我们使用JSON作为前后端数据交换的方式。在前端我们将一个符合JSON格式的数据结构序列化为JSON字符串，然后将它传递到后端，后端将JSON格式的字符串解析后生成对应的数据结构，以此来实现前后端数据的传递。
因为JSON的语法是基于JS的，因此很容易将JSON和js中的对象混淆，但是需要注意的是JSON和js中的对象并不一致，JSON中的对象格式更加严格，比如说在JSON中属性值不能为函数，不能出现NaN这样的属性值等，所以大多数的js对象是不符合JSON对象的格式的。
在JS中由两个函数可以实现js数据结构和JSON格式的转换，一个是JSON.stringify函数，这个函数可以将传入的符合JSON格式的数据结构转换为一个JSON字符串。如果传入的数据结构不符合JSON格式，那么在序列化的时候会对这些值进行对应的特殊处理，使其符合规范。在前端向后端发送数据时，我们可以调用这个函数将数据对象转换为JSON格式的字符串。
另一个函数是JSON.parse，这个函数可以将JSON格式的字符串转换为js的数据结构，如果传入的字符串不是标准的JSON格式的字符串的话，将会抛出错误。当我们从后端接收到JSON格式的字符串时，我们可以使用这个函数将其解析为js中的数据结构，以此来进行数据的访问。
````



### 22、JS延迟加载的方式

````js
JS延迟加载就是等页面加载完成之后再加载JS文件，因为js的加载、解析和执行会阻塞页面的渲染过程，所以JS延迟加载可以提高页面的加载速度。
我了解到的JS延迟加载的方式有几种：
//第一种方式是将JS脚本放在文档的底部，因为页面的加载过程是从上到下，这样做可以使JS脚本尽可能的在最后才加载执行。
//第二种方式是给JS脚本添加defer属性，defer属性会让脚本的加载与文档的解析同步进行，等文档解析完成后再执行JS脚本，从而避免阻塞文档的解析过程。多个设置了defer属性的脚本按规范来说是按顺序执行的，但是在一些浏览器中可能并不是这样。
//第三种方式是给JS脚本添加async属性，async属性会使脚本异步加载，不会阻塞页面的解析过程，但是当脚本加载完成后会立即执行，这个时候如果文档还没有解析完成就会被阻塞。多个async属性的脚本的执行顺序是不可预测的，因为谁先加载完谁就会先执行。
//第四种方式是动态创建DOM标签的方式，我们可以对文档的加载事件进行监听，当文档加载完成后再动态的创建script标签来引入JS脚本。
````



### 23、如何创建Ajax

````js
我对ajax的理解是，它是一种异步通信的方法，通过直接由js脚本向服务器发起http通信，然后根据服务器返回的数据，更新网页的相应部分，而不用刷新整个页面。
创建一个ajax有这样几个步骤
//首先是创建一个XMLHttpRequest对象。
//然后在这个对象上使用open方法创建一个http请求，open方法所需要的参数是请求的方法、请求的地址、是否异步和用户的认证信息。
//在发起请求前，我们可以为这个对象添加一些信息和监听函数。比如说我们可以通过setRequestHeader方法来为请求添加头信息。我们还可以为这个对象添加一个状态监听函数。一个XMLHttpRequest对象一共有5个状态，当它的状态变化时会触发onreadystatechange事件，我们可以通过设置监听函数，来处理请求成功后的结果。当对象的readyState变为4的时候，代表服务器返回的数据接收完成，这个时候我们可以判断请求的状态，如果状态是2xx或者304的话代表返回正常。我们就可以通过response中的数据来对页面进行更新了。
//当对象的属性和监听函数设置完成后，最后我们调用send方法来向服务器发起请求，可以传入参数作为发送的数据体。

//实现
const SERVER_URL = "/server";
let xhr = new XMLHttpRequest();
//创建http请求
xhr.open("GET", SERVER_URL, true);
//设置状态监听函数
xhr.onreadystatechange = function() {
    if (this.readyState !== 4)
        return;
    //当请求成功时
    if (this.status === 200) {
        handle(this.response);
    } else {
        console.error(this.statusText);
    }
};
//设置请求失败时的监听函数
xhr.onerror = function() {
    console.error(this.statusText);
};
//设置请求头信息
xhr.responseType = "json";
xhr.setRequestHeader("Accept", "application/json");
//发送http请求
xhr.send(null);

//promise封装实现
function getJSON(url) {
    //创建一个promise对象
    let promise = new Promise(function(resolve, reject) {
        let xhr = new XMLHttpRequest();
        //新建一个http请求
        xhr.open("GET",url,true);
        //设置状态的监听函数
        xhr.onreadystatechange = function() {
            if (this.readyState !== 4)
                return;
            //当请求成功或失败时，改变promise的状态
            if (this.status === 200) {
                resolve(this.response);
            } else {
                reject(new Error(this.statusText));
            }
        };
        //设置错误监听函数
        xhr.onerror = function() {
            reject(new Error(this.statusText));
        };
        //设置响应的数据类型
        xhr.responseType = "json";
        //设置请求头信息
        xhr.setRequestHeader("Accept", "application/json");
        //发送http请求
        xhr.send(null);
    });
    return promise;
}
````



### 24、浏览器的缓存机制

````js
浏览器的缓存机制指的是在一段时间内保留已接收到的 web 资源的一个副本，如果在资源的有效时间内，用户发起了对这个资源的再一次请求，那么浏览器会直接使用缓存的副本，而不是再次向服务器发起请求。使用 web 缓存可以有效地提高页面的打开速度，减少不必要的网络带宽的消耗。
web 资源的缓存策略一般由服务器来指定，分为两种，分别是强缓存策略和协商缓存策略。
使用强缓存策略时，如果缓存资源有效，则直接使用缓存资源，不必再向服务器发起请求。强缓存策略可以通过两种方式来设置，分别是 http 头信息中的 Expires 属性和 Cache-Control 属性。
服务器通过在响应头中添加 Expires 属性，来指定资源的过期时间。在过期时间以内，该资源可以被缓存使用，不必再向服务器发送请求。这个时间是一个绝对时间，它是服务器的时间，因此可能存在这样的问题，就是客户端的时间和服务器端的时间不一致，或者用户对客户端时间进行了修改，这样就可能会影响缓存命中的结果。
Expires 是 http1.0 中的方式，因为它的一些缺点，在 http 1.1 中提出了一个新的头部属性就是 Cache-Control 属性，
它提供了对资源的缓存的更精确的控制。它有很多不同的值，常用的比如我们可以通过设置 max-age 来指定资源能够被缓存的时间的大小，这是一个相对时间，它会根据这个时间的大小和资源第一次请求时的时间来计算出资源过期的时间，因此相对于 Expires来说，这种方式更加有效一些。常用的还有比如 private ，用来规定资源只能被客户端缓存，不能够被代理服务器所缓存。还有如 no-store ，用来指定资源不能够被缓存，no-cache 代表该资源能够被缓存，但是立即失效，每次都需要向服务器发起请求。
一般来说只需要设置其中一种方式就可以实现强缓存策略，当两种方式一起使用时，Cache-Control 的优先级要高于 Expires 。
使用协商缓存策略时，浏览器会先向服务器发送一个请求，如果资源没有发生修改，则返回一个 304 状态，让浏览器使用本地的缓存副本。
如果资源发生了修改，则返回修改后的资源。协商缓存也可以通过两种方式来设置，分别是 http 头信息中的 Etag 和 Last-Modified 属性。
服务器通过在响应头中添加 Last-Modified 属性来指出资源最后一次修改的时间，当浏览器下一次发起请求时，会在请求头中添加一个 If-Modified-Since 的属性，属性值为上一次资源返回时的 Last-Modified 的值。当请求发送到服务器后服务器会通过这个属性来和资源的最后一次的修改时间来进行比较，以此来判断资源是否做了修改。如果资源没有修改，那么返回 304 状态，让客户端使用本地的缓存。如果资源已经被修改了，则返回修改后的资源。使用这种方法有一个缺点，就是 Last-Modified 标注的最后修改时间只能精确到秒级，如果某些文件在1秒钟以内，被修改多次的话，那么文件已经改变了但是 Last-Modified 却没有改变，这样会造成缓存命中的不准确。
因为 Last-Modified 的这种可能发生的不准确性，http 中提供了另外一种方式，那就是 Etag 属性。服务器在返回资源的时候，在头信息中添加了 Etag 属性，这个属性是资源生成的唯一标识符，当资源发生改变的时候，这个值也会发生改变。在下一次资源请求时，浏览器会在请求头中添加一个 If-None-Match 属性，这个属性的值就是上次返回的资源的 Etag 的值。服务接收到请求后会根据这个值来和资源当前的 Etag 的值来进行比较，以此来判断资源是否发生改变，是否需要返回资源。这种方式比 Last-Modified 的方式更加精确。
当 Last-Modified 和 Etag 属性同时出现的时候，Etag 的优先级更高。使用协商缓存的时候，服务器需要考虑负载平衡的问题，因此多个服务器上资源的 Last-Modified 应该保持一致，因为每个服务器上 Etag 的值都不一样，因此在考虑负载平衡时，最好不要设置 Etag 属性。

强缓存策略和协商缓存策略在缓存命中时都会直接使用本地的缓存副本，区别只在于协商缓存会向服务器发送一次请求。它们缓存不命中时，都会向服务器发送请求来获取资源。在实际的缓存机制中，强缓存策略和协商缓存策略是一起合作使用的。浏览器首先会根据请求的信息判断强缓存是否命中，如果命中则直接使用资源。如果不命中则根据头信息向服务器发起请求，使用协商缓存，如果协商缓存命中的话，则服务器不返回资源，浏览器直接使用本地资源的副本，如果协商缓存不命中，则浏览器返回最新的资源给浏览器。
````



### 25、ajax解决浏览器缓存导致页面一直没刷新问题

````js
为了保证我们读取的信息都是最新的，我们就需要禁止他的缓存功能
//在ajax发送请求前加上xhr.setRequestHeader("If-Modified-Since", "0")
//在ajax发送请求前加上xhr.setRequestHeader("Cache-Control","no-cache")
//在URL后面加上一个随机数："fresh=" + Math.random();
//在URL后面加上时间戳："nowtime=" + new Date().getTime();
//如果是使用jQuery，直接$.ajaxSetup({cache:false}),这样页面所有的ajax都会执行这条语句，就是不需要保存缓存记录
````



### 26、同步和异步

````js
同步指的是当一个进程在执行某个请求的时候，如果这个请求需要等待一段时间才能返回，那么这个进程会一直等待下去，直到消息返回为止再继续向下执行。
异步指的是当一个进程在执行某个请求的时候，如果这个请求需要等待一段时间才能返回，这个时候进程会继续往下执行，不会阻塞等待消息的返回，直到消息返回时系统再通知进程进行处理。
````



### 27、浏览器的同源政策

````js
我对浏览器同源政策的理解是，一个域下的js脚本在未经允许的情况下，不能够访问另一个域的内容。这里的同源指的是两个域的协议、域名、端口号必须相同，否则就不属于同一个域。
同源政策主要限制了三个方面
//第一个是当前域下的js脚本不能够访问其他域下的cookie、localStorage和indexDB
//第二个是当前域下的js脚本不能够访问操作其他域下的DOM和js对象
//第三个是当前域下的ajax无法发送跨域请求。
同源政策的目的主要是为了保证用户的信息安全，它只是对js脚本的一些限制，并不是对浏览器的限制，对于一般的img或者script脚本请求都不会有跨域的限制，这是因为这些操作都不会通过响应结果来进行可能出现安全问题的操作。
````



### 28、★如何解决跨域问题

````js
//浏览器为什么要阻止跨域请求
主要是为了web安全，防止网站被他人恶意攻击
//每次跨域请求都需要到达服务端吗？

//浏览器怎么拦截跨域请求的发出？

//解决跨域
解决跨域的方法我们可以根据想要实现的目的来划分。
首先我们如果只是想要实现主域名下的不同子域名的跨域操作，我们可以通过设置 document.domain 来解决。
//（1）两个页面都通过js强制设置 document.domain 为主域名，这样就可以实现相同子域名的跨域操作，这个时候主域名下的 cookie 就能够被子域名所访问。同时如果文档中含有主域名相同，子域名不同的 iframe 的话，我们也可以对这个 iframe 进行操作。
如果是想要解决不同跨域窗口间的通信问题，比如说一个页面想要和别的页面中的不同源的 iframe 进行通信，我们可以使用 location.hash 或者 window.name 或者 postMessage 来解决。
//（2）使用 location.hash 的方法，比如说a欲与b跨域互相通信，可以通过与a同域的中间页c来实现。 这三个页面，不同域之间利用iframe的location.hash传值，相同域之间直接通过js访问来通信，因为c与a同域，所以c可通过parent.parent访问a页面所有对象，以此来实现双向通信。
//（3）使用 window.name 的方法，该方法的原理主要是在同一个窗口中设置了 window.name 后，不同源的页面也可以访问window.name，所以不同源的子页面b可以首先在 window.name 中写入数据，然后跳转到一个和父级同源的页面c。这个时候级页面a就可以访问同源的子页面c中 window.name 中的数据了，这种方式的好处是可以传输的数据量大。
//（4）使用 postMessage 来解决的方法，这是一个 h5 中新增的一个 api。通过它我们可以实现多窗口间的信息传递，用postMessage来发送跨域数据，再通过对message进行监听window.addEventListener('message', fn)来接收跨域数据，以此来实现不同源间的信息交换。
如果是要解决 ajax 无法提交跨域请求的问题，我们可以使用 jsonp、cors、websocket 协议、服务器代理来解决问题。
//（5）使用 jsonp 来实现跨域请求，它的主要原理是通过动态构建 script  标签来实现跨域请求，因为浏览器对 script 标签的引入没有跨域的访问限制 。通过在请求的 url 后指定一个回调函数，然后服务器在返回数据的时候，构建一个 json 数据的包装，这个包装就是回调函数，然后返回给前端，前端接收到数据后，因为请求的是脚本文件，所以会直接执行，这样我们先前定义好的回调函数就可以被调用，从而实现了跨域请求的处理。这种方式只能用于 get 请求。
//（6）使用 CORS 的方式，CORS 是一个 W3C 标准，全称是"跨域资源共享"。CORS 需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，因此我们只需要在服务器端配置就行。浏览器将 CORS 请求分成两类：简单请求和非简单请求。对于简单请求，浏览器直接发出 CORS 请求。具体来说，就是会在头信息之中，增加一个 Origin 字段。Origin 字段用来说明本次请求来自哪个源。服务器根据这个值，决定是否同意这次请求。如果 Origin 指定的源，不在许可范围内，服务器会返回一个正常的 HTTP 回应。浏览器发现这个回应的头信息中没有包含 Access-Control-Allow-Origin 字段，就知道出错了，从而抛出一个错误，ajax 不会收到响应信息。如果成功的话会包含一些以 Access-Control- 开头的字段。对于非简单请求，浏览器会先发出一次预检请求，来判断该域名是否在服务器的白名单中，如果收到肯定回复后才会发起请求。
//（7）使用 websocket 协议，这个协议没有同源限制。
//（8）使用服务器来代理跨域的访问请求，就是有跨域的请求操作时将发送给后端让后端代为请求，后端再将获取的结果返回。
````



### 29、cookie

````js
我的理解是cookie是服务器提供的一种用于维护会话状态信息的数据，通过服务器发送到浏览器，浏览器再保存在本地。当下一次有同源的请求时，将保存的cookie值添加到请求头部，一起发送给服务端。这可以用来实现记录用户的登陆状态等功能。cookie一般可以存储4k大小的数据，并且只能被同源的网页所访问共享。
服务器端可以使用Set-Cookie的响应头部来配置cookie信息。一条cookie包括了5个属性值expires、domain、path、secure、HttpOnly。其中expires 指定了 cookie 失效的时间，domain 是域名、path是路径，domain 和 path 一起限制了 cookie 能够被哪些 url 访问。secure 规定了 cookie 只能在确保安全的情况下传输，HttpOnly 规定了这个 cookie 只能被服务器访问，不能使用 js 脚本访问。
在发生 xhr 的跨域请求的时候，即使是同源下的 cookie，也不会被自动添加到请求头部，除非显示地规定。
````



### 30、js的几种模块规范

````js
js 中现在比较成熟的有四种模块加载方案。
//第一种是 CommonJS 方案，这种方案里，文件即模块，它通过 require 来引入模块，通过 module.exports 定义模块的输出接口。这种模块加载方案是服务器端的解决方案，它是以同步的方式来引入模块的，因为在服务端文件都存储在本地磁盘，所以读取非常快，所以以同步的方式加载没有问题。但如果是在浏览器端，由于模块的加载是使用网络请求，因此使用异步加载的方式更加合适。

//第二种是 AMD 方案，这种方案使用define来定义模块，用require调用模块，并且采用异步加载的方式来加载模块，模块的加载不影响后面语句的执行。AMD推崇依赖前置，在定义模块的时候就要声明其所依赖的模块。所有依赖这个模块的语句都定义在一个回调函数里，等到加载完成后再执行回调函数。require.js 实现了 AMD 规范。

//第三种是 CMD 方案，这种方案和 AMD 方案都是为了解决异步模块加载的问题，sea.js 实现了 CMD 规范。它和 require.js的区别在于模块定义时对依赖的处理不同和对依赖模块的执行时机的处理不同。CMD推崇依赖就近，只有在用到某个模块的时候才去require。CMD 在依赖模块加载完成后并不执行，只是下载而已，等到所有的依赖模块都加载好后，进入回调函数逻辑，遇到 require 语句 的时候才执行对应的模块，这样模块的执行顺序就和我们书写的顺序保持一致了。

//第四种方案是 ES6 提出的方案，使用 import 和 export 的形式来导入导出模块。ES6模块输出的是值的引用，在编译时输出接口。这种方案和上面三种方案都不同。
````



### 31、requireJS的核心原理

````js
require.js的核心原理是通过动态创建script脚本来异步引入模块，然后对每个脚本的load事件进行监听，如果每个脚本都加载完成了，再调用回调函数。
````



### 32、js类数组对象

````js
一个拥有 length 属性和若干索引属性的对象就可以被称为类数组对象，类数组对象和数组类似，但是不能调用数组的方法。
常见的类数组对象有 arguments 和 DOM 方法的返回结果，还有一个函数也可以被看作是类数组对象，因为它含有 length属性值，代表可接收的参数个数。
常见的类数组转换为数组的方法有这样几种：
//1、通过call调用数组的slice方法来实现转换
Array.prototype.slice.call(arrayLike);
//2、通过call调用数组的splice方法来实现转换
Array.prototype.splice.call(arrayLike, 0);
//3、通过apply调用数组的concat方法来实现转换
Array.prototype.concat.apply([], arrayLike);
//4、通过Array.from方法来实现转换
Array.from(arrayLike);
````



### 33、js中的作用域与变量声明提升

````js
变量提升的表现是，无论我们在函数中何处位置声明的变量，好像都被提升到了函数的首部，我们可以在变量声明前访问到而不会报错。
造成变量声明提升的本质原因是 js 引擎在代码执行前有一个解析的过程，创建了执行上下文，初始化了一些代码执行时需要用到的对象。当我们访问一个变量时，我们会到当前执行上下文中的作用域链中去查找，而作用域链的首端指向的是当前执行上下文的变量对象，这个变量对象是执行上下文的一个属性，它包含了函数的形参、所有的函数和变量声明，而这个对象是在代码解析的时候创建的。这就是会出现变量声明提升的根本原因。
````



### 34、V8引擎的垃圾回收机制

````js
v8 的垃圾回收机制基于分代回收机制，这个机制又基于世代假说，这个假说有两个特点，一是新生的对象容易早死，另一个是不死的对象会活得更久。基于这个假说，v8 引擎将内存分为了新生代和老生代。
新创建的对象或者只经历过一次的垃圾回收的对象被称为新生代。经历过多次垃圾回收的对象被称为老生代。
新生代又被分为 From 和 To 两个空间，To 一般是闲置的。当 From 空间满了的时候会执行 Scavenge 算法进行垃圾回收。当我们执行垃圾回收算法的时候应用逻辑将会停止，等垃圾回收结束后再继续执行。这个算法分为三步：
（1）首先检查 From 空间的存活对象，如果对象存活则判断对象是否满足晋升到老生代的条件，如果满足条件则晋升到老生代。如果不满足条件则移动 To 空间。
（2）如果对象不存活，则释放对象的空间。
（3）最后将 From 空间和 To 空间角色进行交换。

新生代对象晋升到老生代有两个条件：
（1）第一个是判断是对象否已经经过一次 Scavenge 回收。若经历过，则将对象从 From 空间复制到老生代中；若没有经历，则复制到 To 空间。
（2）第二个是 To 空间的内存使用占比是否超过限制。当对象从 From 空间复制到 To 空间时，若 To 空间使用超过 25%，则对象直接晋升到老生代中。设置 25% 的原因主要是因为算法结束后，两个空间结束后会交换位置，如果 To 空间的内存太小，会影响后续的内存分配。

老生代采用了标记清除法和标记压缩法。标记清除法首先会对内存中存活的对象进行标记，标记结束后清除掉那些没有标记的对象。由于标记清除后会造成很多的内存碎片，不便于后面的内存分配。所以为了解决内存碎片的问题引入了标记压缩法。
由于在进行垃圾回收的时候会暂停应用的逻辑，对于新生代方法由于内存小，每次停顿的时间不会太长，但对于老生代来说每次垃圾回收的时间长，停顿会造成很大的影响。 为了解决这个问题 V8 引入了增量标记的方法，将一次停顿进行的过程分为了多步，每次执行完一小步就让运行逻辑执行一会，就这样交替运行。
````



### 35、哪些操作会造成内存泄漏

````js
//第一种情况是我们由于使用未声明的变量，意外的创建了一个全局变量，使这个变量一直留在内存中无法被回收。
//第二种情况是我们设置了 setInterval 定时器，而忘记取消它，如果循环函数有对外部变量的引用的话，那么这个变量会被一直留在内存中，而无法被回收。
//第三种情况是我们获取一个 DOM 元素的引用，而后面这个元素被删除，由于我们一直保留了对这个元素的引用，所以它也无法被回收。
//第四种情况是不合理的使用闭包，从而导致某些变量一直被留在内存当中。
````



### 36、移动端300ms点击延迟，如何解决

````js
移动端点击有 300ms 的延迟是因为移动端会有双击缩放的这个操作，因此浏览器在 click 之后要等待 300ms，看用户有没有下一次点击，来判断这次操作是不是双击。
解决方法：
//通过 meta 标签禁用网页的缩放。
//通过 meta 标签将网页的 viewport 设置为 ideal viewport。
//调用一些 js 库，比如 FastClick
````



### 37、前端路由

````js
//什么是前端路由？
前端路由就是把不同路由对应不同的内容或页面的任务交给前端来做，之前是通过服务端根据 url 的不同返回不同的页面实现的。
//什么时候使用前端路由？
在单页面应用，大部分页面结构不变，只改变部分内容的使用
//前端路由有什么优点和缺点？
优点：从性能和用户体验的层面来比较的话，后端路由每次访问一个新页面的时候都要向服务器发送请求，然后服务器再响应请求，这个过程肯定会有延迟。而前端路由在访问一个新页面的时候仅仅是变换了一下路径而已，没有了网络延迟，对于用户体验来说会有相当大的提升。还比如页面持久性，像大部分音乐网站，你都可以在播放歌曲的同时，跳转到别的页面而音乐没有中断，再比如前后端彻底分离。 开发一个前端路由，主要考虑到页面的可插拔、页面的生命周期、内存管理等
缺点:使用浏览器的前进，后退键的时候会重新发送请求，没有合理地利用缓存。

前端路由一共有两种实现方式，一种是通过 hash 的方式，一种是通过使用 pushState 的方式。
HTML5 History有两个新增的API：history.pushState 和 history.replaceState，这两个 API 都会操作浏览器的历史记录，而不会引起页面的刷新。
Hash的方式需要一个根据监听哈希变化触发的事件( hashchange) 事件。我们用 window.location 处理哈希的改变时不会重新渲染页面，而是当作新页面加到历史记录中，这样我们跳转页面就可以在 hashchange 事件中注册 ajax 从而改变页面内容。 可以为hash的改变添加监听事件：window.addEventListener("hashchange", furfuncRef, false)
````



### 38、节流与防抖

````js
函数防抖是指在事件被触发 n 秒后再执行回调，如果在这 n 秒内事件又被触发，则重新计时。这可以使用在一些点击请求的事件上，避免因为用户的多次点击向后端发送多次请求。
function debounce(fn, wait) {
  var timer = null;
  return function() {
    var context = this,
      args = arguments;
    // 如果此时存在定时器的话，则取消之前的定时器重新记时
    if (timer) {
      clearTimeout(timer);
      timer = null;
    }
    // 设置定时器，使事件间隔指定事件后执行
    timer = setTimeout(() => {
      fn.apply(context, args);
    }, wait);
  };
}

函数节流是指规定一个单位时间，在这个单位时间内，只能有一次触发事件的回调函数执行，如果在同一个单位时间内某事件被触发多次，只有一次能生效。节流可以使用在 scroll 函数的事件监听上，通过事件节流来降低事件调用的频率。
function throttle(fn, delay) {
  var preTime = Date.now();
  return function() {
    var context = this,
      args = arguments,
      nowTime = Date.now();
    // 如果两次时间间隔超过了指定时间，则执行函数。
    if (nowTime - preTime >= delay) {
      preTime = Date.now();
      return fn.apply(context, args);
    }
  };
}
````



### 39、Object.is()和"==="、“==”的区别

````js
使用双等号进行相等判断时，如果两边的类型不一致，则会进行强制类型转化后再进行比较。
使用三等号进行相等判断时，如果两边的类型不一致时，不会做强制类型转换，直接返回 false。
使用 Object.is 来进行相等判断时，一般情况下和三等号的判断相同，但它处理了一些特殊的情况，比如 -0 和 +0 不再相等，两个 NaN 认定为是相等的。
````



### 40、JS事件循环

````js
因为 js 是单线程运行的，在代码执行的时候，通过将不同函数的执行上下文压入执行栈中来保证代码的有序执行。在执行同步代码的时候，如果遇到了异步事件，js 引擎并不会一直等待其返回结果，而是会将这个事件挂起，继续执行执行栈中的其他任务。当异步事件执行完毕后，再将异步事件对应的回调函数加入到与当前执行栈中不同的另一个任务队列中等待执行。任务队列可以分为宏任务队列和微任务队列，当当前执行栈中的事件执行完毕后，js 引擎首先会判断微任务对列中是否有任务可以执行，如果有就将微任务队首的事件压入栈中执行。当微任务队列中的任务都执行完成后再去判断宏任务队列中的任务。
微任务包括了 promise 的回调、node 中的 process.nextTick 、对 Dom 变化监听的 MutationObserver。
宏任务包括了 script 脚本的执行、setTimeout ，setInterval ，setImmediate 一类的定时事件，还有如 I/O 操作、UI 渲染等。
````



### 41、深浅拷贝的实现

````js
浅拷贝指的是将一个对象的属性值复制到另一个对象，如果有的属性的值为引用类型的话，那么会将这个引用的地址复制给对象，因此两个对象会有同一个引用类型的引用。浅拷贝可以使用  Object.assign 和展开运算符来实现。
function shallowCopy(object) {
  // 只拷贝对象
  if (!object || typeof object !== "object") return;

  // 根据 object 的类型判断是新建一个数组还是对象
  let newObject = Array.isArray(object) ? [] : {};

  // 遍历 object，并且判断是 object 的属性才拷贝
  for (let key in object) {
    if (object.hasOwnProperty(key)) {
      newObject[key] = object[key];
    }
  }

  return newObject;
}

深拷贝相对浅拷贝而言，如果遇到属性值为引用类型的时候，它新建一个引用类型并将对应的值复制给它，因此对象获得的一个新的引用类型而不是一个原有类型的引用。深拷贝对于一些对象可以使用 JSON 的两个函数来实现，但是由于 JSON 的对象格式比 js 的对象格式更加严格，所以如果属性值里边出现函数或者 Symbol 类型的值时，会转换失败。
function deepCopy(object) {
  if (!object || typeof object !== "object") return;

  let newObject = Array.isArray(object) ? [] : {};

  for (let key in object) {
    if (object.hasOwnProperty(key)) {
      newObject[key] =
        typeof object[key] === "object" ? deepCopy(object[key]) : object[key];
    }
  }

  return newObject;
}
````



### 42、★XSS攻击及防范

````js
XSS 攻击指的是跨站脚本攻击，是一种代码注入攻击。攻击者通过在网站注入恶意脚本，使其在用户的浏览器上运行，从而盗取用户的信息如 cookie 等。
XSS 的本质是因为网站没有对恶意代码进行过滤，与正常的代码混合在一起了，浏览器没有办法分辨哪些脚本是可信的，从而导致了恶意代码的执行。
//XSS 一般分为存储型、反射型和 DOM 型。
存储型指的是恶意代码提交到了网站的数据库中，当用户请求数据的时候，服务器将其拼接为 HTML 后返回给了用户，从而导致了恶意代码的执行。
反射型指的是攻击者构建了特殊的 URL，当服务器接收到请求后，从 URL 中获取数据，拼接到 HTML 后返回，从而导致了恶意代码的执行。
DOM 型指的是攻击者构建了特殊的 URL，用户打开网站后，js 脚本从 URL 中获取数据，从而导致了恶意代码的执行。
//XSS 攻击的预防可以从两个方面入手，一个是恶意代码提交的时候，一个是浏览器执行恶意代码的时候。
对于第一个方面，我们可以对存入数据库的数据都进行转义处理，但是一个数据可能在多个地方使用，有的地方可能不需要转义，由于我们没有办法判断数据最后的使用场景，所以直接在输入端进行恶意代码的处理，其实是不太可靠的。因此我们可以从浏览器的执行来进行预防，
一种是使用纯前端的方式，不用服务器端拼接后返回。
另一种是对需要插入到 HTML 中的代码做好充分的转义。对于 DOM 型的攻击，主要是前端脚本的不可靠而造成的，所以我们在数据获取渲染和字符串拼接的时候应该对可能出现的恶意代码情况进行判断。
还有一些方式，比如使用 CSP ，CSP 的本质是建立一个白名单，告诉浏览器哪些外部资源可以加载和执行，从而防止恶意代码的注入攻击。通常有两种方式来开启 CSP，一种是设置 HTTP 首部中的 Content-Security-Policy，一种是设置 meta 标签的方式 <meta
http-equiv="Content-Security-Policy">
还可以对一些敏感信息进行保护，比如 cookie 使用 http-only ，使得脚本无法获取。也可以使用验证码，避免脚本伪装成用户执行一些操作。
````



### 43、CSRF攻击及防范

````js
CSRF 攻击指的是跨站请求伪造攻击，攻击者诱导用户进入一个第三方网站，然后该网站向被攻击网站发送跨站请求。如果用户在被攻击网站中保存了登录状态，那么攻击者就可以利用这个登录状态，绕过后台的用户验证，冒充用户向服务器执行一些操作。
CSRF 攻击的本质是利用了 cookie 会在同源请求中携带发送给服务器的特点，以此来实现用户的冒充。
//一般的 CSRF 攻击类型有三种：
第一种是 GET 类型的 CSRF 攻击，比如在网站中的一个 img 标签里构建一个请求，当用户打开这个网站的时候就会自动发起提交。
第二种是 POST 类型的 CSRF 攻击，比如说构建一个表单，然后隐藏它，当用户进入页面时，自动提交这个表单。
第三种是链接类型的 CSRF 攻击，比如说在 a 标签的 href 属性里构建一个请求，然后诱导用户去点击。
//CSRF 可以用下面几种方法来防护：
第一种是同源检测的方法，服务器根据 http 请求头中 origin 或者 referer 信息来判断请求是否为允许访问的站点，从而对请求进行过滤。当 origin 或者 referer 信息都不存在的时候，直接阻止。这种方式的缺点是有些情况下 referer 可以被伪造。还有就是这种方法同时把搜索引擎的链接也给屏蔽了，所以一般网站会允许搜索引擎的页面请求，但是相应的页面请求也可能被攻击者给利用。
第二种方法是使用 CSRF Token 来进行验证，服务器向用户返回一个随机数 Token ，当网站再次发起请求时，在请求参数中加入服务器端返回的 token ，然后服务器对这个 token 进行验证。这种方法解决了使用 cookie 单一验证方式时，可能会被冒用的问题，但是这种方法存在一个缺点就是，我们需要给网站中的所有请求都添加上这个 token，操作比较繁琐。还有一个问题是一般不会只有一台网站服务器，如果我们的请求经过负载平衡转移到了其他的服务器，但是这个服务器的 session 中没有保留这个 token 的话，就没有办法验证了。这种情况我们可以通过改变 token 的构建方式来解决。
第三种方式使用双重 Cookie 验证的办法，服务器在用户访问网站页面时，向请求域名注入一个Cookie，内容为随机字符串，然后当用户再次向服务器发送请求的时候，从 cookie 中取出这个字符串，添加到 URL 参数中，然后服务器通过对 cookie 中的数据和参数中的数据进行比较，来进行验证。使用这种方式是利用了攻击者只能利用 cookie，但是不能访问获取 cookie 的特点。并且这种方法比 CSRF Token 的方法更加方便，并且不涉及到分布式访问的问题。这种方法的缺点是如果网站存在 XSS 漏洞的，那么这种方式会失效。同时这种方式不能做到子域名的隔离。
第四种方式是使用在设置 cookie 属性的时候设置 Samesite ，限制 cookie 不能作为被第三方使用，从而可以避免被攻击者利用。Samesite 一共有两种模式，一种是严格模式，在严格模式下 cookie 在任何情况下都不可能作为第三方 Cookie 使用，在宽松模式下，cookie 可以被请求为 GET 请求，且会发生页面跳转的请求所使用。使用这种方法的缺点是，因为它不支持子域，所以子域没有办法与主域共享登录信息，每次转入子域的网站，都会重新登录。还有一个问题就是它的兼容性不够好。
````



### 44、点击劫持及防范

````js
点击劫持是一种视觉欺骗的攻击手段，攻击者将需要攻击的网站通过 iframe 嵌套的方式嵌入自己的网页中，并将 iframe 设置为透明，在页面中透出一个按钮诱导用户点击。
我们可以在 http 相应头中设置 X-FRAME-OPTIONS 来防御用 iframe 嵌套的点击劫持攻击。通过不同的值，可以规定页面在特定的一些情况才能作为 iframe 来使用。
````





### 45、异步编程的实现方式

````js
//js 中的异步机制可以分为以下几种：
第一种最常见的是使用回调函数的方式，使用回调函数的方式有一个缺点是，多个回调函数嵌套的时候会造成回调函数地狱，上下两层的回调函数间的代码耦合度太高，不利于代码的可维护。
第二种是 Promise 的方式，使用 Promise 的方式可以将嵌套的回调函数作为链式调用。但是使用这种方法，有时会造成多个 then 的链式调用，可能会造成代码的语义不够明确。
第三种是使用 generator 的方式，它可以在函数的执行过程中，将函数的执行权转移出去，在函数外部我们还可以将执行权转移回来。当我们遇到异步函数执行的时候，将函数执行权转移出去，当异步函数执行完毕的时候我们再将执行权给转移回来。因此我们在 generator 内部对于异步操作的方式，可以以同步的顺序来书写。使用这种方式我们需要考虑的问题是何时将函数的控制权转移回来，因此我们需要有一个自动执行 generator 的机制，比如说 co 模块等方式来实现 generator 的自动执行。
第四种是使用 async 函数的形式，async 函数是 generator 和 promise 实现的一个自动执行的语法糖，它内部自带执行器，当函数内部执行到一个 await 语句的时候，如果语句返回一个 promise 对象，那么函数将会等待 promise 对象的状态变为 resolve 后再继续向下执行。因此我们可以将异步逻辑，转化为同步的顺序来书写，并且这个函数可以自动执行。
````



### 46、图片的懒加载和预加载

````js
懒加载也叫延迟加载，指的是在长网页中延迟加载图片的时机，当用户需要访问时，再去加载，这样可以提高网站的首屏加载速度，提升用户的体验，并且可以减少服务器的压力。它适用于图片很多，页面很长的电商网站的场景。懒加载的实现原理是，将页面上的图片的 src 属性设置为空字符串，将图片的真实路径保存在一个自定义属性中，当页面滚动的时候，进行判断，如果图片进入页面可视区域内，则从自定义属性中取出真实路径赋值给图片的 src 属性，以此来实现图片的延迟加载。

预加载指的是将所需的资源提前请求加载到本地，这样后面在需要用到时就直接从缓存取资源。通过预加载能够减少用户的等待时间，提高用户的体验。我了解的预加载的最常用的方式是使用 js 中的 image 对象，通过为 image 对象来设置 scr 属性，来实现图片的预加载。

这两种方式都是提高网页性能的方式，两者主要区别是一个是提前加载，一个是迟缓甚至不加载。懒加载对服务器前端有一定的缓解压力作用，预加载则会增加服务器前端压力。
````



### 47、js拖拽功能的实现

````js
一个元素的拖拽过程，我们可以分为三个步骤，第一步是鼠标按下目标元素，第二步是鼠标保持按下的状态移动鼠标，第三步是鼠
标抬起，拖拽过程结束。

这三步分别对应了三个事件，mousedown 事件，mousemove 事件和 mouseup 事件。只有在鼠标按下的状态移动鼠标我们才会执行拖拽事件，因此我们需要在 mousedown 事件中设置一个状态来标识鼠标已经按下，然后在 mouseup 事件中再取消这个状
态。在 mousedown 事件中我们首先应该判断，目标元素是否为拖拽元素，如果是拖拽元素，我们就设置状态并且保存这个时候鼠标的位置。然后在 mousemove 事件中，我们通过判断鼠标现在的位置和以前位置的相对移动，来确定拖拽元素在移动中的坐标。
最后 mouseup 事件触发后，清除状态，结束拖拽事件。
````



### 48、为什么要使用setTimeout实现setInterval？怎么实现？

````js
setInterval 的作用是每隔一段指定时间执行一个函数，但是这个执行不是真的到了时间立即执行，它真正的作用是每隔一段时间将事件加入事件队列中去，只有当当前的执行栈为空的时候，才能去从事件队列中取出事件执行。所以可能会出现这样的情况，就是当前执行栈执行的时间很长，导致事件队列里边积累多个定时器加入的事件，当执行栈结束的时候，这些事件会依次执行，因此就不能达到间隔一段时间执行的效果。

针对 setInterval 的这个缺点，我们可以使用 setTimeout 递归调用来模拟 setInterval，这样我们就确保了只有一个事件结束了，我们才会触发下一个定时器事件，这样解决了 setInterval 的问题。
// 思路是使用递归函数，不断地去执行 setTimeout 从而达到 setInterval 的效果

function mySetInterval(fn, timeout) {
  // 控制器，控制定时器是否继续执行
  var timer = {
    flag: true
  };

  // 设置递归函数，模拟定时器执行。
  function interval() {
    if (timer.flag) {
      fn();
      setTimeout(interval, timeout);
    }
  }

  // 启动定时器
  setTimeout(interval, timeout);

  // 返回控制器
  return timer;
}
````



### 49、let和const的注意点

````js
声明的变量只在声明时的代码块内有效
不存在声明提升
存在暂时性死区，如果在变量声明前使用，会报错
不允许重复声明，重复声明会报错
````



### 50、什么是尾调用以及尾调用的好处

````js
尾调用指的是函数的最后一步调用另一个函数。我们代码执行是基于执行栈的，所以当我们在一个函数里调用另一个函数时，我们会保留当前的执行上下文，然后再新建另外一个执行上下文加入栈中。使用尾调用的话，因为已经是函数的最后一步，所以这个时候我们可以不必再保留当前的执行上下文，从而节省了内存，这就是尾调用优化。但是 ES6 的尾调用优化只在严格模式下开启，正常模式是无效的。
````



### 50、Promise对象及实现

````js
Promise 对象是异步编程的一种解决方案，最早由社区提出。Promises/A+ 规范是 JavaScript Promise 的标准，规定了一个 Promise 所必须具有的特性。

Promise 是一个构造函数，接收一个函数作为参数，返回一个 Promise 实例。一个 Promise 实例有三种状态，分别是 pending、fulfilled 和 rejected，分别代表了初始状态、已成功和已失败。实例的状态只能由 pending 转变 fulfilled 或者 rejected 状态，并且状态一经改变，就凝固了，无法再被改变了。状态的改变是通过 resolve() 和 reject() 函数来实现的，我们可以在异步操作结束后调用这两个函数改变 Promise 实例的状态，它的原型上定义了一个 then 方法，使用这个 then 方法可以为两个状态的改变注册回调函数。这个回调函数属于微任务，会在本轮事件循环的末尾执行。

const PENDING = "pending";
const RESOLVED = "resolved";
const REJECTED = "rejected";

function MyPromise(fn) {
  // 保存初始化状态
  var self = this;

  // 初始化状态
  this.state = PENDING;

  // 用于保存 resolve 或者 rejected 传入的值
  this.value = null;

  // 用于保存 resolve 的回调函数
  this.resolvedCallbacks = [];

  // 用于保存 reject 的回调函数
  this.rejectedCallbacks = [];

  // 状态转变为 resolved 方法
  function resolve(value) {
    // 判断传入元素是否为 Promise 值，如果是，则状态改变必须等待前一个状态改变后再进行改变
    if (value instanceof MyPromise) {
      return value.then(resolve, reject);
    }

    // 保证代码的执行顺序为本轮事件循环的末尾
    setTimeout(() => {
      // 只有状态为 pending 时才能转变，
      if (self.state === PENDING) {
        // 修改状态
        self.state = RESOLVED;

        // 设置传入的值
        self.value = value;

        // 执行回调函数
        self.resolvedCallbacks.forEach(callback => {
          callback(value);
        });
      }
    }, 0);
  }

  // 状态转变为 rejected 方法
  function reject(value) {
    // 保证代码的执行顺序为本轮事件循环的末尾
    setTimeout(() => {
      // 只有状态为 pending 时才能转变
      if (self.state === PENDING) {
        // 修改状态
        self.state = REJECTED;

        // 设置传入的值
        self.value = value;

        // 执行回调函数
        self.rejectedCallbacks.forEach(callback => {
          callback(value);
        });
      }
    }, 0);
  }

  // 将两个方法传入函数执行
  try {
    fn(resolve, reject);
  } catch (e) {
    // 遇到错误时，捕获错误，执行 reject 函数
    reject(e);
  }
}

MyPromise.prototype.then = function(onResolved, onRejected) {
  // 首先判断两个参数是否为函数类型，因为这两个参数是可选参数
  onResolved =
    typeof onResolved === "function"
      ? onResolved
      : function(value) {
          return value;
        };

  onRejected =
    typeof onRejected === "function"
      ? onRejected
      : function(error) {
          throw error;
        };

  // 如果是等待状态，则将函数加入对应列表中
  if (this.state === PENDING) {
    this.resolvedCallbacks.push(onResolved);
    this.rejectedCallbacks.push(onRejected);
  }

  // 如果状态已经凝固，则直接执行对应状态的函数

  if (this.state === RESOLVED) {
    onResolved(this.value);
  }

  if (this.state === REJECTED) {
    onRejected(this.value);
  }
};
````



### 51、JS设计模式

````js
//单例模式
单例模式保证了全局只有一个实例来被访问。比如说常用的如弹框组件的实现和全局状态的实现。
//策略模式
策略模式主要是用来将方法的实现和方法的调用分离开，外部通过不同的参数可以调用不同的策略。我主要在 MVP 模式解耦的时候用来将视图层的方法定义和方法调用分离。
//代理模式
代理模式是为一个对象提供一个代用品或占位符，以便控制对它的访问。比如说常见的事件代理。
//中介者模式
中介者模式指的是，多个对象通过一个中介者进行交流，而不是直接进行交流，这样能够将通信的各个对象解耦。
//适配器模式
适配器用来解决两个接口不兼容的情况，不需要改变已有的接口，通过包装一层的方式实现两个接口的正常协作。假如我们需要一种新的接口返回方式，但是老的接口由于在太多地方已经使用了，不能随意更改，这个时候就可以使用适配器模式。比如我们需要一种自定义的时间返回格式，但是我们又不能对 js 时间格式化的接口进行修改，这个时候就可以使用适配器模式。
//发布订阅模式和观察者模式的区别
发布订阅模式其实属于广义上的观察者模式。
在观察者模式中，观察者需要直接订阅目标事件。在目标发出内容改变的事件后，直接接收事件并作出响应。
而在发布订阅模式中，发布者和订阅者之间多了一个调度中心。调度中心一方面从发布者接收事件，另一方面向订阅者发布事件，订阅者需要在调度中心中订阅事件。通过调度中心实现了发布者和订阅者关系的解耦。使用发布订阅者模式更利于我们代码的可维护性。
````



### 52、开发中常用的几种content-type

````js
//application/x-www-form-urlencoded
浏览器的原生 form 表单，如果不设置 enctype 属性，那么最终就会以 application/x-www-form-urlencoded 方式提交数据。该种方式提交的数据放在 body 里面，数据按照 key1=val1&key2=val2 的方式进行编码，key 和 val 都进行了 URL转码。
//multipart/form-data
该种方式也是一个常见的 POST 提交方式，通常表单上传文件时使用该种方式。
//application/json
告诉服务器消息主体是序列化后的 JSON 字符串。
//text/xml
该种方式主要用来提交 XML 格式的数据。
````



### 53、封装一个JS类型判断函数

````js
function getType(value) {
  // 判断数据是 null 的情况
  if (value === null) {
    return value + "";
  }

  // 判断数据是引用类型的情况
  if (typeof value === "object") {
    let valueClass = Object.prototype.toString.call(value),
      type = valueClass.split(" ")[1].split("");

    type.pop();

    return type.join("").toLowerCase();
  } else {
    // 判断数据是基本数据类型的情况和函数的情况
    return typeof value;
  }
}
````



### 54、判断一个对象是否为空对象

````js
function checkNullObj(obj) {
  return Object.keys(obj).length === 0;
}
````



### 55、手写一个jsonp

````js
function jsonp(url, params, callback) {
  // 判断是否含有参数
  let queryString = url.indexOf("?") === "-1" ? "?" : "&";

  // 添加参数
  for (var k in params) {
    if (params.hasOwnProperty(k)) {
      queryString += k + "=" + params[k] + "&";
    }
  }

  // 处理回调函数名
  let random = Math.random()
      .toString()
      .replace(".", ""),
    callbackName = "myJsonp" + random;

  // 添加回调函数
  queryString += "callback=" + callbackName;

  // 构建请求
  let scriptNode = document.createElement("script");
  scriptNode.src = url + queryString;

  window[callbackName] = function() {
    // 调用回调函数
    callback(...arguments);

    // 删除这个引入的脚本
    document.getElementsByTagName("head")[0].removeChild(scriptNode);
  };

  // 发起请求
  document.getElementsByTagName("head")[0].appendChild(scriptNode);
}
````



### 56、手写观察者模式

````js
var events = (function() {
  var topics = {};

  return {
    // 注册监听函数
    subscribe: function(topic, handler) {
      if (!topics.hasOwnProperty(topic)) {
        topics[topic] = [];
      }
      topics[topic].push(handler);
    },

    // 发布事件，触发观察者回调事件
    publish: function(topic, info) {
      if (topics.hasOwnProperty(topic)) {
        topics[topic].forEach(function(handler) {
          handler(info);
        });
      }
    },

    // 移除主题的一个观察者的回调事件
    remove: function(topic, handler) {
      if (!topics.hasOwnProperty(topic)) return;

      var handlerIndex = -1;
      topics[topic].forEach(function(item, index) {
        if (item === handler) {
          handlerIndex = index;
        }
      });

      if (handlerIndex >= 0) {
        topics[topic].splice(handlerIndex, 1);
      }
    },

    // 移除主题的所有观察者的回调事件
    removeAll: function(topic) {
      if (topics.hasOwnProperty(topic)) {
        topics[topic] = [];
      }
    }
  };
})();
````



### 57、前端面试题（函数调用）

````js
function Foo() {
  getName = function() {
    alert(1);
  };
  return this;
}
Foo.getName = function() {
  alert(2);
};
Foo.prototype.getName = function() {
  alert(3);
};
var getName = function() {
  alert(4);
};
function getName() {
  alert(5);
}

//请写出以下输出结果：
Foo.getName(); // 2
getName(); // 4   5的函数声明被4的函数表达式覆盖了
Foo().getName(); // 1
getName(); // 1   Foo执行后把全局的getName函数给重写了一次
new Foo.getName(); // 2
new Foo().getName(); // 3
new new Foo().getName(); // 3
````



### 58、一个列表，假设有100000个数据，怎么办

````js
我们需要思考的问题：该处理是否必须同步完成？数据是否必须按顺序完成？
解决办法：
（1）将数据分页，利用分页的原理，每次服务器端只返回一定数目的数据，浏览器每次只对一部分进行加载。
（2）使用懒加载的方法，每次加载一部分数据，其余数据当需要使用时再去加载。
（3）使用数组分块技术，基本思路是为要处理的项目创建一个队列，然后设置定时器每过一段时间取出一部分数据，然后再使用定时器取出下一个要处理的项目进行处理，接着再设置另一个定时器。
````



### 59、★jQuery都有哪些选择器？

````js
ID选择器   $('#' + id)   $('#user')
属性选择器
类选择器   $('.warning')
元素选择器 $('p')
````



### 60、★如何批量替换一个字符串中的多个相同字符

````js
用正则表达式匹配字符，然后再进行替换
用replace方法进行匹配替换
````



### 61、★get和post请求的区别？

````js
get用来获取数据，post用来提交数据
get参数有长度限制（受限于url长度，具体的数值取决于浏览器和服务器的限制，最长2048字节），而post无限制。
get请求的数据会附加在url之 ，以 " ？ "分割url和传输数据，多个参数用 "&"连接，而post请求会把请求的数据放在http请求体中。
get是明文传输，post是放在请求体中，但是开发者可以通过抓包工具看到，也相当于是明文的。
get请求会保存在浏览器历史记录中，还可能保存在web服务器的日志中
````



### 62、★工作中用的ajax是原生的还是经过封装的？

````js
$.ajax并进行了简单的封装
````



### 63、★ES6有哪些新特性？

````js
//箭头函数，它和普通函数有什么区别？
ES6 允许使用“箭头”（=>）定义函数。它主要有两个作用：缩减代码和改变this指向
箭头函数的 this 永远指向其上下文的 this ，也就是定义时所在的对象，任何方法都改变不了其指向，如 call() , bind() , apply()
普通函数的this指向调用它的那个对象
箭头函数有几个使用注意点。
函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。因为它没有this，所以此时的this就是外层的this，因此this就是指向定义时所在的对象
不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。
不可以使用yield命令，因此箭头函数不能用作 Generator 函数。
箭头函数没有原型属性

//Promise
Promise.prototype.then方法：链式操作
Promise.prototype.catch方法：捕捉错误
Promise.prototype.finally方法：在promise结束时，无论结果是fulfilled或者是rejected，都会执行指定的回调函数，避免同样的语句需要在then和catch中各写一次的情况

//扩展运算符
扩展运算符则允许将一个数组分割，并将各个项作为分离的参数传给函数

//let、const
````



### 64、★apply、call和bind函数的区别

````js
都是用来改变函数的this对象的指向的；
第一个参数都是this要指向的对象；
都可以利用后续参数传参；
bind是返回对应函数，便于稍后调用，apply、call是立即调用；
````

