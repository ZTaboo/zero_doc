使用中间件：[GinZap](https://github.com/gin-contrib/zap)

```go
// utiils/Logger.go
package log

import (
    "go.uber.org/zap"
    "go.uber.org/zap/zapcore"
    "gopkg.in/natefinch/lumberjack.v2"
    "os"
)

var Logger *zap.Logger

func InitLogs(logLevel zapcore.Level) {

    encoderConfig := zapcore.EncoderConfig{
        TimeKey:        "time",
        LevelKey:       "level",
        NameKey:        "logger",
        CallerKey:      "caller",
        MessageKey:     "msg",
        StacktraceKey:  "stacktrace",
        LineEnding:     zapcore.DefaultLineEnding,
        EncodeLevel:    zapcore.CapitalLevelEncoder,    // 大写编码器
        EncodeTime:     zapcore.ISO8601TimeEncoder,     // ISO8601 UTC 时间格式
        EncodeDuration: zapcore.SecondsDurationEncoder, //
        EncodeCaller:   zapcore.ShortCallerEncoder,     // 短路径编码器(相对路径+行号)
        EncodeName:     zapcore.FullNameEncoder,
    }

    // 设置日志输出格式
    var encoder zapcore.Encoder
    encoder = zapcore.NewConsoleEncoder(encoderConfig)

    // 添加日志切割归档功能
    hook := lumberjack.Logger{
        Filename:   "./logs/logs.log", // 日志文件路径
        MaxSize:    64,                // 每个日志文件保存的最大尺寸 单位：M
        MaxBackups: 5,                 // 日志文件最多保存多少个备份
        MaxAge:     30,                // 文件最多保存多少天
        Compress:   true,              // 是否压缩
    }

    core := zapcore.NewCore(
        encoder, // 编码器配置
        zapcore.NewMultiWriteSyncer(zapcore.AddSync(os.Stderr), zapcore.AddSync(&hook)), // 打印到控制台和文件
        zap.NewAtomicLevelAt(logLevel), // 日志级别
    )

    // 开启文件及行号
    caller := zap.AddCaller()
    // 开启开发模式，堆栈跟踪
    development := zap.Development()
    // 构造日志
    Logger = zap.New(core, caller, development)

    // 将自定义的logger替换为全局的logger
    zap.ReplaceGlobals(Logger)

}
```

```go
//routers.go
package routers

import (
    "github.com/gin-contrib/zap"
    "github.com/gin-gonic/gin"
    "go.uber.org/zap"
    "time"
)

func SetupRouter() *gin.Engine {
    r := gin.New()
    r.Use(ginzap.Ginzap(zap.L(), time.RFC3339, false))
    r.Use(ginzap.RecoveryWithZap(zap.L(), true))
    //一些路由设置...
    return r
}
```

```go
// main.go
package main

import (
    "go.uber.org/zap"
    "kakkk-test/log"
    "kakkk-test/routers"
)

func main() {
    log.InitLogs(zap.InfoLevel)
    r := routers.SetupRouter()
    r.Run()
}
```

‍
