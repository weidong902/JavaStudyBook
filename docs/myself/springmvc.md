![image-20210701153414761](springmvc.assets/image-20210701153414761.png)







## 面试题：

### 1、springmvc的controller是单例还是多例？

* 默认是单例的。

### 2、如何保证Controller的并发访问安全？

* 不定义成员变量
* 使用多例
* ThreadLocal去处理，将成员变量放置到ThreadLocal中去。

### 

