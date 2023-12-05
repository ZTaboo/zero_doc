## 使用strip减小build包

> windows上build后大小基本都是最小的了，因为没有使用静态链接。而linux上基本一个hello world就几兆大小


```toml
[package]
name = "learn"
version = "0.1.0"
edition = "2021"

[profile.release]
strip = true         # 开启strip修剪;会删除调试信息;需根据使用情况去使用
# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
```

‍
