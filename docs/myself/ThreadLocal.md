ThreadLocal：

>典型的用法：spring事务中使用到事务， 
>

>key 是ThreadLocal对象。
>
>一个ThreadLocal只能装一个值，因为ThreadLocal只是一个map的key，因此想要装多个值的话就需要多个ThreadLocal对象。

![image-20210710233825909](ThreadLocal.assets/image-20210710233825909.png)

>ThreadLocal：首先我们会声明一个ThreadLocal
>
>我们往里面set值时候，实际上指向了当前线程所拥有的一个ThreadLocalMap，
>
>ThreadLocalMap中是一个Entry，Entry调用了WeakReference的构造方法，构造方法中传入的是一个ThreadLocal对象，`Entry extends WeakReference<ThreadLocal<?>>`，弱引用key。
>
>

```java

ThreadLocal<Integer> th =new ThreadLocal<>();
		th.set(1);
		
		
/**
     * Sets the current thread's copy of this thread-local variable
     * to the specified value.  Most subclasses will have no need to
     * override this method, relying solely on the {@link #initialValue}
     * method to set the values of thread-locals.
     *
     * @param value the value to be stored in the current thread's copy of
     *        this thread-local.
     */
    public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);// 首先拿到当前线程的map
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
    }
    
    
    /**
     * Get the map associated with a ThreadLocal. Overridden in
     * InheritableThreadLocal.
     *
     * @param  t the current thread
     * @return the map
     */
    ThreadLocalMap getMap(Thread t) {
        return t.threadLocals;
    }

// Thread类下的

/* ThreadLocal values pertaining to this thread. This map is maintained
     * by the ThreadLocal class. */
    ThreadLocal.ThreadLocalMap threadLocals = null;


 					/**
         * Set the value associated with key.
         *
         * @param key the thread local object
         * @param value the value to be set
         */
        private void set(ThreadLocal<?> key, Object value) {

            // We don't use a fast path as with get() because it is at
            // least as common to use set() to create new entries as
            // it is to replace existing ones, in which case, a fast
            // path would fail more often than not.

            Entry[] tab = table;
            int len = tab.length;
            int i = key.threadLocalHashCode & (len-1);

            for (Entry e = tab[i];
                 e != null;
                 e = tab[i = nextIndex(i, len)]) {
                ThreadLocal<?> k = e.get();

                if (k == key) {
                    e.value = value;
                    return;
                }

                if (k == null) {
                    replaceStaleEntry(key, value, i);
                    return;
                }
            }

            tab[i] = new Entry(key, value);
            int sz = ++size;
            if (!cleanSomeSlots(i, sz) && sz >= threshold)
                rehash();
        }

最终生成的是个Entry，点进去发现，其父类是个弱引用。
  弱引用，其实指的就是ThreadLocal对象。（WeakReference we =new WeakReference(new ThreadLocal());） 弱引用指的就是WeakReference里面的对象。
 					/**
         * The entries in this hash map extend WeakReference, using
         * its main ref field as the key (which is always a
         * ThreadLocal object).  Note that null keys (i.e. entry.get()
         * == null) mean that the key is no longer referenced, so the
         * entry can be expunged from table.  Such entries are referred to
         * as "stale entries" in the code that follows.
         */
        static class Entry extends WeakReference<ThreadLocal<?>> {
            /** The value associated with this ThreadLocal. */
            Object value;

            Entry(ThreadLocal<?> k, Object v) {
                super(k);
                value = v;
            }
        }


```



# [分析ThreadLocal的弱引用与内存泄漏问题-Java8](https://www.cnblogs.com/-beyond/p/13125195.html)

## 一.介绍

