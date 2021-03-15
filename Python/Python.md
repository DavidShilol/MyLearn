[toc]

### ORM

Object-Relation Mapping，作用是在关系数据库和对象之间做一层映射。ORM将业务逻辑和数据逻辑分离，无需再操作千篇一律的SQL操作，用操作对象的思想去完成业务逻辑。

### 装饰器

装饰器能修改其他的函数，能够在不改变函数名和入参的前提下对函数做变动。

```python
import functools


def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print('call %s():' % func.__name__)
        print('args = {}'.format(*args))
        return func(*args, **kwargs)

    return wrapper
  
@log
def test(p):
    print(test.__name__ + " param: " + p)
```

### Unicode与UTF8

unicode包含了字符集定义与编码方案，UTF8是unicode标准的具体实现，是一种编码方式。UTF8为变长编码，需要1-3字节表示文字。

UTF8优点：

+ 能表示非常大的字符集
+ 占用空间小

缺点：

+ 寻找第N个字符复杂度为O(n)，相比于定长编码，无法通过偏移量定位，只能遍历确认每个字符的编码长度后推算

### 从Tornado和Flask看阻塞/非阻塞、同步/异步

首先明确，Tornado同时支持阻塞/非阻塞、同步/异步，Flask天然阻塞式，支持同步/异步。

是否阻塞，关键看处理响应的进程是否挂起，挂起的是非阻塞，因为等待资源时进程挂起，处理器可以处理后续的请求。相反，如果不挂起，一直等待资源，后续的请求都得排队等待。

是否同步，关键是看是否立即返回结果，立即返回结果的是同步，因为同步就是需要当即返回期望值(如银行取钱)。相反，则是异步。

**为什么Tornado支持非阻塞？**

Tornado的IO异步是基于epoll实现的，每隔一段时间就去查看监听的所有socket中读取数据，大大提高了吞吐量。