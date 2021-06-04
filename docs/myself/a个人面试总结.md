## 一、跟谁学：

### 1、主观问题：见有道云笔记

### 2、集合你用过哪些 Hashmap的数据结构 ConcurrentHashMap怎么实现线程安全的

### 3、ThreadLocal如何用的？

* 用于线程间数据的隔离，填充的数据只属于当前线程，变量的数据对别的线程而言相对隔离，

### 4、

### 5、

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

### 循环注入分两种情况：1、构造方法注入 2、set方法注入（单例和多例有区别）

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

####  String str = new String("abc")创建了几个对象？

* 首先，不能直接说创建几个对象，需要分情况去分析：先说结论：一个或两个。
* 如果字符串 abc 之前没有用过，毫无疑问会创建2个对象，一个是new String创建的对象，一个是abc对象的内容创建的对象。
* 如果字符串abc之前用过，那么就值创建一个对象，abc取至缓存池。

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

### 5、

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

### 2、redis数据结构？

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

* 几种结点？临时节点和持久结点之间的区别？

### 4、用户突然反馈服务慢了该如何排查？（CPU、内存、磁盘、I/O、网络带宽）

登录后台服务器或者监控平台，查看系统资源是否达到上限：CPU、内存、磁盘、I/O、网络带宽

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





