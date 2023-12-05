```bash
defer func() {
    if err := recover(); err != nil {
      fmt.Println("error is:", err)
      for i := 3; ; i++ { //跳过前三层panic信息
        _, file, line, ok := runtime.Caller(i)
        if !ok {
          break
        }
        fmt.Println(file, line)
      }
    }
  }()
```
