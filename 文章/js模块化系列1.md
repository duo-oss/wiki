![](Pasted%20image%2020220213171908.png)
# JavaScript模块化
## 关于模块化
### 模块化编程是什么
	模块化编程（modular programming）是一种软件设计技术，它将软件分解为若干独立 的、可替换的、具有预定功能的模块，每个模块实现一个功能，各模块通过接口（输入输出 部分）组合在一起，形成最终程序。



### 为什么要模块化编程
模块化编程有什么有什么优势呢？
-   易设计：较大的复杂问题分解为若干较小的简单问题，使我们可以从抽象的模块功 能角度而非具体的实现角度去理解软件系统，从而整个系统的结构非常清晰、容易 理解，设计人员在设计之初可以更加关注系统的顶层逻辑而非底层细节。
    
-   易实现：模块化设计适合团队开发，因为每个团队成员不需要了解系统全貌，只需 关注所分配的小任务。另外团队可以灵活地增加人手，新人只需直接接手某个模块， 不会影响系统其他模块的开发。
    
-   易测试：每个模块不但可以独立开发，也可以独立测试，最后组装时再进行联合测 试。
    
-   易维护：如果需要修改系统或者扩展系统功能，只需针对特定模块进行修改或者添 加新模块。
    
-   可重用：很多模块的代码都可以不加修改地用于其他程序的开发。
	
模块化编程从1980年代开始广泛传播。这是 SoC 原则的理想 目标。
模块化编程 这么好为什么js没有呢？？

## JavaScript语言的诞生和历史

	JavaScript 因为互联网而生，紧随着浏览器的出现而问世

1994 Netscape网景公司的 Navigator浏览器 市场份额一举超过90% .公司觉得浏览器需要一种可以嵌入网页的脚本语言，用来控制浏览器行为。例如表单验证的小功能，在网页提交浏览器之前，早客户端就可以做好验证。

他们对浏览器脚本语言的设想是：==功能不需要太强，语法较为简单，容易学习和部署==

1995年，Netscape公司雇佣了程序员Brendan Eich开发这种网页脚本语言。Brendan Eich有很强的函数式编程背景(==这个很重要==)，希望以Scheme语言（函数式语言鼻祖LISP语言的一种方言）为蓝本，实现这种新语言。
![Brendan Eich](Brendan%20Eich.png)
1995年5月，Brendan Eich只用了10天，就设计完成了这种语言的第一版。==为了保持简单，这种脚本语言缺少一些关键的功能，比如块级作用域、模块、子类型（subtyping）等等==，但是可以利用现有功能找出解决办法。这种功能的不足，直接导致了后来JavaScript的一个显著特点：对于其他语言，你需要学习语言的各种功能，而对于JavaScript，你常常需要学习各种解决问题的模式。而且由于来源多样，从一开始就注定，JavaScript的编程风格是函数式编程和面向对象编程的一种混合体。

由上你就知道了 按理说一门编程语言 应该本身提供模块化的机制。但是js在诞生的时候时候为了

==功能不需要太强，语法较为简单，容易学习和部署== 


所以当初没有给js设计模块化。

## js模块化的上古时期
### 无模块时代

```js

// mine.js
var name = 'morrain'
var age = 18
 
// a.js
var name = 'lilei'
var age = 15
 
// b.js
var name = 'hanmeimei'
var age = 13

```
```html
//index.html
<script src="./mine.js"></script>
<script src="./a.js"></script>
<script src="./b.js"></script>

<script >

if(xx){ 
//.......
}else{
//xxxxxxxxxxx
} 
for(var i=0; i<10; i++){ //........
}

element.onclick = function(){
//.......
}

</script>
```

代码简单的堆在一起，只要能从上往下依次执行就可以了。
全局变量满天飞，函数定义一大堆。
**1. 全局变量的灾难**

小李写了 mine.js 在里面定义了 变量` age='18'`
小艾写了 a.js 在里面定义了 变量 `age='15'`  小李声明的变量被改写了
小佳在b.js 的代码里定义了 变量`age='13'`   小艾声明的变量被改写了 //悲剧


 **2. 函数命名冲突**

项目中通常会把一些通用的函数封装成一个文件，常见的名字有utils.js、common.js...

