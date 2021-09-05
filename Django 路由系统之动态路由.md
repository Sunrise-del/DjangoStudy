## 写在前面
首先我们要知道，在Django框架中有一个配置文件叫url.py，这个文件里面操纵的就是我们的路由系统。他是用于记录 Django 项目的 URL 映射关系，也就是说，这个文件里面实现了url和我们view.py文件中函数的对应关系。下面我们就来具体说下他们是怎么实现对应的

## 一、一一对应
在url.py文件中，使用这样的格式，就能实现url和view中函数的一一对应关系：如/`login/->def login`

```python
urlpatterns = [
    # path('admin/', admin.site.urls),
    path('select_class/', views.select_class),
    path('add_class/', views.add_class),
    path('del_class/', views.del_class),
    ]
```
如上，我们就实现了如url select_class/和函数views.select_class的一一对应关系，我们在网站输入select_class/的url时，Django后台就会去view.py文件中找相对应的select_class函数，实现其功能后，再把返回的数据传递给前端，待前端页面渲染完毕后，展示在我们的浏览器上。

## 二、url里面加正则表达式

 - **单个正则**

但通常情况下我们需要实现的功能仅靠url与函数一一对应是实现不了的，比如我们需要动态的去更改url，此时就需要在url里面添加正则表达式，再在对应函数中添加一个参数去接收正则表达式的部分。格式如`/add-user/(\w+)/ -> def add_user(request, add_user)`。

```python
# url.py
urlpatterns = [
    # path('admin/', admin.site.urls),
    path('select_class/(/w+)/', views.select_class),
]

# view.py
def select_class(request, s_c):
    print(s_c)
    class_list = mysql_oper.get_all_data("select id,title from class", [])
    return render(request, 'select_class.html', {'class_list': class_list})
```

如上，url.py文件中select_class/(/w+)/中的正则表达式中的值就会传递给view.py文件中select_class函数的s_c参数，此时在浏览器中输入select_class/任意数字字母下划线字符，后台就会去执行select_class函数，而执行的url就是`select_class/任意数字字母下划线字符`。

 - **多个正则**

到这里可能大家又要想，我url可以传一个正则表达式，可以传两个吗？答案是可以的

第一种：格式如`/add-user/(\w+)(\w+)/ -> def add_user(request, add_user1, add_user2)`。这里需要注意的是，正则表达式和函数中的参数必须是按顺序依次匹配的。即在url中填入的第一个正则表达式的字符传递给函数中第一个参数，url中填入的第二个正则表达式的字符传递给函数中第二个参数。当然我们在函数中也可以使用可变参数，这时url中的正则就会默认传递给相对应的可变参数中。如：`/add-user/(\w+)(\w+)/ -> def add_user(request, *args,**kwargs)`

第二种格式：`/add-user/(?P<a1>\w+)(?P<a2>\w+)/ -> def add_user(request, a1, a2)` 这样写后，就不需要一定按照顺序，系统会自动用P<a1>所对应的正则表达式去找函数中的a1形参，a2是同样的道理。当然我们在函数中也可以使用可变参数，这时url中的正则就会默认传递给相对应的可变参数中。如：`/add-user/(\w+)(\w+)/ -> def add_user(request, *args,**kwargs)`

需要注意的是，两种格式不能混合使用。比如不能像这种：`/add-user/(\w+)(?P<a2>\w+)/ -> def add_user(request, a1)`  这样就会提示找不到函数中的参数。即使是函数中使用可变参数作为形参也不行噢。

## 三、url后面的/和$ 

```python
urlpatterns = [
    # path('admin/', admin.site.urls),
    path('select_class/', views.select_class)
    ]
```
我们还是拿上面这个代码例子来说，path函数参数里面的'select_class/‘如果最后没有'/'，会是什么情况呢？
其实这个url里面填写的字符串就相当于一个正则表达式。如果不填'/'，那么我们网站输入url时输入select_class可以匹配到views.select_class，输入select_classhjjasj也可以匹配到views.select_class。因为代码只判断匹配到了select_class就判定匹配成功。

所以如果我们需要填写'/'，另外还有一种方法是在结尾写死`'$'`以示结束，也是同样的道理，代码查询到`'$'`，就会认为匹配到select_class就结束了，如果后面字符就会判定为匹配失败。

## 四、.html
上面我们说了，url里面其实是一个字符串，那我们就可以随便指定字符串的内容，用户在浏览器里面输入网址后，Django只需要判断这个url字符串和代码里面的url字符串是否匹配，如果匹配就执行函数。所以在上面的代码中，我们完全可以把url改为select_class.html，那为什么需要这么改呢？

加了.html的url渲染出来的网站叫静态网站，静态网站最大的特点是快，因为静态网站只要前端网页一访问，只需要去模板里面拿到数据就可以直接展示出来。而且静态网站的seo级别也是很高的。所以我们把代码里的url改为select_class.html，就可以造一个伪静态网站，就比一般的网站快很多。如下面代码，记得加`'$'`

```python
urlpatterns = [
    # path('admin/', admin.site.urls),
    path('select_class.html$', views.select_class)
    ]
```

