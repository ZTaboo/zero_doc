> 设置相关参数隐藏即可


```go
cmd := exec.Command(fmtPath, data)
  if runtime.GOOS == "windows" {
    cmd.SysProcAttr = &syscall.SysProcAttr{HideWindow: true}
}
```
