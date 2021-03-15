# code

1.连接

参数

```
SQLALCHEMY_DATABASE_URI = "mysql+pymysql://root:Exocr123456!@localhost/todo"
```

语句

```python
from flask_sqlalchemy import SQLAlchemy
db = SQLAlchemy(app)
```

2.对象-表结构映射

```python
class Todo(db.Model):
    __tablename__ = 'todo'
    id = db.Column(db.Integer, primary_key=True)
    content = db.Column(db.String(256))
    create_at = db.Column(db.DateTime)
    is_finished = db.Column(db.Boolean)
    finished_at = db.Column(db.DateTime)

    def __repr__(self):
        return '{} - {}'.format(self.id, self.content)

db.create_all()
```

关键点：

+ db.Model
+ \_\_tablename\_\_
+ db.Column
+ db.create_all()

3.增删查改

增

```python
db.session.add(Todo(content='xxx'))
db.session.commit()
```

删

```python
db.session.delete(Todo.query.filter(Todo.id=1).first())
db.session.commit()
```

查

```python
Todo.query.filter(Todo.id==1).first()
```

改

```python
Todo.query.filter(Todo.id==1).first().content = 'hhh'
db.session.commit()
```

涉及修改操作，都需要db.session.commit()