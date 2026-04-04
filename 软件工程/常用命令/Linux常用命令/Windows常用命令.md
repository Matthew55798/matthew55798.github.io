# Windows常用命令

1. 启用管理员账号
2. 安装软件
3. 关闭windows安全中心
4. 

net user *administrator* /active:yes

```bash
Get-ChildItem -Recurse | Select-String -Pattern "EL1020E8301" | 
    Select-Object -Unique Path | 
    ForEach-Object { 
        [PSCustomObject]@{
            FileName = Split-Path $_.Path -Leaf
            Directory = Split-Path $_.Path -Parent
        }
    } | Format-Table -AutoSize
```

```
sc create YourServiceName binPath= "C:\path\to\your\program.exe"  
```

```bash
     netstat -ano | findstr :8080
     
```

```bash
runas /user:Administrator "cmd.exe"
```

允许无密码远程登陆