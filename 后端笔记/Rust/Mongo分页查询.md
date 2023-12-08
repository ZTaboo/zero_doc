> 分页查询不能直接在Cursor上进行分页,需要设置`FindOptions` 配置

```rust
pub async fn get_users(db: Database, page_num: u64, page_size: i64) -> Result<(Vec<db_model::user::User>, u64), String> {  
    let skip = (page_num - 1) * page_size as u64;  
    let find_options = FindOptions::builder()  
        .limit(page_size)  
        .skip(skip)  
        .build();  
    let res = db  
        .collection::<db_model::user::User>(coll::USER)  
        .find(None, find_options)  
        .await  
        .map_err(|e| format!("数据库错误:{}", e))?;  
    let users: Vec<db_model::user::User> = res.try_collect()  
        .await  
        .map_err(|e| format!("数据库错误:{}", e))?;  
    let total = db  
        .collection::<db_model::user::User>(coll::USER)  
        .count_documents(None, None)  
        .await  
        .map_err(|e| format!("数据库错误:{}", e))?;  
    Ok((users, total))  
}
```