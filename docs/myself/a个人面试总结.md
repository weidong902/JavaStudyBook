## 一、跟谁学：

### 1、主观问题：见有道云笔记

### 2、集合你用过哪些？

#### ①、哪些常用的集合？

* ArrayList、HashSet、HashMap、ConcurrentHashMap、

#### ②、介绍：

* ArrayList：存储顺序表结构的数据，底层使用是数组
* HashSet：存储无序的无重复的数据，底层使用的HashMap实例，元素存储到HashMap的key中。
* HashMap：双列集合key:value ;底层数据结构：数组+链表+红黑树（1.8）；线程不安全
* ConcurrentHashMap:线程安全的双列集合，常用于在多线程情况下替换HashMap。

### 2、 Hashmap的数据结构？



### 2、ConcurrentHashMap怎么实现线程安全的？



### 3、ThreadLocal如何用的？

* 用于线程间数据的隔离，填充的数据只属于当前线程，变量的数据对别的线程而言相对隔离，

### 4、Object作为HashMap的key的话对Object有什么要求？

* Hashmap不允许有重复的key，所以要重写它的hashcode和equal方法，以便确认key是否重复.（在get方法中执行了equals方法和hashCode方法。 所以必须要重写两个方法。）

### 5、HashSet存的数是有序的吗？

* HashSet是无序的，hashSet底层是维护了一个hashMap实例，元素存储到了hahsMap的key值中。
* 该类实现了Set接口，不允许出现重复元素，不保证集合中元素的顺序，允许包含值为null的元素，但最多只能一个。
* TreeSet是有序的。

### 5、HashSet怎么保证唯一性

因为hashSet底层采用hashMap实例，hashSet保证唯一的逻辑和HashMap的key值不能重复逻辑一致。

在插入元素时计算hash值得出在hash桶对应的下标，如果数组对应的下标有值了，需要把新值插入到链表，如果存在链表中的某个节点key值和新插入的值相同，则覆盖。


### 6、算法：

###  反转链表？

```java
// ## 请在下方进行输入
// A->B->C->D
// D->C->B->A
public class SingleLinkedListTest {

    static class Node {
        String data;
        Node next;

        public Node(String data) {
            this.data = data;
        }
        public Node next(Node next) {
            this.next = next;
            return next;
        }
    }

    public static void main(String[] args) {

        Node first = new Node("1");
        Node second = new Node("2");
        Node third = new Node("3");
        Node fourth = new Node("4");
        Node five = new Node("5");
        first.next(second);
        second.next(third);
        third.next(fourth);
        fourth.next(five);
        five.next(null);
        Node node=first;
        while (first != null) {
            System.out.print(first.data);
            first = first.next;
            System.out.print("-->");
        }
        System.out.println();
        System.out.println("--------链表反转---------");
        Node cur =node,pre=null,temp=null;
        while(cur!=null){
            temp=cur.next;
            cur.next=pre;
            pre=cur;
            cur=temp;
        }
        while(pre!=null){
            System.out.print(pre.data);
            pre=pre.next;
            System.out.print("->");
        }
    }
}

```


## 二、长城汽车

### [1、Spring如何处理依赖注入的？](https://blog.csdn.net/weixin_39559282/article/details/116101230)

### 循环注入分两种情况：1、构造方法注入   2、set方法注入（单例和多例有区别）

#### 1、构造方法注入：Spring没有办法解决会抛出异常：Requested bean is currently in creation: Is there an unresolvable circular reference?

代码：

```java 
public class AA {
    public AA(BB b) {}
}
public class BB {
    public BB(CC c) {}
}
public class CC {
    public CC(AA a) {}
}
// 测试：
public class SpringCyclicTest {
    public static void main(String[] args) {
        /*Requested bean is currently in creation: Is there an unresolvable circular reference?*/
        ApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
        AA a = (AA) context.getBean("a");
    }
}
```


配置：

```xml
 <bean id="a" class="com.weidd.best.frame.springTest.AA">
        <constructor-arg index="0" ref="b" ></constructor-arg>
    </bean>
    <bean id="b" class="com.weidd.best.frame.springTest.BB">
        <constructor-arg index="0" ref="c"></constructor-arg>
    </bean>
    <bean id="c" class="com.weidd.best.frame.springTest.CC">
        <constructor-arg index="0" ref="a"></constructor-arg>
    </bean>
```

结论：结论已经知道，会抛出循环注入的异常。
分析：
首先，在创建AA类对象的时候，构造方法需要BB类对象；
然后，创建BB类对象的时候构造方法又需要CC类对象；
最后去创建CC类对象时候发现构造方法需要AA类的对象，最终形成环，无法创建。进而抛出异常。

###  String str = new String("abc")创建了几个对象？

* 首先，不能直接说创建几个对象，需要分情况去分析：先说结论：一个或两个。
* 如果字符串 abc 之前没有用过，毫无疑问会创建2个对象，一个是new String创建的对象，一个是abc对象的内容创建的对象。
* 如果字符串abc之前用过，那么就只创建一个对象，abc取至缓存池。

### 2、Setter方法注入-单例

代码：

```java
public class AA {
    private BB b;
    public void setB(BB b) {
        this.b = b;
        System.out.println("BBBB");
    }
}
public class BB {
  private CC c;

    public void setC(CC c) {
        this.c = c;
        System.out.println("CCCCCCCCCC");
    }
}
public class CC {
    private AA a;
    public void setA(AA a) {
        this.a = a;
        System.out.println("AAAAAAAAAA");
    }

}
```

配置：

