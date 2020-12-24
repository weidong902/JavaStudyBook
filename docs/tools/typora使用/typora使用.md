# typora语法教程

#### 快捷键：command + 数字

#一级标题 command + 1

```xml
# 一级标题       command + 1
## 二级标题      command + 2
### 三级标题     command + 3
#### 四级标题    command + 4
##### 五级标题   command + 5
###### 六级标题  command + 6
```

##### 引用 command+option+Q

```java
> 引用

command+option+Q
```

> 引用：

##### 列表

无序列表 command+option+U

```
无序列表
* AAA
* BBB
* CCC

* 多行列表
TAB * 多行列表
TAB TAB * 多行列表

command+option+U
```

* 列表
  * 列表
    * 

有序列表： command+option+O

```
有序列表
1. AAA   
2. BBB
3. CCC

1. 多行列表
TAB 1. 多行列表
TAB TAB 1. 多行列表

command+option+O
```

1. 有序1

###### 任务列表

```java
- [ ]抽烟
- [x]喝酒

```

目录生成（`[toc]+回车键`）

**代码：command+option+C**

```
` String aaa="typora" ` //单行代码 
​```+ 回车 //代码块 
command+option+C
\partial
```

```Java
// JAVA代码块

```



### 数学表达式



#### Math

#### 图片：control+command+i

```
图片快捷键

control+command+i
```

![]()

<img src="/typora-user-images/image-20201212171936069.png" style="zoom:50%;" />

#### 公式

```java
输入$$ + 回车   command+option+B

P_{u,i}=\frac{\sum_{j}^n(sum{_{u,j}})*R_{j,i}}{\sum_{j}^nsim_{u,j}}
```

$$
5=2+3
$$

$$
P_{u,i}=\frac{\sum_{j}^n(sum{_{u,j}})*R_{j,i}}{\sum_{j}^nsim_{u,j}}
$$

#### 表格：option+command+t

###### 样式

```java
|表头1|表头2| + 回车
1
```

| 表头1 | 表头2 |
| :---- | ----- |
|       |       |

##### 图

###### diagrams

![diagrams](typora使用.assets/format,png.png)

###### 流程图

~~~java
```mermaid + 回车
graph LR 横向流程图
graph TD 竖向流程图

graph LR
A[方形] --> B(圆角)
B --> C{ifa}
C --> |a=1| D[result1]
C --> |a=2| E[result2]

~~~

```mermaid
graph LR
A[方形] --> B(圆角)
B --> C{ifa}
C --> |a=1| D[result1]
C --> |a=2| E[result2]
```



###### UML时序图

~~~java
```mermaid
sequenceDiagram + 回车

123
~~~

```mermaid
sequenceDiagram
```



##### 超链接: command+k

```java
[github](https://github.com/)
<https://github.com/>
![图片](url,title)

```

[github超链接](https://github.com/)
https://github.com/

[github](https://github.com/)

GitHub超链接

***

---

[超链接](www.baidu.com)

![winter](typora使用.assets/format,png-20201213220202704.png)

##### 分割线

```java
使用 *** 和 --- 
1
```

------

------

##### 内联样式

![inline](typora使用.assets/format,png-20201213220202613.png)

###### 字体

```
**字体加粗**   command+b
__字体加粗__
*字体倾斜*     command+i
_字体倾斜_
<u>下划线</u>  command+u
~~删除线~~
123456
```

1. **字体加粗**
2. **字体加粗**
3. *字体倾斜*
4. *字体倾斜*
5. 下划线
6. 删除线

###### 注脚

```java
github[^1]
[^1]:https://github.com
12
```

Github[1](https://blog.csdn.net/liumingzhuo/article/details/102496472#fn1)

##### 特殊字符

```java
&copy;     版权
&times;    乘号
&divide;   除号
&plusmn;   加减号
&lt;       小于号
&gt;       大于号
&ne;       不等号
&le;       小于等于
&ge;       大于等于
&nbsp;     空格
&amp;      与
&quot;     双引号
&apos;     单引号
12345678910111213
```

1. © 版权
2. ×乘号
3. ÷除号
4. ± 加减
5. < 小于号
6. \> 大于号
7. ≠不等号
8. ≤ 小于等于
9. ≥大于等于
10. 空格
11. &与
12. "双引号
13. '单引号

[HTML特殊字符编码对照表][https://www.jb51.net/onlineread/htmlchar.htm]

~~删除~~

删除线：~~XX~~





```
标题：#
最大标题：command + 1 或者：#
大标题：command + 2 或者：##
标准标题：command + 3 或者：###
中标题：command + 4 或者：####
小标题：command + 5 或者：#####
插入表格：command + T
插入代码：command + alt +c 
行间公式 command + Alt + b
段落：command + 0
竖线 ： command + Alt +q
有序列表（1.  2.） ：输入数字+“.”之后输入空格 或者：command + Alt + o   
黑点标记：command + Alt + u  
隔离线shift + command +  -
超链接：command + Alt + l
插入链接：command +k
下划线：command +u 
加粗：command +b
搜索：command +f



一级标题：⌘1 (command + 1)
二级标题：⌘2 (command + 2)
三级标题：⌘3 (command + 3)
四级标题：⌘4 (command + 4)
五级标题：⌘5 (command + 5)

段落：⌘o 不生效，快捷键冲突，使用⌃o (control + o)

提升标题级别：⌘= (command + =)
降低标题级别：⌘- (command + -)

表格：⌥⌘T (option + command + T)
代码块：⌥⌘C (option + command + C)
公式块：⌥⌘B (option + command + B)

引用：⌥⌘Q (option + command + Q)
有序列表：⌥⌘O (option + command + O)
无序列表：⌥⌘U (option + command + U)

任务列表：⌥⌘X (option + command + X)
列表缩进：
​ 增加缩进：⌘] ( command + ])
​ 减少缩进：⌘[ ( command + [)

链接引用：⌥⌘L (option + command + L)
脚注：⌥⌘R (option + command + R)

水平分割线：⇧⌘- (shift + command + -)

加粗：⌘B (command + B)
斜体：⌘I (command + I)
下划线：⌘U (command + U)

代码：⇧⌘(shift + command +)

内联公式：⌃M (control + M)
删除线：⌃~ (control + ~)
注释：⌃- (control + -)

超链接:⌘K (command + K)
图像：⌃⌘I (control + command + U)
清除样式：⌘\ (command + )

显示/隐藏侧边栏：⇧⌘L (shift + command + L)
大纲视图：⌃⌘1 (control + command + 1)
文档列表视图：⌃⌘2 (control + command +2)
文件树视图：⌃⌘3 (control + command + 3)
```







