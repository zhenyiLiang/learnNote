

# Redis

### 1. overview

- 特点
  1. 数据存储在内存中，使用磁盘进行持久化
  2. 支持数据类型多
  3. 能将数据复制到任意的从属服务器
- 优点
  1. 快
  2. 支持丰富的数据类型
  3. 原子操作
  4. 实用的工具

### 2. 数据类型

#### String

> String 类型字符串最大为512MB

```redis
SET：设置值
GET：读取值
```

#### hashes

>redis 的hashes 是键值对的集合
>
>每个hash值能存储$ 2^{32}-1 $  个字段-值对

```redis
HMSET 、HGETALL
```

#### Lists

> redis 的lists 是简单的字符串的列表，按照插入的顺序排序，可以在前或后面插入值
>
> list 最长有$2^{32}-1$ 个元素

```redis
lpush: 添加元素
lrange a b:显示 a 到 b 之间的元素
```

#### Sets

> redis 的sets 是无序的字符串集合
>
> set 最大有$2^{32}-1$个元素

```redis
sadd:添加、
smembers: 显示所有元素
```



#### Sorted Sets

> redis 的Sorted Sets 是不重复的有序的集合，每个元素有一个score，Sorted Sets 根据元素的score从小到大排序

```redis
zadd:增加
zrangebyscore: 根据score 排序
```

### 3. redis 命令