```xml
<bean id="a" class="com.weidd.best.frame.springTest.circular2.AA">
        <property name="b" ref="b"></property>
    </bean>
    <bean id="b" class="com.weidd.best.frame.springTest.circular2.BB">
        <property name="c" ref="c"></property>
    </bean>
    <bean id="c" class="com.weidd.best.frame.springTest.circular2.CC">
        <property name="a" ref="a"></property>
    </bean>
```

结论：正常创建并完成依赖注入、
分析：
Spring容器创建对象AA， 首先根据无参构造创建AA，将AA标识符放置到Bean容器池中，进行set注入，需要BB对象；
将BB对象通过无参构造创建，并放到Bean池中，进行set注入，需要CC对象；
通过无参构造创建CC，同时放入到Bean池中，需要进行set注入，发现Bean池中存有AA的对象标识，完成依赖注入。

结论：正常创建并完成依赖注入、
分析：
Spring容器创建对象AA， 首先根据无参构造创建AA，将AA标识符放置到Bean容器池中，进行set注入，需要BB对象；
将BB对象通过无参构造创建，并放到Bean池中，进行set注入，需要CC对象；
通过无参构造创建CC，同时放入到Bean池中，需要进行set注入，发现Bean池中存有AA的对象标识，完成依赖注入。

### 3、Setter方法注入-多例（Requested bean is currently in creation: Is there an unresolvable circular reference?）

代码同2：
配置：(多例)

```xml
 <bean id="a" class="com.weidd.best.frame.springTest.circular2.AA" scope="prototype">
        <property name="b" ref="b"></property>
    </bean>
    <bean id="b" class="com.weidd.best.frame.springTest.circular2.BB" scope="prototype">
        <property name="c" ref="c"></property>
    </bean>
    <bean id="c" class="com.weidd.best.frame.springTest.circular2.CC" scope="prototype">
        <property name="a" ref="a"></property>
    </bean>
```

结论：异常
分析：
对于多例，Spring容器不会进行缓存，只有使用时候才会被创建出来，因此不能提前暴露出Bean标识。

###  [2、BeanFactory和FactoryBean分别是什么？区别？](https://blog.csdn.net/weixin_39559282/article/details/116129431)

* BeanFactory 是一个工厂，是IOC容器（IOC容器应遵守的最基本的接口，是最底层最基本的编码规范）或对象工厂。（ApplicationContext、）

* FactoryBean是一个Bean，但不是一个简单的Bean，他可以生产或修饰对象生成的工厂Bean，它的实现用到了设计模式中的工厂模式和修饰器模式。其中的getObject（）方法，返回的不是工厂Bean自身而是返回被修饰的Bean实例。 工厂模式创建指定类对象的实例。

* Spring中对象的管理是由BeanFactory（也即是IOC容器）管理。

* BeanFactory：可以理解为容器。

* `FactoryBean`：可以理解为为使用工厂模式和装饰者模式为指定类创建实例对象。



### 3、HashMap为什么是线程不安全的？



### 4、ConCurrentHashMap如何保证线程安全的？



### 5、怎么判断一个对象可以被垃圾回收？

两种方式：

* 方式一：引用计数法
  * 为每个对象分配一个引用计数器，每当有地方引用，就给计数器加1，反之计数器减1，但是这个简单的算法无法解决循环引用的问题
* 方式二：可达性分析（标记存活对象）
  * 定义一系列的GC ROOT为起点，从起点开始向下开始搜索，搜索走过的路径为引用链，当一个对象到GC ROOT没有任何引用链相连的话，则此对象可以判定为可以被回收的。
* 作为GC Root 的对象包括：
  * 1、栈中引用的对象；
  * 2、静态变量、常量引用的对象；
  * 3、本地方法栈native方法引用的对象。

### 6、阻塞队列和队列的区别？

### 7、线程池的执行流程？

### 1、FullGC会清除`new Object()`?

* 强引用类型在进行GC的时候是不会被垃圾回收的。

### 2、redis数据结构？

* String类型的存储数的底层是Long

### 3、手动GC会回收创建的对象吗？



## 三、欧科云链

### 1、HashMap扩容怎么实现的？

### 2、redis保持数据一致性？

### 2、数据一致性？

### 3、怎么做服务拆分？

### 4、线程池？

### 5、钱如何流转？

### 6、synchronized和ReetLock区别？

### 7、LinkedList？为什么是双向链表？

* 双向链表：既保留单向链表的特点，同时由于是存储的有双向指针，在做查询的时候，可以只遍历链表长度的一半。



## 四、好未来 自由发挥的面多（面的不行）

### 1、谈谈对Java的认识

### 1、spring事务（工作中对事务的关注）

### 2、分布式事务

### 3、工作中印象深刻的点

### 4、HashMap和ConcurrentHashMap

### 5、红黑树为什么能加快查询？

### 5、线程、线程池

### 6、如何实现黑白名单？

* redis实现



## 五、火花思维

## 一面(20210427)

### 1、MySQL为什么使用B+树而不是B？为什么不是红黑树？

### 2、ArrayList和LinkedList的区别及底层实现？

### 3、HashMap扩容过程？

### 4、ConCurrentHashMap1.7和1.8之间的区别？

### 5、Redis秒杀、事务、单线程模型？内存淘汰策略？超时时间？

### 6、RabbitMQ确认机制？重复消费？

### 7、如何解决缓存穿透问题？布隆过滤器原理。数据存放。

### 8、MySQL中in主键会不会用到索引？

## 二面(20210511)

### 1、类的加载过程

