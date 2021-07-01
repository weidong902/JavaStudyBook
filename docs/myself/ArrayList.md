# 1. List接口

### List接口中的方法：

```java

package java.util;

import java.util.function.UnaryOperator;

public interface List<E> extends Collection<E> {
  
    int size();

    boolean isEmpty();

    boolean contains(Object o);

    Iterator<E> iterator();
    
    Object[] toArray();

    <T> T[] toArray(T[] a);

    boolean add(E e);

  	boolean remove(Object o);

    boolean containsAll(Collection<?> c);

    boolean addAll(Collection<? extends E> c);

    boolean addAll(int index, Collection<? extends E> c);

    boolean removeAll(Collection<?> c);

    boolean retainAll(Collection<?> c);

    default void replaceAll(UnaryOperator<E> operator) {
        Objects.requireNonNull(operator);
        final ListIterator<E> li = this.listIterator();
        while (li.hasNext()) {
            li.set(operator.apply(li.next()));
        }
    }
   
    @SuppressWarnings({"unchecked", "rawtypes"})
    default void sort(Comparator<? super E> c) {
        Object[] a = this.toArray();
        Arrays.sort(a, (Comparator) c);
        ListIterator<E> i = this.listIterator();
        for (Object e : a) {
            i.next();
            i.set((E) e);
        }
    }

    void clear();

    boolean equals(Object o);

    int hashCode();

    E get(int index);
  
    E set(int index, E element);

    void add(int index, E element);

    E remove(int index);

    int indexOf(Object o);

    int lastIndexOf(Object o);


    ListIterator<E> listIterator();

    ListIterator<E> listIterator(int index);

   
    List<E> subList(int fromIndex, int toIndex);

    @Override
    default Spliterator<E> spliterator() {
        return Spliterators.spliterator(this, Spliterator.ORDERED);
    }
}

```



# ArrayList

### 2.1 几个重点

- 底层是数组，初始大小为10
- 插入时会判断数组容量是否足够，不够的话会进行扩容
- 所谓扩容就是新建一个新的数组，然后将老的数据里面的元素复制到新的数组里面
- 移除元素的时候也涉及到数组中元素的移动，删除指定index位置的元素，然后将index+1至数组最后一个元素往前移动一个格

### 2.2 增删改查

1）增

```
 public boolean add(E e) {
    //进行数组容量判断，不够就扩容
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    elementData[size++] = e;
    return true;
 }

 
public void add(int index, E element) {
	//检查是否会越界
    rangeCheckForAdd(index);
	//进行数组容量判断，不够就扩容
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    //将index至数据最后一个元素整体往后移动一格，然后插入新的元素
    System.arraycopy(elementData, index, elementData, index + 1,
                    size - index);
    elementData[index] = element;
    size++;
}
```

2）删

```
public E remove(int index) {
	//判断是否越界
    rangeCheck(index);

    modCount++;
    E oldValue = elementData(index);

    int numMoved = size - index - 1;
    //若该元素不是最后一个元素的话，将index+1至数组最后一个元素整体向前移动一格
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    elementData[--size] = null; // clear to let GC do its work

    return oldValue;
    }
```

3）改

```
public E set(int index, E element) {
    rangeCheck(index);

    E oldValue = elementData(index);
    elementData[index] = element;
    return oldValue;
}
```

逻辑很简单，将数组对应index的元素进行替换

4）查

```
public E get(int index) {
    rangeCheck(index);
    return elementData(index);
}

E elementData(int index) {return (E) elementData[index]; }
```

逻辑很简单，进行数组越界判断，获取数组对应index的元素

### 2.3 总结

以上部分就是ArrayList的增删改查原理，以此也可以解答我们第二个问题了，ArrayList的底层是数组，所以查询的时候直接根据索引可以很快找到对应的元素，改也是如此，找到index对应元素进行替换。而增加和删除就涉及到数组元素的移动，所以会比较慢。



# CopyOnWriteArrayList

### 3.1 几个要点

