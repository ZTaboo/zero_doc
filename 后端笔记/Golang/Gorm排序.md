```go
// desc：倒序 ase 升序
db.Mysql.
        Where("username = ?", account).
        Order("created_at desc").
        Limit(dataModel.PageModel.PageSize).
        Find(&orderData)
// 随机返回值
db.Mysql.Order("RAND()").Limit(8).Find(&resData)
```

‍
