hr 微信

邮件沟通
16薪资 弹性 不打卡 9-7 知春路


面试经历
https://ac.nowcoder.com/discuss/458214?channel=-2&source_id=discuss_terminal_discuss_sim

简单手写题目

公司大牛 bolg
https://blog.csdn.net/weixin_34148508/article/details/91362021
里面有面试题

进程和线程的区别

货车和车厢的比喻很好

2. 在一个不重复无序数组中，找和等于target的数的数目；一个数用一次；

比方说 [1,2,3,4,5,6] target = 6, ans = 4（[1,5]  [6] [2,4]  [1,2,3]）

https://www.nowcoder.com/discuss/446769?source_id=discuss_experience_nctrack&channel=-1
2面


692.  前 K 个高频单词

RESTful API

比如 LRU、快排、堆、快速选择、反转链表

flex: 1 0 auto啥意思(后面两个参数忘了，只记得可能和缩小放大有关)  
node的path模块的resolve和join的区别(一个是解析，一个单纯的拼接)  
有没有用过ts，有哪些特性，然后问了我个enum的问题(我说公司一般都用自己的enum库)  
有没有用过graphql（我说没搞过可视化相关的，就不问了）  
[算法](/jump/super-jump/word?word=%E7%AE%97%E6%B3%95)，[二叉树](/jump/super-jump/word?word=%E4%BA%8C%E5%8F%89%E6%A0%91)对称，手写了一下  
问我来了想搞前端还是后端，我说前端

知乎  专栏
https://www.zhihu.com/column/c_130773972


面经
https://www.nowcoder.com/discuss/561775?type=all&order=recall&pos=&page=1&ncTraceId=&channel=-1&source_id=search_all_nctrack&gio_id=4250489818444A34BC2D7670BBFB7CD6-1658328672372


  
【OS & 网络】  
死锁发生的几个条件  
URL输入之后发生了什么  
进程线程区别  
https介绍  
【数据库】  
什么是事务 事务的优点  
mysql怎么删除表，delete truncate两个的区别  
【手撕代码】  

[完整实现链表反转](/jump/super-jump/practice?questionId=23286)

  
【反问】
https://www.nowcoder.com/practice/75e878df47f24fdc9dc3e400ec6058ca?tpId=117&&tqId=23286



5 restful编程风格

8 进程与线程

9 虚拟内存是什么，有什么好处

虚拟内存是计算机系统内存管理的一种技术。它使得应用程序认为它拥有连续可用的内存（一个连续完整的地址空间），而实际上，它通常是被分隔成多个物理内存碎片，还有部分暂时存储在外部磁盘存储器上，在需要时进行数据交换。与没有使用虚拟内存技术的系统相比，使用这种技术的系统使得大型程序的编写变得更容易，对真正的物理内存（例如RAM）的使用也更有效率。

11 线程同步方式 进程同步方式

12 说一下堆和栈

13 说一下你用过的linux命令(TOP AWK)

14  说一下 Linux权限

Linux把文件的权限划分为读（r）、写（w）、执行（x）三种，每一个文件又有三组r、w、x权限，分别对应文件所属者rwx权限，文件所属者所在用户组的rwx权限，和除了文件所属者和文件所属者所在用户组的其他所有用户。

15 是否还知道其他的linux权限（不知道）  
16 如何 限制进程的内存使用大小 （ 通过系统的ulimit命令限制资源的使用 ）

17 解释一下arp协议（地址解析协议）解析的是什么地址（网络地址，因为在网络层）

18 HTTP是长连接还是短连接？  
在HTTP/1.0中默认使用短连接。也就是说，客户端和服务器每进行一次HTTP操作，就建立一次连接，任务结束就中断连接。当客户端浏览器访问的某个HTML或其他类型的Web页中包含有其他的Web资源（如JavaScript文件、图像文件、CSS文件等），每遇到这样一个Web资源，浏览器就会重新建立一个HTTP会话。 而从HTTP/1.1起，默认使用长连接，用以保持连接特性。使用长连接的HTTP协议，会在响应头加入这行代码： Connection:[keep](/jump/super-jump/word?word=keep)-alive

在使用长连接的情况下，当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，客户端再次访问这个服务器时，会继续使用这一条已经建立的连接。Keep-Alive不会永久保持连接，它有一个保持时间，可以在不同的服务器软件（如Apache）中设定这个时间。实现长连接需要客户端和服务端都支持长连接。HTTP协议的长连接和短连接，实质上是TCP协议的长连接和短连接。  
19 get post区别  
20 springBoot中常用注解（答的不好）

21 radis数据类型，持久化方式，RDB持久化方式原理

当Redis需要做持久化时，Redis会fork一个子进程，子进程将数据写到磁盘上一个临时RDB文件中。 当子进程完成写临时文件后，将原来的RDB替换掉，这样的好处是可以copy-on-write。

22 [redis](/jump/super-jump/word?word=redis)渐进式哈希(rehashing)知道吗（不知道）  

23 解释快排和堆排的过程

24 数据库分页查询怎么实现（limit）

25 数据库事务ACID 隔离级别

26 B树和B+树的区别

[算法题](/jump/super-jump/word?word=%E7%AE%97%E6%B3%95%E9%A2%98)  1 有效的括号 [https://leetcode-cn.com/problems/valid-parentheses/](https://leetcode-cn.com/problems/valid-parentheses/)

2 k个一组翻转[链表](/jump/super-jump/word?word=%E9%93%BE%E8%A1%A8) [https://leetcode-cn.com/problems/reverse-nodes-in-k-group/](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

27 反问 我有什么不足 ？

---


– 标配的[五险一金](https://www.zhihu.com/search?q=%E4%BA%94%E9%99%A9%E4%B8%80%E9%87%91&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A%22463688235%22%7D)和极具竞争力的薪水，持续的期权机制；

– 各种水果零食、咖啡牛奶无限量免费供应，任君挑选；

– 每年十二天带薪年假，旅游工作两不误；

– 补充医疗保险，为你健康保驾护航；

– 弹性工作时间，为结果负责；

– 年度旅游&定期团建活动。