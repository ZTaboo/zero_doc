## Cargo设置

```toml
[workspace]  
members = ["migration"]

[dependencies]  
migration = { path = "migration" }
```

## 迁移数据库:

```bash
sea-orm-cli migrate refresh -u "sqlite://./demo.db"
```

### migrate命令解释

```bash
init - 初始化迁移目录

generate - 生成一个新的空迁移

fresh - 从数据库中删除所有表,然后重新应用所有迁移

refresh - 回滚所有已应用的迁移,然后重新应用所有迁移

reset - 回滚所有已应用的迁移

status - 检查所有迁移的状态

up - 应用待处理的迁移

down - 回滚已应用的迁移
```

### generate使用

```toml
# 数据库映射
entity  生成实体

 --compact-format
          生成紧凑格式的实体文件
  -v, --verbose
          显示调试信息
      --expanded-format
          生成扩展格式的实体文件
      --include-hidden-tables
          生成隐藏表的实体文件（即，表名以下划线开头的表）
  -t, --tables <TABLES>
          仅为指定的表生成实体文件（逗号分隔）
      --ignore-tables <IGNORE_TABLES>
          跳过为指定的表生成实体文件（逗号分隔）[默认: seaql_migrations]
      --max-connections <MAX_CONNECTIONS>
          连接到数据库时要使用的最大连接数。[默认: 1]
  -o, --output-dir <OUTPUT_DIR>
          实体文件输出目录[默认: ./]
  -s, --database-schema <DATABASE_SCHEMA>
          数据库架构
           - 对于MySQL，此参数将被忽略。
           - 对于PostgreSQL，此参数是可选的，默认值为 'public'。[环境变量: DATABASE_SCHEMA=] [默认: public]
  -u, --database-url <DATABASE_URL>
          数据库URL[环境变量: DATABASE_URL=]
      --with-serde <WITH_SERDE>
          自动为实体衍生serde的Serialize / Deserialize特性（none, serialize, deserialize, both）[默认: none]
      --serde-skip-deserializing-primary-key
          为主键字段生成serde字段属性，'#[serde(skip_deserializing)]'，以在反序列化时跳过它们，此标志仅在'--with-serde'为'both'或'deserialize'时有效。
      --serde-skip-hidden-column
          选择在隐藏列上添加跳过属性（即启用 'with-serde' 且列名以下划线开头）
      --with-copy-enums
          自动为生成的枚举衍生Copy特性。
          从数据库生成的枚举默认情况下不包含相关数据，因此可以衍生Copy。

      --date-time-crate <DATE_TIME_CRATE>
          用于生成实体的日期时间库。[默认: chrono] [可能的值: chrono, time]
  -l, --lib
          将索引文件生成为 `lib.rs` 而不是 `mod.rs`。
      --model-extra-derives <MODEL_EXTRA_DERIVES>
          为生成的模型结构添加额外的衍生宏（逗号分隔），例如 `--model-extra-derives 'ts_rs::Ts','CustomDerive'`
      --model-extra-attributes <MODEL_EXTRA_ATTRIBUTES>
          为生成的模型结构添加额外的属性，不需要 `#[]`（逗号分隔），例如 `--model-extra-attributes 'serde(rename_all = "camelCase")','ts(export)'`
      --seaography
          生成Seaography使用的辅助枚举。
  -h, --help
          显示帮助信息（使用 '--help' 查看更多）
```

- 使用

```bash
sea-orm-cli generate entity -u "sqlite://./g_email.db" -o src\entity\
```
