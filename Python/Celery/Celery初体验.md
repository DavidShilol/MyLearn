# Celery初体验

关键词：Celery、RabbitMQ、Python

## 1.安装

### 1.1.RabbitMQ

#### 1.1.1.Erlang

安装RabbitMQ前，需要先安装Erlang。[RabbitMQ-Erlang版本对照表戳我](https://www.rabbitmq.com/which-erlang.html)

本文选择的组合为：RabbitMQ 3.8.3、Erlang 22.x。

**下载**

rpm包下载地址：https://github.com/rabbitmq/erlang-rpm/releases (针对网差的情况，强推localinstall)

**安装**

`yum localinstall -y xxxxxx.rpm`就完事了。

**补充**

本文是在Centos 7操作，安装Erlang之前（其实之后也行）还需要额外的操作：

`yum install -y openssl openssl-devel`

#### 1.1.2.RabbitMQ

rpm包下载地址：https://github.com/rabbitmq/rabbitmq-server/releases/ (辛苦一次，终生受益)

**安装**

`yum localinstall`

#### 1.1.3.设置编码

```shell
export LC_ALL=en_US.UTF-8
source /etc/profile
```

#### 1.1.4.创建RabbitMQ用户

**后台启动RabbitMQ**

`rabbitmq-server -detached`

**创建用户**

`rabbitmqctl add_user myuser mypasswd`

外部将通过*myuser*和*mypasswd*访问RabbitMQ。

`rabbitmqctl add_vhost myvhost`

`rabbitmqctl set_user_tags mytag`

`rabbitmqctl set_permissions -p myvhost myuser ".*" ".*" ".*"`

`rabbitmqctl set_permissions -p / myuser ".*" ".*" ".*"`

### 1.2.Celery

首先，你需要拥有一个写python的环境。

`pip install celery`

## 2.使用

话不多说，先上代码。

**tasks.py**

```python
# coding: utf-8
from celery import Celery

app = Celery('tasks', broker='amqp://user_david:david1234@localhost:5672/vhost_david',
             backend='amqp://user_david:david1234@localhost:5672/vhost_david')


@app.task
def add(x, y):
    return x + y
```

broker指定消息的代理对象，backend指定消息结果的存储对象。

写好代码，通过命令启动celery worker，`celery -A tasks worker --loglevel=info`

这时会看见如下的输出：

```
[tasks]
  . tasks.add

[2020-07-26 14:19:27,712: INFO/MainProcess] Connected to amqp://user_david:**@127.0.0.1:5672/vhost_david
[2020-07-26 14:19:27,755: INFO/MainProcess] mingle: searching for neighbors
[2020-07-26 14:19:28,828: INFO/MainProcess] mingle: all alone
[2020-07-26 14:19:28,897: INFO/MainProcess] celery@exocrdeMBP.lan ready.
```

然后在另外写一个脚本调用刚刚封装好的`add()`：

```python
from tasks import add
result = add.delay(5, 6)
print result.get()
```

执行这个脚本，可以得到求和的结果。在celery的输出则可以看到：

```
[2020-07-26 14:22:01,063: INFO/MainProcess] Received task: tasks.add[50edbd59-22b9-4dfb-bfb2-01dd33565623]  
[2020-07-26 14:22:01,089: INFO/ForkPoolWorker-4] Task tasks.add[50edbd59-22b9-4dfb-bfb2-01dd33565623] succeeded in 0.0234231399991s: 11
```



