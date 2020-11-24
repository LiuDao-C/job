## CSS

### 1、盒子水平垂直居中

这种需求在我之前的项目中非常常见，刚开始我只用了这几种方法，后来学了CSS3，知道flex这种方法非常方便，尤其是在移动开发的时候

1. 定位：三种

   - 子绝父相，top值和left值都为50%，再设置margin-top和margin-left为负的宽度和高度的一半

     不足：必须知道盒子的宽高

   - 子绝父相，top、left、right、bottom值都为0，margin: auto

     不足：不需要知道盒子的宽高，但盒子必须要有宽高

   - 子绝父相，top值和left值都为50%，再设置transform: translate(-50%, -50%)

     不足：不需要宽高，但有兼容性问题

   - 子绝父相，top值和left值都为calc(50% - 50px)

2. flex布局

   ```javascript
   body {
   	display: flex;
   	justify-content: center;
   	align-items: center;
   }
   ```

   不足：适合移动性开发，但有兼容性问题

4. table  display:table-cell

   ```javascript
   /* 把盒子变成行内式，用文本水平垂直居中的方式 */
   body {
   	display: table-cell;
   	vertical-align: middle;
   	text-align: center;
   	/* 固定宽高   */
   	background-color: red;
   	width: 500px;
   	height: 500px;
   }  
   
   .box {
   	display: inline-block;
   }
   ```

   不足：父级元素必须有固定宽高
   
4. grid布局

   ````js
   	.container {
           display: grid;
           justify-items: center;
           align-items: center;
       }
       .box-center {
           text-align: center;
       }
   ````

   



### 2、CSS3的盒模型

1. 标准盒模型

   ```javascript
   box-sizing:content-box;
   // width和height指的是内容的宽高
   ```

2. 怪异盒模型（IE盒模型）

   ```js
   box-sizing:border-box;
   // width和height指的是盒子的宽高
   // bootstrap和常用的UI组件中的盒子多数采用此盒模型
   ```

3. flex弹性伸缩盒模型

   ![flex盒模型](E:\10 web开发\05 job_ready\job\CSS\flex盒模型.png)

4. 多列布局盒模型

````js
盒模型都是由4个部分组成的，分别是margin、border、padding和content
标准盒模型和IE盒模型的区别在于设置width和height时，所对应的范围不同。标准盒模型的width和height属性的范围只包含了content，而IE盒模型的width和height属性的范围包含了border、padding和content。
一般来说，我们可以通过修改元素的box-sizing属性来改变元素的盒模型。
````





### 3、经典布局方案

- 圣杯布局

  ```js
  // 浮动和负margin
  <style>
      html,
      body {
          height: 100%;
          overflow: hidden;
      }
  
      .container {
          height: 100%;
          padding: 0 200px;
      }
  
      .left,
      .right {
          width: 200px;
          min-height: 200px;
          background: lightblue;
      }
  
      .center {
          width: 100%;
          min-height: 400px;
          background: lightsalmon;
      }
  
      .left,
      .center,
      .right {
          float: left;
      }
  
      .left {
          margin-left: -100%;
          position: relative;
          left: -200px;
      }
  
      .right {
          margin-right: -200px;
      }
  </style>
  
  <div class="container clearfix">
      /* 把center放第一位，因为中间的内容是最主要的，要先渲染 */
      <div class="center"></div>
      <div class="left"></div>
      <div class="right"></div>
  </div>
  ```

- 双飞翼布局

  ```js
  // 浮动和负margin
  <style>
      html,
      body {
          height: 100%;
          overflow: hidden;
      }
  
      .container,
      .left,
      .right {
          float: left;
      }
  
      .container {
          width: 100%;
      }
  
      .container .center {
          margin: 0 200px;
          min-height: 400px;
          background: lightsalmon;
      }
  
      .left,
      .right {
          width: 200px;
          min-height: 200px;
          background: lightblue;
      }
  
      .left {
          margin-left: -100%;
      }
  
      .right {
          margin-left: -200px;
      }
  </style>
  
  <body class="clearfix">
      <div class="container">
          <div class="center"></div>
      </div>
      <div class="left"></div>
      <div class="right"></div>
  </body>
  ```

- 使用CALC

  ````js
  .center {
      /* 兼容到IE9 */
      width: calc(100% - 400px);
      min-height: 400px;
      background: #ffa07a;
  }
  ......
  ````

- flex

  ````js
  html,
  body {
      overflow: hidden;
  }
  
  .container {
      display: flex;
      justify-content: space-between;
      height: 100%;
  }
  
  .left,
  .right {
      flex: 0 0 200px;
      height: 200px;
      background: lightblue;
  }
  
  .center {
      flex: 1;
      min-height: 400px;
      background: lightsalmon;
  }
  ````

- 定位

  ````js
  <style>
      html,
      body {
          height: 100%;
          overflow: hidden;
      }
  
      .container {
          position: relative;
          height: 100%;
      }
  
      .left,
      .right {
          position: absolute;
          top: 0;
          width: 200px;
          min-height: 200px;
          background: lightblue;
      }
  
      .left {
          left: 0;
      }
  
      .right {
          right: 0;
      }
  
      .center {
          margin: 0 200px;
          min-height: 400px;
          background: lightsalmon;
      }
  </style>
  ````

  

