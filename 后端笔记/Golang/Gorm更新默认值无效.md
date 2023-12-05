gorm默认更新（updates）默认值，如：0，false，null等不会更新

### 可以使用map更新：

```go
db.Mysql.Model(&dbModel.ShopSort{}).Where("id = ?", data.Id).Updates(map[string]any{"sort_name": data.SortName, "home_page_top": data.HomePageTop})
```

### Save方法保存

```go
db.First(&user)

user.Name = "jinzhu 2"
user.Age = 100
db.Save(&user)
```
