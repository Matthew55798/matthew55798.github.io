# 常用命令 - 速查手册

## OVERVIEW

运维命令速查中心。Linux、Windows、Oracle、Docker 常用操作一网打尽。

## STRUCTURE

```
常用命令/
├── Linux常用命令/
│   └── Windows常用命令.md    # Windows 管理脚本
├── Oracle 常用命令.md         # 数据库运维 SQL
└── docker/
    ├── Docker 安装 MySQL 指南.md
    ├── Docker 安装 Nacos 指南.md
    └── Docker 安装 Redis 指南.md
```

## WHERE TO LOOK

| 任务 | 位置 | 说明 |
|------|------|------|
| Linux 运维 | `Linux常用命令/` | 系统管理命令 |
| Windows 管理 | `Linux常用命令/Windows常用命令.md` | PowerShell、服务管理 |
| Oracle 运维 | `Oracle 常用命令.md` | sqlplus、表空间、导入导出 |
| Docker 部署 | `docker/` | MySQL/Nacos/Redis 安装指南 |
| 查找端口占用 | `Windows常用命令.md` | `netstat -ano` |
| 数据库死锁 | `Oracle 常用命令.md` | `V$LOCK` 查询 |

## NOTES

- 命令按场景组织，直接复制使用
- Docker 指南含完整安装、配置、故障排查流程
- Oracle 文档含生产环境表空间模板
