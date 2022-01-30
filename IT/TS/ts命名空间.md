
# ts 命名空间
## 一.由来

命名空间源自 JavaScript 中的[模块模式](http://www.ayqy.net/blog/%E6%A8%A1%E5%9D%97%E6%A8%A1%E5%BC%8F_javascript%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F2/)：

```js
var MyModule = {};
(function(exports) {
  // 私有变量
  var s = "hello";
  // 公开函数
  function f() {
    return s;
  }
  exports.f = f;
})(MyModule);

MyModule.f();
// 错误 MyModule.s is not a function
MyModule.s();
```

由两部分组成：

-   模块闭包（module closure）：封装模块实现，隔离作用域
-   模块对象（module object）：该模块暴露出去的变量和函数

后来在此基础上扩展出模块动态加载，拆分到多文件等支持

TypeScript 结合模块模式和类模式实现了一种模块机制，即命名空间：

```js
namespace MyModule {
  var s = "hello";
  export function f() {
    return s;
  }
}

MyModule.f();
// 错误 Property 's' does not exist on type 'typeof MyModule'.
MyModule.s;
```

编译产物就是经典的模块模式：

```js
var MyModule;
(function (MyModule) {
  var s = "hello";
  function f() {
    return s;
  }
  MyModule.f = f;
})(MyModule || (MyModule = {}));
MyModule.f();
MyModule.s;
```

## 二.作用

类似于模块，命名空间也是一种组织代码的方式：

```js
namespace Page {
  export interface IPage {
    render(data: object): string;
  }
  export class IndexPage implements IPage {
    render(data: object): string {
      return '<div>Index page content is here.</div>';
    }
  }
}

// 编译结果为
var Page;
(function(Page) {
  var IndexPage = /** @class */ (function() {
    function IndexPage() {}
    IndexPage.prototype.render = function(data) {
      return '<div>Index page content is here.</div>';
    };
    return IndexPage;
  })();
  Page.IndexPage = IndexPage;
})(Page || (Page = {}));
```

同样具有作用域隔离（上例仅暴露出`Page`一个全局变量），也支持按文件拆分模块：

```js
// IPage.ts
namespace Page {
  export interface IPage {
    render(data: object): string;
  }
}

// IndexPage.ts
/// <reference path="./IPage.ts" />
namespace Page {
  export class IndexPage implements IPage {
    render(data: object): string {
      return '<div>Index page content is here.</div>';
    }
  }
}

// App.ts
/// <reference path="./IndexPage.ts" />
/// <reference path="./DetailPage.ts" />
let indexPageContent = new Page.IndexPage().render({value: 'index'})
```

编译结果为：

```js
// IPage.js
空

// IndexPage.js
/// <reference path="./IPage.ts" />
var Page;
(function (Page) {
    var IndexPage = /** @class */ (function () {
        function IndexPage() {
        }
        IndexPage.prototype.render = function (data) {
            return '<div>Index page content is here.</div>';
        };
        return IndexPage;
    }());
    Page.IndexPage = IndexPage;
})(Page || (Page = {}));


// App.js
/// <reference path="./IndexPage.ts" />
/// <reference path="./DetailPage.ts" />
var indexPageContent = new Page.IndexPage().render({ value: 'index' });
```

注意到这里通过三斜线指令引入被拆分出去的“namespace 模块”（而不是像 module 一样 import），仍用`import`的话，会得到报错：

```js
// 错误 File '/path/to/IndexPage.ts' is not a module.ts(2306)
import IndexPage from "./IndexPage";
```js

P.S.另外，可以通过`--outFile`选项生成一个 JS Bundle（默认编译生成对应的同名 JS 散文件）

## 三.三斜线指令

支持 6 种指令：

-   描述文件间依赖：`/// <reference path="./myFile.ts" />`，引用当前目录下的`myFile.ts`
-   描述（类型）声明依赖：`/// <reference types="node" />`，引用`@types/node/index.d.ts`类型声明，对应`--types`选项
-   显式引用内置（类型）库文件：`/// <reference lib="es2015" />`或`/// <reference lib="es2017.string" />`，引用内置（类型）库`lib.es2015.d.ts`或`lib.es2017.string.d.ts`，对应`--lib`编译选项
-   禁用默认库：`/// <reference no-default-lib="true"/>`，编译过程中不加载默认库，对应`--noLib`编译选项，同时标记当前文件为默认库（以致于`--skipDefaultLibCheck`选项能够跳过检查该文件）
-   指定当前模块的 AMD 模块名：`///<amd-module name="NamedModule"/>`，指定 AMD 模块名为`NamedModule`
-   指定 AMD 模块依赖（_已废弃_）：`/// <amd-dependency path="legacy/moduleA" name="moduleA"/>`，依赖`legacy/moduleA`，并指定引入模块名为`moduleA`（`name`属性可选）

P.S.更多示例，见[Triple-Slash Directives](https://www.typescriptlang.org/docs/handbook/triple-slash-directives.html)

形式上以 3 条斜线开头，因此称为三斜线指令（triple-slash directives），其中 XML 标签用来表达编译指令。其它注意事项如下：

-   必须出现在文件首行（注释除外）才有效。也就是说，一条三斜线指令前面只能出现单行注释、多行注释或其它三斜线指令
-   `/// <amd-dependency />`指令已废弃，用`import "moduleName";`代替
-   指定`--noResolve`选项时，忽略掉所有`/// <reference path="..." />`指令（不引入这些模块）

作用上，`/// <reference path="..." />`类似于 CSS 中的`@import`（在指定`--outFile`选项时，模块整合顺序与 path reference 指令顺序一致）

实现上，在预处理阶段会深度优先解析所有三斜线指令，将指定的文件添加到编译过程中

P.S.出现在其它位置的三斜线指令会被当做普通单行注释，不报错，但无效（编译器不认）

## 四.别名

命名空间支持嵌套，因此可能会出现深层嵌套的情况：

```js
namespace Shapes {
  export namespace Polygons {
    export class Triangle { }
    export class Square { }
  }
}
```js

此时可以通过别名来简化模块引用：

```js
import P = Shapes.Polygons;
import Triangle = Shapes.Polygons.Triangle;
let sq = new P.Square();
let triangle = new Triangle();

// 编译后
var P = Shapes.Polygons;
var Triangle = Shapes.Polygons.Triangle;
var sq = new P.Square();
var triangle = new Triangle();
```js

不难发现，这里的`import`是`var`的语法糖：

> This is similar to using var, but also works on the type and namespace meanings of the imported symbol.

因此在_给一个值起别名时会创建一个新的引用_：

> Importantly, for values, import is a distinct reference from the original symbol, so changes to an aliased var will not be reflected in the original variable.

例如：

```js
namespace NS {
  export let x = 1;
}
import x = NS.x;
import y = NS.x;
(x as any) = 2;
y === 1;    // true

// 编译后
var NS;
(function (NS) {
    NS.x = 1;
})(NS || (NS = {}));
var x = NS.x;
var y = NS.x;
x = 2;
y === 1; // true
```js

P.S.`import q = x.y.z`别名语法_仅适用于命名空间_，要求右侧必须是`namespace`访问

## 五.`namespace`与`module`

> TypeScript 1.5 之前只有`module`关键字，不区分内部模块（internal modules）与外部模块（external modules），二者都通过`module`关键字来定义。后来清晰起见，新增`namespace`关键字表示内部模块

（摘自[namespace keyword](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-1-5.html#namespace-keyword)）

简言之，_关键字`module`与`namespace`在语法上完全等价_，例如：

```js
namespace Shape.Rectangle {
  export let a;
  export let b;
  export function getArea() { return a * b; }
}
```js

与

```js
module Shape.Rectangle {
  export let a;
  export let b;
  export function getArea() { return a * b; }
}
```js

的编译结果都是：

```js
var Shape;
(function (Shape) {
  var Rectangle;
  (function (Rectangle) {
    function getArea() { return Rectangle.a * Rectangle.b; }
    Rectangle.getArea = getArea;
  })(Rectangle = Shape.Rectangle || (Shape.Rectangle = {}));
})(Shape || (Shape = {}));
```js

_用`namespace`代替旧的`module`只是为了避免混淆_，或者说是给 ES Module、AMD、UMD 等 Module 概念（所谓的外部模块）让道。因为如果霸占着`module`关键字，实际上定义的不是 Module 而是 Namespace 的话，是很让人迷惑的一件事

## 六.模块与命名空间

### 内部模块与外部模块

也就是说：

-   内部模块：即命名空间，通过`namespace`或`module`关键字声明
-   外部模块：即模块（如ES Module、CommonJS、AMD、UMD 等），不用特别声明，（含有`import`或`export`的）文件即模块

外部模块可以简单理解为_外部文件中的模块_，因为可以在同一文件中定义多个不同的`namespace`或`module`（即内部模块），而无法定义多个ES Module

P.S.毕竟命名空间实质上是[IIFE](http://www.ayqy.net/blog/es-module/#articleHeader4)，与模块加载器无关，不存在文件即模块的加载机制约束

### 概念差异

概念上，TypeScript遵从ES Module规范（文件即模块），通过编译输出CommonJS、AMD、UMD等模块形式

而命名空间源自JavaScript中的[模块模式](http://www.ayqy.net/blog/%E6%A8%A1%E5%9D%97%E6%A8%A1%E5%BC%8F_javascript%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F2/)，算是旧时代的产物，_不建议使用_（用来[声明模块类型](http://www.ayqy.net/blog/%E6%A8%A1%E5%9D%97_typescript%E7%AC%94%E8%AE%B013/#articleHeader7)除外）

### 加载机制差异

模块引入机制上，命名空间需要通过三斜线指令引入，相当于源码嵌入（类似于CSS中的`@import`），会引入额外的变量到当前作用域中

P.S.如果不打包成单文件 Bundle，就需要在运行时引入这些通过三斜线指令引入的依赖（例如通过`<script>`标签）

而模块则通过`import/require`等方式引入，由调用方决定是否通过变量去引用它，而不会主动影响当前作用域

P.S.`import "module-name";`语法就只引入模块（的副作用），不引用并访问模块，具体见[import](http://www.ayqy.net/blog/es-module/#articleHeader11)

### 最佳实践

在模块与命名空间的使用上，有一些实践经验：

-   减少了命名空间嵌套层级，比如只含有静态方法的class通常是不必要的，模块名足够表达语义
-   模块仅暴露一个API时，用export default 更合适，引入更方便，而且调用方不必关注API名
-   要暴露出多个API的话，都直接`export`（数量过多的话，[引入模块对象](http://www.ayqy.net/blog/module_es6%E7%AC%94%E8%AE%B013/#articleHeader5)，如`import * as largeModule from 'SoLargeModule'`）
-   通过re-export扩展现有模块，例如`export as`
-   不要在模块里使用命名空间，因为模块本就具有逻辑结构（文件目录结构）和模块作用域，命名空间提供不了更多好处了

### 参考资料

-   [Namespaces](https://www.typescriptlang.org/docs/handbook/namespaces.html)
    
-   [Namespaces and Modules](https://www.typescriptlang.org/docs/handbook/namespaces-and-modules.html)
    
-   [Triple-Slash Directives](https://www.typescriptlang.org/docs/handbook/triple-slash-directives.html)
    
-   [1.10 Namespaces](https://github.com/Microsoft/TypeScript/blob/master/doc/spec.md#user-content-1.10)
    
-   [What’s the difference between internal and external modules in TypeScript?](https://stackoverflow.com/questions/12841557/whats-the-difference-between-internal-and-external-modules-in-typescript)