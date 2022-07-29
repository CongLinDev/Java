# Redis

## 数据结构

1. string->int sds
2. list->quicklist(ziplist linkedlist)
3. hash->ziplist hashtable
4. set->intset hashtable
5. sorted set->ziplist skiplist

## Redis 快的原因

1. 基于内存
2. 单线程 省去锁和上下文切换的开销
3. IO多路复用
