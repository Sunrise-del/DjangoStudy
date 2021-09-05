### M、T、V

Django将数据交互的过程分为了3个层次：

- Model：数据存储层，处理所有数据相关的业务，和数据库交互，提供数据的增删改查。也就是数据层，所有的数据相关的东西都会在这里处理。
- Template：模板层，处理具体页面的显示。所有前端页面相关的东西都会在这里处理。
- View：业务逻辑层，处理具体的业务逻辑。用来连通Model层和Template。可以简单的理解为后端层，在这里处理具体的逻辑。

所以到这儿我们也就能理解为什么这个设计模式叫MTV了吧，M-->Model、T-->Template、V-->View

### 请求与响应过程

![image-20210905135904625](/Users/jiangxin/Library/Application Support/typora-user-images/image-20210905135904625.png)

1. 用户通过浏览器对服务器发起request请求，服务器接收请求后，通过View的业务逻辑层进行分析，会同时给Model层和Template层发送指令；

2. Model层与数据库进行交互，将交互的结果返回给View层；

3. Template层收到指令后，调用相应的模板，并把这个模板返回给View层；

4. View层收到Model层和Template层的响应后，首先将Model层返回的数据赋给Template层返回的html页面模板，然后再将这个有数据的模板，以正确的响应格式返回给浏览器，浏览器将这个响应解析后，最终呈现给我们。

   