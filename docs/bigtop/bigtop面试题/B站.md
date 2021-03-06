## 技术群26群读者：B站五面面经

原创 三太子敖丙 [三太子敖丙](javascript:void(0);) *今天*

收录于话题

\#大厂面经

16个

上周午休我刷手机的时候看到26群在那疯狂刷恭喜，我以为发生了什么，原来是晨曦进了B站，我也刷了一句恭喜，然后就跟他私聊了。

![图片](B%E7%AB%99.assets/640-20210706133146616)

他我印象还是比较深的，因为他给了很多我视频的建议（虽然很久没录制面试视频了），然后是比较用心那种。

他之前一直比较满足的，结果发现朋友一毕业就20+，然后跳槽直接30+了，他就傻了，大厂非大厂差距那么大，他之前还觉得自己算比较多的，所以我也知道他为了换一家公司准备了很久。

![图片](B%E7%AB%99.assets/640)

怎么说呢，细节我就不说了，现在结局是好的，也希望他能接着鲜衣怒马，不负韶华，后面就分享一下他的面经吧，希望大家都能有所收获。

![图片](B%E7%AB%99.assets/640-20210706133201085)

### **2021.5.20一面** : (几个小时之后通知二面)

1、自我介绍(工作情况、项目概况、用哪些技术、自己负责什么、擅长什么、优势是什么)

答:领导你好；我叫**，16年本科毕业于****大学；毕业后在合肥卫宁，一家医疗软件公司工作，技术上前后端、数据库包括项目部署都做，属于全栈开发，18年来到上海入职京东一直到现在，做的是线下商场的后台及接口开发;老的项目还是mvc，新项目都做了前后端分离，前端用到H5、小程序JQuery、EasyUI、VUE、Angular，后端用到.Net Framwork 4.5,4.6 、NetCore、Java，数据库用到SQLService、Mysql、MongoDB、Redis，中间件用到RabbitMQ消息队列，在公司是核心开发的角色，负责老项目的维护及二次开发，和其他开发相比，我的优势在于沟通交流能力比较强，还有我的责任心和学习能力也是比较强的，如果有幸加入B站，我相信我能快速融入团队，快速掌握公司相关的技术；谢谢领导。

2、介绍下你做的项目及你在工作中的定位

答：我负责一些老项目的维护，像商场、商户的后台、内容管理、广告、推荐位后台、游戏的后台和接口；对外的开发平台接口；新项目包括应用插件、勋章等项目；老项目是主要开发的角色，新项目一般是项目负责人的角色，负责项目的表结构设计、任务的安排及分配，夸部门之间的协作调度。

3、离职的原因

答：1、目前工作5年了，技术上也遇到了瓶颈，目前的公司做的事情大部分都是重复的增删改查，希望换个环境，拓宽下技术和视野，也希望学习更多，更新的技术

2、目前在职级上虽然晋升了，但是在上很难晋升，因为公司的领导都是6、7年的老员工，我自己也希望做管理开发岗

基于以上两个原因，想换一个平台。

4、未来的规划是什么

答：1-2年内花半年左右的时候熟悉公司的技术栈，业务，再花一年多时间沉淀，深入的学习所用的技术且时刻关注学习新技术；2年以后如果有机会我希望可以做开发管理岗，自己带团队。

5、结婚没、住哪，现在的公司在哪

答：已结婚，定居上海，现公司在上海北外滩。

![图片](B%E7%AB%99.assets/640-20210706133217831)

### **2021.5.27二面**(技术) : (一周后通知三面)

1、设计表结构的时候要考虑哪些问题

答：我们目前用的数据库是MongoDB

(1)设计表的时候首先要关注表字段的可扩展性，例如性别这个字段，正常可以用bool类型,true为男，false为女，但是如果是未知呢，所以尽量避免用bool类型，可以用int类型或者枚举类型代替；

(2)其次需要考虑表的可维护性及可读性，例如主表里有奖品信息的字段，如果奖品有多种类型呢，甚至对应多个奖品的情况，我这边选择独立一张表出来通过字段关联存放奖品信息而不选择放在主表字段里

2、关系型数据库和非关系型数据库有什么区别，你们现在用的是哪个数据库，为什么

