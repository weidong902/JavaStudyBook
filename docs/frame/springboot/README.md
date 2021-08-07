# SpringBoot整合各大框架

## 一、SpringBoot整合mybatis

### 1、引入依赖

```xml
		<!--引入数据源-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.2.4</version>
        </dependency>
        <!--MySQL驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.38</version>
        </dependency>

        <!--mybatis-spring-boot-starter-->
        <!-- https://mvnrepository.com/artifact/org.mybatis.spring.boot/mybatis-spring-boot-starter -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.4</version>
        </dependency>
        <!--springboot 整合mybatis版本中默认依赖mybatis，因此不需要引入额外的mybatis，否则会出现冲突-->
```

### 2、配置配置文件：

```yml
server:
  port: 8081
  servlet:
    context-path: /springboot

# 整合mybatis相关配置
spring:
  mvc:
    view:
      prefix: /
      suffix: .jsp
  #数据源配置    
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/bootssm?characterEncoding=UTF-8
    username: root
    password: 3.014/*-+
    # 数据源指定
# mybatis配置
mybatis:
  mapper-locations: classpath:com/weidd/best/springboot_mybatis/mapper/*.xml # 指定mapper配置文件
  type-aliases-package: com.weidd.best.springboot_mybatis.pojo # 指定实体类的包名,默认别名:类名 首字母小写

```

注：springboot自动开启事务，不需要配置 ，只需要在需要添加事务的地方加`@Transactional`注解就可以

#### 最重要的是要在入口类中加入：`@MapperScan()`注解

```java
@SpringBootApplication
// Springboot整合mybatis必须在入口类上加这个注解:@MapperScan （还可以在dao接口上加@Mapper注解，和@MapperScan功能一致）
@MapperScan("com.weidd.best.springboot_mybatis.mapper") // 修饰范围:用在类上,作用:扫描dao接口所在的包,同时将所有dao接口在工厂中创建对象(代理对象)
public class SpringbootMybatisApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootMybatisApplication.class, args);
    }
}
```

