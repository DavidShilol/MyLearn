gin官网：<https://gin-gonic.com/docs/>

github：<https://github.com/gin-gonic/gin>

# 基础知识
## gin.Default()与gin.New()

gin源码
```Go
// Default returns an Engine instance with the Logger and Recovery middleware already attached.
func Default() *Engine {
	debugPrintWARNINGDefault()
	engine := New()
	engine.Use(Logger(), Recovery())
	return engine
}
```

Default()是在New()的基础上增加了Logger和Recovery中间件

## gin.Run()与http.ListenAndServe()

gin.Run()源码
```Go
func (engine *Engine) Run(addr ...string) (err error) {
	defer func() { debugPrintError(err) }()

	address := resolveAddress(addr)
	debugPrint("Listening and serving HTTP on %s\n", address)
	err = http.ListenAndServe(address, engine)
	return
}
```

net/http简单web服务
```Go
package main

import (
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/", indexHandler)

	log.Fatal(http.ListenAndServe(":8080", nil))
}
```

gin.Run()中使用的也是http.ListenAndServe()。区别在于传入的第二个参数，gin中传入的engine是一个`*Engine`类型的对象，该类型实现了handler接口。handler接口的用途是处理程序响应http请求。原生http中传入的是nil，表示使用默认handler类型`http.defaultHandler`。

相关博客点[这里](https://blog.csdn.net/weixin_42515390/article/details/105754814)

## 配置文件
配置文件建议使用INI，使用方法点[这里](https://ini.unknwon.io/docs/intro/getting_started)。

# 实践
教程点[这里](https://eddycjy.com/tags/gin/#2018)

教程源码点[这里](https://github.com/EDDYCJY/go-gin-example/blob/master/README_ZH.md)

## 项目结构
```
go-gin-example/
├── conf
├── middleware
├── models
├── pkg
├── routers
└── runtime
```
+ conf：用于存储配置文件
+ middleware：应用中间件
+ models：应用数据库模型
+ pkg：第三方包
+ routers 路由逻辑处理
+ runtime：应用运行时数据