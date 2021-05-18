# 面试题

## 一、ByMySelf面试题：
### ①理想汽车
####  String str = new String("abc")创建了几个对象？
* 首先，不能直接说创建几个对象，需要分情况去分析：先说结论：一个或两个。
* 如果字符串 abc 之前没有用过，毫无疑问会创建2个对象，一个是new String创建的对象，一个是abc对象的内容创建的对象。
* 如果字符串abc之前用过，那么就值创建一个对象，abc取至缓存池。
#### Spring说下？

## 二、Java基础

### 1.说说何为进程何为线程？
* 进程是系统分配资源的最小单位，进程间是相互独立的。一个程序至少有一个进程，一个进程至少有一个线程。
* 线程是程序执行的最小单元，一个进程可能会有多个线程，各个线程共享进程的资源。
* 线程的开销小，但是不利于资源的管理和维护。进程则是相反的。
### 2.String为什么不能修改?
 * String内容不能改变的理解 String的值不能修改的理解
java中，String类的值声明后是不能修改，有些初学者就难以理解，为什么下面程序明明是修改了String对象的内容，为什么还可以正常运行？
```java
public class JavaTest {
    public static void main(String[] args) {
        String str = "Hello";
        str = str + " World";
        System.out.println("str=" + str);
    }
}
```
> 运行结果为：
> str=Hello World

>【解释】 String类是final类故不可以继承，也就意味着String引用的字符串内容是不能被修改。
 要注意的是：String类对象内容不能修改，但并不代表其引用不能改变，下面通过内存的分配图说明字符串不可改变的真正含义：
可知，String对象内容的改变实际上是通过内存地址“断开-连接”变化来完成的，而原字符串中的内容并没有任何的改变。String str = "Hello";和str = str + " World";实质上是开辟了三个内存空间，str只是由原来指向"hello"变为指向“hello world”而已，而其原来的指向内容，是没有改变的。
 因此，在以后的开发中，若要经常修改字符串的内容，请尽量少用String，因为字符串的指向“断开-连接”会大大降低性能；对于要经常修改内容的情况，建议使用：StringBuilder、StringBuffer

### ThreadLocal归纳下来就2类用途：
* 保存线程上下文信息，在任意需要的地方可以获取！！！
* 线程安全的，避免某些情况需要考虑线程安全必须同步带来的性能损失！！！
* ThreadLocal用于创建线程的本地变量，我们知道一个对象的所有线程会共享它的全局变量，所以这些变量不是线程安全的，我们可以使用同步技术。但是当我们不想使用同步的时候，我们可以选择ThreadLocal变量。
* 每个线程都会拥有他们自己的Thread变量，它们可以使用get()\set()方法去获取他们的默认值或者在线程内部改变他们的值。ThreadLocal实例通常是希望它们同线程状态关联起来是private static属性
ThreadLocal 是线程的局部变量， 是每一个线程所单独持有的，其他线程不能对其进行访问。
当使用ThreadLocal维护变量的时候 为每一个使用该变量的线程提供一个独立的变量副本，即每个线程内部都会有一个该变量，这样同时多个线程访问该变量并不会彼此相互影响，因此他们使用的都是自己从内存中拷贝过来的变量的副本， 这样就不存在线程安全问题，也不会影响程序的执行性能。

但是要注意，虽然ThreadLocal能够解决上面说的问题，但是由于在每个线程中都创建了副本，所以要考虑它对资源的消耗，比如内存的占用会比不使用ThreadLocal要大。

