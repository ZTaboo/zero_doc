github：[https://github.com/shopspring/decimal](https://github.com/shopspring/decimal)

### 实例

```go
package main

import (
  "fmt"
)

func main() {
  a := 2.55
  var b float64 = 0
  fmt.Println((a + b) * 100) // output:254.99999999999997
  // a := decimal.NewFromFloat(2.55)
  // b := decimal.NewFromFloat(0)
  // fmt.Println(a.Add(b).Mul(decimal.NewFromFloat(100))) // output:255
}
```

### 小数保留（四舍五入）

```go
dd := decimal.NewFromFloatWithExponent(12.125321, -2)
fmt.Println(dd)// output：12.13
```
