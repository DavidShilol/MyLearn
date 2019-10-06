# Pagination

django提供了一些分页功能的类，下面是使用的例子

```python
# views.py
from django.core.paginator import EmptyPage, PageNotAnInteger, Paginator
from django.shortcuts import render

def listing(request):
    contact_list = Contacts.objects.all()
    paginator = Paginator(contact_list, 25) # Show 25 contacts per page

    page = request.GET.get('page')
    contacts = paginator.get_page(page)
    return render(request, 'list.html', {'contacts': contacts})

# html
{% for contact in contacts %}
    {# Each "contact" is a Contact model object. #}
    {{ contact.full_name|upper }}<br />
    ...
{% endfor %}

<div class="pagination">
    <span class="step-links">
        {% if contacts.has_previous %}
            <a href="?page=1">&laquo; first</a>
            <a href="?page={{ contacts.previous_page_number }}">previous</a>
        {% endif %}

        <span class="current">
            Page {{ contacts.number }} of {{ contacts.paginator.num_pages }}.
        </span>

        {% if contacts.has_next %}
            <a href="?page={{ contacts.next_page_number }}">next</a>
            <a href="?page={{ contacts.paginator.num_pages }}">last &raquo;</a>
        {% endif %}
    </span>
</div>
```

**class Paginator(object_list, per_page, orphans=0, allow_empty_first_page=True)**

+ 必选参数
  + object_list：需要分页的可迭代对象
  + per_page：每页最大数据行数

- 可选参数
  - orphans：最后一页的最大数据行数
  - allow_empty_first_page：允许没有对象返回，默认为True
- 方法
  - get_page(number)：获取对应页数的Page对象，页码从1开始计数，页码小于1则返回第一页，大于最大页码则返回最后一页，参数非数字则返回第一页
  - page(number)：返回对应页数的Page对象，页码错误会报错
- 属性
  - count：总行数
  - num_pages：总页数
  - page_range：页码取值范围 

**Page objects**

- 方法
  - has_next()
  - has_previous()
  - has_other_pages()：是否有上一页或下一页
  - next_page_number()
  - previous_page_number()
  - start_index()：返回当前页的第一行数据在总的object_list中的1-based索引值
  - end_index()
- 属性
  - object_list：当前页的object_list
  - number：当前页的1-based页码
  - paginator：关联的Paginator对象

