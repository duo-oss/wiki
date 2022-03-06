# babel
## Babel 是一个 JavaScript 编译器
Babel 通过语法转换器来支持新版本的 JavaScript 语法。
Babel 能够转换 JSX 语法
`@babel/preset-env`  是一个插件集合 目的是不用你一个一个把插件引入进来， 只要引入这个 就可以了


---
https://space.bilibili.com/299711132

https://space.bilibili.com/390120104/channel/seriesdetail?sid=458354

[相关专栏]
[政采云讲babel](https://juejin.cn/post/6844904079118827533)



      
      

**新的语法和** **api** **进入** **es** **标准也是有个过程的，这个过程分为这几个阶段：**
阶段 0 - Strawman: 只是一个想法，可能用 babel plugin 实现

阶段 1 - Proposal: 值得继续的建议

阶段 2 - Draft: 建立 spec

阶段 3 - Candidate: 完成 spec 并且在浏览器实现

阶段 4 - Finished: 会加入到下一年的 es20xx spec

有这么多特性要 babel 去转换，每个特性用一个 babel 插件来做。但是特性多啊，也就是说插件多，总不能让用户自己去配一个个插件吧，所以 babel 6 引入了 preset 的概念，就是 plugin 的集合。

  

如果我们想用 es6 语法就用 babel-preset-es2015，es7 就在引入 babel-preset-es2016 等等。如果是想用还没加入标准的特性，则分别用 babel-preset-stage0、babel-preset-stage1 等来引入。这样通过选择不同的 preset，加上手动引入一些插件，就是所有 babel 会做的转换。

  

可以把这个过程理解为集合求并集的过程。