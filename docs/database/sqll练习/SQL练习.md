```sql
-- 学生表
drop table if exists  student;
create table student(
s_id bigint(20) primary key auto_increment comment'学号 主键自增',
s_name varchar(32) comment'学生姓名',
s_birth varchar(14) not NULL comment'学生生日',
s_sex varchar(10) default '男' comment'性别'
)engine =INNODB comment '学生表';


-- 课程表
drop table if exists course;
create table course(
c_id BIGINT(20) PRIMARY key auto_increment comment'课程编号主键自增',
cname varchar(64) not null comment '课程名',
t_id BIGINT(20) comment '老师编号' 
);

-- 教师表
drop table if exists teacher;
create table teacher(
t_id bigint(20) primary key auto_increment,
t_name varchar(32) not null comment '老师名'
);

-- 成绩表
drop table if exists score ;
create table score(
s_id bigint(20) comment '学生编号',
c_id bigint(20) comment '课程编号',
s_score int(3) comment '分数',
primary key (s_id, c_id)
 );


-- 插入学生表测试数据
insert into Student values('01' , '赵雷' , '1990-01-01' , '男');
insert into Student values('02' , '钱电' , '1990-12-21' , '男');
insert into Student values('03' , '孙风' , '1990-05-20' , '男');
insert into Student values('04' , '李云' , '1990-08-06' , '男');
insert into Student values('05' , '周梅' , '1991-12-01' , '女');
insert into Student values('06' , '吴兰' , '1992-03-01' , '女');
insert into Student values('07' , '郑竹' , '1989-07-01' , '女');
insert into Student values('08' , '王菊' , '1990-01-20' , '女');
-- 课程表测试数据
insert into Course values('01' , '语文' , '02');
insert into Course values('02' , '数学' , '01');
insert into Course values('03' , '英语' , '03');

-- 教师表测试数据
insert into Teacher values('01' , '张三');
insert into Teacher values('02' , '李四');
insert into Teacher values('03' , '王五');

-- 成绩表测试数据
insert into Score values('01' , '01' , 80);
insert into Score values('01' , '02' , 90);
insert into Score values('01' , '03' , 99);
insert into Score values('02' , '01' , 70);
insert into Score values('02' , '02' , 60);
insert into Score values('02' , '03' , 80);
insert into Score values('03' , '01' , 80);
insert into Score values('03' , '02' , 80);
insert into Score values('03' , '03' , 80);
insert into Score values('04' , '01' , 50);
insert into Score values('04' , '02' , 30);
insert into Score values('04' , '03' , 20);
insert into Score values('05' , '01' , 76);
insert into Score values('05' , '02' , 87);
insert into Score values('06' , '01' , 31);
insert into Score values('06' , '03' , 34);
insert into Score values('07' , '02' , 89);
insert into Score values('07' , '03' , 98);
```

select * from score;
-- 1、查询"01"课程比"02"课程成绩高的学生的信息及课程分数(将分数表看成两张表)


select  s.* , sc1.s_score '01score', sc2.s_score "02score"
from student s , score sc1 ,score sc2 
where s.s_id=sc1.s_id
and s.s_id=sc2.s_id
and sc1.c_id= 1
and sc2.c_id= 2
and sc1.s_score > sc2.s_score


-- 2、查询"01"课程比"02"课程成绩低的学生的信息及课程分数(类比1)

-- 3、查询平均成绩大于等于60分的同学的学生编号和学生姓名和平均成绩
	--  ROUND(X,D)： 返回参数X的四舍五入的有 D 位小数的一个数字。如果D为0，结果将没有小数点或小数部分。

select sc.s_id , s.s_name, ROUND(avg(sc.s_score),2) avg
from  score sc ,student s
where s.s_id= sc.s_id
GROUP BY sc.s_id
having avg >=60

-- 4、查询平均成绩小于60分的同学的学生编号和学生姓名和平均成绩
		-- (包括有成绩的和无成绩的)
select sc.s_id , s.s_name, ROUND(avg(sc.s_score),2) avg
from  score sc ,student s
where s.s_id= sc.s_id
GROUP BY sc.s_id
having avg <60
union 
select  s.s_id , s.s_name , 0 avg
from student s
where s.s_id not in (select distinct s_id  from score)