### 2、Integer  两个100 是否相等，两个200呢 ？

* IntegerCache为Integer类的缓存类，默认缓存了-128~127的Integer值，如遇到[-128,127]范围的值需要转换为Integer时会直接从IntegerCache中获取。
* IntegerCache是Integer类中的静态内部类，用于缓存数据便于节省内存、提高性能。

从代码中我们可以通过设置java.lang.Integer.IntegerCache.high来设置缓存最大值，最终取值为Maths.max(high, 127)

`-Djava.lang.Integer.IntegerCache.high=255`  最大值可以自定义。

### 3、String、StringBuilder 、StringBuffer比较？

### [4、Spring Bean的生命周期？]()

### 5、三次握手和四次挥手？

### 6、

### 7、两个值，在不引入第三个值的情况下进行数据交换？

### 8、链表求和（先做反转，再去求和。）

## 三面：

### 1、布隆过滤器

* 原理

布隆过滤器本身是一个很长的二进制向量，既然是二进制的向量，那么显而易见的，存放的不是0，就是1。

新建一个16位的布隆过滤器，如图

![img](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/20190925094245218.png)

有一个对象，我们通过

方式一计算他的hash值，得到hash = 2

方式二计算他的hash值，得到hash = 9

方式三计算他的hash值，得到hash = 5

通过三个方法计算得到三个数值，我们把这三个数值对应的布隆过滤器向量值改为1，表明该位置有值。

第二个对象，加入得到值1 6 3，就把1 6 3 改为1

对于布隆过滤器本身来说，并没有存储任何数据，只是计算该数据的位置，然后存储向量值

那么，如果需要判断某个数据是否存在于布隆过滤器，就只需要判断计算出来的所有向量值是否都为1即可

但是：

​	当存储的数据向量不断增多，就可能会出现，2 9 5 向量值都为1，但是实际上没有这个数据的情况，这样就导致了，布隆过滤器只能判断某个数据一定不存在，但是不能保证某个数据一定存在。

另外，因为一个向量位置可能被多个对象映射，所以，布隆过滤器无法删除数据

* 原理？

创建一个长度为100的bit数组，对某一个key经过5个不同的hash函数进行hash，得到5个hash值后，这五个hash值尽可能的保证均匀分布在100个bit范围内。然后将5个hash值对应的bit位值对应的bit位置为1。判断一个key是否已经存在，依次看5个bit位置是否均为1就可以了，如果有任何一个不为1，则说明这个key不存在。此时需要注意，即使是每个bit位的值都为1，也不能肯定这个key一定存在。

![img](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/449cbf998c36cf520d133ce92ca5e5d0.png)

* BloomFilter的核心思想有两点：
  1. 多个hash，增大随机性，减少hash碰撞的概率
  2. 扩大数组范围，使hash值均匀分布，进一步减少hash碰撞的概率。
* 问题？

  * 即使判断每个bit位对应的值为1 ，也不能100%确定该key一定存在。（误判率）
  * 删除困难。（不同的key可能会在同一个bit未上值均为1）

* BloomFilter的应用
  * K-V系统快速判断某个key是否存在
    典型的例子有Hbase，Hbase的每个Region中都包含一个BloomFilter，用于在查询时快速判断某个key在该region中是否存在，如果不存在，直接返回，节省掉后续的查询。
  * 黑名单
    比如邮件黑名单过滤器，判断邮件地址是否在黑名单中
  * 排序(仅限于BitSet)
    仔细想想，其实BitSet在set(int value)的时候，“顺便”把value也给排序了。
  * 网络爬虫
    判断某个URL是否已经被爬取过
  * 缓存穿透
  * 集合元素重复的判断

### 2、rabbitMq

* 几种交换机： 

* 虚拟主机、交换机、队列之间的关系

  

  消费者配置项

  ![image-20210521091849066](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/image-20210521091849066.png)

* ![消费者配置项](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/image-20210520233822737.png)

消费者

![消费者](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/image-20210521091325118.png)

生产者模板

![生产者](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/image-20210521091508784.png)

![image-20210521091743788](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/image-20210521091743788.png)







### 3、zookeeper

#### zk有哪些节点？

（1）PERSISTENT-持久节点

除非手动删除，否则节点一直存在于 Zookeeper 上

（2）EPHEMERAL-临时节点

临时节点的生命周期与客户端会话绑定，一旦客户端会话失效（客户端与zookeeper 连接断开不一定会话失效），那么这个客户端创建的所有临时节点都会被移除。

（3）PERSISTENT_SEQUENTIAL-持久顺序节点

基本特性同持久节点，只是增加了顺序属性，节点名后边会追加一个由父节点维护的自增整型数字。

（4）EPHEMERAL_SEQUENTIAL-临时顺序节点

基本特性同临时节点，增加了顺序属性，节点名后边会追加一个由父节点维护的自增整型数字。



### 4、用户突然反馈服务慢了该如何排查？（CPU、内存、磁盘、I/O、网络带宽）

登录后台服务器或者监控平台，查看系统资源是否达到上限：CPU、内存、磁盘、I/O、网络带宽

网络是否出现问题、看日志中是否有大量的异常信息，查看服务是否有问题、最重要是排查JVM运行信息，去做排查。

* 1、服务器出口带宽不够用

  - 用户访问量上来了，并发量大了，分给用户的出口带宽变小，增加带宽即可加
  - 检查Web服务器的请求日志，是否存在DDos攻击

