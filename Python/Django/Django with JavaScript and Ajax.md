#  Django with JavaScript and Ajax

1.**js_test.html**

```html
<a onclick="urlHtml()"><button>js test</button></a>

<script type="text/javascript">
function urlHtml(){
    var toUrl = "{% url 'ajax:index' %}?param=baby";
    window.location.href=toUrl;
}
</script>
```

2.**views.py**

```python
def index(request):
    print('param:', request.GET.get('param', ''))
    return render(request, 'ajax/index.html')
```

3.**settings.py**

```python
STATIC_URL = '/static/'
STATICFILES_DIRS = [os.path.join(BASE_DIR, "static")]
```

4.**创建静态资源目录**

在app下创建目录static，然后在static下创建目录app_name，也就是你的app名字，最后在刚刚创建的app_name目录下创建js目录。路径为："app_name/static/app_name/js"。

5.**放置jquery文件**

这里用的是jquery.min.js，放在刚刚创建的js目录，也就是说jquery文件的路径为相对Django项目的"/app_name/static/app_name/js/jquery.min.js"。

6.**baby.html**

```html
{% load static %}
<script type="text/javascript" src="{% static 'ajax/js/jquery.min.js' %}"></script>
<a onclick="urlHtml()"><button>test</button></a>

<script type="text/javascript">
function urlHtml(){
    var post_data = {
        "name": "allen",
    };
    $.ajax({
        url: "",
        type: "POST",
        data: post_data,
        success: function(data){
            data = JSON.parse(data);
            if (data["status"] == 1){
                console.log("1");
            }else{
                console.log(data["result"]);
            }
        }
    });
}
</script>
```

7.**views.py**

```python
def baby(request):
    if request.method == "POST":
        if request.is_ajax():
            name = request.POST.get('name', '')
            print('name:', name)
            status = 0
            result = "Error!"
            return HttpResponse(json.dumps({
                "status": status,
                "result": result
                }))
    else:
        return render(request, 'ajax/baby.html')
```



**传数据还有另一个方法**

后台用django.http.JsonResponse，前台取的data就是一个字典，不需要JSON.parse(data)进行转换。

```python
# 利用django提供的JsonResponse方法，替换原先的HttpResponse即可。
# JsonResponse第二个参数是safe，当传入数据非字典时，需要赋值为True，默认值是False
data1 = {'a': 1, 'b': 2}
return JsonResponse(data1)
data2 = [4, 5, 6]
return JsonResponse(data2, safe=True)
```