　　之前使用ThreadLocal的时候，就听过ThreadLocal怎么怎么的可能会出现内存泄漏，具体原因也没去深究，就是一种不清不楚的状态。最近在看JDK的源码，其中就包含ThreadLocal，在对ThreadLocal的使用场景介绍以及源码的分析后，对于ThreadLocal中可能存在的内存泄漏问题也搞清楚了，所以这里专门写一篇博客分析一下，对于ThreadLocal源码分析，可以参考我另一篇博客：[ThreadLocal源码解析-Java8](https://www.cnblogs.com/-beyond/p/13093032.html)。

在分析内存泄漏之前，先了解2个概念，就是内存泄漏和内存溢出：

* 内存溢出（memory overflow）：是指不能申请到足够的内存进行使用，就会发生内存溢出，比如出现的OOM（Out Of Memory）

* 内存泄漏（memory lack）：内存泄露是指在程序中已经动态分配的堆内存由于某种原因未释放或者无法释放（已经没有用处了，但是没有释放），造成系统内存的浪费，这种现象叫“内存泄露”。

　　当内存泄露到达一定规模后，造成系统能申请的内存较少，甚至无法申请内存，最终导致内存溢出，所以内存泄露是导致内存溢出的一个原因。

## 二.问题提出

### 2.1内存原理图

　　下图是程序运行中的内存分布图，简要介绍一下这种图：当前线程有一个threadLocals属性（ThreadLocalMap属性），该map的底层是数组，每个数组元素时Entry类型，Entry类型的key是ThreadLocal类型（也就是创建的ThreadLocal对象），而value是则是ThreadLocal.set()方法设置的value。

　　![img](ThreadLocal.assets/848880-20200614162022266-1246715280.png)

　　需要注意的是ThreadLocalMap的Entry，继承自弱引用，定义如下，关于Java的引用介绍，可以参考：[Java-强引用、软引用、弱引用、虚引用](https://www.cnblogs.com/-beyond/p/10881060.html)

```
/**
 * ThreadLocalMap中存放的元素类型，继承了弱引用类
 */
static class Entry extends WeakReference<ThreadLocal<?>> {
    // key对应的value，注意key是ThreadLocal类型
    Object value;
 
    Entry(ThreadLocal<?> k, Object v) {
        super(k);
        value = v;
    }
}
```

###  

### 2.2问题提出

　　在看了上面ThreadLocal和ThreadLocalMap相关的内存分布以及关联后，提出这样几个问题：

　　1.ThreadLocal为什么会出现内存溢出？

　　2.Entry的key为什么要用弱引用？

　　3.使用弱引用是否就能解决内存溢出？

　　为了回答上面这3个问题，我写了一段代码，后面根据这段代码进行分析：

```
public void step1() {
    // some action
     
    step2();
    step3();
     
    // other action
}
 
// 在stepX中都会创建threadLocal对象
public void step2() {
    ThreadLocal<String> tl = new ThreadLocal<>();
    tl.set("this is value");
}
public void step3() {
    ThreadLocal<Integer> tl = new ThreadLocal<>();
    tl.set(99);
}
```

　　在step1中会调用step2和step3，step2和step3都会创建ThreadLocal对象，当step2和step3执行完毕后，其中的栈内存中ThreadLocal引用就会被清除。

 

## 三.回答问题

###  

　　![img](ThreadLocal.assets/848880-20200614162022266-1246715280.png)

　　现在针对这个图，一步一步的分析问题，中途会得出一些临时的结论，但是最终的结论才是正确的。

 

### 3.1为什么会出现内存泄露

　　现在有2点假设，本小节的分析都是基于这两个假设之上的：

　　1.Entry的key使用强引用，key对ThreadLocal对象使用强引用，也就是上面图中连线5是强引用（key强引用ThreadLocal对象）；

　　2.ThreadLocalMap中不会对过期的Entry进行清理。

　　上面代码中，如果ThreadLocalMap的key使用强引用，那么即使栈内存的ThreadLocal引用被清除，但是堆中的ThreadLocal对象却并不会被清除，这是因为ThreadLocalMap中Entry的key对ThreadLocal对象是强引用。

　　如果当前线程不结束，那么堆中的ThreadLocal对象将会一直存在，对应的内存就不会被回收，与之关联的Entry也不会被回收（Entry对应的value也不会被回收），当这种情况出现数量比较多的时候，未释放的内存就会上升，就可能出现内存泄漏的问题。

　　上面的结论是暂时的，有前提假设！！！最终结论还需要看后面分析。

 

### 3.2若Entry使用弱引用

　　![img](ThreadLocal.assets/848880-20200614162022266-1246715280.png)

　　仍旧有1个假设，就是ThreadLocalMap中不会对过期的Entry进行清理，陈旧的Entry是指Entry的key为null。

　　按照源码，Entry继承弱引用，其Key对ThreadLocal是弱引用，也就是上图中连线5是弱引用，连线6仍为强引用。

　　同样以上面代码为例，step2和step3创建了ThreadLocal对象，step2和step3执行完后，栈中的ThreadLocal引用被清除了；由于堆内存中ThreadLocalMap的Entry key弱引用ThreadLocal对象，根据垃圾收集器对弱引用对象的处理：

> 当垃圾收集器工作时，无论当前内存是否足够，都会回收掉只被弱引用关联的对象。

　　此时堆中ThreadLocal对象会被gc回收（因为之前还有栈中的ThreadLocal ref强引用ThreadLocal对象，但现在ThreadLocal ref被清除了，于是就没有对ThreadLocal的强引用，只有一个弱引用ThreadLocal对象），Entry的key为null，但是value不为null，且value也是强引用（连线6），所以Entry仍旧不能回收，只能释放ThreadLocal的内存，**仍旧可能导致内存泄漏**。

　　在没有自动清理陈旧Entry的前提下，即使Entry使用弱引用，仍可能出现内存泄漏。

 

### 3.3弱引用配合自动回收

　　通过3.2的分析，其实**只要陈旧的Entry能自动被回收，就能解决内存泄漏的问题**，其实JDK就是这么做的。

　　如果看过源码，就知道，ThreadLocalMap底层使用数组来保存元素，使用“线性探测法”来解决hash冲突，关于线性探测法的介绍可以查看：[利用线性探测法解决hash冲突](https://www.cnblogs.com/-beyond/p/7726347.html)

　　在每次调用ThreadLocal类的get、set、remove这些方法的时候，内部其实都是对ThreadLocalMap进行操作，对应ThreadLocalMap的get、set、remove操作。

　　重点来了！重点来了！重点来了！

　　ThreadLocalMap的每次get、set、remove，都会清理过期的Entry，下面以get操作解释，其他操作也是一个意思，大致如下：

　　1.ThreadLocalMap底层用数组保存元素，当get一个Entry时，根据key的hash值（非hashCode）计算出该Entry应该出在什么位置；

　　2.计算出的位置可能会有冲突，比如预期位置是position=5，但是position=5的位置已经有其他Entry了；

　　3.出现冲突后，会使用线性探测法，找position=6位置上的Entry是否匹配（匹配是指hash相同），如果匹配，则返回position=6的Entry。

　　4.在这个过程中，如果position=5位置上的Entry已经是陈旧的Entry（Entry的key为null），此时position=5的key就应该被清理；

　　5.光清理position=5的Entry还不够，为了保证线性探测法的规则，需要判断数组中的其他元素是否需要调整位置（如果需要，则调整位置），在这个过程中，也会进行清理陈旧Entry的操作。

　　上面这5个步骤就保证了每次get都会清理数组中（map）的陈旧Entry，清理一个陈旧的Entry，就是下面这三行代码：

```java
Entry.value = null; // 将Entry的value设为null
table[index] = null;// 将数组中该Entry的位置设置null
size--; // map的size减一
```

　　对于ThreadLocal的set、remove也类似这个原理。

　　有了自动回收陈旧Entry的操作，需要注意的是，在这个时候，key使用弱引用就是至关重要的一点！！！

　　因为key使用弱引用后，当弱引用的ThreadLocal对象被会回收后，该key的引用为null，则该Entry在下一次get、set、remove的时候就才会被清理，从未避免内存泄漏的问题。

## 四.总结

　　在上面的分析中，看到ThreadLocal基本不会出现内存泄漏的问题了，因为ThreadLocalMap中会在get、set、remove的时候清理陈旧的Entry，与Entry的key使用弱引用密不可分。

　　当然我们也可以在代码中手动调用ThreadLocal的remove方法进行清除map中key为该threadLocal对象的Entry，同时清理过期的Entry。