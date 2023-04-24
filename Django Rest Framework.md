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
    DRF使用restful接口，在定义一个页面时，在urls中新建两个path，一个针对全部数据，一个针对一条数据。
  ````
  
  ```python
  from django.urls import path, re_path
  from drfdemo import views
  
  urlpatterns = [
      # path("admin/", admin.site.urls),
      path("student/", views.StudentView.as_view()),						# 针对全部数据
      re_path("student/(\d+)/", views.StudentDetailView.as_view()),		# 针对一条数据，传入对应的id
  ]
  ```

- RPC：远程过程调用。以动作为主的api接口规范。从字面上理解就是访问/调用远程服务端提供的api接口。

## 4. 序列化和反序列化

#### 原始方式

首先，创建序列化类，在其中自定义需要序列化的字段。

```python
from rest_framework.views import APIView
from rest_framework import serializers
# 引入DRF中的response代替传统的HttpResponse
from rest_framework.response import Response
from drfdemo.models import Student


# Create your views here.

class StudentSerializer(serializers.Serializer):
    student_name = serializers.CharField(source="name")		# 如果想要修改Json中的键名，需要指定source="xxx"去找数据库中的字段
    sex = serializers.BooleanField()
    age = serializers.IntegerField()
    class_null = serializers.CharField(required=False)		# required=False表示可以为空，即校验时，可以没有这个字段
```

serializers.Serializer包含几个关键的初始化值：

- instance：序列化时，传入需要序列化的对象
- data：反序列化时，传入需要反序列化的数据
- many：当序列化时传入的对象是queryset，里面包含了很多数据对象，将其置为True，代表全都序列化

DRF中的Response可以处理Json格式

序列化一个集合对象，用于展示全部数据库所有数据：

```python
class StudentView(APIView):

    def get(self, request):
        students = Student.objects.all()
        serializer = StudentSerializer(instance=students, many=True)
        return Response(serializer.data)
```

反序列化post请求中的json数据。使用序列化器中的`is_valid()`接口，判断数据是否合法。这里要保证构建序列化器时，数据字段的定义和数据库中的定义保持一致，保证两者合法性的相同。

可以使用传统的if else来判断合法并保存。这里使用官方文档中的写法，`is_valid()`中传入参数`raise_exception=True`，使其在检测有误时报错，使用try except语句接收这条错误并返回错误信息。

保存时，可以直接从序列化器中，拿到合法的数据对象`validated_data`，使用`create`在数据库中新建一行，并把这新建对象序列化为Json数据返回给前端。

```python
class StudentView(APIView):

    def get(self, request):
        students = Student.objects.all()
        serializer = StudentSerializer(instance=students, many=True)
        return Response(serializer.data)

    def post(self, request):
        serializers = StudentSerializer(data=request.data)
        try:
            serializers.is_valid(raise_exception=True)
            # 插入记录
            stu = Student.objects.create(**serializers.validated_data)
            ser = StudentSerializer(instance=stu, many=False)
            return Response(ser.data)

        except Exception as e:
            print(e)
            return Response(serializers.errors)
        
        # if serializers.is_valid():
        #     # 存入数据库
        #     pass
        # else:
        #     # 错误
        #     return Response(serializers.errors)
```

删除使用delete请求，更新使用put请求。

```python
class StudentDetailView(APIView):

    def get(self, request, id):
        # 查询一条数据
        students = Student.objects.get(pk=id)
        serializer = StudentSerializer(instance=students, many=False)
        return Response(serializer.data)

    def delete(self, request, id):
        # 删除一条数据
        Student.objects.get(pk=id).delete()
        return Response()

    def put(self, request, id):
        # 更新一条数据
        serializers = StudentSerializer(data=request.data)
        try:
            serializers.is_valid(raise_exception=True)
            # 更新记录
            # 这里不能用stu接收update方法的返回值，因为update返回的是更新的条目，而create返回的是插入的对象
            Student.objects.filter(pk=id).update(**serializers.validated_data)
            # 手动获取更细的数据
            stu = Student.objects.get(pk=id)
            ser = StudentSerializer(instance=stu, many=False)
            return Response(ser.data)

        except Exception as e:
            print(e)
            return Response(serializers.errors)
```

#### 进阶方式

使用`serializer.save()`，将原来复杂的写入和新增逻辑解耦，封装到已经定义好的方法内。

save方法通过serializer中传入的有没有instance来判断，是插入还是修改数据。插入数据时，没有instance，所以选择插入数据，调用一个create方法，在serializer中的父类里，create方法只弹出一个异常，告诉你需要自己重写create方法来指定需要插入数据的表。