- 实现了List接口
- 内部持有一个ReentrantLock lock = new ReentrantLock();
- 底层是用volatile transient声明的数组 array
- 读写分离，写时复制出一个新的数组，完成插入、修改或者移除操作后将新数组赋值给array

### 3.2 增删改查

1）增

```
public boolean add(E e) {
	final ReentrantLock lock = this.lock;
	//获得锁
	lock.lock();
	try {
		Object[] elements = getArray();
	    int len = elements.length;
	    //复制一个新的数组
	    Object[] newElements = Arrays.copyOf(elements, len + 1);
	    //插入新值
	    newElements[len] = e;
	    //将新的数组指向原来的引用
	    setArray(newElements);
	    return true;
	} finally {
		//释放锁
	    lock.unlock();
	}
}

   
public void add(int index, E element) {
    final ReentrantLock lock = this.lock;
    lock.lock();
    try {
        Object[] elements = getArray();
        int len = elements.length;
        if (index > len || index < 0)
            throw new IndexOutOfBoundsException("Index: "+index+
                                                ", Size: "+len);
        Object[] newElements;
        int numMoved = len - index;
        if (numMoved == 0)
            newElements = Arrays.copyOf(elements, len + 1);
        else {
            newElements = new Object[len + 1];
            System.arraycopy(elements, 0, newElements, 0, index);
            System.arraycopy(elements, index, newElements, index + 1,
                             numMoved);
        }
        newElements[index] = element;
        setArray(newElements);
    } finally {
        lock.unlock();
    }
}
```

2）删

```
public E remove(int index) {
    final ReentrantLock lock = this.lock;
    //获得锁
    lock.lock();
    try {
        Object[] elements = getArray();
        int len = elements.length;
        E oldValue = get(elements, index);
        int numMoved = len - index - 1;
        if (numMoved == 0)
	        //如果删除的元素是最后一个，直接复制该元素前的所有元素到新的数组
            setArray(Arrays.copyOf(elements, len - 1));
        else {
	        //创建新的数组
            Object[] newElements = new Object[len - 1];
            //将index+1至最后一个元素向前移动一格
            System.arraycopy(elements, 0, newElements, 0, index);
            System.arraycopy(elements, index + 1, newElements, index,
                             numMoved);
            setArray(newElements);
        }
        return oldValue;
    } finally {
        lock.unlock();
    }
}
```

3）改

```
public E set(int index, E element) {
    final ReentrantLock lock = this.lock;
    //获得锁
    lock.lock();
    try {
        Object[] elements = getArray();
        E oldValue = get(elements, index);

        if (oldValue != element) {
            int len = elements.length;
            //创建新数组
            Object[] newElements = Arrays.copyOf(elements, len);
            //替换元素
            newElements[index] = element;
            //将新数组指向原来的引用
            setArray(newElements);
        } else {
            // Not quite a no-op; ensures volatile write semantics
            setArray(elements);
        }
        return oldValue;
    } finally {
	    //释放锁
        lock.unlock();
    }
}
```

4）查

```java
//直接获取index对应的元素
public E get(int index) {return get(getArray(), index);}
private E get(Object[] a, int index) {return (E) a[index];}
```

### 3.3 总结

从以上的增删改查中我们可以发现，增删改都需要获得锁，并且锁只有一把，而读操作不需要获得锁，支持并发。为什么增删改中都需要创建一个新的数组，操作完成之后再赋给原来的引用？这是为了保证get的时候都能获取到元素，如果在增删改过程直接修改原来的数组，可能会造成执行读操作获取不到数据。

### 4 CopyOnWriteArrayList为什么并发安全且性能比Vector好

Vector是增删改查方法都加了synchronized，保证同步，但是每个方法执行的时候都要去获得锁，性能就会大大下降，而CopyOnWriteArrayList 只是在增删改上加锁，但是读不加锁，在读方面的性能就好于Vector，CopyOnWriteArrayList支持读多写少的并发情况。



