### 线上cpu100%如何排查？

### 线上内存100%如何排查？





# 一、跟谁学：

### 1、主观问题：见有道云笔记

### 2、集合你用过哪些？

>#### ①、哪些常用的集合？
>
>* ArrayList、HashSet、HashMap、ConcurrentHashMap、
>
>#### ②、介绍：
>
>* ArrayList：存储顺序表结构的数据，底层使用是数组
>* HashSet：存储无序的无重复的数据，底层使用的HashMap实例，元素存储到HashMap的key中。
>* HashMap：双列集合key:value ;底层数据结构：数组+链表+红黑树（1.8）；线程不安全
>* ConcurrentHashMap:线程安全的双列集合，常用于在多线程情况下替换HashMap。



### 3、 Hashmap的数据结构？

>
>
>

### 4、ConcurrentHashMap怎么实现线程安全的？

>
>
>

### 5、ThreadLocal如何用的？

* 用于线程间数据的隔离，填充的数据只属于当前线程，变量的数据对别的线程而言相对隔离，

### 6、Object作为HashMap的key的话对Object有什么要求？

* Hashmap不允许有重复的key，所以要重写它的hashcode和equal方法，以便确认key是否重复.（在get方法中执行了equals方法和hashCode方法。 所以必须要重写两个方法。）

### 7、HashSet

>#### 存的数是有序的吗？
>
>* HashSet是无序的，hashSet底层是维护了一个hashMap实例，元素存储到了hahsMap的key值中。
>* 该类实现了Set接口，不允许出现重复元素，不保证集合中元素的顺序，允许包含值为null的元素，但最多只能一个。
>* TreeSet是有序的。
>
>#### 2、HashSet怎么保证唯一性
>
>因为hashSet底层采用hashMap实例，hashSet保证唯一的逻辑和HashMap的key值不能重复逻辑一致。
>
>在插入元素时计算hash值得出在hash桶对应的下标，如果数组对应的下标有值了，需要把新值插入到链表，如果存在链表中的某个节点key值和新插入的值相同，则覆盖。
>
>


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

# 二、长城汽车

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

###  String str = new String("abc")创建了几个对象？

* 首先，不能直接说创建几个对象，需要分情况去分析：先说结论：一个或两个。
* 如果字符串 abc 之前没有用过，毫无疑问会创建2个对象，一个是new String创建的对象，一个是abc对象的内容创建的对象。
* 如果字符串abc之前用过，那么就只创建一个对象，abc取至缓存池。

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



# 三、欧科云链

### 1、HashMap扩容怎么实现的？

### 2、redis保持数据一致性？

### 2、数据一致性？

### 3、怎么做服务拆分？

### 4、线程池？

### 5、钱如何流转？

### 6、synchronized和ReetLock区别？

### 7、LinkedList？为什么是双向链表？

* 双向链表：既保留单向链表的特点，同时由于是存储的有双向指针，在做查询的时候，可以只遍历链表长度的一半。



# 四、好未来 自由发挥的面多（面的不行）

### 1、谈谈对Java的认识

>#### java特点：
>
>1.简单易学；
>
>2.面向对象（封装，继承，多态）；
>
>3.平台无关性（ Java 虚拟机实现平台无关性）；
>
>4.可靠性；
>
>5.安全性；
>
>6.支持多线程（ C++ 语言没有内置的多线程机制，因此必须调用操作系统的多线程功能来进行多线程程序设计，而 Java 语言却提供了多线程支持）；
>
>7.支持网络编程并且很方便（ Java 语言诞生本身就是为简化网络编程设计的，因此 Java 语言不仅支持网络编程而且很方便）；
>
>8.编译与解释并存;
>
>

### 2、spring事务（工作中对事务的关注）

### 3、分布式事务

### 4、工作中印象深刻的点

### 5、HashMap和ConcurrentHashMap

### 6、红黑树为什么能加快查询？

### 5、线程、线程池

### 6、如何实现黑白名单？

* redis实现



# 五、火花思维

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


### 3、zookeeper

#### zk有哪些节点？

（1）PERSISTENT-持久节点

除非手动删除，否则节点一直存在于 Zookeeper 上

（2）EPHEMERAL-临时节点

临时节点的生命周期与客户端会话绑定，一旦客户端会话失效（客户端与zookeeper 连接断开不一定会话失效），那么这个客户端创建的所有临时节点都会被移除。

（3）PERSISTENT_SEQUENTIAL-持久顺序节点

基本特性同持久节点，只是增加了顺序属性，节点名后边会追加一个由父节点维护的自增整型数字。

（4）EPHEMERAL_SEQUENTIAL-临时顺序节点（使用此节点做分布式锁）

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

### 7、for循环与foreach

* foreach适用于循环次数未知，或者计算循环次数比较麻烦情况下使用效率更高，但是更为复杂的一些循环还是需要用到for循环效率更高。



