# win解决端口占用问题

查询被占端口号进程

```powershell
netstat -aon|findstr "端口号"
```

查询进程号

```powershell
tasklist|findstr "进程号"
```

关闭进程

```powershell
taskkill /T /F /PID 进程号
```