* 2、服务器负载过高，比如说CPU和内存消耗完了

  - 如果是访问量导致的，那就要考虑服务器扩容
  - 如果是性能的问题，通过日志查到慢查询的代码
  - 查看一下CPU占比较高的进程，然后使用jstack命令生成进程的堆栈信息

* 3、内存使用过高

  - 将没有用的进程杀掉
  - 查看占用进程高的应用的日志，对其做相应用的优化
  - 增加内存

  > 通过pstack这些工具去查对应进程的pid对系统调用的情况来定位故障原因

* 4、数据库瓶颈

  * 如果慢查询比较多。那么就要开发人员或 DBA 协助进行 SQL 语句的优化。
  * 如果数据库响应慢，考虑可以加一个数据库缓存，如 Redis 等。然后，也可以搭建 MySQL 主从，一台 MySQL 服务器负责写，其他几台从数据库负责读。
  
* 5、I/O读写负载过高

  - 因为读数据库导致IO过高，通常是慢查询导致的，交给开发让他们去优化
  - 如果是Web访问量过大而导致大量写日志的，可以考虑暂时将日志关闭，或优化日志
  - 因为写数据库导致IO过高的，增加DB的缓存的内存参数或升级硬盘，将硬盘换成SSD硬盘

* 6、磁盘空间满了

  - 需要及时清理磁盘
  
* 7、网站开发代码没有优化好

### 5、数据库迁移过程中有那些坑？（国产化改造过程中）



### 6、Spring中依赖注入？

7、







#  Other面试

### 1、redis 一致性hash了解吗

### 2、hashmap的扩容机制

### 3、线程池的配置参数和拒绝策略

### 4、线程sleep和wait的区别

### 5、mysql索引原理

### 6、主键索引跟普通索的区别

### 7、垃圾回收机制有了解吗

### 8、怎么分析fullgc和oom的

### 9、用到了什么中间件，配置中心修改了配置，客户端怎么更新的。

### 10、mq怎么保证一致性，消费失败怎么办

### 11、本地缓存怎么监听是热key

### 12、秒杀大并发处理

### 13、接口限流怎么实现，除了redis还有什么方案

### 14、砍价怎么解决并发扣减金额的问题

### 15、分库分表case实现，join查询解决方案

### 16、es的curd原理

### 17、用到了哪些设计模式

### 18、redis是单线程还是多线程的

### 19、数据库的事物隔离级别



## 酷家乐

一面 

1. ### mysql数据库索引结构是怎么样的

 2. ### mysql索引的最左匹配原则是怎样的

###  3.一个联合索引a,b,c如果从b开始查，索引会生效吗，为什么

 4. ### mysql事务的隔离级别，以及它们会有什么问题

 5. ### 串行化会出现幻读吗，串行化是什么意思

 6. ### redis有哪些数据结构，在业务中是怎么使用的

 7. ### redis常见数据结构内部是用什么数据结构来实现的

 8. ### redis有哪些集群方式

 9. ### 详细说一下redis cluster集群方式

 10. ### redis主节点挂了后是怎么选举从节点为主节点的

 11. ### cms垃圾收集器有什么特点

 12. ### 有哪些对象可以作为GC roots

 13. ### synchronize作用域

 14. ### 锁升级过程

 15. ### 聊一下CAS

 16. ### 偏向锁升级为轻量级锁的过程中，原来的锁会怎么处理

 17. ### spring bean的生命周期

 18. ### spring怎么解决循环依赖的

 19. ### spring三级缓存是怎么实现的

 20. ### synchronized作用于代码块的时候是锁的什么，作用于class的时候锁的什么

###  二面

 1. ### cap理论，什么时候保证ca，什么时候保证cp

 2. ### 一个单项链表如果尾节点指向之前的节点就会造成环，怎么判断成环的链表

 3. ### 线程池有哪些参数

 4. ### 线程池原理

 5. ### 什么时候线程池会销毁线程，怎么销毁的

 6. ### 在大量数据的情况下怎么保证数据的一致性和幂等性

 7. ### 怎么防止重复请求导致服务不可用

 8. ### 怎么通过消息中间件保证集群情况下某个服务崩溃了其他服务还可用

 9. ### 怎么保证某个服务的某个任务不可用了，其他任务正常执行（比如通过消息中间件）

 10. ### 乐观锁和悲观锁

 11. ### countdownlatch做什么用的

 12. ### 其中一个使用countdownlatch的线程异常了，怎么使其他线程停止？（如何扩展countdownlatch）

 13. ### rabbitmq主要用来做什么，在执行异步任务的时候如何回调

 14. ### 项目中有哪些比较有挑战性

###  15.使用分布式锁的时候如果服务宕机了，怎么释放锁

 16. ### 在链路追踪的时候怎么设计乐观锁





## 携程一面，1-3年

### 你现在做的项目的QPS是多少？（传统软件项目，能有啥QPS，回答不清楚）

### 那你这个日活是多少？能根据这个计算一下QPS吗？（大概报了一个数，面试官得出的结论是QPS不到1😂）

### 了解到你们这个项目采用了微服务架构，为什么要采用微服务架构？基于什么考虑？（原因就是老项目不行了呗，新项目架构设计的，但肯定不能这么回答，回答的是老项目有性能瓶颈）

### 你说一下这个性能瓶颈是在哪？（回答了用户反应响应变慢，可能是老系统的数据库有瓶颈）

### 那么响应变慢就要拆分微服务吗？实际上微服务并不是用来解决这个问题的？你们有没有从其它考虑优化你们的老项目？

### 线程池有哪几种？说一下有哪些参数？详细说一下这几个参数的作用？

### 在什么情况下，线程池最大线程数会失效？

