# Redis

GitHub地址：https://github.com/redis/redis

+ [docker部署Redis](docker部署Redis.md)

+ [Redis命令](Redis命令.md)

+ [缓存过期和缓存淘汰](缓存过期和缓存淘汰.md)

+ [缓存穿透](缓存穿透.md)

+ [缓存击穿和缓存雪崩](缓存击穿和缓存雪崩.md)

+ [redis数据持久化](redis数据持久化.md)

+ [redis集群](redis集群.md)

## 什么是redis

Redis通常被称为数据结构服务器，这意味着Redis通过一组命令提供对可变数据结构的访问，这些命令是使用带有TCP套接字和简单协议的服务器-客户端模型发送的。因此，不同的进程可以以共享方式查询和修改相同的数据结构。

在Redis中实现的数据结构具有一些特殊属性：

+ redis虽然一直将服务和修改放在内存中，但也会将它们存储在磁盘上。这意味着Redis速度很快，但这也是非易失性的。

+ 数据结构的实现会提高内存效率，因此与使用高级编程语言建模的相同数据结构相比，Redis内部的数据结构可能会使用较少的内存。

+ Redis提供了许多自然可以在数据库中找到的功能，例如复制，持久性的可调级别，集群，高可用性。

另一个很好的例子是将Redis认为是memcached的一个更复杂的版本，其中的操作不仅是SET和GET，而且还包括与列表，集合，有序数据结构等复杂数据类型一起使用的操作。

## Redis内部存储结构

以下是完整的 redisObject  结构，它定义了Redis对象

源码位于：src/server.h

2020.7.21

```c++
typedef struct redisObject {
    unsigned type:4;
    unsigned encoding:4;
    unsigned lru:LRU_BITS; /* LRU time (relative to global lru_clock) or
                            * LFU data (least significant 8 bits frequency
                            * and most significant 16 bits access time). */
    int refcount;
    void *ptr;
} robj;
```

type: 对象所保存的值的类型

```c++
/* The actual Redis Object */
#define OBJ_STRING 0    /* String object. */
#define OBJ_LIST 1      /* List object. */
#define OBJ_SET 2       /* Set object. */
#define OBJ_ZSET 3      /* Sorted set object. */
#define OBJ_HASH 4      /* Hash object. */

/* The "module" object type is a special one that signals that the object
 * is one directly managed by a Redis module. In this case the value points
 * to a moduleValue struct, which contains the object value (which is only
 * handled by the module itself) and the RedisModuleType struct which lists
 * function pointers in order to serialize, deserialize, AOF-rewrite and
 * free the object.
 *
 * Inside the RDB file, module types are encoded as OBJ_MODULE followed
 * by a 64 bit module type ID, which has a 54 bits module-specific signature
 * in order to dispatch the loading to the right module, plus a 10 bits
 * encoding version. */
#define OBJ_MODULE 5    /* Module object. */
#define OBJ_STREAM 6    /* Stream object. */
```

encoding：对象所保存的值的编码

```c++
/* Objects encoding. Some kind of objects like Strings and Hashes can be
 * internally represented in multiple ways. The 'encoding' field of the object
 * is set to one of this fields for this object. */
#define OBJ_ENCODING_RAW 0     /* Raw representation */
#define OBJ_ENCODING_INT 1     /* Encoded as integer */
#define OBJ_ENCODING_HT 2      /* Encoded as hash table */
#define OBJ_ENCODING_ZIPMAP 3  /* Encoded as zipmap */
#define OBJ_ENCODING_LINKEDLIST 4 /* No longer used: old list encoding. */
#define OBJ_ENCODING_ZIPLIST 5 /* Encoded as ziplist */
#define OBJ_ENCODING_INTSET 6  /* Encoded as intset */
#define OBJ_ENCODING_SKIPLIST 7  /* Encoded as skiplist */
#define OBJ_ENCODING_EMBSTR 8  /* Embedded sds string encoding */
#define OBJ_ENCODING_QUICKLIST 9 /* Encoded as linked list of ziplists */
#define OBJ_ENCODING_STREAM 10 /* Encoded as a radix tree of listpacks */
```

lru: LRU time 或 LFU data

refcount: 引用计数

ptr: 是一个指针，指向实际保存值的数据结构，这个数据结构由 type 属性和 encoding 属性决定。
