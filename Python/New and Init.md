# New and Init

new在实例创建之前被调用，用于生成实例并返回实例对象，是一个静态方法。init是实例对象创建完成后被调用，用于设置一些属性的初始值，是一个实例方法。

new是用于控制实例的生成，init是用于控制实例的属性初始化。

**init**

init可以在创建类之后，设置一些属性的初始值。

```python
class Student:
  	def __init__(self, name, age):
      	self.name = name
        self.age = age
        print('create a student')
        print(self.name, self.age)
```

**new**

只有继承了object类的新式类才有new方法。

```python
class Book(object):
  	def __new__(cls):
    		print('new a class')
        return super(Book, cls).__new__(cls)
```

new方法的参数cls表示当前类，运行时自动识别，和类实例方法中的self很相似，都表示当前实例。同时要注意，new方法必须要有返回值，返回实例对象。



new的用途：当继承一些不可变的class（int，str，tuple）时，能够自定义类的实例化过程、以及实现自定义的metaclass。以int为例，在类实例创建的过程中对参数做取绝对值的处理：

```python
class PositiveInteger(int):
  	def __new__(cls, value):
      	print('in new')
        return super(PositiveInteger, cls).__new__(cls, abs(value))
```

```
>>> p = PositiveInteger(-3)
>>> p
in new
3
```

乍一看init和new很像，都是在形如s = PositiveInteger(-3)的过程中执行。实际上，这个句子的执行过程是这样的：先执行new方法，new返回一个实例对象，并隐式调用init方法。