### 4、::before和:after中双冒号和单冒号有什么区别？解释一下这两个符号的作用

````js
//想让插入的内容出现在其他内容前，使用::before,否则，使用::after

在css3中使用单冒号来表示伪类，用双冒号来表示伪元素。但是为了兼容已有的伪元素的写法，在一些浏览器中也可以使用单冒号来表示伪元素。
伪类一般匹配的是元素的一些特殊状态，如hover、link等，而伪元素一般匹配的是特殊位置，比如after、before等。
伪类表示已存在的某个元素处于某种状态，但是通过dom树又无法表示这种状态，就可以通过伪类来为其添加样式。例如a元素的:hover, :active等。
伪元素主要是用来创建一些不存在原有dom结构树种的元素，例如：用::before和::after在一些存在的元素前后添加文字样式等，这些被添加的内容会以具体的UI显示出来，被用户所看到的，这些内容不会改变文档的内容，不会出现在DOM中，不可复制，仅仅是在CSS渲染层加入。
伪元素与伪类的根本区别在于：操作的对象元素是否存在于原来的dom结构里。
````



### 5、CSS中哪些属性可以继承？

````js
每一个属性在定义中都给出了这个属性是否具有继承性，一个具有继承性的属性会在没有指定值的时候，会使用父元素的同属性的值来作为自己的值。
一般具有继承性的属性有，字体相关的属性，font-size和font-weight等。文本相关的属性，color和text-align等。表格的列表布局属性如list-style等。还有光标属性cursor、元素可见性visibility。
当一个属性不是继承属性的时候，我们也可以通过将它的值设置为inherit来使它从父元素那获取同名的属性值来继承。
````



### 6、CSS优先级计算

````js
判断优先级时，首先我们会判断一条属性声明是否有权重，也就是是否在声明后面加上了!important。一条声明如果加上了权重，那么它的优先级就是最高的，前提是它之后不再出现相同权重的声明。如果权重相同，我们则需要去比较匹配规则的特殊性。

一条匹配规则一般由多个选择器组成，一条规则的特殊性由组成它的选择器的特殊性累加而成。选择器的特殊性可以分为4个等级，第一个等级是行内样式，为1000，第二个等级是id选择器，为0100，第三个等级是类选择器、伪类选择器和属性选择器，为0010，第四个等级是元素选择器和伪元素选择器，为0001.规则中每出现一个选择器，就将它的特殊性进行叠加，这个叠加只限于对应的等级的叠加，不会产生进位。选择器特殊性值的比较是从左向右排序的，也就是说以1开头的特殊性值比所有以0开头的特殊性值要大。比如说特殊性值为1000的规则优先级就要比特殊性值为0999的规则高。如果两个规则的特殊性值相等的时候，那么就会根据它们引入的顺序，后出现的规则的优先级要高。
````



### 7、如何居中div

````js
一般常见的几种居中的方法有：

对于宽高固定的元素
1、我们可以利用margin:0 auto来实现元素的水平居中
2、利用绝对定位，设置4个方向的值都为0，并将margin设置为auto，由于宽高固定，因此对应方向实现平分，可以实现水平和垂直方向上的居中
3、利用绝对定位，先将元素的左上角通过top:50%和left:50%定位到页面的中心，然后再通过margin负值来调整元素的中心点到页面的中心
4、利用绝对定位，先将元素的左上角通过top:50%和left:50%定位到页面的中心，然后再通过translate来调整元素的中心点到页面的中心
5、利用flex布局，通过align-items:center和justify-content:center设置容器的垂直和水平方向上为居中对齐，然后它的子元素也可以实现垂直水平居中

对于宽高不定的元素，4和5两种方法，可以实现元素垂直水平居中
````



### 8、CSS3有哪些新特性？（根据项目回答）

````js
//过渡
transition: CSS属性，花费时间，效果曲线（默认ease），延迟时间（默认0）
//动画
animation: 动画名称，一个周期花费时间，运动曲线（默认ease），动画延迟（默认0），播放次数（默认1），是否反向播放动画（默认normal），是否暂停动画（默认running）
//形状转换
transform:rotate（30deg）/ translate(30px, 30px) / scale(.8)
//选择器
p:last-child     a[src$='.pdf']以pdf结尾
//边框图片
border-image: 图片url 图像边界向内偏移 图像边界的宽度(默认为边框的宽度) 用于指定在边框外部绘制偏移的量（默认0） 铺满方式--重复（repeat）、拉伸（stretch）或铺满（round）（默认：拉伸（stretch））;
//弹性布局flex
````



### 9、CSS3的Flex box（弹性盒布局模型），以及适用场景

````js
flex布局是CSS3新增的一种布局方式，我们可以通过将一个元素的display属性值设置为flex从而使它称为一个flex容器，它的所有子元素都会成为它的项目。