### 3.equals和hashcode的区别
* 若重写了equals(Object obj)方法，则有必要重写hashCode()方法。
* 若两个对象equals(Object obj)返回true，则hashCode（）有必要也返回相同的int数。
* 若两个对象equals(Object obj)返回false，则hashCode（）不一定返回不同的int数。
* 若两个对象hashCode（）返回相同int数，则equals（Object obj）不一定返回true。
* 若两个对象hashCode（）返回不同int数，则equals（Object obj）一定返回false。
* 同一对象在执行期间若已经存储在集合中，则不能修改影响hashCode值的相关信息，否则会导致内存泄露问题。
### 4.synchronized和volatile关键字的区别？
synchronized关键字和volatile关键字不是对立的是互补的。
* volatile关键字是线程同步的轻量级实现，所以volatile的性能肯定比synchronized要好。但是volatile只能用于修饰变量；synchronized关键字可以修饰方法和代码块。
* volatile关键字能保证数据的可见性，但不保证原子性。synchronized两者都能保证。
* volatile关键字主要用于解决变量在多个线程之间的可见性，而synchronized关键字解决的是多线程之间访问资源的同步性。

### [synchronized与lock的区别？](https://blog.csdn.net/weixin_39559282/article/details/114273936)
### synchronized的锁升级过程。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210411092711717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210411092807433.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210411092825947.png)