# 六、十荟团（技术栈，springCloud）

## 一面：

### 1、HashMap

* put流程



### 2、ConcurrentHashMap如何保证线程安全的

* 节点和HashMap有什么区别吗？（树的时候）
* 

### 3、LinkedHashMap是怎么保证顺序的？

* HashMap和双向链表合二为一即是LinkedHashMap

* 在hash

### 4、redis如何做分布式锁？

### 5、zk和redis做分布式锁的场景？

* redis 可能在主从复制的时候导致数据的丢失，（为什么？）

### 6、MySQL主键为什么是自增而不能用uuid

* 会大大增加页分裂的可能
* 排序

### 7、ThreadLocal（不行）

### 8、如何保证i++ 的线程安全？

* #### juc的AtomicInteger类

  * 有现成的方法

* 加synchronized

### 9、线程池：

#### 线程池的用处

#### 为什么不能用Excl.

## 二面

### 所了解的原子类？怎么实现原子性的？



### ArrayList，和LInkedList 插入值的时间复杂度



### Redis的线程模型

>> redis 内部使用**文件事件处理器 file event handler**，它是单线程的，所以redis才叫做单线程模型。它采用**IO多路复用机制**同时监听多个 socket，将产生事件的 socket 压入内存队列中，事件分派器根据 socket 上的事件类型来选择对应的事件处理器进行处理。
>
>##### 文件事件处理器的结构：
>
>- 多个 socket
>- IO 多路复用程序
>- 文件事件分派器
>- 事件处理器（连接应答处理器、命令请求处理器、命令回复处理器）
>
>##### 线程模型
>
>多个 socket 可能会并发产生不同的操作，每个操作对应不同的文件事件，但是 IO多路复用程序会监听多个 socket，会将产生事件的 socket 放入队列中排队，事件分派器每次从队列中取出一个 socket，根据 socket 的事件类型交给对应的事件处理器进行处理。
>[![img](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/2136379-20200830233359116-1537526582.png)](https://img2020.cnblogs.com/blog/2136379/202008/2136379-20200830233359116-1537526582.png)
>
>
> 
>
>## 二、一次客户端与redis的完整通信过程[#](https://www.cnblogs.com/mrmirror/p/13587311.html#2264664825)
>
>------
>
>##### 建立连接
>
>1. 首先，redis 服务端进程初始化的时候，会将 server socket 的 AE_READABLE 事件与连接应答处理器关联。
>2. 客户端 socket01 向 redis 进程的 server socket 请求建立连接，此时 server socket 会产生一个 AE_READABLE 事件，IO 多路复用程序监听到 server socket 产生的事件后，将该 socket 压入队列中。
>3. 文件事件分派器从队列中获取 socket，交给连接应答处理器。
>4. 连接应答处理器会创建一个能与客户端通信的 socket01，并将该 socket01 的 AE_READABLE 事件与命令请求处理器关联。
>
>##### 执行一个set请求
>
>1. 客户端发送了一个 set key value 请求，此时 redis 中的 socket01 会产生 AE_READABLE 事件，IO 多路复用程序将 socket01 压入队列，
>2. 此时事件分派器从队列中获取到 socket01 产生的 AE_READABLE 事件，由于前面 socket01 的 AE_READABLE 事件已经与命令请求处理器关联，
>3. 因此事件分派器将事件交给命令请求处理器来处理。命令请求处理器读取 socket01 的 key value 并在自己内存中完成 key value 的设置。
>4. 操作完成后，它会将 socket01 的 AE_WRITABLE 事件与命令回复处理器关联。
>5. 如果此时客户端准备好接收返回结果了，那么 redis 中的 socket01 会产生一个 AE_WRITABLE 事件，同样压入队列中，
>6. 事件分派器找到相关联的命令回复处理器，由命令回复处理器对 socket01 输入本次操作的一个结果，比如 ok，之后解除 socket01 的 AE_WRITABLE 事件与命令回复处理器的关联。
>
>[![img](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/2136379-20200830233422768-1205881078.png)](https://img2020.cnblogs.com/blog/2136379/202008/2136379-20200830233422768-1205881078.png)
>
>
> 
>

### 微服务的了解？

### 什么时候会考虑分库分表？怎么做的 ？

### 算法：力扣102题

* 层序遍历树，将同一层的树放到同一个集合中
* ![image-20210625223945758](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/image-20210625223945758.png)

# 七、当当

## 一面

> #### 技术栈：
>
> * springcloud
>
> #### 业务：
>
> * 支付
> * 购物车
> * 支付流程
>
> 高并发：做促销时候的高并发
>
> redis做分布式锁
>
> kafka、rabbitmq、redis、

### 1、项目介绍：

#### 1、项目框架：

spring+mybatis+jump（前端）：版本：spring版本5.1，mybatis: 3.5.,   mybatis-spring : 2.0

![image-20210626150925411](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/image-20210626150925411.png)



### 2、常用集合有哪些？

#### ArrayList、LinkedList、HashMap、ConcurrentHashMap

#### 什么情况下用ArrayList，什么情况下用LinkedList？

#### LinkedList是什么链表？（双向链表）

#### HashMap 1.7和1.8有哪些不同？

* 1.7头插法，会出现成环，造成死循环；1.7是由数组加链表组成；
* 1.8尾插发，解决了死循环，但是还是会有数据被覆盖的情况；1.8为了解决查询效率低的情况引入了红黑树。

#### CopyOnWriteArrayList(没回答出来)

* 

### 3、String、StringBuffer、StringBuilder

* 问了个字符串反转（竟然是问api的）

### 4、Spring中ApplicationContext的加载过程（Bean的加载过程）【回答的不太好】

###  4、Spring AOP

### 4、@Transactional注解在哪些场景下会失效？（使用aop的思想解释下）参看spring总结

### 5、Mybatis中如何在插入数据后获取主键值（主键值自增）

* 配置上配置

  >### 方式1：
  >
  >MyBatis中向MYSQL插入数据后，对象立刻获取主键的方法 。可以通过实体类中的getId()方法
  >
  >在insert或者insertSelective标签中，加入keyProperty和useGeneratedKeys两个属性，# useGeneratedKeys为true时则表示使用导出主键，keyProperty的值设为主键值。
  >
  >```xml
  ><insert id="insertSelective" parameterType="user" useGeneratedKeys="true" keyProperty="id"> insert into user(id,name) values (#{id,jdbcType=BIGINT},#{name,jdbcType=VARCHAR}) </insert>
  >```
  >
  >### 方式2：
  >
  >AFTER是在执行插入语句之后再执行selectKey。BEFORE是先选择主键，设置keyProperty的值然后执行插入语句。
  >
  >```xml
  ><insert id="insert" parameterType="****" >
  >	<selectKey resultType="java.lang.Integer" order="AFTER" keyProperty="id">
  >	<!-- 注意这里的keyProperty值是实体类中的主键属性名 -->
  >		SELECT LAST_INSERT_ID()
  >	</selectKey>
  > 	INSERT INTO table_name(*,*,*) VALUES(#{?},#{?},#{?})
  ></insert>
  >```
  >
  >


### rabbitmq

* 怎么保证消息到达消费方？

### 线程池的参数？

### 线程池的最佳线程数（IO密集和CPU密集型？）

* IO密集型：
  *  I/O密集型（I/O-bound）线程池设计 ：`最佳线程数 = ((线程等待时间+线程cpu时间)/线程cpu时间*cpu数目)`
* CPU密集型：
  * cpu 密集型（CPU-bound）线程池设计 ： `最佳线程数=cpu核数或者cpu核数±1`



### 100个电话号码通过哪个集合可以快速查出？

* 如上集合的话，我会选择HashMap，可以使用HashMap的continsKey()方法。底层就是一个hash查找，，时间复杂度低（O(1)--O(logn)）



# 8、天鹅到家



>深度学习，大数据，推荐算法，搜索算法
>
>基于java做的深度学习。

## 一面

### 手写快排

```java

```



### rabbitmq：消息是有序的吗？怎么保证有序消费 。

>
>
>

### 常用集合:

>ArrayList,HashSet,
>
>HashMap,ConcurrentHashMap
>
>

### Linux常用命令？

>
>
>

### 如何从日志中查看error，或统计error出现的次数

>theWord是你要搜索的词，一般是异常信息，
>
>filename是日志名称，
>
>perl -e '$/=undef;$_=<>; printf "$&:%dn", s/$&//g while /theWord/;' filename

### String的题

>String a ="abc";
>
>String b ="abc";
>
>String c =new String("abc");
>
>String d =new String("abc");
>
>sout(a==b);// ture
>
>sout(c==d);//false

### 项目拆分：

https://www.cnblogs.com/doit8791/p/9248476.html

## 二面：



>二面聊了项目 ，主要介绍了他们目前做的事情，
>
>基于java的深度学习，推荐，搜索算法。偏大数据，深度学习
>
> 推荐&搜索&广告&用户画像&深度学习整理（https://zhuanlan.zhihu.com/p/51015148）
>
>[Java Web个性化推荐引擎工程师](https://zhuanlan.zhihu.com/p/136398867)





---



# 9、北京银行

>Springboot SpringCloud 

### 聊项目，原因

### 拦截器和过滤器

![image-20210701162748789](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/image-20210701162748789.png)

### Springboot核心注解

### springboot如何初始化一些参数

### 分布式实现：dubbo

### 多线程、线程池（线程池的参数、拒绝策略）

#### 1、讲下dubbo的调用流程。

### redis做限流、Nginx做限流 、Springcloud做限流

### 



>spring 需要再学习下。
>
>springboot+ Springcloud需要看看。



# 10、民生科技

###  1、加解密、加签验签怎么做的？

### 2、mybatis

### 3、项目中大量if…else…优化策略

>将`if`内的逻辑封装成单独的函数提取出来
>
>### 策略模式
>
>有这么一种场景，根据不同的参数走不同的逻辑，其实这种场景很常见。
> 最一般的实现：
>
>```java
>if (strategy.equals("fast")) {
>  // 快速执行
>} else if (strategy.equals("normal")) {
>  // 正常执行
>} else if (strategy.equals("smooth")) {
>  // 平滑执行
>} else if (strategy.equals("slow")) {
>  // 慢慢执行
>}
>```
>
>#### 2.1 多态
>
>```java
>interface Strategy {
>  void run() throws Exception;
>}
>
>class FastStrategy implements Strategy {
>    @Override
>    void run() throws Exception {
>        // 快速执行逻辑
>    }
>}
>
>class NormalStrategy implements Strategy {
>    @Override
>    void run() throws Exception {
>        // 正常执行逻辑
>    }
>}
>
>class SmoothStrategy implements Strategy {
>    @Override
>    void run() throws Exception {
>        // 平滑执行逻辑
>    }
>}
>
>class SlowStrategy implements Strategy {
>    @Override
>    void run() throws Exception {
>        // 慢速执行逻辑
>    }
>}
>```
>
>具体策略对象存放在一个Map中，优化后的实现
>
>```java
>Strategy strategy = map.get(param);
>strategy.run();
>```
>
>上面这种优化方案有一个弊端，为了能够快速拿到对应的策略实现，需要map对象来保存策略，当添加一个新策略的时候，还需要手动添加到map中，容易被忽略。
>
>#### 2.2 枚举
>
>发现很多同学不知道在枚举中可以定义方法，这里定义一个表示状态的枚举，另外可以实现一个run方法。
>
>```java
>public enum Status {
>    NEW(0) {
>      @Override
>      void run() {
>        //do something  
>      }
>    },
>    RUNNABLE(1) {
>      @Override
>       void run() {
>         //do something  
>      }
>    };
>
>    public int statusCode;
>
>    abstract void run();
>
>    Status(int statusCode){
>        this.statusCode = statusCode;
>    }
>}
>```
>
>重新定义策略枚举
>
>```java
>public enum Strategy {
>    FAST {
>      @Override
>      void run() {
>        //do something  
>      }
>    },
>    NORMAL {
>      @Override
>       void run() {
>         //do something  
>      }
>    },
>
>    SMOOTH {
>      @Override
>       void run() {
>         //do something  
>      }
>    },
>
>    SLOW {
>      @Override
>       void run() {
>         //do something  
>      }
>    };
>    abstract void run();
>}
>```
>
>通过枚举优化之后的代码如下
>
>```java
>Strategy strategy = Strategy.valueOf(param);
>strategy.run();
>```
>
>

### 4、dubbo泛化调用使用及原理解析

### 5、项目中用到哪些设计模式？

>工厂模式
>
>单例模式
>
>代理模式

### 6、ThreadLocal（为什么key是弱引用？value什么时候回收）

>### 为什么要使用弱引用
>
>每个thread中都存在一个map, map的类型是ThreadLocal.ThreadLocalMap.
>
>Map中的key为一个threadlocal实例. 这个Map的确使用了弱引用,不过弱引用只是针对key.
>
>每个key都弱引用指向threadlocal.
>
>所以当把threadlocal实例置为null以后,没有任何强引用指向threadlocal实例,所以threadlocal就可以顺利被gc回收
>
>**注意！假如每个key都强引用指向threadlocal，也就是上图虚线那里是个强引用，那么这个threadlocal就会因为和entry存在强引用无法被回收！造成内存泄漏** ，除非线程结束，线程被回收了，map也跟着回收。
>
>

>### 依然出现的内存泄露问题
>
>虽然上述的弱引用解决了key，也就是线程的ThreadLocal能及时被回收，但是value却依然存在内存泄漏的问题。
>
>当把threadlocal实例置为null以后,没有任何强引用指向threadlocal实例,所以threadlocal将会被gc回收.
>
>map里面的value却没有被回收.而这块value永远不会被访问到了. 所以存在着内存泄露,
>
>因为存在一条从current thread连接过来的强引用.
>
>只有当前thread结束以后, current thread就不会存在栈中,强引用断开, Current Thread, Map, value将全部被GC回收.
>
>所以当线程的某个localThread使用完了，马上调用threadlocal的remove方法
>
>





# 水滴筹

>水滴保：做保险的：资金的流入流出，支付流程，保单、退费
>
>技术栈：springcloud

### 需求下来了要考虑哪些？

>他的观点：外系统外服务是否会影响到我们的流程，（中间件，外服务）保证别的服务不能提供服务时候自己项目还能正常运行。
>
>设计、分析、流程图、
>
>软件开发流程：需求确认——概要设计——详细设计——编码——单元测试——集成测试——系统测试——维护 
>
>软件设计思路和方法的一般过程，包括设计软件的功能和实现的算法和方法、软件的总体结构设计和模块设计、编程和调试、程序联调和测试以及编写、提交程序。

### 现有架构的优化？



### 数据库锁？（行锁、表锁）

>/docs/database/mysql/MySQL锁相关.md



### 分布式锁？

* 基于数据库做分布式锁
* 使用redis做分布式锁
* 使用zookeeper做分布式锁

### 数据库sql？

```sql
# 是否会用到索引
select * from table where currentdate between xx and xxx and id>1 order  by id limit 10;
```

### 创建线程的方式？（线程的状态）

三种方式创建线程。



# 12、京东：

>京喜
>
>京喜社区团购
>
>线下店生活渠道
>
>搜索：solr

### 1、创建线程的方式？

> 首先可以说：在不使用线程池的前提下有三种方式：继承java.lang.Thread类和实现java.lang.Runnable接口；JDK1.5以后，增加了两个创建线程的方式，即实现java.util.concurrent.Callable接口和线程池。

### 2、线程池的参数？

>核心线程数、最大线程数、核心线程数、
>
>

### 3、ThreadLocal原理？

>### 原理？
>
>
>
>### 如何防止内存泄漏？
>
>

### 4、几种垃圾收集器？（说的不好）

>### 1、7大垃圾收集器：
>
>* CMS、G1
>
>### 2、CMS和G1：
>
>#### CMS和G1 原理及优缺点：
>
>
>
>

### 5、SpringBoot

>### SpringBoot启动启动过程？
>
>### 如何加载配置文件：加载不同的配置文件？
>
>

### 6、redis

>### 1、过期键的策略？（三种策略）
>
>* 定时删除
>* 惰性删除
>* 定期删除
>
>### 1、内存淘汰策略（六种内存淘汰策略）
>
>* `volatile-lru：`从`已设置过期时间`的数据集（server.db[i].expires）中挑选最近最少使用的数据淘汰
>
>* `volatile-ttl`：从`已设置过期时间`的数据集（server.db[i].expires）中挑选将要过期的数据淘汰
>
>* `volatile-random`：从已设置过期时间的数据集（server.db[i].expires）中随机移除key
>
>* `allkeys-lru`：当内存不足以容纳新写入数据时，在键空间中，移除最近最少使用的key（这个是最常用的）
>
>* `allkeys-random`：从数据集（server.db[i].dict）中任意选择数据淘汰
>
>* `no-eviction`：当内存不足以容纳新写入数据时，新写入操作会报错，无法写入新数据，一般不采用
>
>4.0版本后增加以下两种：
>
>* volatile-lfu：从已设置过期时间的数据集(server.db[i].expires)中挑选最不经常使用的数据淘汰
>* allkeys-lfu：当内存不足以容纳新写入数据时，在键空间中，移除最不经常使用的key
>
>### 四、Redis为什么这么快
>
>1、完全基于内存，绝大部分请求是纯粹的内存操作，非常快速。数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1)；
>
>2、数据结构简单，对数据操作也简单，Redis中的数据结构是专门进行设计的；
>
>3、采用单线程，避免了不必要的上下文切换和竞争条件，也不存在多进程或者多线程导致的切换而消耗 CPU，不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗；
>
>4、使用多路I/O复用模型，非阻塞IO；
>
>5、使用底层模型不同，它们之间底层实现方式以及与客户端之间通信的应用协议不一样，Redis直接自己构建了VM 机制 ，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求；
>
>以上几点都比较好理解，下边我们针对多路 I/O 复用模型进行简单的探讨：
>
>（1）多路 I/O 复用模型
>
>多路I/O复用模型是利用 select、poll、epoll 可以同时监察多个流的 I/O 事件的能力，在空闲的时候，会把当前线程阻塞掉，当有一个或多个流有 I/O 事件时，就从阻塞态中唤醒，于是程序就会轮询一遍所有的流（epoll 是只轮询那些真正发出了事件的流），并且只依次顺序的处理就绪的流，这种做法就避免了大量的无用操作。
>
>**这里“多路”指的是多个网络连接，“复用”指的是复用同一个线程。**采用多路 I/O 复用技术可以让单个线程高效的处理多个连接请求（尽量减少网络 IO 的时间消耗），且 Redis 在内存中操作数据的速度非常快，也就是说内存内的操作不会成为影响Redis性能的瓶颈，主要由以上几点造就了 Redis 具有很高的吞吐量。
>
>### 五、那么为什么Redis是单线程的
>
>官方FAQ表示，因为Redis是基于内存的操作，CPU不是Redis的瓶颈，Redis的瓶颈最有可能是机器内存的大小或者网络带宽。既然单线程容易实现，而且CPU不会成为瓶颈，那就顺理成章地采用单线程的方案了（毕竟采用多线程会有很多麻烦！）Redis利用队列技术将并发访问变为串行访问
>
>1、绝大部分请求是纯粹的内存操作（非常快速）
>
>2、采用单线程,避免了不必要的上下文切换和竞争条件
>
>
>
>### 2、热点key
>
>热点key的处理办法：
>
>* 对热点key进行分散；
>* 加载到内存中。
>
>### 3、redis的部署？
>
>### 4、redis项目中使用？
>
>* 本地映射客户的交易流水号入库，（通过redis生成唯一流水号，）

![image-20210706143252891](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/image-20210706143252891.png)

### 7、rabbitmq

>### 消费的时候如何保证消息的顺序？
>
>生产上没有保证消费时候
>
>

### 8、MySQL性能优化？

[参考](https://blog.csdn.net/qq_39390545/article/details/107020686?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522162555018316780366528301%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=162555018316780366528301&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~hot_rank-2-107020686.pc_v2_rank_blog_default&utm_term=MySQL%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96&spm=1018.2226.3001.4450)

>### 1、性能优化？(最大化使用索引，尽可能避免全表扫描，减少无效数据的查询)
>
>* 表设计：
>
>  * 在表中建立索引，优先考虑where、order by使用到的字段
>  * 给字段以初始值，不设为null；
>  * 固定长度使用char的长度；
>  * 尽量使用数字型字段（如性别，男：1  女：0）使用0、1去存；
>  * 
>
>* 尽可能使用到索引，对索引进行优化
>
>  * **尽量避免在字段开头模糊查询，会导致数据库引擎放弃索引进行全表扫描**
>  * **尽量避免使用in 和not in，会导致引擎走全表扫描**
>  * **尽量避免使用 or，会导致数据库引擎放弃索引进行全表扫描**
>  * **尽量避免进行null值的判断，会导致数据库引擎放弃索引进行全表扫描**（一般给null一个固定值）
>  * **尽量避免在where条件中等号的左侧进行表达式、函数操作，会导致数据库引擎放弃索引进行全表扫描。**
>  * **当数据量大时，避免使用where 1=1的条件。通常为了方便拼装查询条件，我们会默认使用该条件，数据库引擎会放弃索引进行全表扫描。**
>  * **查询条件不能用 <> 或者 !=**
>  * **避免where条件仅包含复合索引非前置列**
>  *  **隐式类型转换造成不使用索引** 
>  * 
>
>* 优化更需要优化的Query
>
>  * 避免出现select \*，只取出自己需要的字段
>
>  * 多表关联查询时，小表在前，大表在后。
>
>  * 避免出现不确定结果的函数
>
>  * 使用表的别名
>
>  * 调整Where字句中的连接顺序
>
>  * 当只要一行的时候使用limit 1 ；在做分页查询的时候，首先使用count（1）去做判断，以其值为条件看是否还进行数据的查询，减少查询的次数；
>
>    
>
>减少数据访问： 设置合理的字段类型，启用压缩，通过索引访问等减少磁盘IO
>返回更少的数据： 只返回需要的字段和数据分页处理 减少磁盘io及网络io
>减少交互次数： 批量DML操作，函数存储等减少数据连接次数
>减少服务器CPU开销： 尽量减少数据库排序操作以及全表查询，减少cpu 内存占用
>利用更多资源： 使用表分区，可以增加并行操作，更大限度利用cpu资源
>
>
>
>### 2、最大数据量？
>
>### 3、分库分表？
>
>### 4、你对建表字段是否该使用not null这个问题怎么看?
>
>* NULL列在行中需要额外的空间以记录其值是否为NULL。 对于MyISAM表，每个NULL列都多花一位，四舍五入到最接近的字节。
>
>* MySQL 中 sum 函数没统计到任何记录时，会返回 null 而不是 0，可以使用 IFNULL(null,0) 函数把 null 转换为 0；
>
>* 在MySQL中使用count（字段），不会统计 null 值，COUNT(*) 才能统计所有行；
>
>* MySQL 中使用诸如 =、<、> 这样的算数比较操作符比较 NULL 的结果总是 NULL，这种比较就显得没有任何意义，需要使用 IS NULL、IS NOT NULL 或 ISNULL() 函数来比较
>
>  

### 9、MySQL中锁相关？

>/docs/database/mysql/MySQL锁相关.md

### 10、spring事务失效原因(transactional注解失效从AOP讲解)

>https://zhuanlan.zhihu.com/p/145669970
>
>

### 11、 CPU 密集型 和 IO密集型 的区别，如何确定线程池大小？

>* CPU密集型也叫计算密集型： **cpu 密集型（CPU-bound）线程池设计  最佳线程数=cpu核数或者cpu核数±1**
>* IO密集型指的是系统的CPU性能相对硬盘、内存要好很多： I/O密集型（I/O-bound）线程池设计 **最佳线程数 = ((线程等待时间+线程cpu时间)/线程cpu时间\*cpu数目)**
>
>

### 12、拦截器和过滤器

### 13、java并发包





# 阿里

###  1、项目

>### 对账如何做？
>
>### 钱的流转？
>
>### 如何保证一个账户扣钱另一个账户？（事务）
>
>

###  2、TCP协议：

>### 三次握手和四次挥手？
>
>#### 三次握手？
>
>**第一次握手**：建立连接时，客户端发送 syn 包(syn=j)到服务器，并进入 SYN_SEND 状态，等待服务器确认； 
>
>**第二次握手**：服务器收到 syn 包，必须确认客户的 SYN（ack=j+1），同时自己也发送一个 SYN 包（syn=k），即 SYN+ACK 包，此时服务器进入 SYN_RECV 状态； 
>
>**第三次握手**：客户端收到服务器的 SYN＋ACK 包，向服务器发送确认包 ACK(ack=k+1)，此包发送完毕，客户端和服务器进入 ESTABLISHED 状态，完成三次握手。
>
>![img](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/20150511164615901.jpeg)
>
>#### 四次挥手？
>
>那TCP 的四次握手，**是为了保证通信双方都关闭了连接**，具体过程如下：
>
>![img](a%E4%B8%AA%E4%BA%BA%E9%9D%A2%E8%AF%95%E6%80%BB%E7%BB%93.assets/20150603155803953.png)
>
>1）客户端 A 发送一个 FIN，用来关闭客户 A 到服务器 B 的数据传送；
>2）服务器 B 收到这个 FIN，它发回一个 ACK，确认序号为收到的序号加 1。和 SYN 一样，一个 FIN 将占用一个序号；
>3）服务器 B 关闭与客户端 A 的连接，发送一个 FIN 给客户端 A；
>4）客户端 A 发回 ACK 报文确认，并将确认序号设置为收到序号加 1。

### 3、事务的隔离级别？

>### 四种隔离级别：
>
>Read Uncommitted（读未提交内容）-->  Read Committed（读已提交）--> Repeatable Read（可重复读）--> Serializable（串行化）
>
>可重复读讲的不行。
>
>

### 4、MySQL的优化？

>
>
>

##### 5、优化过程？

>explain看优化过程。
>
>主要关注哪些指标？
>
>
>
>



### 6、MySQL的最左原则？

>a,b,c 
>
>a,c 能用到索引吗？
>
>

### 7、B树和B+树的对比？



### 8、redis中间件？

>### 1、Redis缓存和数据库的数据不一致问题？
>
>我的回答：先删除缓存再插库。
>
>但是如果缓存失败的话，这笔交易就拒绝吗？
>
>### 2、redis为什么是单线程？
>
>### 3、redis的哨兵模式？
>
>### 4、单机挂了，哨兵怎么处理？
>
>* 
>
>

### 9、集合

>### HashMap的数据结构及jdk的优化？（1.8比之前的优化）
>
>数据结构
>
>扩容
>
>
>
>

### 10、有用过java的并发包？

>
>
>

### 11、线程池？

>线程流转。
>
>

### 12、zookeeper

>选举机制？
>
>

### 13、介绍

>飞猪--》业务平台--》订单与结算
>
>订单列表
>
>结算：履约流程、订单资金操作、资金记账（银行履历）
>
>



# 14、国美在线







----

# Other

## 阿里（菜鸟）



2、快排、堆排和归并都是O(nlong n)的算法，为什么jdk会选择快速排序？

3、为何jdk中快速排序在元素个数小于30时会采用插入排序？

4、HashMap是线程安全的吗？如果不安全可能造成什么问题？

5、HashTable是线程安全的吗？怎么实现的？

6、常用并发包下的类？

7、redis持久化方式？

8、redis为什么这么快？

9、介绍自己比较熟悉的项目和项目中遇到的难点？

## 二面

1、JVM类加载机制分别每一步做了什么工作？

2、JVM运行时数据区包括哪些部分？

垃圾收集有哪些算法？各自的特点？

如何确定被清除的对象？

3、JVM中垃圾回收器有哪些？区别是什么？

4、MySQL索引类型和区别？聚簇索引和非聚簇索引的区别？

5、事务的隔离级别？

6、springBean创建过程中用到了哪些设计模式？

7、谈谈IOC和AOP

8、概述下spring中Bean的生命周期

9、从源码的角度，描述下springmvc的这个访问或调用流程。

10、谈谈线程池的参数列表和拒绝策略

11、请概述AQS

## 三面

13、请概述下jdk8中的ConcurrentHashMap？

14、为什么kafka这么快？

15、什么是零拷贝？

16、假设有16G的可用内，有个72G的文件（文本文件，每一行都是一个独立的字符串）请读取文件里的内容，必将排序后的结果保存在另一个文件中？

17、http和https协议的区别？https具体原理？

18、项目内存或者CPU占用率过高如何排查？

### 19、MySQL中死锁是怎么回事？怎么解决或避免？

20、文档中的单词查找功能你会如何实现？

21、请设计一个高可用，高伸缩的缓存系统？

22、谈谈zk的投票机制？

23、dubbo协议为什么采用异步单一长链接？

dubbo服务暴露的过程？



24、对于dubbo和springcloud给出二者的优缺点，描述服务降级与服务熔断，二者之间的区别以及使用场合？

zk分布式锁的几种实现方式？各自优缺点？

如何实现分布式锁的？

如何保证redis和MySQL的数据一致性？

>

如何实现并发限流？



## 水滴筹，1-3年

### 介绍一下你现在做的项目？

### 给我画一下你现在做的项目的业务架构图吧？

### 说一下OpenFegin的调用过程？

### 能给我说一下SpringBoot和SpringMVC的区别吗？

>spring boot只是一个配置工具,整合工具,辅助工具.
>
>springmvc是框架,项目中实际运行的代码
>
>**Spring 框架**就像一个家族，有众多衍生产品例如 boot、security、jpa等等。但他们的基础都是Spring 的ioc和 aop，ioc 提供了依赖注入的容器， aop解决了面向横切面的编程，然后在此两者的基础上实现了其他延伸产品的高级功能。
>
>**Spring MVC提供了一种轻度耦合的方式来开发web应用。**它是Spring的一个模块，是一个web框架。通过Dispatcher Servlet, ModelAndView 和 View Resolver，开发web应用变得很容易。解决的问题领域是网站应用程序或者服务开发——URL路由、Session、模板引擎、静态Web资源等等。
>
>**Spring Boot实现了自动配置，降低了项目搭建的复杂度**。它主要是为了解决使用Spring框架需要进行大量的配置太麻烦的问题，所以它并不是用来替代Spring的解决方案，而是和Spring框架紧密结合用于提升Spring开发者体验的工具。同时它集成了大量常用的第三方库配置(例如Jackson, JDBC, Mongo, Redis, Mail等等)，Spring Boot应用中这些第三方库几乎可以零配置的开箱即用(out-of-the-box)。
>
>Spring Boot只是承载者，辅助你简化项目搭建过程的。如果承载的是WEB项目，使用Spring MVC作为MVC框架，那么工作流程和你上面描述的是完全一样的，因为这部分工作是Spring MVC做的而不是Spring Boot。
>
>对使用者来说，换用Spring Boot以后，项目初始化方法变了，配置文件变了，另外就是不需要单独安装Tomcat这类容器服务器了，maven打出jar包直接跑起来就是个网站，但你最核心的业务逻辑实现与业务流程实现没有任何变化。
>
>
>
>**总结：**Spring 最初利用“工厂模式”（DI）和“代理模式”（AOP）解耦应用组件。大家觉得挺好用，于是按照这种模式搞了一个 MVC框架（一些用Spring 解耦的组件），用开发 web 应用（ SpringMVC ）。然后发现每次开发都写很多样板代码，为了简化工作流程，于是开发出了一些“懒人整合包”（starter），这套就是 Spring Boot。
>
>
>
>所以，用最简练的语言概括就是：
>
>Spring 是一个“引擎”；
>
>Spring MVC 是基于Spring的一个 MVC 框架；
>
>Spring Boot 是基于Spring4的条件注册的一套快速开发整合包。
>
>

### 说一下HashMap的原理吧？

### 给我简单画一下它的put流程？

### HashMap在什么时候会树化，什么时候会反树化，说一下？

### 说一下ConcorrentHashMap是怎么保证线程安全的？

### 你在项目中用过多线程开发吗？详细介绍一下？

###  说一下JVM的内存分区？

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

​	



## 京东

### 1.自我介绍

### 2.介绍项目，针对项目问一下具体业务

###  3.mysql索引类型

>1.普通索引
>2.唯一索引
>3.主键索引
>4.组合索引
>5.全文索引

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

###  14.手写RPC框架怎么实现，应该注意什么？



#  Other面试

### 1、redis 一致性hash？

### 2、hashmap的扩容机制

### 3、线程池的配置参数和拒绝策略

###  4、线程sleep和wait的区别

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

### 20 、如果有一个cookie，每一个线程都带着，一个cookie内容大小是30字节，现在有300个线程，本地磁盘有一个黑名单文件，黑名单中存在500万个cookie，每一个cookie占一行，现在要求在毫秒级的响应中找到黑名单的cookie



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