答：关系型数据库包括SqlServe、MySql、Oracle；非关系型数据库有MongoDB、Redis

(1)nosql的存储格式是key,value形式、文档形式、图片形式等等，所以可以存储基础类型以及对象或者是集合等各种格式，而数据库则只支持基础类型

(2)nosql数据库将数据存储于缓存之中，关系型数据库将数据存储在硬盘中，自然查询速度远不及nosql数据库

(3)关系型数据库可以用SQL语句方便的在一个表以及多个表之间做非常复杂的数据查询

(4)关系型数据库支持事务，安全性高

我们存储的会员数据量比较大，且每个月都产品过亿的订单及日志数据，所以选择非关系型数据库MongoDB

3、数据库查询优化了解吗，说说怎么优化(索引相关)

答：可以先在查询分析器中查看执行语句的时间，分析语句是否用到了索引，索引是否生效

(1)尽量避免select*，需要哪些字段就查询哪些字段

(2)一些公用的数据可以先拿到内存里，后面要用直接拿，而不是每次用到的时候重新去数据库取，主要是减少数据库交互，提供性能

4、说说索引(聚集索引、非聚集索引，联合索引、最左匹配原则、非聚集索引要回表查询)

答：索引分为聚集索引和非聚集索引，用非聚集索引查询一般需要再次回表查询，因为非聚集索引存储的是主键ID，聚集索引查询就好似书的目录，用索引可以快速定位某一块区域数据，如果不用索引就需要全表扫描；还有多个字段组成联合索引，一般要符合最左匹配原则；

in or 关键字就不走索引，like模糊匹配，前通配不走索引，后通配走索引

5、如果用非聚集索引怎样可以不用回表呢(select id from a where name="111")

答：如果只查询ID，用非聚集索引就不用回表，因为非聚集索引上就存的是ID

6、为什么使用索引后查询效率高呢

答：这个就涉及到索引和数据的结构了，类比书籍的目录页，索引通过B+树的结构存放数据，横向数据越多，纵向的高低越低，和磁盘的交互就越少，查询得速度就越快

7、有用过redis吗，你项目中使用场景是什么？说说你对redis的理解

答:我们项目中用到的redis场景不多，一般用于存储计算的数据，我们有些通过BI统计计算的数据，每次计算耗时比较长，这些数据可以存放到redis中，设置一天有效期；后面访问的时候就比较快了

redis的类型有string类型，hsah，可以用于购物车的场景，List，朋友圈展示，set，共同好友，zset 排名；

redis因为数据都在内容中所以查询起来非常快，但是它会通过RDB和AOF持久化数据到磁盘里去

RDB是在指定的时间间隔内生成数据集的时间点快照，它保存了 Redis 在某个时间点上的数据集

AOF 文件是一个只进行追加操作的日志文件

一般当redis挂了数据恢复时会组合来用，先用RDB恢复某个时间节点的数据，后面一部分丢失的数据再用AOF数据去恢复

8、java项目做过了多久时间，能自己独立搭一个java项目吗

答：目前刚接触java相关项目，用了2个月左右，参考其他项目，框架由架构师搭好，我们主要还是写的业务代码，目前还不能自己独立搭一个java项目，但是我相亲给与一定的时候学习，是可以独立完成项目的搭建的。

9、你有什么想问的吗(问了开发语言、项目概况)

答：1、我想了解下咱们的项目大概是做什么的(内部的一些库存盘点等系统)

2、咱们用的开发语言是什么(主要用java、也会有一些go语言相关的)

![图片](B%E7%AB%99.assets/640-20210706133148375)

### **2021.6.7三面**(项目) : (三天后通知四面)

1、介绍下你现在的研发团队的情况(产品、运维、前端开发、后端开发、测试)

答：8个左右产品，3个运维，1个DB，10个前端，25个左右后端，10个左右测试

2、现在这家公司做什么业务？你负责哪些项目？

答：做的是线上商场的系统，用户端主要是H5和小程序，会给商场提供一些功能，包括商户的活动，优惠，团购等；提供发优惠券、问卷、一些游戏等功能；我主要负责的项目有后台商场商户的维护二次开发、内容管理、插件整合后台售卖，游戏的后台，对外的开放平台接口等；、

