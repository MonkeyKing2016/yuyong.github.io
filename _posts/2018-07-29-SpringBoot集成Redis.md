---
layout: post
title: 'SpringBoot集成Redis'
subtitle: '简单的集成SpringDataRedis'
date: 2018-07-29
categories: SpringBoot 集成
cover: ''
tags: SpringBoot Redis
---

> 该demo简单为SpringBoot集成Redis教程
>
> 编辑器：IDEA
>
> 所用技术：SpringBoot Redis Maven
>
> 版本：SpringBoot-2.0.5
>
> demo地址：[SpringBoot集成Redis](https://github.com/MonkeyKing2016/spring-boot-demo-share/tree/master/springboot-redis)
>
> [SpringDataRedis文档地址](https://docs.spring.io/spring-data/data-redis/docs/2.0.5.RELEASE/reference/html/)

## 创建项目方式

### 通过 IDEA 编辑器创建。

创建方式参见

[简单的SpringBoot项目搭建](https://blog.monkeykingshare.top/2018/07/28/SpringBoot%E7%AE%80%E5%8D%95%E7%9A%84%E7%A4%BA%E4%BE%8Bdemo.html)

只需在第三步中添加`NoSql`组件中的`Redis`组件即可

### 修改pom配置依赖

在`pom.xml`中修改如下依赖

```xml
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

## properties配置

可以根据自己的需要动态调整。

```properties
# Redis服务器主机。
spring.redis.host=localhost
# 登录redis服务器的密码。
spring.redis.password=
# 启用SSL支持。
spring.redis.ssl=false
# 池在给定时间可以分配的最大连接数。使用负值无限制。
spring.redis.jedis.pool.max-active=8
# 池中“空闲”连接的最大数量。使用负值表示无限数量的空闲连接。
spring.redis.jedis.pool.max-idle=8
# 连接分配在池被耗尽时抛出异常之前应该阻塞的最长时间量（以毫秒为单位）。使用负值可以无限期地阻止。
spring.redis.jedis.pool.max-wait=-1ms
# 目标为保持在池中的最小空闲连接数。这个设置只有在正面的情况下才有效果。
spring.redis.jedis.pool.min-idle=0
# Redis服务器端口。
spring.redis.port=6379
# （哨兵模式，不使用则不用开启）Redis服务器的名称。
# spring.redis.sentinel.master=
# （哨兵模式，不使用则不用开启）主机：端口对的逗号分隔列表。
# spring.redis.sentinel.nodes=
# 以毫秒为单位的连接超时。
spring.redis.timeout=3000ms
```

## RedisTemplate配置

```java
@Configuration
@EnableCaching
public class RedisConfig {

    @Bean
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<Object, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(factory);
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        template.setKeySerializer(new StringRedisSerializer());
        template.setValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();
        return template;
    }
}
```

## 测试类

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class RedisApplicationTests {

   @Autowired
   private RedisTemplate<String,String> redisTemplate;

   @Test
   public void contextLoads() {
      String key = "redis:test:01";
      redisTemplate.opsForValue().set(key,"HelloWord");
      String result = redisTemplate.opsForValue().get(key);
      System.out.println(result);
   }

}
```

输出结果

`HelloWord`

redis 命令行执行命令查询结果

```bash
127.0.0.1:6379> get redis:test:01
"HelloWord"
```

## 进阶操作

我不喜欢像别人一样弄些工具类去访问`redis`。其实这些工具类`spring data redis` 皆有提供。

| 接口                  | 描述                                                    |
| --------------------- | ------------------------------------------------------- |
| *密钥类型操作*        |                                                         |
| GeoOperations         | Redis的地理空间操作的像`GEOADD`，`GEORADIUS`...）       |
| HashOperations        | Redis哈希操作                                           |
| HyperLogLogOperations | Redis的HyperLogLog操作，比如（`PFADD`，`PFCOUNT`，...） |
| ListOperations        | Redis列表操作                                           |
| SetOperations         | Redis设置了操作                                         |
| ValueOperations       | Redis字符串（或值）操作                                 |
| ZSetOperations        | Redis zset（或排序集）操作                              |
| *键绑定操作*          |                                                         |
| BoundGeoOperations    | Redis键绑定地理空间操作。                               |
| BoundHashOperations   | Redis散列键绑定操作                                     |
| BoundKeyOperations    | Redis键绑定操作                                         |
| BoundListOperations   | Redis列出键绑定操作                                     |
| BoundSetOperations    | Redis设置键绑定操作                                     |
| BoundValueOperations  | Redis字符串（或值）键绑定操作                           |
| BoundZSetOperations   | Redis zset（或有序集）键绑定操作                        |

> [Working with Objects through RedisTemplate](https://docs.spring.io/spring-data/data-redis/docs/2.0.5.RELEASE/reference/html/#redis:template)

例如只使用`ValueOperations`

在测试类中编写如下代码

```java
@Resource(name = "redisTemplate")
private ValueOperations<String, String> valueOperations;

@Test
public void valueOperations() {
   String key = "redis:test:valueOperations:01";
   valueOperations.set(key,"HelloWord");
   String result = valueOperations.get(key);
   System.out.println(result);
}
```

效果和上面测试中使用`redisTemplate`效果相同