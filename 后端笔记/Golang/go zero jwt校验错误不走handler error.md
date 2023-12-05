在`rest.MustNewServer`中添加`rest.WithUnauthorizedCallback`，调用httpx.error即可，然后在`httpx.SetErrorHandler`自定义相关错误

参考：[https://github.com/zeromicro/go-zero/issues/398](https://github.com/zeromicro/go-zero/issues/398)

```go
package main

import (
  "context"
  "flag"
  "fmt"
  "github.com/golang-jwt/jwt/v4"
  "github.com/zeromicro/go-zero/core/conf"
  "github.com/zeromicro/go-zero/core/logx"
  "github.com/zeromicro/go-zero/rest"
  "github.com/zeromicro/go-zero/rest/httpx"
  "gozeroApi/internal/common/errorz"
  "gozeroApi/internal/config"
  "gozeroApi/internal/handler"
  "gozeroApi/internal/svc"
  "net/http"
  "reflect"
)

var configFile = flag.String("f", "etc/gozeroapi-api.yaml", "the config file")

func main() {
  flag.Parse()

  var c config.Config
  conf.MustLoad(*configFile, &c)

  // 重点，如果不添加这里将无法输出自定义的错误处理
  server := rest.MustNewServer(c.RestConf, rest.WithUnauthorizedCallback(func(w http.ResponseWriter, r *http.Request, err error) {
    httpx.Error(w, err)
  }))
  defer server.Stop()
  // 自定义错误返回信息内容
  httpx.SetErrorHandler(func(err error) (int, interface{}) {
    logx.Debug("error type:", reflect.TypeOf(err).String())
    switch e := err.(type) {
    case *errorz.CodeError:
      return e.Code, e.ErrorData()
    case *jwt.ValidationError:
      return 400, &errorz.CodeErrorResponse{
        Code: 400,
        Msg:  err.Error(),
      }
    default:
      return 402, &errorz.CodeErrorResponse{
        Code: 402,
        Msg:  err.Error(),
      }
    }
  })
  httpx.SetErrorHandlerCtx(func(ctx context.Context, err error) (int, interface{}) {
    switch e := err.(type) {
    case *errorz.CodeError:
      return e.Code, e.ErrorData()
    default:
      return 402, nil
    }
  })
  ctx := svc.NewServiceContext(c)
  handler.RegisterHandlers(server, ctx)
  fmt.Printf("Starting server at %s:%d...\n", c.Host, c.Port)
  server.Start()
}
```

‍
