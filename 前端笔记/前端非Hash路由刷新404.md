配置nginx添加如下内容

```nginx
location / {
        try_files $uri $uri/ /index.html;
    }
```
