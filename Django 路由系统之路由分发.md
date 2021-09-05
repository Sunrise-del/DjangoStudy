## 写在前面
我们知道，Django项目里面肯定会有多个人协同完成不同应用的情况，那么这种情况下，url与view的关系应该怎么处理呢。如果大家都用项目里面那个urls.py文件，是不是有搞混的风险呢。比如A和B的url可能会同时对于一个后台view函数。这种问题，Django问我们提供了路由分发的方法来解决。
它可以使每一个应用都拥有自己的urls.py文件，然后再去自己的函数里面找对应关系，这样就不会有多个人操作一个文件的问题，也就不会有混乱的问题出现了。
## 实现方式
 - 1、**在我们项目的urls.py文件中进行如下操作**
![在这里插入图片描述](https://img-blog.csdnimg.cn/1d64fe579b9140389e61b3b42dd27ae0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5Lq65rCU5bCP5aec,size_20,color_FFFFFF,t_70,g_se,x_16)
其中`path('student/', include('student.urls'))`这一句是指现在项目中的urls.py文件中匹配"student/"，匹配成功了后，去student应用中的urls.py文件继续进行匹配。
如果我们有多个app，也是一样的逻辑。
 - 2、**在我们应用的urls.py文件中进行如下操作**
![在这里插入图片描述](https://img-blog.csdnimg.cn/c701d2adbd78426598438504f4a35be4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5Lq65rCU5bCP5aec,size_20,color_FFFFFF,t_70,g_se,x_16)
在项目urls.py文件中匹配成功了后，再在相应的应用内进行匹配。



我们可以看到，这两个文件连起来，就组合成了我们url与view函数的对应关系：用户在浏览器输入url后，会先在第一层项目的urls.py文件中找url与view的对应关系，匹配到了student应用后，继续在student中的urls.py文件中找url与view的对应关系，也就是说，如果浏览器输入的url是`http://127.0.0.1:8000/student/index/`，那么我们就会去执行views.index()函数，执行完后，将结果再返回给浏览器。

如果项目中有多个应用，这样操作就达到了路由分发的效果。