### 说一下类的加载过程？

### 说一下为什么要分代？

### 说一下CMS垃圾回收器垃圾回收的过程？

### 说一下LinkedHashMap是怎么实现的？

### 说一下ConcorrentHashMap是怎么实现的？1.7和1.8有什么不同？

### 详细说一下DI、IOC、AOP?

### 我们现在有一些接口，我们想要打印它们地入参和出参日志，应该怎么办？

### 假如我现在有个接口，它有部分查询第三方，有部分查询数据库，它的查询速度也不一样，我们该怎么优化这个接口？

### 简单总结：只面了二十分钟，答的很差，其实问的都很常规，只是准备得不充分，发挥得也不好，浪费了难得的机会。还是要加强准备，很多知识最好还是真正地去掌握原理，而不是单纯地背一下。目标还是争取能进一个中型以上的互联网公司，毕竟不想以后还有人拿简历顶着我的脑袋：”你的QPS连1都不到，你拿什么和我斗？



## 上周面的水滴筹，1-3年，记得的就这么多了。

### 介绍一下你现在做的项目？

### 给我画一下你现在做的项目的业务架构图吧？

### 说一下OpenFegin的调用过程？

### 能给我说一下SpringBoot和SpringMVC的区别吗？

### 说一下HashMap的原理吧？

### 给我简单画一下它的put流程？

### HashMap在什么时候会树化，什么时候会反树化，说一下？

### 说一下ConcorrentHashMap是怎么保证线程安全的？

### 你在项目中用过多线程开发吗？详细介绍一下？

### 说一下JVM的内存分区？

### 说一下垃圾回收算法？

### 说一下几种常见的垃圾回收器？

### 为什么jdk1.9之后默认采用G1垃圾回收器？它有什么优势吗？

### 说一下Redis的常见数据类型？它们都适合用在什么场景？

### Redis的两种回收策略说一下？

### 给我介绍一下MongoDB的存储结构？在什么场景下适合使用MongoDB？

### 说一下MySQL、Redis、MongoDB三者的区别？以及它们各自适用的场景？

### 能说一下InnoDB和MyISAM的区别吗？

### 说一下MySQL的事务隔离级别？

### 能给我画一下MySQL索引的结构吗？

### 简单总结：这个是朋友内推的，面试官也和这个朋友比较熟，面试环境比较私密，氛围也很轻松，所以虽然答得一般，但是差不多聊了一个半小时。



## 京东

### 1.自我介绍

### 2.介绍项目，针对项目问一下具体业务

### 3.mysql索引类型

### 4.mysql索引结构

### 5.如何做分页的（分页插件如何实现的）

### 6.分页关键字，limit 5含义，limit5,10含义

### 7.如何进行sql语句优化的

### 8.redis是怎么用的

### 9.如何解决缓存不一致问题

### 10.场景题：一个表中有千万级或亿级数据，如何解决查询慢问题。（答得分库分表大致思路）

### 11.如何解决分库分表分页查询和组合条件查询问题

### 12.说一下你对springboot的理解

### 13.了解RPC框架吗（简单说了一下dubbo和zookeeper）

### 14.手写RPC框架怎么实现，应该注意什么（直接答得没有了解过）

### 15.rabbitMQ的exchange类型及介绍

### 16.RabbitMq的优势

### 17.MQ之间的优缺点

### 18.xxl-job阻塞处理策略有哪些

### 19.最近了解了什么新技术（说了一些最近的规划）

### 20.有什么想问我的（问了一下面试部门涉及的业务）

### 14.手写RPC框架怎么实现，应该注意什么？

### 

## 2020年3月15日面试

### 1，mq主要用在哪些场景下？

### 2，怎么保证mq消息一致性？ 怎么100%投递成功？

### 3，常用的数据结构？讲hashmap源码？

### 4，链表结构和红黑树结构的实例结构有哪些？都用过哪些？

### 5，树有一种实例对象？treemap用过没有？

### 6，数组的常用实例？arraylist和linkedlist的区别简单说一下

### 7，线程安全的一种map是什么

### 8，0.3的默认基本数据类型是什么？基本数据类型几大类？

### 9，java的八大数据类型的自动升序

### 10，如果对订单中的金额计算用什么对象进行操作？BigDecimal类都有哪些常用方法？

### 11，简单描述下jvm的基本流程

### 12，对spring源码有了解过吗？

### 13，常用的线程实例化的方式有哪些？

### 14，如何让一个对象尽快的进入GC？

### 15，你平常会对一个可能会觉得出现异常的代码块如何进行处理？

### 16，事务的回滚一般是放在catch块还是finally块

### 17，oracle和mysql最简单的区别是什么？

### 18，oracle表自增用的什么？（序列）

### 19，mysql索引的优化用了哪些？简单讲一下？

### 20，执行计划的关键字是什么？会关注里面哪些重要指标？

### 21，表关联的常用三种方法是哪些？join on 默认是什么?(我居然忘了inner join)

### 22,redis常用的基本数据类型有哪些？

### 23，redis最常用的jar包？(jedis，redisson)

### 24,通过redis实现一个分布式锁你会怎么做？

### 25,redis的锁机制怎么实现的

### 25，使用redis你会注意哪些问题？redis的缓存穿透和雪崩是怎么一回事？

### 26，常用的开发工具是什么？idea和eclipse你更喜欢用哪个？

### 27，简述一下nginx？

### 28，linux常用命令？怎么看磁盘内存cpu?

### 29,对我们公司有什么要了解的



## spring cloud有哪些特性

