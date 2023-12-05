修改配置文件:`/lib/systemd/system/docker.service`

```json
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://127.0.0.1:2375
```