```python
class StudentSerializer(serializers.Serializer):
    name = serializers.CharField()
    sex = serializers.BooleanField()
    age = serializers.IntegerField()
    class_null = serializers.CharField()

    def create(self, validated_data):
        # 将数据插入表
        new_student = Student.objects.create(**validated_data)
        return new_student
```

这样，在插入数据时，只需要使用`serializer.save()`

```python
    def post(self, request):
        serializers = StudentSerializer(data=request.data)
        try:
            serializers.is_valid(raise_exception=True)
            # 插入记录
            serializers.save()
            return Response(serializers.data)

        except Exception as e:
            print(e)
            return Response(serializers.errors)
```

同理，新增数据，要同时传入更新的对象instance。只有queryset对象可以update，而如果传入queryset，序列化器会自动解包。所以必须传入单个对象，从数据库根据主键重新取queryset然后update。

```python
class StudentSerializer(serializers.Serializer):
    name = serializers.CharField()
    sex = serializers.BooleanField()
    age = serializers.IntegerField()
    class_null = serializers.CharField()

    def create(self, validated_data):
        new_student = Student.objects.create(**validated_data)
        return new_student

    def update(self, instance, validated_data):
        Student.objects.filter(pk=instance.pk).update(**validated_data)
        updated_students = Student.objects.get(pk=instance.pk)
        return updated_students
```

```python
class StudentDetailView(APIView):

    def get(self, request, id):
        students = Student.objects.get(pk=id)
        serializer = StudentSerializer(instance=students, many=False)
        return Response(serializer.data)

    def delete(self, request, id):
        Student.objects.get(pk=id).delete()
        return Response()

    def put(self, request, id):
        student = Student.objects.get(pk=id)
        serializers = StudentSerializer(instance=student, data=request.data)
        try:
            serializers.is_valid(raise_exception=True)
            # 更新记录
            serializers.save()
            return Response(serializers.data)

        except Exception as e:
            print(e)
            return Response(serializers.errors)
```

#### Model Serializer

和ModelForm一样，使用class meta导入已经创建好的model，自动构建field，create和update。

```python
class StudentModelSerializer(serializers.ModelSerializer):
    # 想更改class_null的field的名字，将其指向表中的class_null
    class_num = serializers.CharField(source="class_null")
    class Meta:
        model = Student
        # 排除掉class_null和主键，其余field自动生成
        exclude = ["id", "class_null"]
```

#### Model Serialzer 自定义校验

类似modelform的clean_xxx钩子方法。验证不通过需要抛出异常。

```python
class PublishModelSerializer(serializers.ModelSerializer):
    class Meta:
        model = Publish
        fields = "__all__"
        
    # 和modelform一样，创建钩子方法
    def validate_name(self, value):
        if name.endswith("出版社"):
            return value
        else:
            # 验证失败
            raise serializers.ValidationError("出版社名称必须以出版社结尾")
```

## 5. 视图

DRF提供的视图的主要作用：

- 控制序列化器的执行（校验，保存，转换数据）
- 控制数据库模型的操作

Rest Framework提供了众多的通用视图基类与扩展类，以简化试图的编写

### 5.1 GenericAPIView 通用视图类

继承自APIView，主要增加了操作序列化器和数据库查询方法。作用是为下面的Mixin扩展类的执行提供方法支持。通常在使用时，可搭配一个或多个Mixin扩展类。

提供的关于序列化器使用的属性与方法。

- `get_serializer_class(self)`：获取序列化器的类
- `get_serializer(self, *args, **kwargs)`：获取序列化类的实例对象
- `get_queryset(self)`：获取查询集
- `get_object(self)`：获取单一的资源对象

直接在定义类时，在最开始定义类变量传入查询的全表结果以及自定义的序列化器类。后面所有的方法，都可以调用genericapiview获取到（增删改查方法全部可以复用，不需要自己传参）。

```python
class BookSerializers(serializers.ModelSerializer):
    class Meta:
        model = Book
        fields = "__all__"
        

class BookView(GenericAPIView):
    
    queryset = Book.objects.all()
    serializer_class = BookSerializers

    def get(self, request):
        serializer = self.get_serializer(instance=self.get_queryset(), many=True)
        return Response(serializer.data)

    def post(self, request):
        serializers = self.get_serializer(data=request.data)
        try:
            serializers.is_valid(raise_exception=True)
            # 插入记录
            serializers.save()
            return Response(serializers.data)

        except Exception as e:
            print(e)
            return Response(serializers.errors)


class BookDetailView(GenericAPIView):
    
    queryset = Book.objects.all()
    serializer_class = BookSerializers
    
    def get(self, request, pk):
        serializer = self.get_serializer(instance=self.get_object(), many=False)
        return Response(serializer.data)

    def delete(self, request, pk):
        self.get_object().delete()
        return Response()

    def put(self, request, pk):
        serializers = self.get_serializer(instance=self.get_object(), data=request.data)
        try:
            serializers.is_valid(raise_exception=True)
            # 更新记录
            serializers.save()
            return Response(serializers.data)

        except Exception as e:
            print(e)
            return Response(serializers.errors)
```