### int(1)和int(10)区别

### 什么是聚簇索引和非聚簇索引

### explain执行计划都有哪些列分别代表什么含义

### 乐观锁和悲观锁区别

### 算法题 实现并返回，2个长整数的字符串相加的和，比如"1234567890" + "99" = "1234567989"，不能直接转成bigdeciaml 或 long相加

### spring bean 加载过程

### 带几个人，怎么控制代码质量 定期codereview 交叉codeteview

### b树和b+树区别

### 介绍一下你熟悉的设计模式





## 阿里一面：

### ArrayList 和 linkedlist 区别。ArrayList 是否会越界。

### ArrayList 和 hashset 有何区别。hashset 存的数是有序的么。

### volatile 和 synchronized 区别

### 多态的原理

### 数据库引擎 Innodb 和 myisam 区别

### Redis 的数据结构

### Redis 是基于内存的么

### Redis 的 list zset 的底层实现

### http 和 https 的区别，tcp 握手过程

### jvm 垃圾回收算法手写冒泡

### 手写单例包括多线程下

### Java 线程间怎么实现同步，notify()与 notifyAll()的区别

### 数据库的悲观锁和乐观锁应用场景。

### 排序算法的复杂度，快速排序非递归实现。

### 海量数据过滤，黑名单过滤一个 url。

### 二面：

### list set map 底层使用什么实现的有哪些典型实现

### hashmap 扩容是怎么扩容的，为什么是 2 的幂

### concurrenthashmap 为什么线程安全，采用了什么措施应对高并发

### 线程池的参数有什么意义

### Springmvc 请求流程

### Spring IOC，autowired 如何实现

### 说下Spring boot

### SpringClound 的基本架构设计

### Dubbo 和 SpringClound 的区别在哪里，优劣势

### 说说一致性 Hash 算法

## 三面：

### 分布式架构设计哪方面比较熟悉

### 讲讲你对 CDN 的了解，与分布式缓存和本地缓存的区别

### 多线程和高并发有什么区别

### 高并发下有哪些常用的技术解决方案，举三个高并发场景设计例子

### 说一个你对 JVM 优化的实际案例，包括实际步骤和方法

### Docker 有使用过和了解吗？Docker 和 JVM 的区别是什么？

### Docker 的基本架构和使用场景？

### 负载均衡有接触过哪些开源框架，优劣势是什么？

### 数据库分库分表需要怎样来实现？

### 数据库端的常用优化策略？

### 如果让你来设计秒杀系统，你的设计思路是什么，为什么要这样设计？

## 面试总结：

### java 的基础知识点：主要围绕在集合类和多线程等：ArrayList、LinkedList、HashSet、HashpMap 的数据结果，以及如何扩容、以及 ConcurrentHashMap 相关的多线程安全等。

### JVM 的内存分配：几个常见的垃圾回收算法以及原理、还有对应的 JVM 优化参数需要牢记。

### 网络：TCP 的三次握手等网络都必问，重点掌握网络协议。

### Redis：作为分布式缓存的主力，基本也是 BAT 每次必考，重点是 Redis 的数据结构、内存、 算法、持久化，以及与别的缓存 memcached 的优劣势。

### 多线程：状态流转、多线程的实现，以及与高并发的区别等。

### Spring 框架问得是最多的，BAT 非常喜欢问，重点掌握。

### 最后就是分布式架构设计

### 常用的分布式架构设计方案：单点登录、分布式缓存、存储、消息的选型，还有就是数据 库端的优化方案(需要提前了解)。

### 最好能提前了解深入一个类似秒杀这样的项目，如果面试官问到类似的项目，你能把设计 思路讲出来，这对你的面试结果是很大的加分项。



## D轮公司

### 1.线程上下文切换，线程状态，线程池

### 2.mysql数据引擎，回表

### 3.项目经验，项目难点介绍，项目重构介绍

### 4.springIOC注入流程



## 面试问题：

### 分布式及redis原子性lru脚本

### mysql数据结构

### AQS源码

### mybatis执行流程及源码

### 红黑树时间复杂度空间复杂度分析



### 主要问题 HashMap底层实现，时间复杂度问题

### JVM调优

### 类加载机制

### Mysql数据结构，sql优化



## 美团

**背景**

小伙伴面的是美团，求职方向是团队技术负责人。此次面试以连环追问的方式为主，喜欢刨根问底，非常考研面试者的技术功底。以下是整理出的`部分面试题`，我们看看这些题该如何回答。

1. `先让写个线程安全的单例模式`

```
public class Singleton {

    private static volatile Singleton instance;

    private Singleton() {
    }

    public static Singleton getInstance(){
       if(null != instance) return instance;
       synchronized (Singleton.class){
           if (null == instance){
               instance = new Singleton();
           }
       }
       return instance;
    }

}
```

- 考察线程安全的单例模式，一方面是对基础的了解，另一方面以单例模式作为入口考察并发编程的知识点。
- 在这里你要知道，并发编程的三要素包括：原子性、可见性、有序性。
- 那具备三要素知识点的单例模式，主要是如上案例中的`双重检查锁`和CAS忙等策略。而双重检查锁的单例方式，对于面试官和求职者来说，可以往下继续聊的点就有了。
- 当然，如果你没有写出这样一种单例模式，大概率会被面试官引导过来。在面试的过程中最好的方式是主动和引导面试官走，也称对脾气，也就是技术臭味相投的感觉。

1. `高并发下，单例模式会存在哪些问题？`

