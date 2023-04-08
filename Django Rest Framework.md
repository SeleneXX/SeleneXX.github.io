# Django Rest Framework

## 1. CBV

与传统django不同，传统django在view.py中，写了很多方法，每一个url绑定一个方法，这是FBV，function based view。

Class Based View在view中写类，在url中调用类的方法，将传统django优化成面向对象。

CBV示例：

urls.py

````python
# FBV
path('login/', view.login),
#CBV
path('login/', view.loginClass.as_view()),
````

view.py

````python
class loginClass(View):
    
    def get(self, request):
        pass
        
    def post(sefl, request):
        pass
````

FBV在调用时不需要加括号因为如果加了括号，对应的函数就执行了，需要给django传一个方法对象，让django调用。

CBV通过调用View类中的```as_view()```方法，返回View.view方法。实际执行的时候就是如下语句

````python
path('login/', View.view),
````

一旦浏览器发起get请求，执行```View.view```，自动帮你分发请求到get，post等方法。

## 2. 前后端分离

传统的django，使用render，将使用模板渲染好的html代码，直接发送给客户端。前后端高度耦合，如果需要调整前端页面，可能需要大量修改后端代码。

前后端分离后，客户端浏览器先请求一个前端页面，在渲染前端页面的时候，如果缺失数据，则向后端请求数据。后端返回数据，给前端渲染。数据使用JSON格式。而django.models查出来默认是queryset格式，不能直接传给前端，需要先序列化为JSON再传。

![](F:\note\SeleneXX.github.io\picture\indepent_frontend_backend-16318493319127.png)

这样，后端的代码，可以供各种客户端使用，浏览器，app，小程序，都可以使用这一套后端代码。

django本身提供了序列化功能，但是不好用。

````python
from django.core import serializers
````

DRF提供了非常强大的序列化和反序列化功能。

此外，django接受post请求的数据，必须是特定的格式：x-www-form-urlencoded，不能是JSON。

DRF重构了View方法，修改了post请求的request，使得JSON可以传入。存在request.data里。以后就不需要request.post了

## 3. API

应用程序编程接口，即应用程序对外提供了一个操作数据的入口。为了使接口更容易使用和维护，需要接口实现规范。

常见的规范：

- restful：以资源为主的api接口规范。

  - 把服务端提供的所有的数据/文件都看成资源， 那么通过api接口请求数据的操作，本质上来说就是对资源的操作了.

    因此，restful中要求，我们把当前接口对外提供哪种资源进行操作，就把**资源的名称写在url地址**。

  - web开发中操作资源，最常见的最通用的无非就是增删查改，所以restful要求在地址栏中声明要操作的资源是什么。然后通过**http请求动词**来说明对该资源进行哪一种操作.

  访问路径不变，方法不同：

  ````
  /admin/
  	get			-->查看所有资源
  	post		-->添加
  /admin/1
  	get			-->查看单个资源
  	delete		-->删除单个资源
  	put			-->更新单个资源
  	patch		-->更新单个资源
  ````

- RPC：远程过程调用。以动作为主的api接口规范。从字面上理解就是访问/调用远程服务端提供的api接口。

  



