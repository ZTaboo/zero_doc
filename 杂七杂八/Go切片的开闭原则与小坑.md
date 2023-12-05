## 开闭规则

```go
package main

import "fmt"

func main() {
    slice := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
    fmt.Printf("slice:%v,pointer:%p,cap:%d,len:%d\n", slice, slice, cap(slice), len(slice))
    s1 := slice[0:5]
    fmt.Printf("slice:%v,pointer:%p,cap:%d,len:%d\n", s1, s1, cap(s1), len(s1)) //如果从0索引开始，则与源切片指针地址一致，长度改变容量一致
    s2 := slice[1:5]
    fmt.Printf("slice:%v,pointer:%p,cap:%d,len:%d\n", s2, s2, cap(s2), len(s2))
    s3 := slice[2:6]
    fmt.Printf("slice:%v,pointer:%p,cap:%d,len:%d\n", s3, s3, cap(s3), len(s3)) //从非0索引开始则会产生一个新的内存指针，容量从开始的索引到源数组结束的容量
    s4 := slice[2:]
    fmt.Printf("slice:%v,pointer:%p,cap:%d,len:%d\n", s4, s4, cap(s4), len(s4)) // 如果有某个切片的起始位置和内容和s4一致，并且容量大于等于s4，则他们的指针内存一致
}
```

```bash
# output
slice:[0 1 2 3 4 5 6 7 8 9],pointer:0xc00010c0f0,cap:10,len:10
slice:[0 1 2 3 4],pointer:0xc00010c0f0,cap:10,len:5
slice:[1 2 3 4],pointer:0xc00010c0f8,cap:9,len:4
slice:[2 3 4 5],pointer:0xc00010c100,cap:8,len:4
slice:[2 3 4 5 6 7 8 9],pointer:0xc00010c100,cap:8,len:8
```

### 简单总结

1. 如果新切片截取的源切片索引是从0开始，则新切片容量与源切片容量一致，并且指针内存地址一致
2. 如果新切片截取的源切片索引是从非0开始，则创建新的指针内存，容量是源切片的长度减去新切片开始索引的长度（有点不知道咋描述，具体自己看实验吧...）
3. 如 `s3`和 `s4` 两个例子，s4的新切片和s3切片的起始索引一直，并且内容一直，则他们的内存地址一致，前提是它们的源切片是一致的

## 一个小坑

```go
package main

import "fmt"

func main() {
    slice := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
    fmt.Printf("slice:%v,pointer:%p,cap:%d,len:%d\n", slice, slice, cap(slice), len(slice))
    s1 := slice[2:5]
    fmt.Printf("slice:%v,pointer:%p,cap:%d,len:%d\n", s1, s1, cap(s1), len(s1))
    s2 := s1[2:6]
    fmt.Printf("slice:%v,pointer:%p,cap:%d,len:%d\n", s2, s2, cap(s2), len(s2))
}
```

```bash
# output
slice:[0 1 2 3 4 5 6 7 8 9],pointer:0xc0000125a0,cap:10,len:10
slice:[2 3 4],pointer:0xc0000125b0,cap:8,len:3
slice:[4 5 6 7],pointer:0xc0000125c0,cap:6,len:4
```

> 这里会发现`s2` 的结果神奇的有了`s1` 切片所不存在的参数，下面再做一个实验


```go
package main

import "fmt"

func main() {
    slice := []int{0, 1, 2, 3, 4, 5, 6, 7, 8, 9}
    fmt.Printf("slice:%v,pointer:%p,cap:%d,len:%d\n", slice, slice, cap(slice), len(slice))
    s1 := slice[2:5]
    fmt.Printf("slice:%v,pointer:%p,cap:%d,len:%d\n", s1, s1, cap(s1), len(s1))
    s1 = append(s1, 100)
    fmt.Printf("slice:%v,pointer:%p,cap:%d,len:%d\n", s1, s1, cap(s1), len(s1))
    fmt.Printf("slice:%v,pointer:%p,cap:%d,len:%d\n", slice, slice, cap(slice), len(slice))
}
```

```bash
# output
slice:[0 1 2 3 4 5 6 7 8 9],pointer:0xc0000125a0,cap:10,len:10
slice:[2 3 4],pointer:0xc0000125b0,cap:8,len:3
slice:[2 3 4 100],pointer:0xc0000125b0,cap:8,len:4
slice:[0 1 2 3 4 100 6 7 8 9],pointer:0xc0000125a0,cap:10,len:10
```

> 虽然`s1` 的内存地址创建了，我们看到的只有三条内容，但底层数组并非此长度，因此内容还是源切片的内容，当我们修改了`s1`的内容，源切片的内容也被改变了，这个问题仅限新切片的长度比源切片的长度少时存在，因为超过源切片容量后会拓展出一个新的切片


### 指定闭合切片的位置

```go
s2 := s1[2:5:5]
fmt.Printf("slice:%v,pointer:%p,cap:%d,len:%d\n", s2, s2, cap(s2), len(s2))
```

```bash
# output
slice:[4 100 6],pointer:0xc0000125c0,cap:3,len:3
```

> 我们可以根据需要来指定切片的闭合位置，如上所示。


‍