在url中传入id时，需要绑定pk作为名称，使得genericapiview可以通过名字使用该变量。

```python
path("book/", views.BookView.as_view()),
re_path("book/(?P<pk>\d+)/", views.BookDetailView.as_view()),
```

### 5.2 Mixin混合类

把增删改查查封装到了5个单独的类，需要用到哪个功能，就在自定义的类中多继承对应的类。

```python
from rest_framework.mixins import ListModelMixin, CreateModelMixin, UpdateModelMixin, RetrieveModelMixin, DestroyModelMixin

class PublishSerializers(serializers.ModelSerializer):
    class Meta:
        model = Publish
        fields = "__all__"
        

class PublishView(ListModelMixin, CreateModelMixin, GenericAPIView):
    queryset = Publish.objects.all()
    serializer_class = PublishSerializers

    def get(self, request):
        return self.list(request)

    def post(self, request):
        return self.create(request)


class PublishDetailView(RetrieveModelMixin, UpdateModelMixin, DestroyModelMixin, GenericDetailView):
    queryset = Publish.objects.all()
    serializer_class = PublishSerializers

    def get(self, request, pk):
        return self.retrieve(request, pk)

    def put(self, request, pk):
        return self.update(request, pk)

    def delete(self, request, pk):
        return self.destroy(request, pk)
```

在封装：rest_framework.generics中，完全封装好了上面的5种方法到两个类中：ListCreateAPIView, RetrieveUpdateDestroyAPIView。

ListCreateAPIView包含了get和post方法，RetrieveUpdateDestroyAPIView包含了get， put，delete方法。

只需要给自定义的类继承这两个类，就实现了5种接口。

```python
from rest_framework.generics import GenericAPIView, ListCreateAPIView, RetrieveUpdateDestroyAPIView

class AuthorSerializers(serializers.ModelSerializer):
    class Meta:
        model = Author
        fields = "__all__"
        

class AuthorView(ListCreateAPIView):
    queryset = Author.objects.all()
    serializer_class = AuthorSerializers


class AuthorDetailView(RetrieveUpdateDestroyAPIView):
    queryset = Author.objects.all()
    serializer_class = AuthorSerializers
```

### 5.3 ViewSet类

重新构建分发机制。ViewSetMixin类中，重新编写了as_view函数，使得自己编写的函数名可以关联到增删改查查5个方法。

通过闭包，传入映射字典，封装了view函数，然后通过for循环，对每个键值对建立映射。

```python
def view(request, *args, **kwargs):
    # 哪个类调用了as_view，cls就是哪个类。url中，FactoryView调用了as_view，所以拿到了FactoryView类
    self = cls(**initkwargs)
    
    # action = {"get": "get_all", "post": "add_obj"}
    for method, action in actions.items():
        # 去self中找到自定义的get_all方法
        handler = getattr(self, action)
        # 把原来self中写的get方法，重新定向给找到的get_all方法
        setattr(self, method, handler)

    self.request = request
    self.args = args
    self.kwargs = kwargs
	# 分发对应的方法到新设置的get_all方法
    return self.dispatch(request, *args, **kwargs)
```

##### GenericViewSet

在genericapiview的基础上，加上了Mixin重新分发的机制。直接继承所有的Mixin混合类，和genericviewset，并在路由中设置映射。

```python
class FactoryView(GenericViewSet, ListModelMixin, CreateModelMixin, RetrieveModelMixin, UpdateModelMixin, DestroyModelMixin):
    queryset = Factory.objects.all()
    serializer_class = FactorySerializers
```

```python
path("factory/", views.FactoryView.as_view({"get": "list", "post": "create"})),
re_path("factory/(?P<pk>\d+)/",
        views.FactoryView.as_view({"get": "retrieve", "delete": "destroy", "put": "update"})),
```

##### ModelViewSet

封装了上面5个类+genericviewset，直接继承这个即可。

```python
class FactoryView(ModelViewSet):
    queryset = Factory.objects.all()
    serializer_class = FactorySerializers
```

## 6. 路由

可以使用drf封装好的工具，直接生成路由项，不需要手写。注意，需要使用viewset把所有功能封装到一个类内。

```python
from rest_framework import routers
router = routers.DefaultRouter()
# 自动生成两句路由命令
router.register('factory', views.FactoryView, basename='factory')

urlpatterns = [
    "xxx"
]

urlpatterns += router.urls
```

