[MySQL官方参考手册](https://dev.mysql.com/doc/refman/5.7/en/)

| MySQL数据库知识汇总 | MySQL数据库知识汇总 |
|--|--|
|  MySQL学习笔记一索引|[MySQL学习笔记一索引](https://blog.csdn.net/weixin_39559282/article/details/115075213) |
| 数据库性能优化 |   [数据库性能优化](https://blog.csdn.net/weixin_39559282/article/details/115284142)|
|执行流程、数据库架构、磁盘与内存结构|执行流程、数据库架构、磁盘与内存结构|
|数据事务|[数据事务](https://blog.csdn.net/weixin_39559282/article/details/116159057)|
|MySQL中Explain|[MySQL中Explain](https://blog.csdn.net/weixin_39559282/article/details/116561659)|

## 为什么要使用数据库？
* 数据可以永久保存；使用SQL语句查询修改效率高；管理数据方便。
## 数据库三大范式：
* 范式1：每一列都不可再分； 
* 范式2：在范式1的基础上，非主键列完全依赖主键，不是依赖主键的一部分；
* 范式3：在范式2的基础上，非主键完全依赖于主键不能依赖于其他非主键。
## MySQL数据库索引：
### 什么是索引：
数据库索引，是数据库管理系统(DBMS)中一个排序的数据结构，以协助快速查询、 更新数据库表中数据。
### 索引的作用：
数据是以文件的形式存放在磁盘上面的，每一条数据都有它的磁盘地址，如果没有索引要从百万行数据中检索一条数据，只能依次遍历全部数据直至找到。有了索引只需在索引里面检索这条数据就行。索引是一种专门用来检索的数据结构，我们找到数据存放的磁盘地址以后就可以拿到数据。（就像我们在从现代汉语大词典中查找某个成语一样不能能从第一页翻到最后去找，会根据拼音或偏旁去找。索引可以理解为书的目录或排序规则。）
### 如何创建索引：
* 方式1：建表时候创建索引
```sql
	CREATE TABLE `user` (
	  `id` int(255) NOT NULL AUTO_INCREMENT COMMENT '用户ID',
	  `code` varchar(255) DEFAULT NULL COMMENT '用户编号',
	  `name` varchar(255) NOT NULL COMMENT '用户姓名',
	  `password` varchar(255) DEFAULT NULL COMMENT '用户密码',
	  `grade` int(255) DEFAULT NULL COMMENT '用户等级',
	  PRIMARY KEY (`id`) USING BTREE,
	  UNIQUE KEY `code` (`code`) USING BTREE,
	  key index_name (`name`) 	
	) ENGINE=InnoDB AUTO_INCREMENT=8 DEFAULT CHARSET=utf8mb4 ROW_FORMAT=DYNAMIC
```
* 方式2：  
```sql
ALTER TABLE user ADD INDEX index_name (name);
```
* 方式3：
```sql
CREATE INDEX index_name ON user (name);
```
### 索引的优缺点：
* 优点：
	* 可以大大加快检索的速度；
	* 通过使用索引可以在查询过程中使用优化隐藏器提升系统性能。
* 缺点
	*  创建和维护索引需要消耗时间。增删改的时候索引需要动态维护会降低执行的效率。
### 索引类型：
在 InnoDB 里面，索引类型有三种，普通索引(Normal)、`唯一索引(主键索引是特殊的唯一 索引 Unique)`、全文索引(Fulltext)。

Myisam和Innodb（5.7）均支持Fulltext索引。(MySQL 中的全文索引，有两个变量，最小搜索长度和最大搜索长度，对于长度小于最小搜索长度和大于最大搜索长度的词语，都不会被索引。通俗点就是说，想对一个词语使用全文索引搜索，那么这个词语的长度必须在以上两个变量的区间内。)
```sql
select * from user where match(name) against('张'); #字段长度需要在某一范围方可生效 
```
>// MyISAM
最小搜索长度：ft_min_word_len = 4;
最大搜索长度：ft_max_word_len = 84;
// InnoDB
innodb_ft_min_token_size = 3;
innodb_ft_max_token_size = 84;

## InnoDB 逻辑存储结构
[MySQL官方文档](https://dev.mysql.com/doc/refman/5.7/en/innodb-disk-management.html)
[14.12.2文件空间管理](https://dev.mysql.com/doc/refman/5.7/en/innodb-file-space.html)
MySQL 的存储结构分为 5 级:表空间、段、簇、页、行。
​![数据库表](https://img-blog.csdnimg.cn/20210322134233234.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70)
* 表空间：
表空间可以看做是 InnoDB 存储引擎逻辑结构的 最高层，所有的数据都存放在表空间中。分为:系统表空间、独占表空间、通用表空间、 临时表空间、Undo 表空间。
* 段 segment：
表空间是由段组成，常见段有数据段、索引段、回滚段等，段是一个逻辑 的概念。一个 ibd 文件(独立表空间文件)里面会由很多个段组成。创建一个索引会创建两个段，一个是索引段，一个是数据段。（索引段管理非叶子节点的数据，数据段管理叶子节点的数据。）
* 簇
簇（区）每个区的大小为1M（1M/16k=64个连续的页）。一个表空间有2^3个页，每页默认大小为16k，一个表空间最多可以存64T的数据。
* 页
* 行
## 索引存储模型的推演过程
有序数组（利于查询不利于修改）--》二叉查找树（左小右大、可能会出现链化）--》平衡二叉树（左右子树深度绝对值不能超过1、左旋右旋）--》B+树（分叉数去减少树深度）
### 平衡二叉树到B+树的推演
* 当我们用树结构存储索引的时候，访问一个节点就要和磁盘进行一次IO，InNoDB操作数磁盘的最小单位就是页，大小为16k（16384字节）。一个树的节点就是16k的大小，如果一个节点只存储一个键值+数据+引用，可能大小只有十几字节或几十字节，远远达不到16k容量大小，此时导致浪费大量的空间。如果每个节点的数据都很少，从索引找到我们要的数据就需要索引大量的数据，进行大量的IO。机械硬盘时代每次从磁盘读取数据需要10ms左右的寻址时间，交互次数越多，消耗的时间越多。
* 如上问题的解决方法：	
	*  	每个节点存储更多的数据
	* 节点上关键字数量越多，指针越多，意味着有更多的分叉
	* 分叉越多，树的深度就会减少，二叉树就从高高瘦瘦变得矮矮胖胖。（多路）
### MySQL数据库为什么不用B tree，而是选择B+ tree
* B+tree是在BTree的基础上的一种优化，使其更适合实现外存储的索引结构、
* BTree的每个节点不仅包含数据的key值，还有data的值。而每一页存储空间是有限的，如果data的数据较大的话会导致每个节点（每页）存储的存储的key值较少，当存储数据量很大的话会导致树的深度较大，也即会增大磁盘的IO次数，进而影响查询的效率。
* B+Tree所有数据记录都是按照键值的大小存储在叶子节点上，而非叶子节点只存储key值，这样大大加大每一页的存储数量，同时降低B+Tree的深度。因此树从`高高瘦瘦，变得矮矮胖胖`。虽然变丑了但是查询效率起来了。`胖子是潜力股。`
###  MySQL数据库为什么不用红黑树
* 在大规模数据存储的时候，红黑树往往出现由于树的深度过大而导致磁盘IO读写频繁，进而导致效率底下，。
### B+树的特点：
* 关键字的数量更路数相同；
* B+树的根节点和枝节点不存放数据，只在叶子节点存储数据；（树的深度为2时可存储两千万左右的数据）
* 每个叶子节点会增加一个指向相邻叶子节点的指针，它的最后一个数据会指向下一个叶子节点的第一个数据（双向链表结构）；
* 左闭右开 [) 的区间。
### InnoDB中B+树的特点：
* Btree的变种，B+ 树能解决的问题都能解决；（每个节点存储更多的数据，路数更多）
* 扫库、扫表的能力强（如果需要全表扫描，不需要遍历整棵树，仅需要遍历叶子节点就可以拿到所以数据）
* B+树的磁盘读写能力比Btree更强（根节点和枝节点不存储数据，所以一个节点能存储更多的关键字，一次磁盘加载的关键字更多）
* 排序能力更强（叶子节点上有下一个数据区的指针，数据形成链表）
* 效率更稳点（所有的数据在叶子节点，IO的次数相同）
## Hash索引
### 什么是hash索引：
* hash是以KV的形式检索数据，	它会根据索引字段生成哈希码和指针，指针指向数据。
### 哈希索引的特点
* 它的时间复杂度为O(1)，查询速度比较快。因为hash索引里面的数据不是顺序存储的，所以不能用于排序；
* 在查询的时候根据键值计算哈希码，它只能支持等值查询(= IN )，不支持范围查询(>, <, >=, <=, between and )。
* 如果字段重复很多的话会出现大量的哈希冲突。
### 哈希索引在InnoDB中的使用。
* INNODB只支持显示创建B+Tree索引，对于一些热点数据页，InNoDB会自动创建自适应索引，也就是在B+树的基础上，创建Hash索引，此过程是隐式的。
## Mysql数据存储文件
* 每张Innodb有两个文件：.frm 和 .ibd
* Myisam 的表有三个文件：.frm、.MYD、.MYI 
* .frm是MySQL表结构的定义文件，无论何存储引擎都会生成。
### MyISAM
#### MyISAM文件解析
* .MYD文件D代表Data，是MyISAM的数据文件存放记录数据。
* .MYI文件I代表Index，是MyIASM的索引文件，存放的是索引。
#### MyISAM查找数据的过程：
* 由于MyIASM的B+Tree里面叶子节点存储的是数据文件对应的磁盘地址。所以从索引文件.MYI中找到键值后，会在数据文件.MYD中取出响应的数据记录。
* 对主键索引和辅助索引检索的方式都是一样的。
### InnoDB
#### InnoDB查找数据的过程
* InnnoDB只有一个文件.ibd，它是以主键为索引来组织数据的存储的所有的索引文件和数据文件都在.ibd文件中。
* 主键索引的叶子节点上直接存储的是数据。	
#### 思考：树的高度为3时，要进行几次磁盘IO？
* 1-3次：（顶层是常驻内存的）,但是为什么还是会有三次呢？
* 3次出现的原因在于，当主键索引时，3层最多是2次，但是当非主键索引时，会出现回表的可能。
#### 思考：深度为3的B+Tree可以存储多少数据？
INnoDB存储引擎中页大小为16KB，一般表的主键为int类型（占4个字节）或bigint（占8个字节），指针类型一般也是4~8个字节，也即是一页（B+Tree的一个节点）中大概可以存储（16KB/(8+8B)=1K个键值。当深度为3层时候，可存储1024*1024*1024=10亿个数据。实际上有的节点不一定会被填满，另外还有数据可能也不止那么多，但是千万级别还是可以达到的。因此，B+Tree树的深度，一般在`2~4层就足够了`

#### 聚簇索引（聚集索引）和非聚集索引？
* 将数据存储与索引放到了一起，索引结构的叶子节点保存了行数据（聚集索引）。
* 将数据与索引分开存储索引结构的叶子节点指向了数据对应的位置。
注： 在InnoDB中在聚簇索引之上建立的索引称为辅助索引，非聚簇索引都是辅助索引，像复合索引、前缀索引、唯一索引，辅助索引的叶子节点存储的不再是行的物理位置，而是主键值，辅助索引在访问数据库总是需要二次查找。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210407201558445.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70)
* Innodb中组织数据的方式叫做聚集索引（主键索引叫聚集索引，非主键都是非聚集索引）
* InnoDB中主键是这样存储的那除了主键之外的索引，如name上建立了普通索引，检索和存储的方式？
 InNoDB中主键索引和辅助索引是有主次之分的。
* 辅助索引存储的是辅助索引和主键值。如果根据辅助索引查询，会根据主键值在主键索引中去查询，最终取得数据。（例：通过name辅助索引查询name=“张三”，它会在叶子节点上找到主键值，即id=1，然后再到主键索引的叶子节点拿到数据）
#### 思考：为什么辅助索引中存的是主键值而不是主键的的磁盘地址？
>B Tree 实现一个节点存储多个关键字，保持平衡，是由于分叉和合并的操作存在（增删改查的存在），此时的地址值是会变化的，因此辅助索引中存储的是主键值。

#### 思考2：如果一张表中没有主键？
>如果定义了primary key ，则InNoDB会选择主键作为聚集索引；
>如果没有显示的定义主键，则InnoDB会选择一个不为空的值为唯一索引作为主键；
>如果也没有不为空的值，则会选择内置6字节长度的RowID作为隐藏的聚集索引，随着行记录的写入而主键递增。

### MyISAM
#### MyISAM
## 索引使用原则
###  列的离散度
* 离散度公式：count(distinct(column_name)):count(*) --(列的全部不同值和所有数据行的比例，数据行相同时，分子越大，离散度越大。)【重复越少，离散度越大】
	* 在name上和gender上建立索引的区别，在于离散度的大小。（`离散度越大越有利于索引的检索`） 
### 联合索引的最左匹配（最左包含）原则
* 原则1：最左匹配原则（最左包含）。
* 原则2：MySQL存储引擎为了在查询时候使用到索引，在查询过程中动态调整字段顺序，以便更好的使用到索引。
#### 创建联合索引
```sql
alter table drop index com_name_phone;
alter table add index  com_name_phone(name,phone);
```
* 联合索引在B+树中是复合的数据结构，它按照从左到右的顺序建立索引树，（name在左，phone在右。）如上联合索引，name是有序的，phone是无序的，只有当name相同时，phone才有序。
* 此时，如果查询语句是`select * from user where phone ="xxx";` 则不会用到联合索引。
#### 联合索引的命中(最左匹配原则【最左包含】)
```sql
drop table  if exists TB1 ;
create table TB1(
id int primary key auto_increment,
name varchar(32) ,
phone varchar(11) 
);
alter table TB1 add index com_name_phone(name,phone);
insert  into TB1 (name,phone) values("张三","33333333333"),
("李四","44444444444"),("王五","55555555555");
```
* 使用到联合索引
```sql
explain select * from TB1 where name ="张三" 
and phone ="33333333333";
```
![联合索引](https://img-blog.csdnimg.cn/20210328172024910.png)
* 使用到联合索引
```sql
explain select * from TB1 where name ="张三";
```
![SHIYOGN](https://img-blog.csdnimg.cn/20210328172149382.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zOTU1OTI4Mg==,size_16,color_FFFFFF,t_70)
* 未使用到联合索引
```sql
explain select * from TB1 where phone ="33333333333";
```
![未使用](https://img-blog.csdnimg.cn/20210328172229626.png)
注：
>当我们创建索引index(a,b,c)；
>相当于是常见了三个索引index(a),index(a,b),index(a,b,c);
>查询时出现 where b=?和where b=? and where c=?和where  c =?
>是不会用到索引的。

## 索引的创建
### 索引创建注意事项：
* 在用于where判断、order by 排序、join on等字段上创建索引
* 索引个数不能过多。（浪费空间，修改时变慢）
* 区分度低的字段（如：性别）不要创建索引。（离散度越高，越有利于索引检索）
* 更新频繁的值不要作为主键或索引。（页分裂）
* 组合索引将区分度高的字段放在前面。
* 创建组合索引，（利用索引命中的最左原则）
### 什么情况下用不到索引？
* 索引列上使用函数（replace、substr、concat、sum count avg）表达式、计算（+-*/）
* 字符串类型不加引号会出现隐式转换
* like条件中前面加% 。
* 负向查询，not like不可以、!= (<>) not in 在某些情况下可以。
* 查询语句中使用多列索引，只有在查询条件中使用到了第一个字段，索引才会生效。
* 查询语句中使用or关键字。如果or前后两个条件都是索引，那么查询才会用到索引。如果or前后有一个条件不是索引，那么查询将不会用到索引。
## 面试
###  MySQL中 in是否能用到索引？






