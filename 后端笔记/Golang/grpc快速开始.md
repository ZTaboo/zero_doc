## protoc官方地址

官网：[https://github.com/protocolbuffers/protobuf/releases](https://github.com/protocolbuffers/protobuf/releases)

```bash
# 直接安装
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
```

> protoc需要进官网`releases` 下载后丢到`/usr/bin` 下，目录可随机


## 安装`protoc-gen-go-grpc`

官网：[https://github.com/protocolbuffers/protobuf-go/releases](https://github.com/protocolbuffers/protobuf-go/releases)

```bash
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
```

## 安装go依赖

```bash
go get google.golang.org/grpc
```

## 生成`pb` 和`grpc.pb`文件

```bash
protoc --go-grpc_out=. --go_out=. <proto file>
```

## 常见错误

```bash
protoc-gen-go: program not found or is not executable
Please specify a program using absolute path or make sure the program is available in your PATH system variable
--go_out: protoc-gen-go: Plugin failed with status code 1.
```

> 此报错为环境未找到`protoc-gen-go` ，刷新下环境变量即可


- 
没生成`grpc.pb.go` 文件或者`pb.go` 文件是因为需要在`proto` 文件中填写`service`或者`message`语法

- 
使用格式化功能需要安装`vscode-proto3`插件还需要安装`clang-format` 插件，并且进入[https://releases.llvm.org/download.html](https://releases.llvm.org/download.html)下载`LLVM` 安装clang-format

   - `linux` 安装命令：`sudo apt install clang-format`
