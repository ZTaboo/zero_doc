> 查询端口占用，杀掉相应程序


- 查询端口

```bash
netstat -ano | findstr 8080
```

- 查询进程名

```bash
tasklist | findstr 5800
```

- 杀死进程

```bash
taskkill -f -t -im <fileName>
```
