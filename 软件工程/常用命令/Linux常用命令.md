# Linux 常用命令

> Linux 系统运维命令速查

## 目录

- [Windows常用命令](/软件工程/常用命令/Linux常用命令/Windows常用命令.md)

## 待补充内容

Linux 常用命令待后续整理补充。

---

## 常用命令分类

### 文件操作

```bash
# 查看文件内容
cat filename
less filename
head -n 20 filename
tail -n 20 filename

# 文件搜索
find /path -name "*.log"
grep -r "keyword" /path
```

### 系统监控

```bash
# CPU和内存
top
htop
free -h

# 磁盘
df -h
du -sh /path
```

### 网络相关

```bash
# 端口查看
netstat -tlnp
ss -tlnp

# 连接测试
ping host
curl -I url
```

### 进程管理

```bash
# 查看进程
ps aux | grep process_name

# 杀进程
kill -9 PID
pkill process_name
```