小李定义了一个函数：
`function formatData(){   }`

小艾想实现类似功能，于是这么写：
`function formatData2(){   }`

小佳又有一个类似功能，于是：
`function formatData3(){   }`

......

避免命名冲突就只能靠这样的方式人肉进行。

为了避免全局变量造成的冲突，人们想到或许可以用多级命名空间来进行管理，于是，代码就变成了这个风格：

```js
// mine.js
app.mine = {}
app.mine.name = 'morrain'
app.mine.age = 18

// a.js
app.moduleA = {}
app.moduleA.name = 'lilei'
app.moduleA.age = 15

// b.js
app.moduleB = {}
app.moduleB.name = 'hanmeimei'
app.moduleB.age = 13
```
```html
<script src="./mine.js"></script>
<script src="./a.js"></script>
<script src="./b.js"></script>
```
此时，已经开始有隐隐约约的模块化的概念，只不过是用命名空间实现的。这样在一定程度上是解决了命名冲突的问题， b.js 模块的开发者，可以很方便的通过 `app.moduleA.name` 来取到模块A中的名字，但是也可以通过 `app.moduleA.name = 'rename'`  来任意改掉模块A中的名字，而这件事情，模块A却毫不知情！这显然是不被允许的

  减少 Global 上的变量数目
  本质是对象，一点都不安全

## JS 可以承担更大的使命
js因浏览器而生，浏览器的日益壮大，倒逼jS越来越强大。随着Google 2004年推出重度使用js开发的Gmail 的火爆。人们发现js
这门语言可以承担更大的使命。于是人们纷纷用js开编写更多复杂的功能和网页应用。
不仅是google, 你也可以看淘宝网站的变化
![](淘宝首页对比.png)

随着业务进一步复杂，Ajax 诞生以后，前端能做的事情越来越多，代码量飞速增长。js越来越需要模块化。


聪明的开发者又开始利用 JavaScript 语言的函数作用域，

### 闭包 _IIFE_ 模式
```js
var Module = (function(){
    var _private = "safe now";
    var foo = function(){
        console.log(_private)
    }
    return {
        foo: foo
    }
})()

Module.foo();
Module._private; // undefined

```
```js
// mine.js
app.mine = (function(){
    var name = 'morrain'
    var age = 18
    return {
        getName: function(){
            return name
        }
    }
})()
 
// a.js
app.moduleA = (function(){
    var name = 'lilei'
    var age = 15
    return {
        getName: function(){
            return name
        }
    }
})()
 
// b.js
app.moduleB = (function(){
    var name = 'hanmeimei'
    var age = 13
    return {
        getName: function(){
            return name
        }
    }
})()

```
```html
// index.html
<script src="./mine.js"></script>
<script src="./a.js"></script>
<script src="./b.js"></script>
```
现在 b.js 模块可以通过 

app.moduleA.getName() 来取到模块A的名字，但是各个模块的名字都保存在各自的函数内部，没有办法被其它模块更改。这样的设计，已经有了模块化的影子，每个模块内部维护私有的东西，开放接口给其它模块使用，

但依然不够优雅，不够完美。譬如上例中，模块B可以取到模块A的东西，但模块A却取不到模块B的，因为上面这三个模块加载有先后顺序，互相依赖。

2006年 jQuery ,YUI 等库发布了， 前端迎来了 "库"时代

引入依赖 JQ
```js
var Module = (function($){
    var _$body = $("body");     // we can use jQuery now!
    var foo = function(){
        console.log(_$body);    // 特权方法
    }

    // Revelation Pattern
    return {
        foo: foo
    }
})(jQuery)

Module.foo();
```



 **3. 依赖关系不好管理**

b.js依赖a.js，标签的书写顺序必须是

```html
<script type="text/javascript" src="a.js"></script>
<script type="text/javascript" src="b.js"></script>
```

这样看是不是觉得也还凑合，那因为是demo 是例子。

当一个前端应用业务规模足够大后，这种依赖关系又变得异常难以维护。

OK 接下来才是惨淡的现实
一个网页 脚本依赖 依赖