3、说说你从0到1的一个项目，你在项目中承担什么角色

答：前端时间做的勋章的项目，功能大概是会员消费一定金额后完成任务后点亮对应的勋章并发放奖励，用户端用小程序，后台页面用VUE，后台接口用java，我承担的是项目负责人的角色

(1)根据产品提出的需求及UE设计项目的表结构

(2)根据需求细分任务到具体责任人，整体把控项目的周期(1个半月左右)

(3)基础控件数据的提供；核心业务逻辑的开发，vue页面、java接口的demo页开发

(4)跨部门协调以及项目的部署

4、如果让你重新做一次你举得可以改进的项目，你觉得有哪些点可以优化的

答：我的一个客户经理项目需要跨表查询，当时领导提供的方案是在MongoDB上建视图，MongoDB4.6及以上版本才支持视图，且查询效率慢，如果重新做我觉得可以换成Mysql关系型数据库，因为可以多表关联查询

前端用框架，这个项目的前端用的html+原生js，这里的动态查询写了几百行的DOM元素操作，费时费力。

5、一个数组如果让你找到最大数，你会怎么弄？

答：减少空间的话可以用冒泡排序找；减少时间的话可以用二分法或者分段找到每一块最大的再继续找。

6、如果我们现在要做一个库存盘点的系统，你怎么设计表结构，设计哪些表

答：库存表包含的字段有 物品的类别，名称、编号、初始数量、剩余数量等信息；

分配表包含的字段有 人员信息，包含手机号，微信号，名称等、物品类型、物品名称、编号

还有一些分配记录表，日志表，记录分配和修改的信息，方便后面做库存盘点统计用

7、公司采购一批电脑，新员工入职领电脑后本人扫码确认领取，这块代码你如何实现(后台设置领取人信息、微信扫码确认，获取微信相关信息和后台设置的人员信息比较，如果一致则确认本人已领取)

答：可以结合微信小程序，员工入职分配电脑，后台录入相关人员，相关人员拿到电脑后扫码确认领导电脑，扫码后微信授权，获取人员相关信息与后台录入的信息比较(姓名、手机号、微信号等)；如果一致则修改后台确认字段参数，如果不一致做出提示并通知相关人员

8、你有什么想问的吗(问了团队的人员情况)

答：咱们目前的团队有多少，还准备招几人呢；咱们的内部项目已经启动了吗

![图片](B%E7%AB%99.assets/640-20210706133216780)

### **2021.6.16四面**(部门主管):(一周后通知HR面)

1、说说你们公司的人员结构和你的位置及承担的角色

答：见3面1

2、项目中遇到的难点，如何解决，以后怎么避免

答：在去年疫情刚开始的时候刚来上海的前两个星期在家办公，我做的客户经理项目要上线，当时测试环境测得差不多了，发到线上测试的时候发现数据只能读不能写，因为测试环境数据库是单台的，线上是主从多台的，因为是新项目、4.6以上版本才支持MongoDB视图，MongoDB的驱动升级到了最新，网上资料也很少，查阅了相关资料和问同事最后阅读了相关源码后发现需要加一个参数才可以写到主库里，中间时间大概花了3天左右，但是上线一个星期后另外一个部门的领导告诉我们MongoDB官网又升级了驱动，现在直接可以用了，不用加那个参数了，我可以认为刚好那几天我们升级的最新驱动是官网发布的有问题的驱动，一周后他们解决后又发布了最新驱动;这是我遇到比较坑的一个难点吧。

后续想要避免这样的问题，我们得把现在稳定运行的驱动打成自己的Nuget包，并将此次问题上传公共文档(wiki)，后续有其他部门同事也需要使用MongDB驱动的时候可以用我们内部打的驱动避免不可预知的意外，同时，我们测试环境的生产环境尽量保持一致，避免某些问题上线后才能发现

3、在项目中如何对接(对接前端，第三方，其他后端应用)

答：(1)大部分项目已经做了前后端分离，我们作为后端开发写完接口提供swagger文档给前端开发，里面有具体的接口地址，入参，出参，其他不清楚的地方在单独沟通下

