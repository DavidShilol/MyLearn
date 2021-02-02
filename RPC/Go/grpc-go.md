## 依赖环境

### Go
省略

### Protocol buffer compiler---protoc
1. [下载](https://github.com/google/protobuf/releases)预编译的安装包
2. `unzip protoc-xxxxx.zip -d $HOME/protoc`
3. `export PATH="$PATH:$HOME/protoc/bin"`

### Go plugins for the protocol compiler

`go get -u github.com/golang/protobuf/protoc-gen-go`

执行之后会在$GOPATH/bin下生成插件，需要设置环境变量

`export PATH=$PATH:$GOPATH/bin`

### Go module
设置环境变量
```
export GO111MODULE=on
export GOPROXY=https://goproxy.io,direct
```

main.go目录下创建go.mod文件

```
go mod init server
```

在这之后便可以顺利运行程序了

## 编译
`protoc -I. --go_out=plugins=grpc:./user user.proto`

+ `-I.`
  相对于当前目录的proto文件路径
+ `--go_out=plugins=grpc:your/path`
  生成的文件存放在当前目录下的your/path目录下
+ `user.proto`
  proto文件名

## 项目注意事项
在启用GO MODULE的前提下，所有的工程必须在$GOPATH/src/github.com/底下。