> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/love521314123/article/details/120412583?spm=1001.2014.3001.5506)

Redis
-----

> [gitee](https://so.csdn.net/so/search?q=gitee&spm=1001.2101.3001.7020) 地址：[https://gitee.com/infiniteStars/redis](https://gitee.com/infiniteStars/redis)  
> 根据 B 站狂神说视频整理得到的，建议一边看视频，一边看笔记。

### 1.NoSQL

*   NoSQl== Not Only SQL
*   泛指非关系型数据库。

#### Nosql 特点

*   方便扩展（数据之间没有关系，很好扩展！）
    
*   大数据量高性能（Redis 一秒可以写 8 万次，读 11 万次）
    
*   数据类型是多样型的！（不需要事先设计数据库，随取随用）
    

#### Nosql 的四大分类

> ##### KV 键值对

*   新浪：Redis
    
*   美团：Redis + Tair
    
*   阿里、百度：Redis + Memcache
    

> ##### 文档型数据库（bson 数据格式）

*   MongoDB(掌握)
    
*   基于分布式文件存储的数据库。C++ 编写，用于处理大量文档。
    
*   MongoDB 是 RDBMS 和 NoSQL 的中间产品。MongoDB 是[非关系型数据库](https://so.csdn.net/so/search?q=%E9%9D%9E%E5%85%B3%E7%B3%BB%E5%9E%8B%E6%95%B0%E6%8D%AE%E5%BA%93&spm=1001.2101.3001.7020)中功能最丰富的，NoSQL 中最像关系型数据库的数据库。  
    ConthDB
    

> ##### 列存储数据库

*   HBase(大数据必学)
    
*   分布式文件系统
    

> ##### 图关系数据库

*   用于广告推荐，社交网络
    
*   **Neo4j**、InfoGrid
    

### 2.Redis

#### 1. 简介

*   Redis（Remote Dictionary Server )，即远程字典服务。
    
*   是一个开源的使 **C 语言编写**、**支持网络**、可基于内存亦可持久化的日志型、Key-Value 数据库，并提供多种语言的 API。
    

> Redis 能干什么？

*   内存存储、持久化，内存是断电即失的，所以需要持久化（RDB、AOF）
*   高效率、用于高速缓冲
*   发布订阅系统
*   地图信息分析
*   计时器、计数器 (浏览量)

> 特性

*   多样的数据类型
*   持久化
*   集群
*   事务

#### 2. 在 Linux 系统上安装

1.  到官网下载安装包 https://redis.io/
2.  传输到 opt 文件夹下
3.  解压 tar -zxvf redis-6.2.3.tar.gz
4.  基本的环境安装

```
yum install gcc-c++
make
make install

```

5.  默认安装路径 usr/local/bin
    
6.  将配置文件 copy
    

```
mkdir dconfig
cp /opt/redis-6.2.3/redis.conf dconfig


```

7.  redis 默认不是后台启动，需要更改配置

```
vim redis.conf

```

![](https://i-blog.csdnimg.cn/blog_migrate/2d48f8ceadcbc140a33d09722cc98d00.png)

*   按 ESC 退出编辑模式
*   按 ： 号进入底线命令模式
*   按 wq 退出并保存

8.  启动 redis 服务

```
redis-server dconfig/redis.conf 

```

![](https://i-blog.csdnimg.cn/blog_migrate/4678bd20926db9b633b38b919fed974d.png)

9.  测试连接是否成功  
    ![](https://i-blog.csdnimg.cn/blog_migrate/e123d255d63e40cfaef0a98b3a76225d.png)
    
10.  查看 redis 进程是否开启
    

```
ps -ef|grep redis

```

![](https://i-blog.csdnimg.cn/blog_migrate/e4155a486ecb61941214fa70bb66a26b.png)

11.  退出 Redis

```
shutdown
exit

```

#### 3.Redis 基础知识

*   一共有 16 个数据库，默认使用第 0 个。
*   一些基本命令

```
select 3  #切换数据库
dbsize    #查看数据库存放文件的大小
keys *   #查看某个数据库所有的属性
flushall   #清空所有内容
flushdb    #清空当前库


```

*   Redis 是单线程的。

> Redis 是单线程的，为什么还这么快？

**redis 是将所有的数据都放在内存中。对于内存系统来说，没有上下文切换效率最高，而多线程会进行上下文切换（CPU 做的）。所以，单线程去操作是效率最高的。**

#### 4. 启动 Redis

1.  进入 usr/local/bin 文件夹
    
2.  ```
    redis-server dconfig/redis.conf  #连接
    
    ```
    
3.  测试连接
    

```
redis-cli -p 6379
ping

```

### 3. 数据类型

Redis 是一个开源（BSD 许可）的，内存中的数据结构存储系统，它可以用作**数据库、缓存和消息中间件**。 它支持多种类型的数据结构，如 [字符串（strings）](http://www.redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://www.redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://www.redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://www.redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://www.redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://www.redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://www.redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://www.redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://www.redis.cn/topics/replication.html)，[LUA 脚本（Lua scripting）](http://www.redis.cn/commands/eval.html)， [LRU 驱动事件（LRU eviction）](http://www.redis.cn/topics/lru-cache.html)，[事务（transactions）](http://www.redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://www.redis.cn/topics/persistence.html)， 并通过 [Redis 哨兵（Sentinel）](http://www.redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://www.redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）。

#### 1.Redis-Key

在 redis 中无论什么数据类型，在数据库中都是以 key-value 形式保存，通过进行对 Redis-key 的操作，来完成对数据库中数据的操作。

*   一些命令

```
exists name    #是否存在name这个属性
expire name 10  #10秒后过期
ttl name    #查看当前key的剩余时间
type name  #查看当前数据类型

```

#### 2.String

```
set key1 v1
append key1 "hello"   #在后边添加内容，双引号可以加也可以不加
strlen key1  #获取字符串的长度

===============================
set views 0
incr views  #增加1
decr views  #降低一
incrby views 10  #一次增加10
decrby views 5  #一次减少5

============================
getrange key1 0 3   # 获取【0,3】的字符串
getrange key1 0 -1   # 获取所有的字符串
setrange key1  1 xx   # 从1开始往后替换

============================
setex key3 30 123   # 设置30秒后自动过期
setnx  mykey  hello  # key不存在创建，存在则创建失败

===========================
mset key1 v1 key2 v2 key3 v3  #批量创建key
mget key1 key2 key3
msetnx key1 v1 key4 v4   # msetnx 原子性操作，要么一起成功，要么一起失败

==========================
set user:1 {name:dz,age:18}   # 创建一个对象
get user:1    # "{name:dz,age:3}"


```

#### 3.List

*   双向链表 lpush 从左侧放入 rpush 从右侧放入

```
lpush list 1   #向list中存值
lpush list 2
lpush list 3
rpush list 4   #从右侧存值

===============================
lrange list  0 2  #获取值 倒着取值
lrange list  0 -1 #取所有的值
lindex list 1     #从左边 下标取值  没有rindex
===============================
lpop list  #从左边移除值
rpop list  #从右边移除值

=====================
Llen   # 获取长度
lrem list 1 1  #移除指定的值  第一个参数是数量  第二个是移除的元素  


```

#### 4.Hash

#### 5.Hash

#### 6.Zset

*   有序列表

#### 7.Geospatial 地理位置

```
geoadd china:city 116.40 39.90 beijing    #插入数据 可以一次插入多个  参数含义：描述 经度 纬度 名称

geopos china:city beijing   # 获取指定位置的经度和纬度
   1) "116.39999896287918091"
   2) "39.90000009167092543"

```

> 获取两站之间的位置 单位

*   **m** 表示单位为米。
*   **km** 表示单位为千米。
*   **mi** 表示单位为英里。
*   **ft** 表示单位为英尺。

```
geodist china:city beijing shanghai km    #距离单位默认为 m 

```

#### 8.Hyperloglog 基数统计

#### 9.Bitmap 位图场景

### 4. 事务

#### 1. 基础命令

*   Redis 单条命令保证原子性，但是事务不保证原子性。
    
*   原子性：要么同时成功，要么同时失败。
    
*   Redis 事务本质：一组命令的集合。
    
*   特点：1. 一次性 （一次执行完一组命令） 2. 顺序性 3. 排他性（在执行过程中，其他命令不能干扰）
    
*   所有在事务中的命令，并没有被执行。只有发起**执行命令**时才会被执行。
    
*   事务的三个命令
    

1.  开启事务（multi）
2.  命令入队（…）
3.  执行事务 (exec)
4.  取消事务 （discard）

```
127.0.0.1:6379> multi   #开启事务
OK
127.0.0.1:6379(TX)> set k1 v1  #入队
QUEUED
127.0.0.1:6379(TX)> set k2 v2
QUEUED
127.0.0.1:6379(TX)> get k1
QUEUED
127.0.0.1:6379(TX)> exec  #执行事务
1) OK
2) OK
3) "v1"

```

#### 2. 事务错误

1.  编译型异常（代码错误！命令有错！）所有的命令都不执行
2.  运行时异常（语法错误 ，例如 1/0） 其他命令可以正常执行，错误命令抛出异常。

#### 3. 乐观锁（watch）

*   在执行事务之前开启 watch。
    
*   Watch 命令用于监视一个 (或多个) key ，如果在事务执行之前这个 (或这些) key 被其他命令所改动，那么事务将被打断。返回 nil
    
*   在事务执行失败之后，watch 也失效了。
    
*   在事务执行时，被其他命令改动，此时 取消 watch 命令（unwatch)，事务任然会被打断。
    

### 5.Jedis

*   [Jedis](https://link.jianshu.com?t=https://github.com/xetorthio/jedis) 是 Redis 官方推荐的 Java 连接开发工具。

1.  导入依赖

```
<dependencies>
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>3.2.0</version>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>fastjson</artifactId>
        <version>1.2.62</version>
    </dependency>
</dependencies>

```

2.  下载 redis https://github.com/tporadowski/redis/releases 开启 redis 服务

![](https://i-blog.csdnimg.cn/blog_migrate/2aff1074df0c6ddbf92d5e96895674a6.png)

3.  测试连接是否成功

```
  public static void main(String[] args) {
        Jedis jedis = new Jedis("127.0.0.1", 6379);
        System.out.println(jedis.ping());
    }

```

4.  出现这个就成功了。

![](https://i-blog.csdnimg.cn/blog_migrate/cf8522d5ead13a308d651451bd728b89.png)

### 6.SpringBoot 集成 Redis

#### 1. 使用 redis

*   在创建 springboot 项目时，数据库选择 NoSQL 中的 redis。
*   在测试类中进行测试 输出值就成功了。

```
 @Autowired
private RedisTemplate redisTemplate;
@Test
void contextLoads() {
    redisTemplate.opsForValue().set("name","dzsq");    //操作String类
    System.out.println(redisTemplate.opsForValue().get("name"));
}

```

#### 2.redis 存储对象

*   不能直接存储对象，先要将对象序列化。

1.  获取对象后进行序列化

```
@Test
public void test01() throws JsonProcessingException {
    User user = new User("dz", 3);
    //将对象序列化
    String jsonUser = new ObjectMapper().writeValueAsString(user);
    redisTemplate.opsForValue().set("user",jsonUser);
    System.out.println(redisTemplate.opsForValue().get("user"));
}

```

2.  在对象类上进行序列化

```
public class User implements Serializable {
    private String name;
    private int age;
}

```

#### 3. 配置 RedisTemplate

*   使用 springboot 自带的 redisTemplate，在 redis 控制台获取值时 是一串我们看不懂的字符。所以需要配置自己的 RedisTemplate。

```
@Configuration
public class RedisConfig {
    @Bean
    @SuppressWarnings("all")
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<String, Object>();
        template.setConnectionFactory(factory);
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();
        // key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        // hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        // value序列化方式采用jackson
        template.setValueSerializer(jackson2JsonRedisSerializer);
        // hash的value序列化方式采用jackson
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();
        return template;
    }
}

```

#### 4. 封装工具类

```
package com.dz.utils;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;
import org.springframework.util.CollectionUtils;

import java.util.Collection;
import java.util.List;
import java.util.Map;
import java.util.Set;
import java.util.concurrent.TimeUnit;

@Component
public final class RedisUtil {
    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    // =============================common============================
    /**
     * 指定缓存失效时间
     * @param key  键
     * @param time 时间(秒)
     */
    public boolean expire(String key, long time) {
        try {
            if (time > 0) {
                redisTemplate.expire(key, time, TimeUnit.SECONDS);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 根据key 获取过期时间
     * @param key 键 不能为null
     * @return 时间(秒) 返回0代表为永久有效
     */
    public long getExpire(String key) {
        return redisTemplate.getExpire(key, TimeUnit.SECONDS);
    }


    /**
     * 判断key是否存在
     * @param key 键
     * @return true 存在 false不存在
     */
    public boolean hasKey(String key) {
        try {
            return redisTemplate.hasKey(key);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 删除缓存
     * @param key 可以传一个值 或多个
     */
    @SuppressWarnings("unchecked")
    public void del(String... key) {
        if (key != null && key.length > 0) {
            if (key.length == 1) {
                redisTemplate.delete(key[0]);
            } else {
                redisTemplate.delete((Collection<String>) CollectionUtils.arrayToList(key));
            }
        }
    }


    // ============================String=============================

    /**
     * 普通缓存获取
     * @param key 键
     * @return 值
     */
    public Object get(String key) {
        return key == null ? null : redisTemplate.opsForValue().get(key);
    }

    /**
     * 普通缓存放入
     * @param key   键
     * @param value 值
     * @return true成功 false失败
     */

    public boolean set(String key, Object value) {
        try {
            redisTemplate.opsForValue().set(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 普通缓存放入并设置时间
     * @param key   键
     * @param value 值
     * @param time  时间(秒) time要大于0 如果time小于等于0 将设置无限期
     * @return true成功 false 失败
     */

    public boolean set(String key, Object value, long time) {
        try {
            if (time > 0) {
                redisTemplate.opsForValue().set(key, value, time, TimeUnit.SECONDS);
            } else {
                set(key, value);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 递增
     * @param key   键
     * @param delta 要增加几(大于0)
     */
    public long incr(String key, long delta) {
        if (delta < 0) {
            throw new RuntimeException("递增因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key, delta);
    }


    /**
     * 递减
     * @param key   键
     * @param delta 要减少几(小于0)
     */
    public long decr(String key, long delta) {
        if (delta < 0) {
            throw new RuntimeException("递减因子必须大于0");
        }
        return redisTemplate.opsForValue().increment(key, -delta);
    }


    // ================================Map=================================

    /**
     * HashGet
     * @param key  键 不能为null
     * @param item 项 不能为null
     */
    public Object hget(String key, String item) {
        return redisTemplate.opsForHash().get(key, item);
    }

    /**
     * 获取hashKey对应的所有键值
     * @param key 键
     * @return 对应的多个键值
     */
    public Map<Object, Object> hmget(String key) {
        return redisTemplate.opsForHash().entries(key);
    }

    /**
     * HashSet
     * @param key 键
     * @param map 对应多个键值
     */
    public boolean hmset(String key, Map<String, Object> map) {
        try {
            redisTemplate.opsForHash().putAll(key, map);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * HashSet 并设置时间
     * @param key  键
     * @param map  对应多个键值
     * @param time 时间(秒)
     * @return true成功 false失败
     */
    public boolean hmset(String key, Map<String, Object> map, long time) {
        try {
            redisTemplate.opsForHash().putAll(key, map);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 向一张hash表中放入数据,如果不存在将创建
     *
     * @param key   键
     * @param item  项
     * @param value 值
     * @return true 成功 false失败
     */
    public boolean hset(String key, String item, Object value) {
        try {
            redisTemplate.opsForHash().put(key, item, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }

    /**
     * 向一张hash表中放入数据,如果不存在将创建
     *
     * @param key   键
     * @param item  项
     * @param value 值
     * @param time  时间(秒) 注意:如果已存在的hash表有时间,这里将会替换原有的时间
     * @return true 成功 false失败
     */
    public boolean hset(String key, String item, Object value, long time) {
        try {
            redisTemplate.opsForHash().put(key, item, value);
            if (time > 0) {
                expire(key, time);
            }
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 删除hash表中的值
     *
     * @param key  键 不能为null
     * @param item 项 可以使多个 不能为null
     */
    public void hdel(String key, Object... item) {
        redisTemplate.opsForHash().delete(key, item);
    }


    /**
     * 判断hash表中是否有该项的值
     *
     * @param key  键 不能为null
     * @param item 项 不能为null
     * @return true 存在 false不存在
     */
    public boolean hHasKey(String key, String item) {
        return redisTemplate.opsForHash().hasKey(key, item);
    }


    /**
     * hash递增 如果不存在,就会创建一个 并把新增后的值返回
     *
     * @param key  键
     * @param item 项
     * @param by   要增加几(大于0)
     */
    public double hincr(String key, String item, double by) {
        return redisTemplate.opsForHash().increment(key, item, by);
    }


    /**
     * hash递减
     *
     * @param key  键
     * @param item 项
     * @param by   要减少记(小于0)
     */
    public double hdecr(String key, String item, double by) {
        return redisTemplate.opsForHash().increment(key, item, -by);
    }


    // ============================set=============================

    /**
     * 根据key获取Set中的所有值
     * @param key 键
     */
    public Set<Object> sGet(String key) {
        try {
            return redisTemplate.opsForSet().members(key);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 根据value从一个set中查询,是否存在
     *
     * @param key   键
     * @param value 值
     * @return true 存在 false不存在
     */
    public boolean sHasKey(String key, Object value) {
        try {
            return redisTemplate.opsForSet().isMember(key, value);
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 将数据放入set缓存
     *
     * @param key    键
     * @param values 值 可以是多个
     * @return 成功个数
     */
    public long sSet(String key, Object... values) {
        try {
            return redisTemplate.opsForSet().add(key, values);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 将set数据放入缓存
     *
     * @param key    键
     * @param time   时间(秒)
     * @param values 值 可以是多个
     * @return 成功个数
     */
    public long sSetAndTime(String key, long time, Object... values) {
        try {
            Long count = redisTemplate.opsForSet().add(key, values);
            if (time > 0)
                expire(key, time);
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 获取set缓存的长度
     *
     * @param key 键
     */
    public long sGetSetSize(String key) {
        try {
            return redisTemplate.opsForSet().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 移除值为value的
     *
     * @param key    键
     * @param values 值 可以是多个
     * @return 移除的个数
     */

    public long setRemove(String key, Object... values) {
        try {
            Long count = redisTemplate.opsForSet().remove(key, values);
            return count;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }

    // ===============================list=================================

    /**
     * 获取list缓存的内容
     *
     * @param key   键
     * @param start 开始
     * @param end   结束 0 到 -1代表所有值
     */
    public List<Object> lGet(String key, long start, long end) {
        try {
            return redisTemplate.opsForList().range(key, start, end);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 获取list缓存的长度
     *
     * @param key 键
     */
    public long lGetListSize(String key) {
        try {
            return redisTemplate.opsForList().size(key);
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }
    }


    /**
     * 通过索引 获取list中的值
     *
     * @param key   键
     * @param index 索引 index>=0时， 0 表头，1 第二个元素，依次类推；index<0时，-1，表尾，-2倒数第二个元素，依次类推
     */
    public Object lGetIndex(String key, long index) {
        try {
            return redisTemplate.opsForList().index(key, index);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }


    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     */
    public boolean lSet(String key, Object value) {
        try {
            redisTemplate.opsForList().rightPush(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 将list放入缓存
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     */
    public boolean lSet(String key, Object value, long time) {
        try {
            redisTemplate.opsForList().rightPush(key, value);
            if (time > 0)
                expire(key, time);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }

    }


    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @return
     */
    public boolean lSet(String key, List<Object> value) {
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }

    }


    /**
     * 将list放入缓存
     *
     * @param key   键
     * @param value 值
     * @param time  时间(秒)
     * @return
     */
    public boolean lSet(String key, List<Object> value, long time) {
        try {
            redisTemplate.opsForList().rightPushAll(key, value);
            if (time > 0)
                expire(key, time);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 根据索引修改list中的某条数据
     *
     * @param key   键
     * @param index 索引
     * @param value 值
     * @return
     */

    public boolean lUpdateIndex(String key, long index, Object value) {
        try {
            redisTemplate.opsForList().set(key, index, value);
            return true;
        } catch (Exception e) {
            e.printStackTrace();
            return false;
        }
    }


    /**
     * 移除N个值为value
     *
     * @param key   键
     * @param count 移除多少个
     * @param value 值
     * @return 移除的个数
     */

    public long lRemove(String key, long count, Object value) {
        try {
            Long remove = redisTemplate.opsForList().remove(key, count, value);
            return remove;
        } catch (Exception e) {
            e.printStackTrace();
            return 0;
        }

    }

}

```

*   测试

```
@Test
public void test02(){
    redisUtil.set("name","dzsq");
    System.out.println(redisUtil.get("name"));
}

```

*   工具类中的方法总结

```
expire(String key, long time)    //指定缓存失效时间
getExpire(String key)            //根据key 获取过期时间
hasKey(String key)               //判断key是否存在
del(String... key)               //删除缓存
get(String key)                  //获取值
set(String key, Object value)    //设置值  
set(String key, Object value, long time)  //放入值并设置过期时间
incr(String key, long delta)     //递增 参数：delta 增加几
decr(String key, long delta)     //递减

```

#### 5. 实现增删改查功能

*   主要在 Service 中进行操作
*   查询单个用户时，先在缓存中查找，如果没有，在从数据库中进行查找，再保存到缓存中，
*   删除用户，先将数据库中的数据删除，再删除缓存中的数据
*   修改用户，先更新数据表，成功之后，删除原来的缓存，再更新缓存

### 7. 持久化

*   Redis 是内存数据库。如果不将数据库状态保存在硬盘中。断电即失。所以 Redis 提供了持久化技术。

#### 1.RDB（Redis DataBase）

> 操作原理

*   在指定时间间隔内，将内存中的数据集快照写入数据库 ；在恢复时候，直接读取快照文件，进行数据的恢复 ；
    
*   在进行 RDB 的时候，redis 的主线程是不会做 io 操作的，主线程会 fork 一个子线程来完成该操作；
    
*   Redis 调用 forks。同时拥有父进程和子进程。子进程将数据集写入到一个临时 RDB 文件中。
    
*   当子进程完成对新 RDB 文件的写入时，Redis 用新 RDB 文件替换原来的 RDB 文件，并删除旧的 RDB 文件。
    
*   缺点是最后一次持久化后的数据可能丢失。
    
*   默认是使用 RDB 方式来进行持久化。
    
*   rdb 文件默认名 dump.rdb
    

> 触发规则

1.  满足 save 规则

[外链图片转存失败, 源站可能有防盗链机制, 建议将图片保存下来直接上传 (img-Ez8Ot7B3-1632283167906)(C:\Users\86155\AppData\Roaming\Typora\typora-user-images\image-20210527164816684.png)]

2.  执行 flushall 命令
3.  退出 redis 。 命令（shutdown）

> 恢复数据

只要 rdb 文件放在 redis 的启动目录就行了。 启动目录： /usr/local/bin

**优点：**

1.  适合大规模的数据恢复
2.  对数据的完整性要求不高

**缺点：**

1.  需要一定的时间间隔进行操作，如果 redis 意外宕机了，这个最后一次修改的数据就没有了。
2.  fork 进程的时候，会占用一定的内存空间。

#### 2.AOF(Append only File)

> 原理

*   将所有的命令都记录下来，恢复数据时全部都执行一遍。

默认保存在 appendonly.aof 文件中。

默认不开启。想要开启的话，将 appendonly 后边的 No 改为 yes。

![](https://i-blog.csdnimg.cn/blog_migrate/8ef5ee31ff5caa6ebe1d766821c77090.png)

如果这个 aof 文件被修改过，这时候 redis 是启动不起来的，需要修改这个 aof 文件。

redis 给我们提供了一个工具，执行下边命令，会自动修复。

```
redis-check-aof --fix appendonly.aof   

```

**优点**

1.  每一次修改都会同步，文件的完整性会更加好
2.  每秒同步一次，可能会丢失一秒的数据
3.  从不同步，效率最高

**缺点**

1.  相对于数据文件来说，aof 远远大于 rdb，修复速度比 rdb 慢！
2.  Aof 运行效率也要比 rdb 慢，所以我们 redis 默认的配置就是 rdb 持久化

#### 3. 对比

<table><thead><tr><th></th><th>RDB</th><th>AOF</th></tr></thead><tbody><tr><td>启动优先级</td><td>低</td><td>高</td></tr><tr><td>体积</td><td>小</td><td>大</td></tr><tr><td>恢复速度</td><td>快</td><td>慢</td></tr><tr><td>数据安全性</td><td>丢数据</td><td>根据策略决定</td></tr></tbody></table>

### 8.Redis 发布订阅

Redis 发布订阅 (pub/sub) 是一种消息通信模式：发送者 (pub) 发送消息，订阅者 (sub) 接收消息。

![](https://i-blog.csdnimg.cn/blog_migrate/8d125df84d0d136fafd488132f7821c2.png)

> 示例

**订阅端**

```
SUBSCRIBE dzsq   #订阅一个名叫dzsq的频道
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "dzsq"
3) (integer) 1
1) "message"
2) "dzsq"
3) "hello,world"

```

**发布端**

```
PUBLISH dzsq "hello,world"   #发布消息
(integer) 1

```

**命令**（下边三个比较重要）

<table><thead><tr><th>命令</th><th>描述</th></tr></thead><tbody><tr><td>PSUBSCRIBE pattern [pattern…]</td><td>订阅一个或多个符合给定模式的频道。</td></tr><tr><td>PUNSUBSCRIBE pattern [pattern…]</td><td>退订一个或多个符合给定模式的频道。</td></tr><tr><td>PUBSUB subcommand [argument[argument]]</td><td>查看订阅与发布系统状态。</td></tr><tr><td>PUBLISH channel message</td><td>向指定频道发布消息</td></tr><tr><td>SUBSCRIBE channel [channel…]</td><td>订阅给定的一个或多个频道。</td></tr><tr><td>SUBSCRIBE channel [channel…]</td><td>退订一个或多个频道</td></tr></tbody></table>

### 9. 主从复制

#### 1. 概念

*   主从复制，是指将一台 Redis 服务器的数据，复制到其他的 Redis 服务器。前者称为主节点（Master/Leader）, 后者称为从节点（Slave/Follower）， 数据的复制是单向的！只能由主节点复制到从节点（主节点以写为主、从节点以读为主）。
    
*   默认情况下，每台 Redis 服务器都是主节点，一个主节点可以有 0 个或者多个从节点，但每个从节点只能由一个主节点。
    

> 作用

1.  数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余的方式。
    
2.  故障恢复：当主节点故障时，从节点可以暂时替代主节点提供服务，是一种服务冗余的方式
    
3.  负载均衡：在主从复制的基础上，配合读写分离，由主节点进行写操作，从节点进行读操作，分担服务器的负载；尤其是在多读少写
    
    的场景下，通过多个从节点分担负载，提高并发量。
    
4.  高可用基石：主从复制还是哨兵和集群能够实施的基础。
    

#### 2. 环境配置

```
127.0.0.1:6379> info replication   #查看当前库的信息
# Replication
role:master
connected_slaves:0    #从机的数量

```

#### 3. 一主二从配置

== 默认情况下，每台 Redis 服务器都是主节点；== 我们一般情况下只用配置从机就好了！

使用`SLAVEOF host port` 就可以为从机配置主机了。 使用这个命令的服务器就变为从机了。

#### 4. 使用规则

*   从机只能读，不能写，主机可读可写但是 多用于写。
    
*   当主机断电宕机后，默认情况下从机的角色不会发生变化 ，集群中只是失去了写操作，当主机恢复以后，又会连接上从机恢复原状。
    
*   当从机断电宕机后，若不是使用配置文件配置的从机，再次启动后作为主机是无法获取之前主机的数据的，若此时重新配置称为从机，又可以获取到主机的所有数据。
    
*   第二条中提到，默认情况下，主机故障后，不会出现新的主机，有两种方式可以产生新的主机：
    
    *   从机手动执行命令 slaveof no one , 这样执行以后从机会独立出来成为一个主机
    *   使用哨兵模式（自动选举）

### 10. 哨兵模式

*   主从切换技术的方法是：当主服务器宕机后，需要手动把一台从服务器切换为主服务器，这就需要人工干预，费事费力，还会造成一段时间内服务不可用。这不是一种推荐的方式，更多时候，我们优先考虑哨兵模式。

**哨兵的作用**

*   通过发送命令，监控 Redis 服务器的运行状态，包括主服务器和从服务器。
*   当哨兵监测到 master 宕机，会自动将 slave 切换成 master，然后通过发布订阅模式通知其他的从服务器，修改配置文件，让它们切换主机。

**哨兵的核心配置**

```
sentinel monitor mymaster 127.0.0.1 6379 1
#监控master
#数字1表示 ：当一个哨兵主观认为主机断开，就可以认为主机故障，然后开始选举新的主机。

```

### 11. 缓存穿透与雪崩

#### 1. 缓存穿透（查不到）

##### 1. 概念

在默认情况下，用户请求数据时，会先在缓存 (Redis) 中查找，若没找到即缓存未命中，再在数据库中进行查找，数量少可能问题不大，可是一旦大量的请求数据（例如秒杀场景）缓存都没有命中的话，就会全部转移到数据库上，造成数据库极大的压力，就有可能导致数据库崩溃。网络安全中也有人恶意使用这种手段进行攻击被称为洪水攻击。

##### 2. 解决方案

1.  布隆过滤器

*   对所有可能查询的参数以 Hash 的形式存储，以便快速确定是否存在这个值，在控制层先进行拦截校验，校验不通过直接打回，减轻了存储系统的压力。

2.  缓存空对象

*   一次请求若在缓存和数据库中都没找到，就在缓存中放一个空对象用于处理后续这个请求。
    
*   这样做有一个缺陷：存储空对象也需要空间，大量的空对象会耗费一定的空间，存储效率并不高。解决这个缺陷的方式就是设置较短过期时间
    
*   即使对空值设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口的不一致，这对于需要保持一致性的业务会有影响。
    

#### 2. 缓存击穿（量太大，缓存过期）

##### 1. 概念

相较于缓存穿透，缓存击穿的目的性更强，一个存在的 key，在缓存过期的一刻，同时有大量的请求，这些请求都会击穿到数据库，造成瞬时数据库请求量大、压力骤增。这就是缓存被击穿，只是针对其中某个 key 的缓存不可用而导致击穿，但是其他的 key 依然可以使用缓存响应。

比如热搜排行上，一个热点新闻被同时大量访问就可能导致缓存击穿。

##### 2. 解决方案

1.  设置热点数据永不过期

*   这样就不会出现热点数据过期的情况，但是当 Redis 内存空间满的时候也会清理部分数据，而且此种方案会占用空间，一旦热点数据多了起来，就会占用部分空间。

2.  加互斥锁 (分布式锁)

*   在访问 key 之前，采用 SETNX（set if not exists）来设置另一个短期 key 来锁住当前 key 的访问，访问结束再删除该短期 key。保证同时刻只有一个线程访问。这样对锁的要求就十分高。

#### 3. 缓存雪崩

##### 1. 概念

大量的 key 设置了相同的过期时间，导致在缓存在同一时刻全部失效，造成瞬时数据库请求量大、压力骤增，引起雪崩。

##### 2. 解决方案

1.  redis 高可用

*   这个思想的含义是，既然 redis 有可能挂掉，那我多增设几台 redis，这样一台挂掉之后其他的还可以继续工作，其实就是搭建集群

2.  限流降级

*   这个解决方案的思想是，在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如对某个 key 只允许一个线程查询数据和写缓存，其他线程等待。

3.  数据预热

*   数据加热的含义就是在正式部署之前，我先把可能会被大量访问的数据预先访问一遍，这样就会将数据加载到缓存中。在即将发生大并发访问前手动触发加载缓存不同的 key，设置不同的过期时间，让缓存失效的时间点尽量均匀。