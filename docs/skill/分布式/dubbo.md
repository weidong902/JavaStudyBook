# Dubbbo

## Dubbo：（RPC框架、服务治理和资源调度）

* 什么是Dubbo？

  ![image-20210516105209144](dubbo.assets/image-20210516105209144.png)

  ## 架构的演变

  ![image-20210516135356629](dubbo.assets/image-20210516135356629.png)



![image-20210516105740161](dubbo.assets/image-20210516105740161.png)



![image-20210516134701602](dubbo.assets/image-20210516134701602.png)

## 好的架构不是设计出来的，而是演化出来的 。

## Dubbo架构

![image-20210516135607935](dubbo.assets/image-20210516135607935.png)



### dubbo的配置优先级

* 方法级优先，接口次之，全局配置再次之
* 如果优先级别一样，消费方优先，提供方次之。





![image-20210521155643898](dubbo.assets/image-20210521155643898.png)

![image-20210521155550246](dubbo.assets/image-20210521155550246.png)



一致性hash会有热点问题。dubbo引入虚拟结点160份虚拟节点 

![image-20210521155912271](dubbo.assets/image-20210521155912271.png)















