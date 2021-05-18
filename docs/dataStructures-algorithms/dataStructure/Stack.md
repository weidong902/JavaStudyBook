# Stack：栈

## Stack

### 定义：

栈是一种线性数据结构；相比数组，栈操作的是数组的子集，只能从栈顶操作元素。Last In First Out（LIFO）先入后出。

### 应用：

撤销操作、程序调用使用的系统栈、递归

* 自定义栈的接口。

```java
package com.weidd.best.dataStructureandAlgorithms.dataStructure.Stack;

/**
 * @program: SortDemo
 * @author: weidd
 * @date: 2021-05-13 15:32
 * <p>
 * 自定义一个Stack 接口.
 * 后期基于不同的数据结构去实现该接口.
 **/
public interface StackDemo<E> {
    // 判断是否为空
    public boolean isEmpty();

    // 新增
    public void push(E e);

    // 删除
    public E pop();

    //查询栈顶元素
    public E peek();

    // 获取元素个数
    public int getSize();
}


```

* 栈接口的实现类

```java
package com.weidd.best.dataStructureandAlgorithms.dataStructure.Stack;

import com.weidd.best.dataStructureandAlgorithms.dataStructure.Array.ArrayDemo;

/**
 * @program: SortDemo
 * @author: weidd
 * @date: 2021-02-02 20:54
 **/
public class ArrrayStack<E> implements StackDemo<E> {
    //基于自定义动态数组实现的栈
    private ArrayDemo<E> arrayDemo;

    public ArrrayStack(int capacity) {
        arrayDemo = new ArrayDemo<>(capacity);
    }

    public ArrrayStack() {
        arrayDemo = new ArrayDemo<>();
    }

    // 判断是否为空
    public boolean isEmpty() {
        return arrayDemo.isEmpty();
    }

    // 新增
    public void push(E e) {
        arrayDemo.addLast(e);
    }

    // 删除栈顶
    public E pop() {
        return arrayDemo.deleteLast();
    }

    //查询栈顶元素
    public E peek() {
        return arrayDemo.getLast();
    }


    // 获取元素个数
    public int getSize() {
        return arrayDemo.getSize();
    }

    // 获取容量
    public int getCapacity() {
        return arrayDemo.getCapacity();
    }

    public String toString() {
        StringBuilder sb = new StringBuilder();
        sb.append("ArrayStrack: [");
        for (int i = 0; i < arrayDemo.getSize(); i++) {
            sb.append(arrayDemo.get(i));
            if (i != arrayDemo.getSize() - 1)
                sb.append(", ");
        }
        sb.append("] top");
        return sb.toString();

    }

}

```

* 自定义栈的实现类

```java
package com.weidd.best.dataStructureandAlgorithms.dataStructure.Stack;

/**
 * @program: SortDemo
 * @author: weidd
 * @date: 2021-05-13 20:59
 * 自定义Stack测试.
 **/
public class StackMain {
    public static void main(String[] args) {
        ArrrayStack<Integer> arrrayStack = new ArrrayStack<>();
        for (int i = 0; i < 10; i++) {
            arrrayStack.push(i);
        }
        System.out.println(arrrayStack);

        arrrayStack.push(111);
        System.out.println(arrrayStack);
        arrrayStack.pop();
        System.out.println(arrrayStack);

    }
}

```

* 使用到栈的算法题：力扣 : 20. 有效的括号

```java
package com.weidd.best.dataStructureandAlgorithms.dataStructure.Stack;

import org.junit.Test;

import java.util.HashMap;
import java.util.Stack;

/**
 * @program: SortDemo
 * @author: weidd
 * @date: 2021-02-24 20:27
 * 力扣 : 20. 有效的括号
 * 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。
 * <p>
 * 有效字符串需满足：
 * <p>
 * 左括号必须用相同类型的右括号闭合。
 * 左括号必须以正确的顺序闭合。
 **/
public class StackTest {
    public boolean isValid(String s) {
        int n = s.length();
        if (n == 0 || n % 2 == 1)
            return false;
        //遍历字符串，左括号入栈；右括号，则比较栈顶括号是否能对应上，对应上弹出左，对应不上，return false。
        HashMap<Character, Character> map = new HashMap<Character, Character>();
        map.put(')', '(');
        map.put('}', '{');
        map.put(']', '[');
        Stack<Character> stack = new Stack<Character>();
        for (int i = 0; i < n; i++) {
            Character ch = s.charAt(i);
            if (map.containsKey(ch)) {
                if (stack.isEmpty() || stack.peek() != map.get(ch))
                    return false;
                stack.pop();
            } else {
                stack.push(ch);
            }
        }
        return stack.isEmpty();
    }

    public boolean isValid1(String s) {
        //特判
        int n = s.length();
        if (n == 0 || n % 2 == 1)
            return false;
        //遍历字符串，左括号入栈；右括号，则比较栈顶括号是否能对应上，对应上弹出左，对应不上，return false。
        Stack<Character> stack = new Stack<Character>();
        for (int i = 0; i < n; i++) {
            Character ch = s.charAt(i);
            if (ch == '(' || ch == '[' || ch == '{')
                stack.push(ch);
            else {
                if (stack.isEmpty()) return false;
                Character topChar = stack.pop();
                if (ch == ')' && topChar != '(') return false;
                if (ch == ']' && topChar != '[') return false;
                if (ch == '}' && topChar != '{') return false;
            }
        }
        return stack.isEmpty();
    }

    @Test
    public void test1() {
        HashMap<Character, Character> map = new HashMap<Character, Character>();
        String s = "()[]{}";
        System.out.println(isValid1(s));

    }
}

```

