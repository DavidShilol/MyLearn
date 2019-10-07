# ISSUE: CLOSE_WAIT

## 导读

### 一、四次挥手

CLOSE_WAIT是四次挥手模型中的状态，模型如图所示：

<img src="images/四次挥手.png" width="70%" style="float: left;">

[四次挥手过程](https://blog.csdn.net/O9A0MA/article/details/90731748)

理解要点：

+ TCP是全双工模式，即发送数据的同时也能接收数据。
+ TCP连接的两端都可以主动发起关闭请求。