## 二、网络编程
### 1.socket 用的什么协议? 
TCP协议.
​​​​![网络协议](https://img-blog.csdnimg.cn/20210331112214220.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70)
应用层：TFTP，`HTTP`，SNMP，FTP，SMTP，DNS，Telnet 等等
传输层：TCP，UDP
网络层：IP，ICMP，OSPF，EIGRP，IGMP
数据链路层：SLIP，CSLIP，PPP，MTU

### HTTP 和 HTTPS 区别？
1、HTTPS  协议需要到 CA （Certificate Authority，证书颁发机构）申请证书，一般免费证书较少，因而需要一定费用。(以前的网易官网是http，而网易邮箱是 https 。)
2、HTTP 是超文本传输协议，信息是明文传输，HTTPS 则是具有安全性的 SSL 加密传输协议。
3、HTTP 和 HTTPS 使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。
4、HTTP 的连接很简单，是无状态的。HTTPS 协议是由 SSL+HTTP 协议构建的可进行加密传输、身份认证的网络协议，比 HTTP 协议安全。(无状态的意思是其数据包的发送、传输和接收都是相互独立的。无连接的意思是指通信双方都不长久的维持对方的任何信息。)

## 三、JVM
### 垃圾回收算法
#### 标记-清除？
* 分为标记和清除两步骤，先将有用的对象进行标记，标记完成后，将未标记的对象全部清除。效率低（标记的过程中需要遍历所有的GC ROOT，清除的过程中也是要遍历堆中的所有对象）；同时还会带来大量的内存碎片问题。
#### 复制算法
* 复制算法产生是为了解决效率问题。它将内存分为大小相同的两份区域，每次使用其中的一块儿，当内存使用完后，将还存活的对象复制到另一块儿内存中，然后将当前内存清空。虽然解决了效率问题，但是内存空间只有一般使用到，空间利用率低。
* 但是在当出现年轻代和老年代的内存结构后：Eden+S0+S1，根据研究98%的对象都是朝生夕死，所以实际能存活的对象不多，完全不用浪费一半的内存。默认比例为>  8：1：1。
* 最终Survivor放不下的对象进入老年代。（年龄在15、大对象也会直接到老年代。）
#### 标记整理
* 针对老年代的垃圾回收，进入老年代的对象的存活率较高，这种频繁的复制对性能影响较大。因此常采用标记整理的算法。
* 标记整理：标记处存活的对象，然后将所有存活的对象都向一端移动，然后清理掉边界以外的对象。
### GC ROOT？
* 标记算法中，如何标记一个对象是否存活？简单通过引入计数法，给对象设置一个引用计数器，每当有引用，计数器就会+1，反之计数器-1，但是无法解决循环引用的问题。
* Java通过可达性分析算法来达到标记存活对象的目的，定义一系列的GC ROOT为起点，从起点向下搜索，搜索走过的路径为引用链，当一个对象没有任何一个引用链指向的话就可以判定此对象可以被回收。
	* 而可以作为GC ROOT的对象包括：（没回答出来）
		* 栈中的引用对象
		* 静态变量、常量引用的对象
		* 本地方法栈native方法引用的对象

### 简单参数含义：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210405194413201.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210405194451828.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70)


### 内存泄漏与内存溢出的区别
* 内存泄漏:对象无法得到及时的回收，持续占用内存空间，从而造成内存空间的浪费。
* 内存溢出:内存泄漏到一定的程度就会导致内存溢出，但是内存溢出也有可能是大对象导致的。
### 方法区中主要回收的无用的类，如何判断一个类是无用的类？
* 该类的所有实例都已经被回收，也就是Java堆中不存在该类的任何实例；
* 加载该类的ClassLoader已经被回收；
* 该类对应的java.lang.Class对象没有被任何地方引用，无法在任何地方通过反射访问该类的方法。

## 四、框架
### spring
* aop ,ioc 使用的java中的什么技术?
a:aop :

## 五、MySQL
### 最左前缀原则：（最左包含原则）
* 1.最左前缀原则（左包含原则）
* 2.MySQL引擎在查询为了更好的利用索引，在查询过程中会动态调整查询字段的顺序以便利用索引。
例如：
```sql
create table test(
id int primary key auto_increment,
name varchar(32) not null,
age int(3) not null default 0,
addr varchar(64),
key(name,age,addr)
);
insert into test(name,age,addr)values("张三",33,"北京西城"),("李四",44,"北京东城"); 
show index from test;
explain select * from test where name ="张三" and  age=33 and addr ="北京西城"; #用到
explain select * from test where name ="张三"; #用到
explain select * from test where name ="张三" and addr ="北京西城"; #用到
explain select * from test where age=33 and addr ="北京西城" and name ="张三"; #用到
explain select * from test where age=33 and addr ="北京西城"; #未用到
```
## 六、Redis















# 面试必问
## 一、JVM
### ①JVM
#### 1、运行时数据区：
![在这里插入图片描述](Java%E9%9D%A2%E8%AF%95%E6%95%B4%E7%90%86--%E4%B8%AA%E4%BA%BA.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70.png)
* **方法区**：各线程共享的内存区域，在虚拟机启动时创建，用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。 
	* 在jdk8 中就是metaspace（与数据区） ，jdk6或7就是perm space
	* class文件中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息就是常量池，用于存放编译时期生成的各种字面量和符号引用，这部分将在类加载后进入方法区的运行时常量池中。
*  **堆 heap** ：堆是Java虚拟机所管理内存中最大的一块儿，在虚拟机启动时创建，被所有线程共享。对象的实例及数组都在堆上分配。
* **虚拟机栈Virtual Machine Stacks：** 虚拟机栈是一个线程执行的区域，保存着一个线程中方法的调用状态。换句话说：一个Java线程的运行状态，由一个虚拟机栈来保存，所有的虚拟机栈都是线程私有的，独立的，随线程创建而创建。每一个方法都是一个栈帧，调用方法压栈，方法执行完成，弹栈。
* **程序计数器（The PC Register）：**程序计数器占用内存空间很小，由于Java虚拟机的多线程轮流切换，通过分配执行时间来实现线程间的切换，在任意时刻一个处理器只能处理一个线程的一条指令。因此，为了线程切换后能够恢复到正确的执行位置，每条线程需要一个独立的程序计数器（线程私有。）
* **本地方法栈（Native Method Stacks）** 如果当前线执行的方法是native类型的，这些方法会在本地方法栈中执行。

#### 2、Jvm类加载器？
![在这里插入图片描述](Java%E9%9D%A2%E8%AF%95%E6%95%B4%E7%90%86--%E4%B8%AA%E4%BA%BA.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70-20210510134956144.png)
* 1)Bootstrap ClassLoader 负责加载$JAVA_HOME中 jre/lib/`rt.jar` 里所有的class或 Xbootclassoath选项指定的jar包。由C++实现，不是ClassLoader子类。
* 2)Extension ClassLoader 负责加载java平台中扩展功能的一些jar包，包括$JAVA_HOME中 `jre/lib/*.jar` 或 -Djava.ext.dirs指定目录下的jar包。
* 3)App ClassLoader 负责加载classpath中指定的jar包及 Djava.class.path 所指定目录下的类和 jar包。
* 4)Custom ClassLoader 通过java.lang.ClassLoader的子类自定义加载class，属于应用程序根据 自身需要自定义的ClassLoader，如tomcat、jboss都会根据j2ee规范自行实现ClassLoader。
##### 加载原则：
* 检查某个类是否已经加载：顺序是自底向上，从Custom ClassLoader 到BootStrap ClassLoader 逐层检查，只要某个ClassLoader已经加载，就是为已加载此类，保证此类只加载一次。
* 加载顺序：自定向下，就是由上层逐层尝试加载此类。
#### 3、双亲委派机制
* **定义**：如果一个类加载器在接到加载类的请求时，它首先不会自己尝试去加载这个类，而是把这个请求任务委托给父类加载器去完成，依次递归，如果父类加载器可以完成加载任务，就成功返回。只有父类加载器无法完成加载任务时才会自己去加载。
* **优势**：Java类随着加载它的类加载器一起具备一种带有优先级的层级关系。举例如：`Java中的Object类`，他存放在`rt.jar`之中，无论哪个一个类加载器要加载这个类，最终都是委派给处于模型最顶端的启动类加载器进行加载，因此Object在各种类加载环境中都是同一个类。如果不采用双亲委派模型，那么由各个类加载器自己去加载的话，那么系统中就会有多种不同的Object类。
#### 4、Java对象内存布局
![在这里插入图片描述](Java%E9%9D%A2%E8%AF%95%E6%95%B4%E7%90%86--%E4%B8%AA%E4%BA%BA.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70-20210510134956192.png)
* 一个对象在内存中包括3部分：对象头、实例数据、对其填充。
#### 5、JVM内存模型
#### 图：
* 一块是非堆区，一块是堆区。
	* 堆区分为两大块：一块是Old区，一块儿是Young区。
	* Young区分为两大块：一个是Survivor区（S0+S1），一块是Eden区。比例为：Eden：S0：S1=8：1：1 （S0和S1一样大，也可以加From和To）