- 单例模式就是以防止线程不安全和提高代码执行效率而设计的。
- 双重锁定检查(DCL，Double Check Lock)，也就是为此设计的。*别被 DCL 唬住喽*

1. `可见性和指令重排是怎么回事？`

- 可见性，Java 编程语言允许线程访问共享变量，为了确保共享变量能被准确和一致地更新，线程应该确保通过排它锁单独获得这个变量。
- 指令重排，在程序执行过程中为了性能考虑,，编译器和 CPU 可能会对指令重新排序。对于并发多线程场景下，指令重排会产生不确定的执行效果。

1. `voliate 关键字为什么可以解决可见性和指令重排？`

**可见性**

- 首选，volatile 关键字修饰的共享变量可以提供这种可见性规范，也叫做读写可见。
- 被 volatile 关键字修饰的共享变量在转换成汇编语言时，会加上一个以 lock 为前缀的指令，当CPU发现这个指令时，立即将当前内核高速缓存行的数据回写到内存，同时使在其他内核里缓存了该内存地址的数据无效。
- 另外，在早期的 CPU 中，是通过在总线加 LOCK# 锁的方式实现的，但这种方式开销较大。所以Intel开发了缓存一致性协议，也就是 MESI 协议，该解决缓存一致性。
- `volatile 的好处`，volatile 是一种非锁机制，这种机制可以避免锁机制引起的线程上下文切换和调度问题。所以，volatile 的执行成本比 synchronized 更低。
- `volatile 的不足`，volatile 关键字只能保证可见性，不能保证原子性操作。
- 此外如果你还能回答出，Unsafe.loadFence(); 保证在这个屏障之前的所有读操作都已经完成。的一些使用，那么面试绝对可以加分。

**指令重排**

```
public volatile boolean sign;
    descriptor: Z
    flags: ACC_PUBLIC, ACC_VOLATILE
```

- 从字节码层面，添加 ACC_VOLATILE，在汇编指令的打印会有 `lock addl $0x0,(%rsp)s`
- 从 JVM 层面，JMM 提供了8个 Happen-Before 规则来约束数据之间竞争、4个内存屏障 (LL LS SL SS)和As-if-serial
- 从硬件层面，sfence、lfence、mfence

1. `除了双重检查锁之外，还有什么别的办法实现线程安全的单例模式。`

```
public enum Singleton {

    INSTANCE;
    public void test(){
        System.out.println("hi~");
    }

}

@Test
public void test() {
    Singleton_07.INSTANCE.test();
}    
```

- 如果再让写一种差异比较大，方式不太一样的单例模式，那么 Effective Java 作者推荐使用枚举的方式这个时候可以甩出来秀一波了。
- 这种方式解决了最主要的；线程安全、自由串行化、单一实例。

1. `你看过什么源码吗？JVM 的源码是否看过？`

- 看过Dubbo、Spring、Mybatis等框架和一些中间件的源码，关于 JVM 多线程、并发、锁等核心内容的源码有所了解。
- 其实这块问你的内容，主要考察你对技术的学习是否有核心深度。如果你能对此有所回答，那么会得到面试官的一定认可。

1. `Dubbo是怎么工作的？`

- 简单来说，引入 Dubbo 服务的接口提供方与接口消费方，通过注册与拉取接口信息，把服务双方通过 Socket 进行连接。之后接口通信的时候通过代理类传输 Socket 请求，再由接口提供方通过反射调用真实服务，最终把接口执行信息返回给调用端。
- 在整个的回答过程中，可能会聊到注册中心、代理、反射、通信模型以及 Netty 的相关知识，这主要包括了通信协议的定义、半包粘包以及流量整形和各类知识。

1. `Dubbo通信是用的什么协议？`

![img](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/interview-28-2.png)

- Dubbo 主要支持这些协议：dubbo://、rmi://、hessian://、http://、webservice://、thrift://
- rmi 协议：走java二进制序列化，多个短连接，适合消费者和提供者数量差不多，适用于文件的传输，一般较少用
- dubbo协议： 默认就是走此协议，单一长连接，NIO 异步通信，基于 hessian 作为序列化协议
- hessian协议：走 hessian 序列化协议，多个短连接，适用于提供者数量比消费者数量还多，适用于文件的传输，一般较少用
- http协议：走json序列化
- 此外序列化的方式还有protobuf，protobuf 全称 Google Protocol Buffers，是 google 开发的一套用于数据存储，网络通信时用于协议编解码的工具库。

9 . `属于网络传输中的哪一层？`

![img](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/interview-28-3.png)

- TCP/IP 四层模型，是 OSI 七层模型的简化，泛指众多(TCP,UDP,IP等)协议。
- Dubbo 协议是 TCP 协议之上的协议，采用单一长连接和 NIO 异步通讯。

1. `TCP/IP协议来说建立连接的时候，为什么需要三次握手？`

- 三次握手是在安全可靠的基础上最少握手次数的方案，而两次握手并不能保证可靠性，四次握手又浪费了传输效率。
- TCP 传输控制协议，是一个面向连接的协议。在TCP/IP协议中，TCP协议提供可靠的连接服务，连接是通过三次握手进行初始化的。对于这部分知识可以深入了解下，资料也很丰富。

1. `你刚才说你们公司用的是Kafka，那么你觉得什么场景下要用到Kafka呢？`

