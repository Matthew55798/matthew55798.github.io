# 数据库知识库导航

**Scope:** 数据库技术与实践

## OVERVIEW

关系型与 NoSQL 数据库知识。涵盖事务原理、SQL 优化、分布式架构、缓存与搜索引擎。

## STRUCTURE

```
数据库/
├── Redis.md              # 缓存、数据结构、持久化、集群
├── Elasticsearch.md      # 搜索引擎、索引、查询 DSL
└── RDB/                  # 关系型数据库
    ├── 事务.md           # ACID、隔离级别、MVCC
    ├── SQL优化.md        # 执行计划、索引优化、慢查询
    └── 分布式数据库.md    # 分库分表、主从复制、一致性
```

## WHERE TO LOOK

| 主题 | 文件 |
|------|------|
| 缓存/内存数据库 | `Redis.md` |
| 搜索引擎/日志分析 | `Elasticsearch.md` |
| 事务与并发控制 | `RDB/事务.md` |
| SQL 性能调优 | `RDB/SQL优化.md` |
| 分布式架构 | `RDB/分布式数据库.md` |

## NOTES

- RDB 文档聚焦通用原理，不限定具体数据库产品
- Redis 涵盖单机与 Cluster 模式
- Elasticsearch 侧重搜索场景与聚合查询
