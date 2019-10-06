# 通过URL传参

**普通视图传参**

+ 传少量参数

  ```python
  # urls.py
  path('book/<int:page>/<str:name>/', views.book, name='book')
  
  # views.py
  def book(request, page, name):
      return HttpResponse('book page:{} name:{}'.format(page, name))
  ```

  

+ 传多个参数

```python
# urls.py
path('book/<int:page>/<str:name>/<int:price>/', views.book, name='book')

# views.py
def book(request, **kwargs):
    return HttpResponse('book page:{} name:{} price:{}'.format(page, name, price))
```

**类视图传参**

```python
# urls.py
path('book/<int:page>/<str:name>/', views.BookView.as_view(), name='book')

# views.py
class BookView(generic.ListView):
    model = Book
    template_name = 'book_manage/book.html'
    
    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['page'] = self.kwargs.get('page', 0)
        context['name'] = self.kwargs.get('name', '')
```

类视图中，url的参数，在后台存放在self.kwargs这个字典中