ACG智慧工业事业部_react前端研发工程师
工作职责
-负责工业大数据产品的研发工作 -技术视野广阔，有主导前端技术方案设计的能力和经验，能够主导大型项目研发 -配合产品、设计等其他角色，为业务提供优秀的技术支持 -与团队配合完成产品的迭代升级
任职资格
-5年及以上前端开发工作经验 -熟悉W3C标准，精通HTML5/CSS3 -熟悉JavaScript的核心技术，包括并不限于es6、ajax、dom、bom等 -熟悉常见页面布局方式，移动端响应式页面布局方式，熟悉css性能优化方式 -掌握面向对象编程思想，对表现与数据分离有深刻理解，熟练使用 React -熟悉Nodejs，能利用Nodejs开发工具，掌握常用的构建打包工具，如gulp、webpack、rollup等 -有前端性能优化经验，包括语言层面和架构层面 -熟练使用各种调试、抓包工具，能独立分析、解决和归纳问题 -热爱技术研发工作，有良好的工作责任感和团队合作能力

----
## 挂了

倒在了  链表 和 ts 类型体操上面

```js
//题目 数组转链表

//我的错误答案

let array=[3,2,4,1,5,6]

function node(key,num,nextKey){
    return {
        head:key,
        value:num,
        next:nextKey
    }
}
function getNodeList(arr){
    let nodeList={}
      for (let i = 0; i < arr.length; i++) {
        nodeList[i]= node(i,arr[i],i+1)
      }  
      return nodeList
}
getNodeList(array)


// 正确答案

function arrToList(arr){
   let header={index:0,value:arr[0],next:null}
   let obj=header
   for (let i = 1; i < array.length; i++) {
     obj.next={index:0,value:arr[0],next:null}
     obj= obj.next
   }
   return header
}


```
![](Pasted%20image%2020220722183240.png)

```ts

const a = { a: 1, b: 2, c: 3 };

type TA = keyof typeof a; //"a" | "b" | "c"

type Tb = valueOf typeof a; //1 | 2 | 3
//  
```