一个容器默认有两条轴，一个是水平的主轴，一个是与主轴垂直的交叉轴。我们可以
使用flex-direction来指定主轴的方向，//row | row-reverse | column | column-reverse
使用justify-content来指定元素在主轴上的排列方式，//flex-start左对齐 | flex-end | center | space-between两端对齐 | space-around每个项目两侧的间隔相等;
使用align-items来指定元素在交叉轴上的排列方式，还可以//flex-start | flex-end | center | baseline文字基线对齐 | stretch项目未设置高度或设为auto，将占满容器高度;
使用flex-wrap来规定当一行排列不下时的换行方式。//nowrap | wrap | wrap-reverse
flex-flow属性是flex-direction属性和flex-wrap属性的简写形式

对于容器中的项目，我们可以使用order属性来指定项目的排列顺序（数值越小越靠前，默认0），还可以使用flex-grow来指定当排列空间有剩余的时候，项目的放大比例，还可以使用flex-shrink来指定当排列空间不足时，项目的缩小比例。
````



### 10、用纯CSS创建一个三角形的原理是什么？

````js
采用的是相邻边框连接处的均分原理
将元素的宽高设为0，只设置border，把任意三条边隐藏掉（颜色设为transparent），剩下的就是一个三角形

#demo {
  width: 0;
  height: 0;
  border-width: 20px;
  border-style: solid;
  border-color: transparent transparent red transparent;
}
````



### 11、为什么需要清除浮动，清除浮动的方式

````js
在非IE浏览器（如Firefox）下，当容器的高度为auto，且容器的内容中有浮动（float为left或right）的元素，在这种情况下，容器的高度不能自动伸长以适应内容的高度，使得内容溢出到容器外面而影响（甚至破坏）布局的现象。
清除浮动主要是为了解决，父元素因为子级元素浮动引起的内部高度为0的问题
//清除浮动的方式
（1）使用clear属性清除浮动。
在最后一个浮动标签后，新加一个标签，给其设置clear：both；这会增加额外无意义标签
//优点：简单，代码少，浏览器兼容性好。
//缺点：需要添加大量无语义的html元素，代码不够优雅，后期不容易维护。
（2）使用CSS的：after伪元素
通过CSS伪元素在容器的内部元素最后添加了一个看不见的空格"020"或点"."，并且赋予clear属性来清除浮动。需要注意的是为了IE6和IE7浏览器，要给clearfix这个class添加一条zoom:1;触发haslayout。
.clearfix:after{
  content: "020"; 
  display: block; 
  height: 0; 
  clear: both; 
  visibility: hidden;  
  }

.clearfix {
  /* 触发 hasLayout */ 
  zoom: 1; 
  }
（3）使用BFC块级格式化上下文来清除浮动。
给浮动元素的容器添加overflow:hidden;或overflow:auto;可以清除浮动，另外在 IE6 中还需要触发 hasLayout ，例如为父元素设置容器宽高或设置 zoom:1。在添加overflow属性后，浮动元素又回到了容器层，把容器高度撑起，达到了清理浮动的效果。
（4）给浮动的元素的容器添加浮动
//这样会使其整体浮动，影响布局
//内容增多的时候容易造成不会自动换行导致内容被隐藏掉，无法显示要溢出的元素
因为BFC元素不会影响外部元素的特点，所以BFC元素也可以用来清除浮动的影响。
````



### 12、浏览器如何解析CSS选择器

````js
样式系统从关键选择器开始匹配，然后左移查找规则选择器的祖先元素。只要选择器的子树一直在工作，样式系统就会持续左移，直到和规则匹配，或者是因为不匹配而放弃该规则。

试想一下，如果采用从左至右的方式读取CSS规则，那么大多数规则读到最后（最右）才会发现是不匹配的，这样做会费时耗能，
最后有很多都是无用的；而如果采取从右向左的方式，那么只要发现最右边选择器不匹配，就可以直接舍弃了，避免了许多无效匹配。
````



### 13、★CSS传统样式和CSS预处理器的区别

````js
//变量
CSS预处理器支持任何变量（例如：颜色、数值、文本），然后你可以在任意地方引用变量。
Sass的声明变量必须是"$"开头，后面紧跟变量名和变量值，而且变量名和变量值需要使用冒号（：）分隔开；
//混合
Mixins可以将一部分样式抽出，作为单独定义的模块，被很多选择器重复使用。
Sass样式中声明Mixins时需要使用“@mixin”，然后后面紧跟Mixins的名，他也可以定义参数，同时可以给这个参数设置一个默认值，但参数名是使用“$”符号开始，而且和参数值之间需要使用冒号（：）分开。
在选择器调用定义好的Mixins需要使用“@include”，然后在其后紧跟你要调用的Mixins名。
//嵌套
CSS预处理器语言中的嵌套指的是在一个选择器中嵌套另一个选择器来实现继承，从而减少代码量，并且增加了代码的可读性
//继承
Sass和Stylus的继承是把一个选择器的所有样式继承到另个选择器上。在继承另个选择器的样式时需要使用“@extend”开始，后面紧跟被继承的选择器
````

