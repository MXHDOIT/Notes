# Redis

* 概念：Redis是一款高性能的MySQL系列的非关系型数据库(NoSQL)

<hr >

#### 关系型数据库与非关系型数据库的区别

| 关系型数据库           | 非关系型数据库       |
| ---------------------- | -------------------- |
| MySQL,Oracle.....      | Redis,hbase....      |
| 数据之间有关联关系     | 数据之间没有关联关系 |
| 数据存储在硬盘的文件上 | 数据存储在内存中     |
| 二维表                 | key-value            |

#### 经常查询一些不太经常发生变化的数据

![image-20200708224122452](C:\Users\apple\AppData\Roaming\Typora\typora-user-images\image-20200708224122452.png)

<hr >

#### Redis

* Redis是用C语言开发的一个开源的高性能键值对（key-value）数据库，官方提供测试数据，50个并发执行 100000个请求,读的速度是110000次/s,写的速度是81000次/s ，且Redis通过提供多种键值数据类型来适应不同场景下 的存储需求，目前为止Redis支持的键值数据类型如下： 

  1.  字符串类型 string 
  2.  哈希类型 hash 
  3.  列表类型 list 
  4.  集合类型 set 
  5.  有序集合类型 sortedset 

* 命令操作：

  *  redis的数据结构： 

    * redis存储的是：key,value格式的数据，其中key都是字符串，value有5种不同的数据结构 
    * value的数据结构：
      1. 字符串类型 string 
      2. 哈希类型 hash ： map格式 
      3. 列表类型 list ： linkedlist格式。支持重复元素 
      4. 集合类型 set ： 不允许重复元素 
      5. 有序集合类型 sortedset：不允许重复元素，且元素有顺序 

  * 字符串类型 string

    1. 存储：set key value

       ```Redis
       eg：set username zhangsan 
       ```

    2. 获取：get key

       ```
       eg：get username
       ```

    3. 删除：del key

       ```
       eg：del username
       ```

  * 哈希类型 hash

    1. 存储：hset key field value

       ```
       hset myhash username lisi
       hset myhash password 123
       ```

    2. 获取：

       1. hget key field：获取指定的field对应的值

          ```
           hget myhash username
          ```

       2. hgetall key：获取所有的field和value

          ```
          hgetall myhash
          ```

    3. 删除：hdel key field

       ```
       hdel myhash username
       ```

  * 列表类型 list:可以添加一个元素到列表的头部（左边）或者尾部（右边） 

    1. 存储：

       1.  lpush key value: 将元素加入列表左表 
       2.  rpush key value：将元素加入列表右边

       ```
       lpush myList a
       lpush myList b
       lpush myList c
       ```

    2. 获取：lrange key start end ：范围获取 

       ```
       lrange myList 0 -1
       ```

    3. 删除：

       1.  lpop key： 删除列表最左边的元素，并将元素返回 
       2. rpop key： 删除列表最右边的元素，并将元素返回

  * 集合类型 set：不允许重复元素

    1. 存储：sadd key value 

       ```
       sadd myset a
       ```

    2. 获取：smembers key:获取set集合中所有元素

       ```
       smembers myset
       ```

    3. 删除：srem key value:删除set集合中的某个元素 

       ```
       srem myset a
       ```

  * 有序集合类型 sortedset：不允许重复元素，且元素有顺序.每个元素都会关联一个double类型的分数。redis 正是通过分数来为集合中的成员进行从小到大的排序。

    1. 存储：zadd key score value

       ```
       zadd mysort 60 zhangsan
       ```

    2. 获取：zrange key start end [withscores] 

       ```
       zrange mysort 0 -1
       zrange mysort 0 -1 withscores
       ```

    3. 删除：zrem key value

       ```
       zrem mysort lisi
       ```

  * 通用命令：

    1. **keys *：查询所有的键**
    2. **type key：获取键对应的value的类型**
    3. **del key：删除指定的key value**

<hr>
#### Redis持久化

* redis是一个内存数据库，当redis服务器重启，获取电脑重启，数据会丢失，我们可以将redis内存中的数据 **持久化保存到硬盘的文件中**
* Redis持久化机制：
  1. RDB：默认方式，不需要进行配置，默认使用这种机制
     * 在一定的间隔时间中，检测key的变化情况，然后持久化数据 
  2. AOF：日志记录的方式，可以记录每一条命令的操作。可以每一次命令操作后，持久化数据

<hr>

#### Java客户端Jedis

* java操作redis数据库的工具

* 使用步骤：

  1. 下载jedis的jar包

  2. 使用：

     1. 获取连接

        ```
        Jedis jedis = new Jedis("localhost",6379);
        ```

     2. 操作

        ```
        jedis.set("username","zhangsan");
        ```

     3. 关闭连接

        ```
        jedis.close();
        ```

* 注意事项：使用Redis缓存一些不经常发生变化的数据

  * 常和数据库配合使用
    * 数据库的数据一旦发生改变，则需要更新缓存
    * 数据库的表执行增删改的相关操作，需要将Redis缓存数据情况，再次存入
    * 在service对应的增删改方法中，将Redis数据删除

<hr>

