# react技术解密总结

#react 
## 浏览器渲染帧
主流浏览器的刷新频率是60HZ 
也就是1s刷新60次    16.6ms算作是一帧的时间
js可操作 DOM 浏览器的GUI线程 和JS线程是互斥的 所以**JS脚本执行**和**浏览器布局、绘制**不能同时执行。

在每一帧 16.6ms的时间内 浏览器要做的事情

> js脚本执行 -- 样式布局  -- 样式绘制



每当js执行的时间过长 超过了 16.6 这一帧，这次刷新就没有时间执行**样式布局**和**样式绘制**，只能在下一帧绘制， 表现上就会有卡顿感，或者不响应 点击等事件

新版（react18）想如何解决这个问题， 就是在浏览器的每一帧 留给js 一个特定的时间 
比如5ms ,剩下的时间教给 绘制。如果这5ms的时间不够的话， 就教给下一帧继续执行。也就是时间分片的原理

>这种将长任务分拆到每一帧中，像蚂蚁搬家一样一次执行一小段任务的操作，被称为`时间切片`（time slice）

在旧版本的react的架构中 **Reconciler**和**Renderer**是交替工作的，

## React15架构的缺点

在**Reconciler**中，`mount`的组件会调用[mountComponent (opens new window)](https://github.com/facebook/react/blob/15-stable/src/renderers/dom/shared/ReactDOMComponent.js#L498)，`update`的组件会调用[updateComponent (opens new window)](https://github.com/facebook/react/blob/15-stable/src/renderers/dom/shared/ReactDOMComponent.js#L877)。这两个方法都会递归更新子组件。

### [递归更新的缺点](https://react.iamkasong.com/preparation/oldConstructure.html#%E9%80%92%E5%BD%92%E6%9B%B4%E6%96%B0%E7%9A%84%E7%BC%BA%E7%82%B9)





由于递归执行，所以更新一旦开始，中途就无法中断。当层级很深时，递归更新时间超过了16ms，用户交互就会卡顿。

---
## 新的架构

在React16中，**Reconciler**与**Renderer**不再是交替工作。当**Scheduler**将任务交给**Reconciler**后，**Reconciler**会为变化的虚拟DOM打上代表增/删/更新的标记，
整个**Scheduler**与**Reconciler**的工作都在内存中进行。只有当所有组件都完成**Reconciler**的工作，才会统一交给**Renderer**。

### 双缓存

在内存中 提前准备好一个东西，当内存里准备完整之后， 瞬间替换掉 现有的 提升用户体验

### 双缓存Fiber树