```html
<script src="zepto.js"></script>
<script src="jhash.js"></script>
<script src="fastClick.js"></script>
<script src="iScroll.js"></script>
<script src="underscore.js"></script>
<script src="handlebar.js"></script>
<script src="datacenter.js"></script>
<script src="deferred.js"></script>
<script src="util/wxbridge.js"></script>
<script src="util/common.js"></script>
<script  src="util/login.js"></script>
<script  src="util/base.js"></script>
<script  src="util/city.js"></script>
<script  src="util/date.js"></script>
<script  src="util/cookie.js"></script>
<script  src="app.js"></script>
...

```
![](Pasted%20image%2020220211183045.png)
是的你没有看错 无比真实。。。

-   难以维护 _Very difficult to maintain!_
    
-   依赖模糊 _Unclear Dependencies_
    
-   请求过多 _Too much HTTP calls_

![](成龙奔溃.png)

后来 YUI在模块化上进行了丰富的尝试
```js
// YUI - 编写模块
YUI.add('dom', function(Y) {
  Y.DOM = { ... }
})

// YUI - 使用模块
YUI().use('dom', function(Y) {
  Y.DOM.doSomeThing();
  // use some methods DOM attach to Y
})

// hello.js
// 编写一个hello 模块，并且hello模块 并且引用了上面定义好的 dom 模块 ，声明 requires 对其的依赖， 并且 用Y.DOM 进行使用
YUI.add('hello', function(Y){
    Y.sayHello = function(msg){
        Y.DOM.set(el, 'innerHTML', 'Hello!');
    }
},'3.0.0',{
    requires:['dom']
})

```

```js

// main.js
YUI().use('hello', function(Y){
    Y.sayHello("hey yui loader");
})
```

另一个案例
```js
// do.js
YUI().add('doSomething',function(Y){
      Y.do=function(){};
} , version , {requires:['node','event']});

YUI().add('doSomething-v2', function(Y){} , version ,{
      use:['base','upgrade']
});

// main.js
YUI({
	modules: {
		'doSomething': {
			fullpath: 'do.js',
			requires: ['node','event']
		}
	}
}).use('doSomething',function(Y){
	//TODO
});
```

即便如此 YUI 还是要写很多 诸如此类的代码
```js
script(src="/path/to/yui-min.js") // YUI seed 
script(src="/path/to/my/module1.js") // add('module1') 
script(src="/path/to/my/module2.js") // add('module2') 
script(src="/path/to/my/module3.js") // add('module3')
```
```js
YUI().use('module1', 'module2', 'module3', function(Y) { 
// you can use all this module now

});
```
当然 雅虎也注意到了请求过多等问题。于是加上了Combo 合并请求的机制
原来的多个请求例如
```js
script(src="http://yui.yahooapis.com/3.0.0/build/yui/yui-min.js") 
script(src="http://yui.yahooapis.com/3.0.0/build/dom/dom-min.js")

```
可以合并为一个请求
```js
script(src="http://yui.yahooapis.com/combo? 3.0.0/build/yui/yui-min.js& 3.0.0/build/dom/dom-min.js")
```
服务端会把两个js文件 合并到一起返回。从而减少了 http 请求次数。但这种方式需要 服务器的支持。 有些库雅虎上面没有。 自己公司还是在服务器上部署一套 Combo 配置比较麻烦。

总的来说 **而YUI3的精巧的动态模块化机制已经达到了一定的高度，但是后面出现的CommonJs规范使js的模块化标准到达了新的高度**
后续雅虎也停止对 YUI的维护了。然而JQuery 依然在维护。JQuery永不为奴！

## 银瓶乍破水浆迸：
2009年CommonJS模块化规范

2009是个重要的年份

征服世界的第一步是跳出浏览器,从此js可以进行服务端开发。只要有js解释引擎的地方，既可以运行js

