## 安装

 - 第一种：使用终端在线安装

```bash
$ sudo pip3 install django

# 还可以指定版本号来进行安装
$ sudo pip3 install django==2.2.10
```

 - 第二种：在官方网站下载 Django 2.2.10 安装包，通过下面的命令解压并安装：
 
```bash
$ tar -zxvf Django-2.2.10.tar.gz 
$ cd Django-2.2.10
$ sudo python3 setup.py install
```
 - 安装完成后，进入Python解释器导入django包查看是否安装成功
 
```bash
>>>import django
>>>django.get_version()
2.2.10
```
## 创建项目
 - 1.使用如下命令创建Django项目：`django-admin startproject 项目名称`
使用此命令后，即可在当前目录下创建一个Django的文件夹。即成功创建了Django项目，我们可以在终端使用manage.py命令行工具运行Django服务器，也可以使用pycharm编译器打开文件，直接在编译器中运行项目。

- 2.使用pycharm编译器创建Django项目
点击File-New Project，在弹出的弹窗中选择Django，选择正确的虚拟环境，点击创建项目即可![在这里插入图片描述](https://img-blog.csdnimg.cn/fd5b94db4a8b489c8291fef7ee01a6a5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5Lq65rCU5bCP5aec,size_20,color_FFFFFF,t_70,g_se,x_16)
不过不知道为啥，我使用pycharm创建时，每次都会报错说已经项目名已经存在（确定项目名没有重复），百度了没找到合适的答案，如果有大佬知道怎么整，欢迎告知啊hhhhha。

## 配置文件结构
执行创建项目的文件后，django会自动生成一系列配置文件：
 - manage.py文件：管理Django项目的重要命令行工具，主要用于启动项目、创建应用和完成数据库的迁移等。
 - __ init __.py文件：用于标识当前所在的目录是一个 Python 包，如果在此文件中，通过 import 导入其他方法或者包会被 Django 自动识别。
 - settings.py文件： Django 项目的重要配置文件。项目启动时，settings.py 配置文件会被自动调用，而它定义的一些全局为 Django 运行提供参数，在此配置文件中也可以自定义一些变量，用于全局作用域的数据传递。
 - urls.py文件：用于记录后端函数和前端url映射关系的一个文件。它属于项目的基础路由配置文件，路由系统就是在这个文件中完成相应配置的，项目中的动态路径必须先经过该文件匹配，才能实现 Web 站点上资源的访问功能。
 - wsgi.py文件：WSGI（Web Server Gateway Interface）服务器程序的入口文件，主要用于启动应用程序。它遵守 WSGI 协议并负责网络通讯部分的实现，只有在项目部署的时候才会用到它。它用于定义Django用的socket。
 - templates.py文件：模板文件，也就是存放前端页面html文件的。自己试了一下，这个文件只有在使用pycharm创建Django项目时才自动创建，使用命令行创建的时候不会自动创建。不过没关系，自己在项目下创建一个也可以的，在settings.py文件里配置好就行

## 创建应用
在Django项目所在的文件下，使用manage.py提供的startapp命令创建：`python manage.py startapp 应用名称`
eg：
```bash
python3 manage.py startapp app01
```

## 配置settings.py文件
 - **在`INSTALLED_APPS`列表中添加自己创建的应用。**
 默认情况，INSTALLED_APPS中会自动包含下图中注释解释的应用，它们都是Django自动生成的。我们只需要把我们自己创建的应用加在这个列表中就行。如图，student就是我自己创建的app：
![在这里插入图片描述](https://img-blog.csdnimg.cn/62e12cae0f9c46078412a659badd5a38.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5Lq65rCU5bCP5aec,size_20,color_FFFFFF,t_70,g_se,x_16)
 - **在`TEMPLATES`列表中配置`templates.py`文件路径**
 如果是用pycharm创建的项目，会自己生成templates.py文件，这里也会自己生成templates路径。但如果是命令行创建的项目，就需要自己创建一个templates.py文件，并且配置templates文件路径了。
如图，os.path.join(BASE_DIR)指的是当前目录，所以我们需要把templates文件放在项目根目录下，这样程序才能准确的找到我们的templates.py文件：
 ![在这里插入图片描述](https://img-blog.csdnimg.cn/db5023a8aa7f4cd0aef00dbe7a7ee982.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5Lq65rCU5bCP5aec,size_20,color_FFFFFF,t_70,g_se,x_16)
![在这里插入图片描述](https://img-blog.csdnimg.cn/f02cbe1b683a479bb3548ad8990465fa.png)
 - **css、js等静态文件创建及配置路径**
如果项目中需要用到css、js等静态文件，也是需要创建文件夹并配置路径的：我们需要首先在项目根目录下创建一个文件夹用于存放这些文件，文件名起什么都可以，但最好规范一点点hhha，可以叫static。
然后在setting.py文件中`STATIC_URL = '/static/'`这一句代码下面，先加一个`STATICFILES_DIRS`元组，在元组里面添加`os.path.join(BASE_DIR, "static"),`代码即可。这里要注意要在配置路径最后加逗号，不然会报错。如图
![在这里插入图片描述](https://img-blog.csdnimg.cn/dfc1c797e87b4e8cb84cc797547b5421.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5Lq65rCU5bCP5aec,size_20,color_FFFFFF,t_70,g_se,x_16)

 

参考链接：
http://c.biancheng.net/django/
https://www.liujiangblog.com/course/django/88
