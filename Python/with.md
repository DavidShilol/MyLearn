# with

**相关概念**

+ 上下文管理协议(Context Management Protocol)：协议必须实现`__enter__()`和`__exit__()`,

+ 上下文管理器(Context Manager)：支持上下文管理协议的对象，即实现了`__enter__()`和`__exit__()`的对象。当执行with语句时，上下文管理器会在进入with语句体前调用enter方法，退出语句体后调用exit方法。除了用with调用外，也可以直接调用其方法来使用。

+ 运行时上下文(runtime context)：由上下文管理器创建，通过上下文管理器的enter和exit方法实现。with语句支持>运行时上下文这一概念。

+ 上下文表达式(Context Expression)：with之后的表达式，表达式返回一个上下文管理器对象。

+ 语句体(with-body)：with包裹起来的代码块。

**with语法**

with语句的语法格式

```python
with context_expression [as target(s)]:
  	 with-body
```

操作文件对象

```python
with open() as f:
  	pass
```