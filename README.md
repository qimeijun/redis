# redis

> 简介


`redis`是一款开源的可持久化型、`key-value`数据库，并提供多种API。<br/>

`redis` 的`key`只能是`string`类型的字符串，`value`支持五种数据类型`string, Set, List, Hash, ZSet`.<br/>

> 一、`string` 字符串

最简单的数据类型，一个`key`对应一个`value`， 这里的`value`的支持三种数据类型：字符串、浮点型、数字型。<br/>

![String类型](https://github.com/qimeijun/redis/blob/master/string.png)

简单命令：
 
1、`set key-name value`: 设置value值<br/>
2、`get key-name` : 根据key-name 获取value值<br/>
3、`incr key-name` :  根据key-name 让value自增，对数据类型有用<br/>
4、`desc key-name` : 根据key-name 让value自减，对数据类型有用<br/>
5、`append key-name hello` : 在key-value 原有的值后面追加`hello`<br/>
6、`strlen key-name` : 获取key-value的value值的长度<br/>
7、`mset key1 value1 key2 valu2..`： 同时设置多个值<br/>
8、`mget key1 key2...`：同时得到多个值<br/>
9、`del key-name` : 删除key-name<br/>
10、`getset key-name value2`： 将key-name的value值改为value2<br/>

> 二、`List` 链表型

`List` 是双向链表数据类型，支持反向遍历、查找等操作，有序。<br/>

能够存储多个`value`值，允许重复的value值。从左往右<br/>


![List类型](https://github.com/qimeijun/redis/blob/master/list.png)

简单命令：
1、`lpush key-name value`: 向链表key-name左边插入value<br/>
2、`rpush key-name value`：向链表key-name右边插入value<br/>
3、`lpop key-name` : 向key-name右边弹出元素<br/>
4、`rpop key-name` : 向key-name左边弹出元素<br/>
5、`llen key-name`: 获取key-name列表中的个数<br/>
6、`lrange key-name start stop`: 获取key-name列表中start 到 stop 之间的元素<br/>
7、`lrem key-name count value`: 删除元素，当count>0时从左边开始数，count< 0时从右边开始数，count=0时会删除所有值为value的元素<br/>
8、`ltrim key-name start stop`: 保留key-name中指定的值，包括start和stop<br/>

> 三、`Set`集合型

`Set` 是集合数据类型，无序，能够存储多个`value`值，不能存储重复的value值。


![Set类型](https://github.com/qimeijun/redis/blob/master/set.png)

常用命令：
1、`sadd key-name value1 value2...`: 给key-name 设置多个value值<br/>
2、`srem key-name value1 value2...` : 删除key-name 中的指定值<br/>
3、`smembers key-name`: 获取key-name中的所有元素<br/>
4、`sismember key-name`： 判断key-name是否存在<br/>
5、`scard key-name` : 获取key-name中的元素个数<br/>
6、`spop key-name`： 从key-name中随机弹出一个元素 <br/>


> 四、`Hash` 散列型

`Hash`是散列数据类型，无序，能够存储多个`key-value`值。

![Hash散列](https://github.com/qimeijun/redis/blob/master/hash.png)

常用命令：
1、`hset key-name field-name value`: 给key-name 设置值<br/>
2、`hmset key-name field-name1 value1 [field-name2 value2...]`：设置多个值<br/>
3、`hget key-name field`: 获取key-name中field字段中的元素<br/>
4、`hmget key-name field1 [field2]`: 获取key-name 中的多个field字段元素<br/>
5、`hgetall key-name `: 获取key-name中的全部字段及元素<br/>
6、`hexists key-name field` ： 判断key-name中的field字段是否存在<br/>
7、`hsetnx key-name field value`: 当key-name中的field不存在时赋值<br/>
8、`hdel key-name field `: 删除key-name中的field 字段<br/>
9、`hkeys key-name` : 获取key-name 中的所有字符<br/>
10、`hvals key-name`: 获取key-name中的所有元素<br/>
11、`hlen key-name`: 获取key-name字段数量<br/>


> 五、`ZSet` 有序集合

`ZSet`有序集合数据类型，有序，能够存储多个`value`值，但是`value`只能是分值（分值就是指浮点型数据），集合中会根据`value`值默认排序.


![ZSet有序集合](https://github.com/qimeijun/redis/blob/master/zset.png)

常用命令：
 1、`zadd key-name count1 value1 [count2 value2...] `: 给key-name 设置多个值 <br/>




> 设置过期时间

我们可以给redis的key 设置过期时间，如果过了设定的时间，key 就会自动被删除。<br/><br/>

命令：<br/>
1、`expire key-name seconds`： 设置时间单位为秒<br/>
2、`ttl key-name`:  查看key-name的过期时间<br/>
3、`persist key-name`: 取消key-name的过期时间设置<br/>
<br/>
*精确度：* 如果在过期时间一秒内被访问，那么过期时间就将会被延迟一秒钟，在2.6.0 版本进行改进，延迟被降低在1毫秒之内。<br/>



> 持久化保存

redis是一种内存数据库，当服务器关机或者出现其他故障，redis中的数据很容易丢失，这样我们就需要将数据持久化保存，才不会丢失数据。<br/>

##### 1、快照持久化
这种持久化是redis默认开启的，一次性将数据保存到硬盘当中，但是当数据过多时，这种方法就不合适了。<br/><br/>

我们也可以手动持久化保存：<br/>
 `bgsave`: 异步保存数据到磁盘<br/>
    `lastsave`: 返回上次保存的时间戳<br/>
    `shutdown`: 同步保存数据到磁盘并且关闭redis服务器<br/>
    `bgrewriteaof`: 当日志过长时就用AOF持久化优化存储<br/>

 #####  2、append only file（AOF持久化）

 这种持久化操作是把用户的每个命令都保存到文件当中，保存crud 操作，但是首先得开启AOF持久化操作，同样需要修改redis.conf，当AOF持久化操作开启时redis中的数据会被清空，所有在安装redis的时候就需要开启AOF操作。启动AOF操作之后在项目目录下会有一个appendonly.aof文件。<br/><br/>

命令：<br/>
        `appendfsync always`:每次收到命令就立即强制写入磁盘，性能最慢，但是保证完全的持久化<br/>
       `appendfsync everysec`:每秒钟强制写入磁盘一次，在性能和持久化方面最了很好的折中<br/>
     `appendfsync no`:完全依赖[操作系统](http://lib.csdn.net/base/operatingsystem)，性能好的时候就持久，不好的时候就不持久化<br/>
  
