### 一、概述

Redis （Remote Dictionary Server）远程字典服务

`key-value数据库`

### 二、Redis常用命令

```shell
ping  # 查看当前连接是否正常
keys *  # 查看所有key
flushall # 清空所有库的内容
set key value # 设置key-value值
get key # 获取key的value值
exists key # 判断key是否存在
move key db # 删除db库的key
expire key seconds  # 设置key的过期时间为seconds
type key # 获取key的类型
ttl key # 查看key的过期时间
```

###  三、Redis数据类型

#### 五大数据类型



##### 1. string

>String是Redis中最常用的一种数据类型，也是Redis中最简单的一种数据类型。首先，表面上它是字符串，但其实他可以灵活的表示字符串、整数、浮点数3种值。Redis会自动的识别这3种值

##### 2. list

> 实际上list是一个链表

##### 3. set

##### 4. hash

> 适合存对象

##### 5. zset（有序集合）

#### 三种特殊类型

##### Geospatial(地理位置)

#####  Hyperloglog（基数）

#####  Bitmap(位存储)

### 四、Redis配置



### 五、Redis的两种数据持久化方式

#### RDB

#### AOF

### 六、Redis主从同步

###  七、Redis发布订阅功能

### 八、Redis缓存击穿、穿透、雪崩





