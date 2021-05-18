# Array 数组

学习笔记：[liuyubobobobo老师的数据结构课程](https://www.bilibili.com/video/BV1L5411L7XV?p=10&spm_id_from=pageDriver)

## 数组：

### 定义：线性数据结构

### Array:自定义数组类，实现动态扩容。

* 添加、查询、删除、修改、动态扩容、支持泛型、简单时间复杂度、均摊时间复杂度及防止复杂度震荡。

```java
package com.weidd.best.dataStructure.Array;

/**
 * @author: weidd
 * @date: 2021-05-12 20:57
 * <p>
 * 学习视频:liuyubobobo老师视频
 * 封装数组:
 **/
public class ArrayTest<E> {
    private E data[];
    private int size;

    // 初始化传入数组的容量,capacity
    public ArrayTest(int capacity) {
        data = (E[]) new Object[capacity];
        size = 0;
    }

    // 无参构造,默认容量10
    public ArrayTest() {
        this(10);

    }

    // 获取数组中元素的个数
    public int getSize() {
        return size;
    }

    // 获取数组的容量
    public int getCapacity() {
        return data.length;
    }

    // 判断数组是否为空
    public boolean isEmpty() {
        return size == 0;
    }

    //结尾增加元素
    public void addLast(E e) {
       /* if (size == data.length)
            throw new IllegalArgumentException("addLast 失败, 数组已满~");
        data[size++] = e;*/
        add(size, e);
    }

    //指定位置增加元素
    public void add(int index, E e) {
        if (index < 0 || index > size)
            throw new IllegalArgumentException("add失败, 数据角标非法~");

        // 动态扩容
        if (size == data.length)
//            throw new IllegalArgumentException("add失败, 数组已满~");
            resize(data.length << 1);

        for (int i = size - 1; i >= index; i--) {
            data[i + 1] = data[i];
        }
        data[index] = e;
        size++;
    }

    //开头增加元素
    public void addFirst(E e) {
        add(0, e);
    }

    //获取元素
    public E get(int index) {
        if (index < 0 || index > size - 1)
            throw new IllegalArgumentException("获取失败 ,传入角标异常~");
        return data[index];
    }

    //set元素
    public void set(int index, E e) {
        if (index < 0 || index > size - 1)
            throw new IllegalArgumentException("set 失败 ,传入角标异常~");
        data[index] = e;
    }

    // 是否包含
    public boolean contains(E e) {
        for (int i = 0; i < size; i++) {
            if (data[i].equals(e))
                return true;
        }
        return false;
    }

    // find查找元素对应的索引
    public int find(E e) {
        for (int i = 0; i < size; i++) {
            if (data[i].equals(e))
                return i;
        }
        return -1;
    }

    // 移除指定索引的元素
    public E deleteIndex(int index) {

        if (index < 0 || index >= size)
            throw new IllegalArgumentException("移除失败,传入角标有误~");
        E e = data[index];
        for (int i = index + 1; i < size; i++) {
            data[i - 1] = data[i];
        }
        data[size - 1] = null;
        size--;

        //动态缩容: 当当前数组的长度等于数组容量的1/2时,进行缩容
        if (size == data.length >> 2 && data.length >> 1 != 0) { //懒缩容
            resize(data.length >> 1);
        }
        return e;
    }

    // 删除第一个元素
    public E deleteFirst() {
        return deleteIndex(0);
    }

    // 删除最后一个元素
    public E deleteLast() {
        return deleteIndex(size - 1);
    }

    //移除元素
    public void deleteElment(E e) {
        int i = find(e);
        if (i != -1)
            deleteIndex(i);
    }


    //toString
    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append(String.format("Array: size =%d, capacity=%d\n", size, data.length));
        sb.append("[");
        for (int i = 0; i < size; i++) {
            sb.append(data[i]);
            if (i != size - 1)
                sb.append(",");
        }
        sb.append("]");

        return sb.toString();
    }

    //数组扩容
    private void resize(int capacity) {
        E[] newData = (E[]) new Object[capacity];
        for (int i = 0; i < size; i++) {
            newData[i] = data[i];
        }
        data = newData;
    }
}

```