2009年1月的时候，Kevin Dangoor 写了这篇文章 [https://www.blueskyonmars.com/2009/01/29/what-server-side-javascript-needs/](https://www.blueskyonmars.com/2009/01/29/what-server-side-javascript-needs/)，提名为 ServerJS规范。喊出 **“javascript: not just for browsers any more!”**，2009年8月，更名为 CommonJS 。现在只剩下了规范。

	2013年5月，Node.js包管理器Npm的作者Isaac Z. Schlueter，宣布Node.js已经废弃了CommonJS，Node.js核心开发者应避免使用它 ，现在，node文档有专门CommonJS模块


严格地说这里的CommonJS 是一种规范。NodeJS的模块化是CommonJS的一种实现。这两种并不严格相等。
先看看 CommonJS 规定了什么？

### 模块作用域

1.  在一个模块中，有一个变量“require”，也是一个函数。
    
    1.  “require”函数接受一个模块标识符。
    2.  “require”返回外部模块的导出API。
    3.  如果存在依赖循环，则外部模块可能没有在其传递依赖之一所需的时间完成执行；在这种情况下，“require”返回的对象必须至少包含外部模块在调用导致当前模块执行的 require 之前准备好的导出。
    4.  如果请求的模块无法返回，“require”必须抛出错误。
2.  在模块中，有一个名为“exports”的变量，该对象是模块在执行时可以添加其 API 的对象。
    
3.  模块必须使用“exports”对象作为唯一的导出方式。

### 模块标识符

1.  模块标识符是由正斜杠分隔的“术语”字符串。
2.  术语必须是驼峰式标识符、“.”或“..”。
3.  模块标识符可能没有像“.js”这样的文件扩展名。
4.  模块标识符可以是 “相对路径” 或 “绝对路径（Top-level identifiers）” 。如果以“.”或者 ”..”开头，则模块标识符是“相对路径”。
5.  绝对路径从概念模块名称空间根目录中解析出来。
6.  相对路径是相对于写入和调用“require”的模块的标识符进行解析的。


简单例子
```js

exports.add = function() {
    var sum = 0, i = 0, args = arguments, l = args.length;
    while (i < l) {
        sum += args[i++];
    }
    return sum;
};
 
// increment.js
var add = require('math').add;
exports.increment = function(val) {
    return add(val, 1);
};
 
// program.js
var inc = require('increment').increment;
var a = 1;
inc(a); // 2
```

### NodeJS模块

CMJ规范是很好的开始，但[node的cmd模块](http://nodejs.cn/api/modules.html)更重要，毕竟大家都认这个

**模块封装器**，在执行模块代码之前，Node.js 将使用如下所示的函数封装器对其进行封装：

```js
(function(exports, require, module, __filename, __dirname) {
// 模块代码实际存在于此处
});

```
举个例子当你看 webpack等构建工具的 配置文件的时候，都是按照node的模块规范来写的
下面代码的关键字 `require`,`__dirname` 等 
```js
//craco.config.js
const path = require('path');
const fs = require('fs');
const CracoLessPlugin = require('craco-less');
const WebpackBar = require('webpackbar');
const MonacoWebpackPlugin = require('monaco-editor-webpack-plugin');

const {
	when,
	whenDev,
	whenProd,
	whenTest,
	ESLINT_MODES,
	POSTCSS_MODES,
} = require('@craco/craco');

const rewireEntries = [
 {
   name: 'share',
   entry: path.resolve(__dirname, './src/share.tsx'),
		outPath: 'share.html',
 },
];\

module.exports={
	babel: {
		plugins: ['babel-plugin-styled-components'],
	},
	devServer:{
	//...
	}
}
```
**模块加载是同步的，加载结束才会继续执行。因此也可以实现动态依赖，但会加大打包器静态分析难度**

即：**变量声明不会污染全局，模块拥有自己的私有变量**

**require.cache**，模块在第一次加载后被缓存到`require.cache`，这意味着（类似其他缓存）每次调用 `require('foo')` 都会返回完全相同的（单例）对象（当然，你也可篡改`require.cache`以多次加载）。相同对象，也即是修改返回的对象，会对之后的require()对象造成改动，你导出primitive类型当我没说。

**exports 对 module.exports 的引用**，所以直接修改 `exports` 没有效果。



### 同步加载对服务器/本地环境并不是问题，浏览器环境才是问题！

![](Pasted%20image%2020220212140731.png)

## 铁骑突出刀枪鸣: 浏览器的模块化方案AMD/CMD

### AMD & RequireJS

![](Pasted%20image%2020220212145419.png)

当初 ServerJS改名为CommonJS，就是想把这套规范推行到 浏览器端。然而。浏览器和服务器的 应用场景很不一样。很多人大牛反对 在浏览器里执行CommonJS 规范。并试图建立起浏览器的端的模块化规范。AMD

真正的 AMD 规范在这里：[Modules/AsynchronousDefinition](http://wiki.commonjs.org/wiki/Modules/AsynchronousDefinition)。AMD 规范一直没有被 CommonJS 社区认同.

Modules/1.0:
```js
var a = require("./a") // 执行到此处时，a.js 才同步下载并执行
``` 

AMD:
```js
define(["require"], function(require) {
  // 在这里，模块 a 已经下载并执行好
  // ...
  var a = require("./a") // 此处仅仅是取模块 a 的 exports
})
```
AMD 里提前下载 a.js 是浏览器的限制，没办法做到同步下载，这个社区都认可。

但执行，AMD 里是 Early Executing，Modules/1.0 里是第一次 require 时才执行。这个差异很多人不能接受，包括持 Modules/2.0 观点的也不能接受。

这个差异，也导致实质上 Node 的模块与 AMD 模块是无法共享的，存在潜在冲突。

####  模块书写风格有争议

AMD 风格下，通过参数传入依赖模块，破坏了 **就近声明** 原则。比如：
```js
define(["a", "b", "c" ], function(a, b, c) {
    // 等于在最前面申明并初始化了要用到的所有模块
   if (false) {
       // 即便压根儿没用到某个模块 b，但 b 还是提前执行了
       b.foo()
   }

})
```
AMD 规范的演进，离不开 RequireJS,AMD 的流行，很大程度上取决于 RequireJS 作者的推广.

还有就是 AMD 下 require 的用法，以及增加了全局变量 define 等细节，当时在社区被很多人不认可。

### CMD & Sea.js

那时 RequireJS 虽然很火，但真不够完善。在实际使用 RequireJS 的过程中，遇到不少问题。玉伯 在不断给 RequireJS 提建议，但不断不被采纳后，开始萌生了自己写一个 loader 的念头。

这就是 ==Sea.js==。

Sea.js 借鉴了 RequireJS 的不少东西，尽可能去掉了学院派的东西，加入了不少实战派的理念。

```js
//seajs

define(function(require, exports) {
	var a = require('./a');
	a.doSomething();
	exports.foo = 'bar';
	exports.doSomething = function() {};
});

```
```js
// RequireJS 兼容风格 
define('hello', ['jquery'], function(require, exports, module) {
	return { 
		foo: 'bar',
		doSomething: function() {} 
	};
});
```

比较一下 AMD vs CMD

```js
// AMD recommended
define(['a', 'b'], function(a, b){
    a.doSomething();    // 依赖前置，提前执行
    b.doSomething();
})
```
```js
// CMD recommanded
define(function(require, exports, module){
    var a = require("a");
    a.doSomething();
    var b = require("b");
    b.doSomething();    // 依赖就近，延迟执行
})
```

### 总结
RequireJs和Sea.js都是利用动态创建script来异步加载 js 模块的。
大概是 函数 toString 方法通过对factory回调toString拿到函数的代码字符串，然后通过正则匹配获取require函数里面的字符串依赖

这也是为什么二者都不允许require更换名称或者变量赋值，也不允许依赖字符串使用变量，只能使用字符串字面量的原因。

![](Pasted%20image%2020220212154454.png)

## 曲终收拨当心画,四弦一声如裂帛：纷纷涌现的构建工具

 随着 nodejs大火。基于 nodejs 的一系列自动化工具的出现，也标志着前端进入了新的时代。
- 2011 - Browserify
- 2012 - Grunt
- 2013 - Gulp
- 2014 - Webpack 


### Browserify
Browserify 通过预编译的方法，让Javascript前端可以直接使用Node后端的程序。我们可以用一套代码完成前后端，不仅工作量变少了，程序重用性增强，还可以直接在浏览器中使用大量的NPM第三方开源库的功能
- 前端JS可以使用npm包
	- 我们知道 npm 中有非常丰富的功能包，但没法在浏览器中直接用，因为他们是按照 nodejs 模块化标准写的，使用 require 和 module.exports 引用和构造模块，浏览器不支持此类语法，所以需要浏览器端模块化工具的支持，这样就相当于给浏览器端增加了 npm 库
- 模块化开发
	- 现在前端JS代码越来越多，可以通过模块化，把一个大的JS代码分割成不同的模块，存储在不同文件中，提高项目规范化，便于开发和维护

开发时使用nodejs的模式，正常使用 require 和 module.exports，在部署前使用 Browserify 进行编译 Browserify 会对代码进行解析，整理出代码中的所有模块依赖关系，然后把相关的模块代码都打包在一起，形成一个完整的JS文件，这个文件中不会存在 require 这类的模块化语法，变成可以在浏览器中运行的普通JS

dev开发阶段
```js
// main.js
var uniq = require('uniq'); 
var nums = [1, 2, 2, 3, 4, 5, 5, 5, 6];
console.log(uniq(nums));

//结果 [1,2,3,4,5,6]
```
打包
```shell
$ browserify main.js > bundle.js
```
前端html 引入打包好的文件
```html
<script src="bundle.js"></script>

```

### grunt 
- Grunt 是一个 JavaScript 自动化任务处理工具，是一个工具框架，有很多插件扩展它的功能。当我们需要做大量的重复工作时，比如：压缩／缩小／单元测试等，Grunt能够自动化（automation）地帮我们完成这些工作。
- Grunt 基于Node.js，Grunt及它的插件都作为一个包，可以用NPM安装进行管理，所以，NPM生成的package.json项目文件，记录当前项目中用到的 Grunt 插件。
- Grunt会调用Gruntfile.js文件，解析里面的任务并执行相应操作，需要安装Grunt-cli，也就是命令行的 Grunt，当然 Grunt-cli安装的并不是Grunt 。


### gulp
gulp 出现的比grunt   晚一年，也就更新颖，配置方式更简单。但功能和grunt 很类似。

grunt 运用配置的思想来写打包脚本，一切皆配置，所以会出现比较多的配置项，诸如option,src,dest等等。而且不同的插件可能会有自己扩展字段，导致认知成本的提高，运用的时候要搞懂各种插件的配置规则。  
gulp 是用代码方式来写打包脚本，并且代码采用流式的写法，只抽象出了gulp.src, gulp.pipe, gulp.dest, gulp.watch 接口,运用相当简单。经尝试，使用gulp的代码量能比grunt少一半左右

### webpack 
![](Pasted%20image%2020220212162653.png)
webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。主要是用来将前端资源打包、压缩、优化。

webpack 不仅仅将js 当做模块了。
webpack支持AMD和CommonJS，以及其他的一些模块系统，并且兼容多种JS书写规范，可以处理模块间的依赖关系，所以具有更强大的JS模块化的功能，它能对静态资源进行统一的管理以及打包发布。
作为一款 Grunt和Gulp的替代产品，Webpack受到大多数开发者的喜爱，因为它能够编译打包CSS，做CSS预处理，对JS的方言进行编译，打包图片，代码压缩等等。

在Webpack的世界里有两个最核心的概念：
1.一切皆模块
正如js文件可以是一个“模块（module）”一样，其他的（如css、image或html）文件也可视作模 块。因此，你可以require(‘myJSfile.js’)亦可以require(‘myCSSfile.css’)。这意味着我们可以将事物（业务）分割成更小的易于管理的片段，从而达到重复利用等的目的。

2.按需加载
传统的模块打包工具（module bundlers）最终将所有的模块编译生成一个庞大的bundle.js文件。但是在真实的app里边，“bundle.js”文件可能有10M到15M之大可能会导致应用一直处于加载中状态。因此Webpack使用许多特性来分割代码然后生成多个“bundle”文件，而且异步加载部分代码以实现按需加载。

---
每年冒出这么多的构建工具，实在让前端开发者崩溃。
![](Pasted%20image%2020220212163735.png)

### 
## 东船西舫悄无言，唯见江心秋月白： _ES6 MODULE

既然模块化开发的呼声这么高，作为官方的ECMA必然要有所行动，js模块化很早就列入草案，终于在2015年6月份发布了ES6正式版。


ES6 在语言标准的层面上，实现了模块功能，而且实现得相当简单，完全可以取代现有的 CommonJS 和 AMD 规范，成为浏览器和服务器通用的模块解决方案。

ES6 模块的设计思想，是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。比如，CommonJS 模块就是对象，输入时必须查找对象属性。
ES6只要增加了 `export` 、`import` 、`module` 等命令。

```js

// circle.js
export function area(radius) {
  return Math.PI * radius * radius;
}

export function circumference(radius) {
  return 2 * Math.PI * radius;
}

// main.js
import * as circle from './circle';
console.log(circle)   // {area:area,circumference:circumference}


// others.js


import {a} from './xxx.js'

a.foo = 'hello'; // 合法操作
a = {}; // Syntax Error : 'a' is read-only;

```
### 浏览器支持es6 模块

浏览器加载 ES6 模块，也使用`<script>`标签，但是要加入`type="module"`属性。

```html
<script type="module" src="./foo.js"></script>
```

上面代码在网页中插入一个模块`foo.js`，由于`type`属性设为`module`，所以浏览器知道这是一个 ES6 模块。

浏览器对于带有`type="module"`的`<script>`，都是异步加载，不会造成堵塞浏览器，即等到整个页面渲染完，再执行模块脚本，等同于打开了`<script>`标签的`defer`属性

```html
<script type="module" src="./foo.js"></script>
<!-- 等同于 -->
<script type="module" src="./foo.js" defer></script>
```
如果网页有多个`<script type="module">`，它们会按照在页面出现的顺序依次执行。

`<script>`标签的`async`属性也可以打开，这时只要加载完成，渲染引擎就会中断渲染立即执行。执行完成后，再恢复渲染。

```html
<script type="module" src="./foo.js" async></script>
```

一旦使用了`async`属性，`<script type="module">`就不会按照在页面出现的顺序执行，而是只要该模块加载完成，就执行该模块。

ES6 模块也允许内嵌在网页中，语法行为与加载外部脚本完全一致。

```html
<script type="module">
  import utils from "./utils.js";

  // other code
</script>
```

举例来说，jQuery 就支持模块加载。

```html
<script type="module">
  import $ from "./jquery/src/jquery.js";
  $('#message').text('Hi from jQuery!');
</script>
```

对于外部的模块脚本（上例是`foo.js`），有几点需要注意。

-   代码是在模块作用域之中运行，而不是在全局作用域运行。模块内部的顶层变量，外部不可见。
-   模块脚本自动采用严格模式，不管有没有声明`use strict`。
-   模块之中，可以使用`import`命令加载其他模块（`.js`后缀不可省略，需要提供绝对 URL 或相对 URL），也可以使用`export`命令输出对外接口。
-   模块之中，顶层的`this`关键字返回`undefined`，而不是指向`window`。也就是说，在模块顶层使用`this`关键字，是无意义的。
-   同一个模块如果加载多次，将只执行一次。

### babel 

*需要注意的是 并不是所有的浏览器都支持ES6,你在项目开发的时候可以愉快的使用 es6 模块化语法。配合babel 转化成es5支持的模块法语法*



如今，ES6模块化已经深入我们日常项目开发中，像vue、react项目搭建项目，组件化开发处处可见，其也是依赖模块化实现。

###  rollup

2015 年，前端的ES module发布后，rollup应声而出。
相比于browserify的CommonJs，rollup专注于ES module。
rollup编译ES6模块，提出了Tree-shaking，根据ES module静态语法特性，删除未被实际使用的代码，支持导出多种规范语法，并且导出的代码非常简洁，如果看过 vue 的dist 目录代码就知道导出的 vue 代码完全不影响阅读。

### Vite
Vite 是 vue 的作者尤雨溪在开发 vue3.0 的时候开发的一个 **基于原生 ES-Module 的前端构建工具**。
在ES6出现之前，我们的代码模块化都是使用的**社区规范**
ES6 出现之后，代码模块化有了**语言规范**，即 ES-Module

模块化方案有很多，基于这些方案的工具也有很多。这里先放个结论：**抛弃社区规范，使用语言规范成为前端模块化开发的趋势**

利用 ES6 的 import 会发送请求去加载文件的特性，拦截这些请求，做一些预编译，省去 webpack 冗长的打包时间


## 最后


	历史不是过去，历史正在上演。随着 W3C 等规范、以及浏览器的飞速发展，前端的模块化开发会逐步成为基础设施。一切终究都会成为历史，未来会更好。


关注公众号：【程序员铎声】