![在这里插入图片描述](Java%E9%9D%A2%E8%AF%95%E6%95%B4%E7%90%86--%E4%B8%AA%E4%BA%BA.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70-20210510134955993.png)
#### 对象创建所在的区域：
一般情况下，新创建的对象都会被分配到Eden区，一些特殊的大对象会直接分配到Old区。
#### 对象创建所在的区域
* 一般情况下，新创建的对象会被分配到Eden区，一些特殊的大对象会被直接分配到Old区。

### ②、垃圾算法
#### 为什么需要分代回收？
#### 如何判断一个对象是否需要被垃圾回收？
一般有两种方法来判断：
* 引用计数器法：为每个对象创建一个引用计数，有对象引用时计数器 +1，引用被释放时计数 -1，当计数器为 0 时就可以被回收。它有一个缺点不能解决循环引用的问题；
* 可达性分析算法：从 GC Roots 开始向下搜索，搜索所走过的路径称为引用链。当一个对象到 GC Roots 没有任何引用链相连时，则证明此对象是可以被回收的。
	* 作为GCRoots根的有：
#### 深拷贝和浅拷贝
* 浅拷贝（shallowCopy）只是增加了一个指针指向已存在的内存地址。
* 深拷贝（deepCopy）是增加了一个指针并且申请了一个新的内存，使这个增加的指针指向这个新的内存。
### JMM内存模型：
![在这里插入图片描述](Java%E9%9D%A2%E8%AF%95%E6%95%B4%E7%90%86--%E4%B8%AA%E4%BA%BA.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70-20210510134956142.png)
![在这里插入图片描述](Java%E9%9D%A2%E8%AF%95%E6%95%B4%E7%90%86--%E4%B8%AA%E4%BA%BA.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70-20210510134956021.png)


