#### 排序：

```mysql
-- 排序后取第一条：
select * from achievement   where  courseNo='3' order by score desc  limit 1;
-- 排序：如果数据类型为非数值型时候可通过cast函数进行转换或使用MySQL绝对值函数ABS ：

select * from achievement   where  courseNo='3' order by cast(score as DECIMAL) desc  limit 1 ;
select * from achievement   where  courseNo='3' order by ABS(score) desc  limit 1 ;
```

#### 拼接：

```mysql
-- concat
select concat("KC",Aid,courseName) from  achievement；

```

限制、约束(CONSTRAINT)：