(2)对接第三方我们提供对外统一的文档地址，里面提供了接口地址、加密方式demo、客户端dll、调用示例(java .net、python等)，入参，出参等

(3)对内调用其他后台接口，一般对方提供Nuget客户，客户端里有入参，返回值，如果没有客户端需要自己这边根据对方提供的入参，返回值调用接口

4、平时怎么跟同事沟通交流

答：我觉得工作中，除了责任心，工作能力之外最重要的就是沟通能力，这取决于人的性格、更取决于沟通的技巧

(1)不要抱着有事我找你，没事我都不认识你的态度去交流，平时需求对接正常沟通交流，人家有事找你，即使不是你负责的在不耽误其他事的情况下帮忙解答一下，早上上班的见面了打个招呼，吃饭的时候聊聊天，或者经常跟自己组的人或者关系比较好的人一起吃饭聊天，遇到不熟的也可以聊聊，这样你后面沟通的时候很轻松，而且作为程序员人脉也很重要，以后不在一个公司了，技术问题可以相互交流，后面想跳槽也可以相互内推

(2)多分享分享，可以是工作中的经验，也可以是掌握的技术知识点，当然人际关系更多的是物质的分享，作为程序员工资也不低，不在乎那几十块钱吧，平时买点零食、水果之类的啊，边上的人发一发，俗话说拿人手短，不管怎么样人家心里觉得你这个人不错；前段时间我第一次做java要接消息队列，没弄过，不会弄，我找java架构师，他比较忙，总是敷衍一下，或者发些链接资料让我看，我还是搞不定啊，等下班晚上看他在吃外卖的时候，我买了瓶饮料过去：“春哥，喝瓶饮料，顺势说我接的队列的还有问题，有时间了帮我看下呗”，果然第二天写了个demo给我，到我电脑前帮我运行起来了，搞定。

5、以后的规划

答：我想1-2年内在掌握现有业务技术的前提下学习更多更新的技术；后面有机会的话也是希望做开发管理岗位；俗话说不想当将军的小兵不是好小兵，后面如果有机会也会去独立带团队。

6、离职的原因

答：(1)目前用的技术并不新，且在公司呆了3年左右，现在大部分都是业务代码，重复的增删改成，现在想去换个环境接触下不同的技术及业务，学习更多的东西

(2)目前的同事在公司的年限比较久，稳定性也比较高，目前我想做开发管理岗在现公司的几率也不大，也希望在新公司掌握现有业务后如果有机会也是向上升吧；

7、现在的薪资多少

答：N*14

8、你有什么想问的

答：咱们的主要开发语言是java吗，因为我是做.net的，中间有过度学习期吗

![图片](B%E7%AB%99.assets/640-20210706133147965)

### **2021.6.25HR面** : (上午面完发完流水，下午5点最后谈薪资)

1、说说你最值得说的项目，场景是什么，周期多久

答：我做的勋章项目吧，会员消费一定金额后会点亮一些勋章，并且在勋章墙里可以看到一些勋章的点亮进度，比如我今天购物结束准备回去了，看到我的勋章墙里有一个勋章显示990/1000，再消费10元就能获得这个勋章了，那我再买个10块钱东西吧，可以促进会员消费；

这个项目的背景是销售已经做了ppt把这个项目卖出去了好几家，已经跟公司签了合同了，所以需要我们紧急开发，周期在一个半月左右，我是项目负责人

2、后面的规划是什么

答:见四面5

3、你和那些晋升特别快的人比起来有哪些不同(学习能力的不同，但是勤能补拙)

答:我觉得咱们大部分人的智商、学习能力都差不多，但是总有那些厉害的人，学历高，学习快，能力强，晋升快，我和他们比起来肯定有差距，但是我觉得这些固然是你的资本，但是我们学历能力也还可以，加上我们肯学，我们在某个方向或者某块技术也能成为天花板的存在，最近流行一个字词"躺平"，我不赞同，我觉得现在的人，尤其是我们程序员，即使不能成为某个技术某个时代的引领者，也要成为这些最新技术的学习者，跟上时代的脚步，不停的学习，这样才能提高你的价值。

