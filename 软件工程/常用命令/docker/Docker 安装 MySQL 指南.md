# Docker 安装 MySQL 指南

本文档提供了详细的步骤来使用 Docker 安装 MySQL，并配置 root 用户进行远程访问。

## **安装 Docker（如果尚未安装）**

### **CentOS**

```bash
# 安装 Docker
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io -y

# 启动 Docker
sudo systemctl start docker
sudo systemctl enable docker

# 将当前用户添加到 docker 组
sudo usermod -aG docker $USER
newgrp docker

```

### **Ubuntu**

```bash
# 安装 Docker
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y

# 启动 Docker
sudo systemctl start docker
sudo systemctl enable docker

# 将当前用户添加到 docker 组
sudo usermod -aG docker $USER
newgrp docker

```

## **使用 Docker 安装 MySQL**

### **1. 拉取 MySQL 镜像**

```bash
# MySQL 8.0 (推荐)
docker pull mysql:8.0

# 或者 MySQL 5.7# docker pull mysql:5.7
```

### **2. 创建数据卷（持久化数据）**

```bash
# 创建数据卷
docker volume create mysql_data

```

### **3. 运行 MySQL 容器**

```bash
# 启动 MySQL 8.0
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -e MYSQL_DATABASE=ruoyi-vue-pro\
  -v mysql_data:/var/lib/mysql \
  mysql:8.0

# 如果需要自定义配置文件# mkdir -p /etc/mysql/conf.d# vi /etc/mysql/conf.d/my.cnf# 添加配置内容# 然后添加挂载: -v /etc/mysql/conf.d:/etc/mysql/conf.d
```

### **4. 验证 MySQL 运行状态**

```bash
# 查看容器状态
docker ps

# 查看容器日志
docker logs mysql

```

## **配置 root 远程访问权限**

### **方法一：进入容器配置**

```bash
# 进入 MySQL 容器
docker exec -it mysql bash

# 登录 MySQL
mysql -u root -p
# 输入密码: 123456# 执行以下 SQL 命令允许远程访问
CREATE USER 'root'@'%' IDENTIFIED BY '123456';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
FLUSH PRIVILEGES;

# 验证用户权限
SELECT user, host FROM mysql.user WHERE user = 'root';
SHOW GRANTS FOR 'root'@'%';

# 退出 MySQL 和容器exit
exit

```

### **方法二：使用自定义配置文件**

```bash
# 创建自定义配置目录mkdir -p /etc/mysql/conf.d

# 创建自定义配置文件cat > /etc/mysql/conf.d/my.cnf << 'EOF'
[mysqld]
bind-address = 0.0.0.0
default-authentication-plugin = mysql_native_password
EOF

# 重新创建并启动 MySQL 容器
docker stop mysql
docker rm mysql
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -e MYSQL_DATABASE=seckill \
  -v mysql_data:/var/lib/mysql \
  -v /etc/mysql/conf.d:/etc/mysql/conf.d \
  mysql:8.0

```

## **连接测试**

### **本地测试连接**

```bash
# 从容器内部测试
docker exec -it mysql mysql -u root -p
# 输入密码: 123456
```

### **远程测试连接**

从其他服务器或本地机器使用 MySQL 客户端测试连接：

```bash
mysql -h 服务器IP -u root -p
# 输入密码: 123456
```

## **常见问题解决**

### **1. 无法连接到 MySQL**

检查防火墙设置：

```bash
# 对于 CentOS/RHEL
sudo firewall-cmd --permanent --add-port=3306/tcp
sudo firewall-cmd --reload

# 对于 Ubuntu/Debian
sudo ufw allow 3306/tcp
sudo ufw reload

```

### **2. 容器无法启动**

检查日志：

```bash
docker logs mysql

```

### **3. 密码认证问题（MySQL 8.0 特有）**

MySQL 8.0 默认使用 `caching_sha2_password` 认证插件，而一些旧版本客户端可能不支持。解决方法：

```sql
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
FLUSH PRIVILEGES;

```

### **4. 数据库连接字符串**

在 Spring Boot 应用中使用以下连接字符串：

```
spring.datasource.url=jdbc:mysql://localhost:3306/seckill?characterEncoding=utf-8&useSSL=false
spring.datasource.username=root
spring.datasource.password=123456

```

对于远程连接，将 `localhost` 替换为服务器 IP 地址。

## **安全建议**

1. **不要使用默认的 root 用户**：为应用创建专用用户
2. **使用强密码**：不要使用简单的密码如 `123456`
3. **限制 IP 访问**：只允许特定 IP 访问
4. **使用 SSL/TLS 加密连接**
5. **定期更新 MySQL 版本**
6. **备份数据**：定期备份数据库

## **Docker 常用命令参考**

```bash
# 启动/停止/重启容器
docker start mysql
docker stop mysql
docker restart mysql

# 删除容器
docker rm mysql

# 查看日志
docker logs mysql
docker logs -f mysql# 实时查看# 进入容器
docker exec -it mysql bash

# 查看容器状态
docker ps
docker ps -a# 显示所有容器，包括已停止的
```