```go
package main

import (
    "fmt"
    "os"
    "os/exec"
    "strconv"
)

func main() {

    pid := os.Getpid()
    fmt.Printf("进程 PID: %d \n", pid)

    prc := exec.Command("ps", "-p", strconv.Itoa(pid), "-v")
    out, err := prc.Output()
    if err != nil {
        panic(err)
    }

    fmt.Println(string(out))
}
```

```bash
进程 PID: 28259 
  PID TTY      STAT   TIME  MAJFL   TRS   DRS   RSS %MEM COMMAND
28259 pts/0    Sl+    0:00      0   648 102535 1224  0.0 ./main
```

‍