-- 5、查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩

select s.s_id, s.s_name , count(sc.c_id), IFNULL(sum(sc.s_score), 0)
from student s left join score sc 
on s.s_id=sc.s_id
group by s.s_id ,s.s_name;
			
-- 6、查询"李"姓老师的数量 

select * from teacher;
select  count(1) count
from teacher t
where t_name like "李%"

-- 7、查询学过"张三"老师授课的同学的信息
	-- 分步拆解： 先将老师和课程发生关系，再将课程和学生发生关系，最后整合。 先找共性，桥节点
select s.* from student s ,score sc  
where s.s_id=sc.s_id
and sc.c_id =(select c1.c_id from teacher t, course c1 where t.t_id =c1.t_id and  t.t_name="张三");

-- 8、查询没学过"张三"老师授课的同学的信息 
	-- 逆向思维去考虑

select * from student s 
where s.s_id 
not in (select s.s_id from student s ,score sc  
where s.s_id=sc.s_id
and sc.c_id =(select c1.c_id from teacher t, course c1 where t.t_id =c1.t_id and  t.t_name="张三"));

-- 9、查询学过编号为"01"并且也学过编号为"02"的课程的同学的信息

select s.*
from student s ,  score sc1 ,score sc2
where s.s_id =sc1.s_id 
and s.s_id =sc1.s_id 
and sc1.c_id='01'
and sc1.c_id='02'




​	
-- 10、查询学过编号为"01"但是没有学过编号为"02"的课程的同学的信息
​			

​			

-- 11、查询没有学全所有课程的同学的信息 

-- 12、查询至少有一门课与学号为"01"的同学所学相同的同学的信息 


​			
-- 13、查询和"01"号的同学学习的课程完全相同的其他同学的信息 


-- 14、查询没学过"张三"老师讲授的任一门课程的学生姓名 


-- 15、查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩 


-- 16、检索"01"课程分数小于60，按分数降序排列的学生信息


-- 17、按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩
	
-- 18.查询各科成绩最高分、最低分和平均分：以如下形式显示：课程ID，课程name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率

-- 19、按各科成绩进行排序，并显示排名

-- 20、查询学生的总成绩并进行排名


-- 21、查询不同老师所教不同课程平均分从高到低显示 


-- 22、查询所有课程的成绩第2名到第3名的学生信息及该课程成绩


-- 23、统计各科成绩各分数段人数：课程编号,课程名称,[100-85],[85-70],[70-60],[0-60]及所占百分比


-- 24、查询学生平均成绩及其名次 


-- 25、查询各科成绩前三名的记录
			-- 1.选出b表比a表成绩大的所有组
			-- 2.选出比当前id成绩大的 小于三个的


-- 26、查询每门课程被选修的学生数 


-- 27、查询出只有两门课程的全部学生的学号和姓名 

-- 28、查询男生、女生人数 


-- 29、查询名字中含有"风"字的学生信息


-- 30、查询同名同性学生名单，并统计同名人数 
		




-- 31、查询1990年出生的学生名单
		


-- 32、查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列 


-- 33、查询平均成绩大于等于85的所有学生的学号、姓名和平均成绩 


-- 34、查询课程名称为"数学"，且分数低于60的学生姓名和分数 


-- 35、查询所有学生的课程及分数情况； 



 -- 36、查询任何一门课程成绩在70分以上的姓名、课程名称和分数； 

​		

-- 37、查询不及格的课程


-- 38、查询课程编号为01且课程成绩在80分以上的学生的学号和姓名； 


-- 39、求每门课程的学生人数 


-- 40、查询选修"张三"老师所授课程的学生中，成绩最高的学生信息及其成绩

​	




-- 41、查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩 

​	

-- 42、查询每门功成绩最好的前两名 
		-- 牛逼的写法
	

-- 43、统计每门课程的学生选修人数（超过5人的课程才统计）。要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列  


-- 44、检索至少选修两门课程的学生学号 


-- 45、查询选修了全部课程的学生信息 



-- 46、查询各学生的年龄



-- 47、查询本周过生日的学生


-- 48、查询下周过生日的学生
	

-- 49、查询本月过生日的学生

-- 50、查询下月过生日的学生