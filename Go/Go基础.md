# Go环境相关
## Go module
类似于python-pip，centos-yum，go module用于管理依赖包。使用方法有两种：
1. go get
go默认不启用go module管理包，go get下载的包将放在`$GOPATH/src`目录底下。
2. go mod
环境变量设置`export GO111MODULE=on`后，所有依赖包将由go module管理。所有模块的数据将缓存在`$GOPATH/pkg/mod`和`$GOPATH/pkg/sum`。详细介绍点[这里](https://zhuanlan.zhihu.com/p/103534192)
## proxy代理
go get和go mod download被墙，需要借助镜像代理服务下载资源。

设置环境变量如下：
```shell
export GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin
export GOPATH=/Users/exocr/Desktop/goproject
export GO111MODULE=on
export GOPROXY=https://goproxy.io,direct
```

说明

+ GOROOT：go安装包的根目录
+ PATH：go指令目录
+ GOPATH：所有go项目的根目录
+ GO111MODULE：开启go module功能，解决包依赖管理，开启后使用go get下载的包，都放在$GOPATH/src/pkg。这时需要用go mod引入这些包。具体为执行一下指令
  ```shell
  go mod init yourprojectname
  go mod edit -require github.com/gin-gonic/gin@latest
  ```
+ GOPROXY：下载资源的代理服务器，PROXY镜像代理官网点[这里](https://goproxy.io/zh/docs/introduction.html)


## GoLand

### 安装
CentOS下，下载软件压缩包,使用bin/goland.sh便可以启动软件。

生成软件快捷方式的方法：
```shell
cat ~/.local/share/applications/goland.desktop
[Desktop Entry]
Version=1.0
Encoding=UTF-8
Name=GoLand
Type=Application
Terminal=false
Name[en_US]=JetBrains GoLand 2019.3.1
Exec=/home/david/GoLand-2020.3.1/bin/goland.sh
Comment[en_US]=GoLand-2019.3.1
Comment=GoLand-2019.3.1
GenericName[en_US]=
Icon=/home/david/GoLand-2020.3.1/bin/goland.png
```

### 不能输入中文的问题
首先进入解压该软件的根目录下，进入bin文件夹中，找到启动该软件的goland.sh文件，使用gedit打开，或者使用vi命令打开，进入文档模式：
在注释之后的首行添加：
```
export XMODIFIERS="@im=ibus"
export GTK_IM_MODULE="ibus"
export QT_IM_MODULE="ibus"
```
保存退出，重新执行.sh文件可以发现已经可以输入中文了