| 特性   | Kafka                                                    | RocketMQ                                       | ActiveMQ                                                     | RabbitMQ                                                 |
| :----- | :------------------------------------------------------- | :--------------------------------------------- | :----------------------------------------------------------- | :------------------------------------------------------- |
| 吞吐量 | 高吞吐量，可达 10w 级别                                  | 高吞吐量，可达 10w 级别                        | 1w 级别，吞吐量相交比较低                                    | 1w 级别，吞吐量相交比较低                                |
| 时效性 | 延迟在 ms 级                                             | 延迟在 ms 级                                   | 延迟在 ms 级                                                 | 延迟在微妙级，延迟最低                                   |
| 可用性 | 天然的分布式系统，数据有副本机制，可用性非常高           | 分布式架构，可用性非常高                       | 主从架构，可用性较高                                         | 同 ActiveMQ                                              |
| 维护性 | 基于 Java 和 Scala 语言 实现，社区活跃度高，维护成本较低 | 基于 Java 语言实现，社区活跃度高，维护成本较低 | 基于 Java 语言实现，消息队列场景功能很完备，但社区活跃度较低，维护成本较高 | 基于 erlang 语言开发，社区活跃度一般，小团队维护成本较高 |

- 首先要知道，开源社区有很多优秀的队列中间件，比如 RabbitMQ、ActiveMQ、RocketMQ、Kafka，有些大厂还有自研的 MQ 队列中间件。
- 解耦、消峰、驱动等场景下的业务时以及日志采集系统、监控系统、流式处理等各类场景中都离不开MQ队列。

1. `你觉得对于一个消息中间件来说，需要达到哪些要求？`

- 高吞吐、低延时、可用性和可维护性，是消息队列中间件的核心要求。
- 同时你可以说出，中间件的作用是解决共性凝练和复用，从本质上提升代码的复用性、松耦合和互操作的标准机制，从而提升研发整体交付效率。

1. `Kafka是如何做到高吞吐量的？`

- Kafka是分布式消息系统，需要处理海量的消息，Kafka的设计是把所有的消息都写入速度低容量大的硬盘，以此来换取更强的存储能力，但实际上，使用硬盘并没有带来过多的性能损失。主要围绕顺序读写、零拷贝、文件分段、批量发送和数据压缩几个方面提高吞吐率。

1. `接下来是关于一些项目的问题`

- 在公司做过哪些项目，主要是哪方面？基础中间件还是业务项目？这个项目的主要难点在哪里？怎么解决的？
- 项目做到现在，有没有一些数据上的评价指标，表示这个项目做得不错？如果没有，那么现在让你自己评价一下，你觉得应该从哪些方面做考量？
- 对于一个系统来说，如何确保它的稳定性？（线程池用的哪种？为什么要用这几种？怎么用的？）
- 你在项目当中扮演什么样的角色？系统Owner还是开发？
- 你们团队的人员分布是怎么样的？内编几个，外包几个？如果带外包的话，如何管理外包？

1. `面试复盘`

- 基础知识要牢固，JVM内存分布，多线程，基本是面试必问的。其中多线程并发的源头：可见性，有序性，原子性相关的知识点一定要深入理解，能讲多清楚就讲多清楚，因为这些涉及到CPU，内存，指令集等底层知识，是多线程理论的源头。这方面的东西，如果面试官本身具备一定实力的话，是很容易深究下去的，如果只是浅尝辄止，一知半解，很容易暴露自己底层知识的不牢。逼格就会降低一个大的档次。
- 要有一定动手能力，一些常见的设计模式要随时随地可以写出来，如果有多种方案实现的话，需要都讲出来，并指出其中的优缺点。
- 分布式理论要扎实，要深入理解常见RPC框架（如dubbo）和消息队列的设计和实现原理。一个框架的出现，以及平时业务开发所遇不到的各种高阶知识点（比如dubbo的重试机制，熔断，降级服务等，kafka相比ActiveMQ等其它消息队列，优势在哪里，为什么它可以支撑那么高的吞吐量等等），一定是为了解决某些特定问题才会出现的，了解了问题源头，才能更好理解框架的全局设计思想，然后才能更好地吸收框架里面的各种解决方案以及背后体现出来的思想。
- 网络基础要夯实，http,https,TCP/IP协议以及网络通信的基础知识还是要知道的。
- 实际项目一定要学会总结，让人看到你做项目的亮点（代码规范，可维护性，可扩展性，稳定性，性能优化，访问量，以及一些其它的数据指标）。如果没有，一定要想办法自己加入一些可信的东西出来。毕竟面试官无法验证你说的是真是假，如果说得很像那么回事，面试官应该还是会认可的。

## 三、总结

- 如果你不希望面试时自己的薪资、待遇、职位，被别人安排来摆弄去，就要不断的在开发过程中提炼、总结、归纳自己的知识项，找到个人的价值力量，不断的去扩宽和加深。
- 我特别同意本文我的小伙伴的总结，也是此次求职者的分享；“程序员不是一个木偶人，不只是被动的接需求或者任务，也不能知识埋头苦干”。公司是没有永远稳定的，互联网的变化也更多，只有自己拥有了留下的本事和走出去的能力，才是个人的最安全区域。
- 最后希望每一个研发小伙伴都能有所积累和沉淀，把那些视频当电影、看文章当小说的毛病改改，要学会动起来，验证起来，实践才能让一个知识更能落到自己的脑子里。



### [聊聊Mysql索引和redis跳表 ---redis的有序集合zset数据结构底层采用了跳表原理 时间复杂度O(logn)(阿里)](https://www.cnblogs.com/aspirant/p/11475295.html)

redis使用跳表不用B+数的原因是：redis是内存数据库，而B+树纯粹是为了mysql这种IO数据库准备的。B+树的每个节点的数量都是一个mysql分区页的大小(阿里面试)
