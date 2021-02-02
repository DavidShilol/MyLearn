## rpc环境
1. runtime
   
   `pip install grpcio`
2. protoc
   
   `pip install grpcio-tools`

## protobuf语言指南
参考博客点[这里](https://studygolang.com/articles/30433)
### 基础知识
一种数据交换格式，由三部分组成
+ proto文件
+ protoc，编译器
+ runtime，运行时库

使用 protobuf 的过程:

`编写 proto 文件 -> 使用 protoc 编译 -> 添加 protobuf 运行时 -> 项目中集成`

更新 protobuf 的过程:

`修改 proto 文件 -> 使用 protoc 重新编译 -> 项目中修改集成的地方`
### 简单示例

一个.proto文件可以定义多个消息格式，定义一个消息的格式如下

```protobuf
syntax="proto3";                    //文件第一行指定使用的protobuf版本，如果不指定，默认使用proto2
package services;                   //定义proto包名,可以为.proto文件新增一个可选的package声明符，可选
option go_package = ".;services";     //声明编译成go代码后的package名称，可选的，默认是proto包名

message ProdRequest{                //messaage可以理解为golang中的结构体,可以嵌套
    int32 prod_id=1;                //变量的定义格式为：[修饰符][数据类型][变量名] = [唯一编号] ,同一个message中变量的编号不能相同
}

message ProdResponse{
    int32 pro_stock=1;
}

service ProdService{                                     //定义服务
    rpc GetProdStock (ProdRequest) returns (ProdResponse);  //rpc方法
}
```

### 变量类型
+ int32/sint32/sfixed32
+ int64/sint64/sfixed64
+ float
+ double
+ bool
+ string
+ bytes
+ enum

### 修饰符
repeated
如果一个字段被repeated修饰，则表示它是一个列表类型的字段，相当于golang里的切片

```protobuf
message SearchRequest {
  repeated string args = 1 // 列表类型
}
```

reserved
如果你希望可以预留一些数字标签或者字段可以使用reserved修饰符

```protobuf
message Foo {
  reserved 2, 15, 9 to 11;
  reserved "foo", "bar";
  string foo = 3 // 编译报错，因为‘foo’已经被标为保留字段
}
```

### 导入文件
```protobuf
import "myproject/other_protos.proto"; // 这样就可以引用在other_protos.proto文件中定义的message,不能导入不使用的.proto文件
```

### 定义服务
在你的 .proto 文件中指定 service,然后在service里定义rpc方法即可，要注意指定参数和返回值
```protobuf
service RouteGuide {
   rpc GetFeature(Point) returns (Feature) {}
}
```

### 四种类型的service方法
gRPC 允许你定义4种类型的 service 方法

简单rpc
客户端使用存根发送请求到服务器并等待响应返回，就像平常的函数调用一样

```protobuf
service RouteGuide {
   rpc GetFeature(Point) returns (Feature) {}
}
```

服务端流式rpc
通过在 响应返回参数 类型前插入 stream 关键字，可以指定一个服务器端的流方法。客户端发送请求到服务器，拿到一个流去读取返回的消息序列。 客户端读取返回的流，直到里面没有任何消息。

```protobuf
service RouteGuide {
   rpc ListFeatures(Rectangle) returns (stream Feature) {}
}
```

客户端流式rpc
通过在 请求参数 类型前指定 stream 关键字来指定一个客户端的流方法。客户端写入一个消息序列并将其发送到服务器，同样也是使用流。一旦客户端完成写入消息，它等待服务器完成读取返回它的响应。

```protobuf
service RouteGuide {
   rpc RecordRoute(stream Point) returns (RouteSummary) {}
}
```

双向流式rpc
通过在请求和响应前加 stream 关键字去制定方法的类型。两个流独立操作，因此客户端和服务器可以以任意喜欢的顺序读写：比如， 服务器可以在写入响应前等待接收所有的客户端消息，或者可以交替的读取和写入消息，或者其他读写的组合。

```protobuf
service RouteGuide {
   rpc RouteChat(stream RouteNote) returns (stream RouteNote) {}
}
```

### 编译
```shell
# 编译 proto 文件
python -m grpc_tools.protoc --python_out=. --grpc_python_out=. -I. helloworld.proto

python -m grpc_tools.protoc: python 下的 protoc 编译器通过 python 模块(module) 实现, 所以说这一步非常省心
--python_out=. : 编译生成处理 protobuf 相关的代码的路径, 这里生成到当前目录
--grpc_python_out=. : 编译生成处理 grpc 相关的代码的路径, 这里生成到当前目录
-I. helloworld.proto : proto 文件的路径, 这里的 proto 文件在当前目录
```

编译后生成的代码:
helloworld_pb2.py: 用来和 protobuf 数据进行交互
helloworld_pb2_grpc.py: 用来和 grpc 进行交互

### xxx_pb2.py和xxx_pb2_grpc.py详解
xxx_pb2_grpc.py
+ 对应proto文件的server内容
+ 服务端代码继承xxxServicer类，具体实现各个API逻辑
+ 客户端代码使用xxxStub类，此类天然包含client向server发消息的具体实现，使用类实例的API函数便可完成交互

xxx_pb2.py
+ 对应proto文件的message内容
+ 服务端/客服端传输数据时，需要用proto文件定义过的消息类型封装数据

简而言之，用pb2_grpc做连接交互，用pb2做数据封装。

### ThreadPoolExecutor与ProcessPoolExecutor
+ 计算频繁的场景，适合用进程池。在Python中由于GIL的限制，多线程只能使用一个CPU
+ IO频繁(计算需求不高)的场景，适合用线程池
+ 两者使用上差异不大

### 最大连接数与活跃连接数
+ 当活跃连接数>=最大连接数，后续的请求将会被拒绝
+ 每一次收到请求后，活跃连接数加1
+ 请求处理完成后，活跃连接数减1

### grpc之worker
+ workers可以是多个进程下的单一线程，也可以是一个进程下的多个线程
+ 主线程启动grpc server之后，主线程将创建一个守护线程。守护线程会在死循环中不断读取活跃连接
+ 当守护线程读取到活跃连接，便会取对应的接口handler提交到线程池中
+ 线程池中接收到任务，将由空闲线程处理请求，并在请求完成后向连接的客户端返回数据