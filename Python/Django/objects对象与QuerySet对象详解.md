# objects对象与QuerySet对象详解

**objects对象**

模型.objects实际上是django.db.models.Manager的对象，是一个空壳类 Manager对象的方法是从QuerySet类拷贝来的，如all、filter、order_by等方法

**QuerySet对象**

+ 常用方法

  + get

    返回某行记录

  + filter

    返回满足条件的QuerySet，是记录的集合

  + exclude

    与filter相反，是filter记录的补集

  + annotate

    返回QuerySet，为QuerySet添加字段，字段的值是聚合函数的结果

  + order_by

    返回排序后的QuerySet，升序order_by("price")，降序order_by("-price")

  + aggregate

    返回一个存放聚合计算结果的字典。如Avg计算，key为"计算字段"+"__avg"（两个下划线）

+ 聚合函数

  聚合函数有Avg、Count、Max等等，使用这些方法，需要导入：`from django.db.models import Avg, Count, Max`

+ 查询表达式

  + Q

    我们可以把条件查询放进Q表达式中，完成复杂的条件查询。比如filter(Q(name='allen')|Q(name='jack'))，查询名字为allen或jack的所有记录。Q的组合运算符有："**|**", "**,**", "**~**"，分别对应sql中的**or**，**and**，**补集**

  + F

    F方法允许在未连接数据库的情况下，引用数据库字段，而且内存中并未真正获取该字段的值。只是借F方法生成原声SQL，在数据库执行

+ annotate

  比较两个执行逻辑：

  ```python
  b = Book.objects.annotate(author_name=F('author__name'))
  for bb in b:
      if b.author_name == 'allen':
          print(b)
  ```

  ```python
  b = Book.objects.filter(author__name='allen')
  for bb in b:
      print(b)
  ```

  如第一段代码，使用了annotate，为QuerySet类添加了author_name字段，所有的处理交给python后台处理。第二段将判断交给数据库完成，这会生成大量sql语句，影响效率。