## 线程、线程池
### 线程与进程：
* 首先进程是CPU分配资源的最小单元，一个程序至少有一个进程，一个进程至少有一个线程，线程执行任务的最小单元；进程之间是独立的，但是线程之间是会共享同一个进程的资源。进程便于管理，但线程则相反。

### 线程池
![线程池](Java%E9%9D%A2%E8%AF%95%E6%95%B4%E7%90%86--%E4%B8%AA%E4%BA%BA.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70-20210510134955989.png)
### 线程池的处理流程：
![在这里插入图片描述](Java%E9%9D%A2%E8%AF%95%E6%95%B4%E7%90%86--%E4%B8%AA%E4%BA%BA.assets/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70-20210510134956114.png)




#### 线程池的拒绝策略：
* ThreadPoolExecutor.`AbortPolicy`:丢弃任务并抛出RejectedExecutionException异常。 
*  ThreadPoolExecutor.`DiscardPolicy`：丢弃任务，但是不抛出异常。 
* ThreadPoolExecutor.`DiscardOldestPolicy`：丢弃队列最前面的任务，然后重新提交被拒绝的任务 
* ThreadPoolExecutor.`CallerRunsPolicy`：由调用线程（提交任务的线程）处理该任务
####  线程池不允许使用Executors去创建，而是通过ThreadPoolExecutor的方式。
“线程池不允许使用Executors去创建，而是通过ThreadPoolExecutor的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险”
* FixedThreadPool和SingleThreadExecutor => 允许的请求队列长度为Integer.MAX_VALUE，可能会堆积大量的请求，从而引起OOM异常
* CachedThreadPool => 允许创建的线程数为Integer.MAX_VALUE，可能会创建大量的线程，从而引起OOM异常
## Java的集合框架体系
### ArrayList
### HashMap、ConcurrentHashMap
#### 哈希冲突的解决方法
## Spring
## 锁相关
### synchronized
### 线程池
### ThreadLocal
### 锁相关
### CAS
## MySQL数据库
### 1、讲一讲聚簇索引与非聚簇索引？
* 在 InnoDB 里，索引B+ Tree的叶子节点存储了整行数据的是主键索引，也被称之为聚簇索引，即将数据存储与索引放到了一块，找到索引也就找到了数据。
* 而索引B+ Tree的叶子节点存储了主键的值的是非主键索引，也被称之为非聚簇索引、二级索引。
* 聚簇索引与非聚簇索引的区别：
* 非聚集索引与聚集索引的区别在于非聚集索引的叶子节点不存储表中的数据，而是存储该列对应的主键（行号）
* 对于InnoDB来说，想要查找数据我们还需要根据主键再去聚集索引中进行查找，这个再根据聚集索引查找数据的过程，我们称为回表。第一次索引一般是顺序IO，回表的操作属于随机IO。需要回表的次数越多，即随机IO次数越多，我们就越倾向于使用全表扫描 。
* 通常情况下， 主键索引（聚簇索引）查询只会查一次，而非主键索引（非聚簇索引）需要回表查询多次。当然，如果是覆盖索引的话，查一次即可
* 注意：MyISAM无论主键索引还是二级索引都是非聚簇索引，而InnoDB的主键索引是聚簇索引，二级索引是非聚簇索引。我们自己建的索引基本都是非聚簇索引。
### 数据库三大范式
### 最左前缀法则
### 覆盖索引
## Redis
#### 1、redis中的过期策略
#### 1）Redis项目中用来做什么 
#### 2） Redis分布式锁怎么实现的
#### 3）Redisson的看门狗机制
#### 4）Redis底层数据结构
#### 5）Redis持久化机制
#### 3.秒杀是怎么做的
#### 1）怎么测试秒杀
#### 2）Redis怎么库存预热
## 网络编程
### TCP UDP 
### 1、url输入到浏览器，详细过程讲一下
### RabbitMQ
### Dubbo
#### 1、Dubbo是什么？
* Dubbo是阿里巴巴开源的基于 Java 的高性能 RPC 分布式服务框架，现已成为 Apache 基金会孵化项目。

###  Zookeeper

# 未完待续~
