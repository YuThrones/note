# Redis 笔记

## 数据类型

* Redis有五种基本数据类型，string、hash、list、set和zset（sorted set：有序集合）

### Hash(哈希)

* 使用`HMSET` 和 `HGET`命令可以分别设置和获取一个hash对应field的value

### List(列表)

* 使用`lpush`可以在数组的头部或者尾部加入元素

* 使用`lrange`可以获取指定范围的元素

### Set(集合)

* 使用 `sadd` 命令添加元素到对应集合中，重复插入将被忽略

### zset(sorted set:有序集合)

* 跟set不同的是每个元素会关联一个double类型的分数，将成员从小到大排序，成员唯一，但是分数可以重复

## Redis 命令

* 使用 `redis-cli` 可以启动redis客户端

* Redis键命令的基本语法如下
  ```redis
    COMMAND KEY_NAME
  ```

* 常用命令
  ```
  DEL key
  ```