4、期望薪资是多少？近一年的工资流水发我下

答：期望薪资28K

5、以前都是远程面，今天过来主要是看下工作环境，熟悉下路线，本着双方都负责的态度见面下，加下微信

最终历时一个多月2021.6.28我拿到了B站的offer；

虽然卡我薪资了，没达到我预期，但是也可以接受吧，因为我去年年底晋升加薪了，说我才涨薪半年，原来是14薪，现在是15，总包涨幅30%左右，试用期6个月，每天15餐补，8点半以后有加班餐

转正后每个月多1000补贴

## 面试准备

1、前面找一些小公司面试练练手，找找感觉，总结统计下不会的问题，然后再去找相关资料学习相关知识点，我这2个月大概面试有近10加公司吧，一共拿了3家offer，前面很多都没过，但是为后面拿更好的offer做准备

2、我是如何准备的呢

(1)首先就是简历，关注"三太子敖丙"里面有简历模板，参考下，我是参考了里面的模板修改了，然后在脉脉里找了字节的内推人员，他再帮我简历的一些问题给出了建议

(2)投简历，我用的是Boss直聘，大家也可以用智联招聘、脉脉内推等

(3)学习知识点，我下载了CSDN,关注了一些博主，也可以在里面搜到相关的知识点，如果找视频我一般在软件哔哩哔哩里找相关视频学习;算法一般在“力扣”、“牛客”里刷题，CSDN、牛客也可以找到一些面经参考；

前面我都是按部就班的准备，后面B站一面过了准备二面的时候，我特意告诉HR我要准备一个星期后再二面，这一个星期我的节奏是，早上起来在地铁上找一篇系统的知识点视频看。

比如redis，到公司后正常上班，下班后地铁里也是看视频，到家后看面试视频，并把不会的知识点列出来，我这主要看的是抖音里敖丙面试清华的那个视频，我看了3-4遍。

第一遍简单的过了一下。

第二遍我在遇到不会的知识点的时候记录下来，后面查资料，因为有些答案视频没有，而且我不确定面试者答的是否准确，是否完整。

第三遍我主要是学习下清华面试者的风格，他给我的感觉就是自信，他回答任何问题都是波澜不惊，即使不会的问题他说不会你也不觉得反感,学学他的逻辑性，语速。

第四遍我带着我媳妇一起听，我会把这些问题用自己的回答告诉我媳妇，甚至做一些补充，她是做前端的，我会举例子，如果把她教明白了，我觉得我自己掌握的应该是ok的；

晚上睡觉前我也会看听视频直到睡着，加深印象；没有压力就没有动力，如果不是准备B站的面试，我可能做不到这么努力认真的学习吧。

**平时的学习积累也很重要，平时也不要懈怠哦。**

很多知识点大家最好去是理解这记，当然能和生活的一些经历做出类比那能更好的帮助你理解；

举个例子；线程池的知识点 类比成去海底捞吃饭

整个海底捞就是一个线程池

海底捞里面一共有50个座位——50个核心线程

外面等待区——队列

外面等待区有20座位——70个最大线程数

人太多了，后面的人要排队2个一小时以上，服务员会告诉你，排队时间较久，要不您逛一会，晚点再过来——几种拒绝策略

这样应该就能很好理解线程池和里面一些参数的作用了吧。

### 我面试准备的知识点：

1、设计模式：单例、工厂等，最好能手写单例

2、Redis、Mongdb、Mysql；结构、用户，索引，sql优化、数据如何持久化、备份等

3、RabbitMQ队列，模式，用法

4、线程、进程、线程池

5、http请求，三次握手、四次挥手的过程，https和http的区别，MVC请求过程，路由解析

6、GC，垃圾回收机制的几种算法

7、二叉树、平衡二叉树、B树、B+树、红黑树

8、多线程、并发，锁，锁升级过程

9、事务、隔离级别等

因为我是做.Net转java的，没有准备java相关的知识点，不过我也看了一些，JDK1.7到1.8

数据结构多了红黑树，这些一些参数阈值、系数，链表到红黑树的相互转换，头插改尾插等；

最后祝大家都能找到自己满意的工作。