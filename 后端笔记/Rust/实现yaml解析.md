```rust
use std::fs;  
  
use serde::{Deserialize, Serialize};  
use serde_yaml::from_str;  
  
#[derive(Debug, Serialize, Deserialize)]  
struct Config {  
    name: String,  
    age: i32,  
}  
  
fn main() {  
    let con = fs::read_to_string("./src/yaml解析/config.yaml").unwrap();  
    let config: Config = from_str(&con).unwrap();  
    println!("name:{},age:{}", config.name, config.age);  
}
```

```toml
# cargo.toml
...
[dependencies]
# features用于使用宏来进行序列化和反序列化
serde = { version = "1.0.188", features = ["derive"] }  
serde_yaml = "0.9.25"
```
