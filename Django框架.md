# Django ：

### 概念

是用Python开发的一个免费开源的Web框架，可以用于快速搭建高性能，优雅的网站！Django 的主要目标是使得开发复杂的、数据库驱动的网站变得简单。

采用了MVC的框架模式，即模型M，视图V和控制器C，也可以称为MVT模式，模型M，视图V，模板T。

# 一、虚拟环境

### 1、配置

安装：pip install virtualenv  

pip install virtualenvwrapper-win  

配置环境变量：环境变量（WORKON_HOME）：路径

### 2、命令

```python
1.创建虚拟环境
mkvirtualenv  虚拟环境名

2.显示所有虚拟环境
workon

3.进入指定虚拟环境
workon  虚拟环境名

4.退出虚拟环境
deactivate

5.删除虚拟环境
rmvirtualenv   虚拟环境名
```

# 二、django命令

```python
项目
# 创建项目：django-admin startproject 项目名
# 运行项目：python manage.py runserver  后可加ip
# 创建子路由：python manage.py startapp 子路由名

模型类
# 迁移：python manage.py makemigrations
# 数据库生成表：python manage.py migrate

管理员用户
# 创建超级管理员用户：python manage.py createsuperuser 
# 修改用户密码：python manage.py changepassword username

静态文件
# 静态文件收集：python  manage.py  collectstatic

添加数据
# 将项目下的 sql 文件添加到数据中中:mysql -u用户名 -p密码 -D 数据库名< xxx.sql

shell环境
# 进入shell环境：python  manage.py  shell

数据库
# 清空数据库：python manage.py flush
# 运行haystack的命令创建索引：python manage.py rebuild_index
```

# 三、Setting

### 1、数据库配置

##### 1、mysql

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',   # 指定数据库引擎
        'NAME': 数据库名,  
        'POST':3306,    # 端口号
        'HOST':'127.0.0.1',    # ip，默认
        'USER':'root',   # 数据库账号，默认
        'PASSWORD':密码,
        # 'ATOMIC_REQUESTS': True,    # 开启全局事务，不建议开启
    },
    # 可以指定多个库，再复制一份，改一下前面名字即可
}
```

> 指定项目所用的mysql 数据库

##### 2、redis

```python
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/0",     # 0指的是使用一号库
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        }
    },
    'session': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379/1',     # 使用二号库
        'OPTIONS': {
            'CLIENT_CLASS': 'django_redis.client.DefaultClient',
        }
    },
}
```

> 指定项目所用的redis 数据库

##### 3、指定session存储

```python
SESSION_CACHE_ALIAS = 'session'
# session存储缓存设置（选择以下之一即可）
SESSION_ENGINE = 'django.contrib.sessions.backends.cache'     # 只存储于redis
SESSION_ENGINE = 'django.contrib.sessions.backends.db'     # 只存储于mysql
SESSION_ENGINE = 'django.contrib.sessions.backends.cached_db'     # mysql和redis都存储
```

> session的三种存储位置

### 2、日志

```python
LOGGING = {
    # 版本
    'version': 1,
    # 是否禁用已存在的日志器
    'disable_existing_loggers': False,
    'formatters': {
        'verbose': {
            'format': '{levelname} {asctime} {module} {lineno:d} {message}',
            'style': '{',
        },
        'simple': {
            'format': '{levelname} {module} {lineno:d} {message}',
            'style': '{',
        },
    },
    'filters': {
        'require_debug_true': {
            '()': 'django.utils.log.RequireDebugTrue',
        },
    },
    'handlers': {
        'console': {
            'level': 'DEBUG',
            'filters': ['require_debug_true'],
            'class': 'logging.StreamHandler',
            'formatter': 'simple'
        },
        'file': {
            'level': 'INFO',
            # 这个handler可以记录一组日志文件
            'class': 'logging.handlers.RotatingFileHandler',
            'filename': os.path.join(BASE_DIR, 'logs/django.log'),   # 需要创建文件
            # 单个日志文件最大字节数
            'maxBytes': 300*1024*1024,
            # 日志文件个数
            'backupCount': 10,
            'formatter': 'verbose'
        },
    },
    'loggers': {
        'django': {
            'handlers': ['console', 'file'],
            'level': 'INFO',  # 日志器接收的最低级别
            'propagate': True,
        },
    },
}
```

### 3、注册子应用

```python
INSTALLED_APPS = [
    'haystack', 
    'rest_framework',      # DRF框架
    'users.apps.UsersConfig',     # 子应用注册（或者直接users）
]
```

> 创建的子应用需要在此注册才能使用

### 4、配置文件

```python
1.BASE_DIR为当前工程的根目录
BASE_DIR = Path(__file__).resolve().parent.parent
# 意思是项目根目录为文件所在路径的父级目录的父级目录
Django会依此来定位工程内的相关文件，我们也可以使用该参数来构造文件路径
2. DEBUG
调试模式，创建工程后初始值为True，即默认工作在调试模式下。
作用：
修改代码文件，程序自动重启
Django程序出现异常时，向前端显示详细的错误追踪信息
而非调试模式下，仅返回Server Error (500)
注意：部署线上运行的Django不要运行在调式模式下，记得修改DEBUG=False
```

### 5、本地语言与时区

```python
Django支持本地化处理，即显示语言与时区支持本地化。

本地化是将显示的语言、时间等使用本地的习惯，这里的本地化就是进行中国化，中国大陆地区使用简

体中文，时区使用亚洲/上海时区，注意这里不使用北京时区表示。
初始化的工程默认语言和时区为英语和UTC标准时区
将语言和时区修改为中国大陆信息
LANGUAGE_CODE = 'zh-hans'

TIME_ZONE = 'Asia/Shanghai'

USE_I18N = True

USE_L10N = True

# USE_TZ = True
```

### 6、静态文件

```python
项目中的CSS、图片、js都是静态文件。一般会将静态文件放到一个单独的目录中，以方便管理。在
html页面中调用时，也需要指定静态文件的路径，Django中提供了一种解析的方式配置静态文件路
径。静态文件可以放在项目根目录下，也可以放在应用的目录下，由于有些静态文件在项目中是通用
的，所以推荐放在项目的根目录下，方便管理。
为了提供静态文件，需要配置两个参数：
STATICFILES_DIRS 存放查找静态文件的目录
STATIC_URL 访问静态文件的URL前缀

在setting.py中修改静态文件的两个参数为
STATIC_URL = '/static/'
STATICFILES_DIRS = [
    BASE_DIR / 'static'
]

MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

例如，我们向static目录中添加一个index.html文件，在浏览器中就可以使用
127.0.0.1:8000/static/index.html来访问。

注意
Django 仅在调试模式下（DEBUG=True）能对外提供静态文件。
当DEBUG=False工作在生产模式时，Django不再对外提供静态文件，需要是用collectstatic命令来收集
```

### 7、中间件

```python
MIDDLEWARE = [
    '子路由.middlewarse.自定义中间件名'           # 添加自定义中间件
]
```

> 执行顺序 先调用最下面的中间件，再先从上往下执行，再从下往上

自定义中间件：

```python
1、在子应用文件下新建（自定义）.py文件（middlewarse.py）
2、自定义中间件
3、setting中添加中间件

# 自定义中间件（闭包）
def outer1(fuc):
    print('调用前1')
    def inner(*args,**kwargs):
        print('执行前1')
        data = fuc(*args,**kwargs)
        print('执行后1')
        return data
    return inner
    
# 在setting.py中的MIDDLEWARE[] 中添加中间件的路径:'子路由名.middlewarse.自定义中间件名'
```

### 8、其它配置

子应用查找路径

```python
# 将多个子应用放在一个apps文件夹中
import os,sys

sys.path.insert(0,BASE_DIR)
sys.path.insert(1,os.path.join(BASE_DIR,'apps'))
```

> 将子应用全部存储进一个 文件夹（这里用的是apps） 所用

```python
'# 自定义用户模型（使用了自带的用户验证模型类所用），子路由名+创建的模型类名
AUTH_USER_MODEL = 'user.User'

# 在项目根目录的__init__.py文件夹中连接mysql数据库
import pymysql

# 连接mysql数据库
pymysql.install_as_MySQLdb()
```

> 第一行是使用了用户验证模型类用
>
> 
>
> 后两个在项目根目录的__init__.py文件夹中 用

# 四、url

### 1、总路由

```python
from django.contrib import admin
from django.urls import path,include
from django.conf import settings
from django.conf.urls.static import static

# include：指定子路由的urls文件 
# namespace命名空间的作用：避免不同应用中的路由使用了相同的名字发生冲突，使用命名空间区别开
urlpatterns = [
    path('user/',include('user.urls'),namespace='books'),   # 指定子路由
	# 在主路由中添加MEDIA_ROOT的访问
    re_path(r'^media(?P<path>.*)$', serve, {'document_root': settings.MEDIA_ROOT}),
    re_path(r'^static(?P<path>.*)$', serve, {'document_root': settings.STATIC_ROOT}),
    
] 
```

resverse反解析：

```python
from django.urls import reverse # 注意导包路径 
使用reverse函数，可以根据路由名称，返回具体的路径，如：

def say(request): 
	url = reverse('books:index') # 返回 /books/index/
	print(url) 
	return HttpResponse('say')

# 对于未指明namespace的，reverse(路由name)
# 对于指明namespace的，reverse(命名空间namespace:路由name)
```

### 2、子路由

**需要在子路由中新建url.py**

```python
from django.urls import re_path,path
from . import views

# 给总路由起别名，方便调用
app_name = 'books'
urlpatterns = [
    # JWT 路由，不用写视图
	path('login/', obtain_jwt_token),
    # ^ -->开头  $ -->结尾  [a-z] -->字母从a到z  + -->前一个字符  {8} -->出现8次
    re_path(r'^weather/(?P<city>[a-z]+)/(?P<year>\d{8})/$', views.weather),
    # 指定视图类
    path('register/', views.RegisterView.as_view(), name='register'),
    # 三级视图升级版：需要在sa_view（{请求方法：方法名}）
    re_path(r'^husbandset/$', views.HusbandModelViewSet.as_view({'get': 'latesd'})),
    
    path('users/<int:user_id>/',views.UserUpdateView.as_view(),name='user_update')
]
```

> re_path：主要接收正则表达式参数  所用 
>
> path：普通路由，也可以指定接收参数类型

# 五、View视图

### 1、请求与响应

**响应类的使用**

```python
# 导入响应包
from django.http import HttpResponse,JsonResponse

# 返回http响应
# content -> 内容  type -> 内容类型   status -> 返回的状态码  charset -> 编码格式
return HttpResponse(content='文件传输成功',content_type='text/html', status=404)
# 返回json响应，字典会自动转换为json类型,safe：允许非字典形式传递
return JsonResponse(data=data,safe=False)
```

> HttpResponse：返回普通响应
>
> JsonResponse：返回 json数据 响应
>
> 还有许多参数可以选择

**接收get、post数据**

```python
def func(request):
    # GET请求
    a = request.GET.getlist('a')   # getlist -->获取多个参数列表
    b = request.GET.get('b')     # 获取单个参数
    # POST请求（表单数据）
    name = request.POST.get('name')
```

**接收json字符串数据**

```python
# JSON类型
# 方法一
json_bytes = request.body


# 方法二
from django.http import QueryDict

QueryDict(request.body)
# <QueryDict: {'csrfmiddlewaretoken': ['4l7nKLNzZan2xGTXU6nfkGJ3ssHZSrAEefjjPUWDBezP3Er6g5v0uC2jNcVvypJm'], 'username': ['ggggg'], 'mobile': ['13434343434'], 'is_active': ['on']}>
```

> 方法一：接收的是json字符串类型的数据，需要解码转换为python字典
>
> 
>
> 方法二：使用QueryDict类，将传过来的body数据，转换为QueryDict类型

**接收file字节数据**

```python
# File类型（获取列表）
file_data = request.FILES.getlist('tu')

# 方法一：
with open(images,'wb')as f:   # 写入文件
    for i in file_data[0].chunks():
        f.write(i)
        # 方法二：read获取字节数据，直接写入
        file_data.read()
```

> 后面方法：用于将接收的二进制数据写入文件

### 2、cookie和sission

**cookie：**

- 设置cookie：

  ```python
  # 设置cookie
  response = HttpResponse('PYTHON')
  # 设置cookie（可多次设置,键值对形式） 设置过期时间：expires ->时间字符 max_age ->秒
  response.set_cookie('键名','值',expires=60)
  
  # 将cookie进行加盐（加密）
  response.set_signed_cookie('weight', '50', salt='aslkxmciowklashdkah')
  ```

  > 步骤：实例响应数据  >  设置cookie（可以使用 加盐-加密的方法）
  >
  > 
  >
  > cookie介绍：保存在客户端，每次您拨打相应网站，浏览器都会查找该网站的 cookies

  设置cookie常用参数

  ```python
  key ： cookie的键                 value ： cookie的值
  max_age ： 有效期（秒）           expires ： 有效期(int整型， datetime对象)
  path ： cookie所在路径，'/'（根路径）。
  domain ：域名下的所有路径都能访问cookie。
  secure ：值为布尔类型，当为True 时，只允许https 对cookie进行访问。
  httponly ：值为布尔类型，当为True 时，不允许前端js 获取cookie。
  samesite ：lax或strict时，表示不允许携带cookie进行跨源请求。
  ```

- 获取cookie：

  ```python
  # 通过键名获取cookie
  cookies = request.COOKIES.get('键名')
  ```

**sission：**

- 设置

  ```python
  # 字典类型
  request.session['键名'] = 值
  # 设置session的有效期（默认2 周，None表示默认），0表示会话，传整数表示以秒为单位
  request.session.set_expiry(None)
  ```

  > session介绍：存储于服务器中

- 获取

  ```python
  username = request.session.get('键名')
  ```

- 清除

  ```python
  # 删除session
  del request.session['yaowei']     # 只删除值
  request.session.clear()       # 只清空所有session 数据的值
  request.session.flush()       # 将存储的session整个删除
  ```

  > 三种删除方式
  >

**区别：**

```python
cookie和session的区别?
Cookie的根本作用就是在客户端存储用户访问网站的一些信息 ，Session的根本作用就是在服务端存储用户和服务器会话的一些信息 
cookie 缺陷：增加了流量 安全性成问题 HTTP请求中的Cookie是明文传递的 大小限制在4KB左右
cookie 是保存在客户端，session 的是保存在服务端
cookie 不安全（对敏感数据，需要加密） 
session因为有session_id的存在往往要借助cookie来实现，但是非必要，只能说是一种通用性交换的实现方案
如果cookie删除了，或者是浏览器禁用了，那么session也会失效，
session, 可以放在文件中，数据库中，或者内存，都可以，一般用户验证这种场合都会用到session
```

### 3、连接redis

```python
# 导入模块，用于连接数据库
from django_redis import get_redis_connection

# 创建连接对象，参数表示使用默认的数据库
conn = get_redis_connection('default')
# 建立管道对象
pl = redis_conn.pipeline()
# 添加数据
pl.set('age',100)
# 管道执行redis命令
pl.execute()
```

### 4、指定模板

```python
from django.shortcuts import render
# 模板函数
def tem_html(request):
    # render -> 指定模板文件的，参数1 ：request ， 2：html文件，3：内容（字典），可以不添
    return render(request,'example2.html',context=data)
```

> 如果要返回数据，就要写context

### 5、重定向

```python
from django.shortcuts import redirect,reverse
from  django.http import HttpResponseRedirect

def name_redirect(request):
    # 方法一： 重定向路由名
    # return HttpResponseRedirect('/get_mysql')
    # 方法二： 子应用名 + 需跳转的路由名
    return redirect(reverse('books:tem_html'))
```

> 重定向的两种方法

### 6、类视图

**装饰器：**

```python
from django.view import View
# 类视图装饰器
from django.utils.decorators import method_decorator

定义闭包函数func
# @method_decorator(装饰器名,name='方法名')
# 装饰类时，需要写name（dispatch：所有方法，或者写需要装饰方法名：get、post、put等请求方法）
# 装饰函数时，放在函数上面，不需要传name的值
@method_decorator(func,name='get')
class View_(View):
    # 不能修改方法名
	def get(self,request):
        return HttpResponse('get方法！')
    def post(self,request):
        return HttpResponse('post方法！')		
```

> View：最底层的视图类

**密码加密：**

```python
# 对密码进行加密
from django.contrib.auth.hashers import make_password

# 对密码进行加密  make_password(原密码，固定字符串，前缀)
password = make_password(password, password, 'pbkdf2_sha256')
```

> 对密码进行加密

**用户登录状态保持：**

```python
# 登录、退出登录
from django.contrib.auth import login,logout

# check_password：验证密码
user.check_password(password)
# 实现的状态保持
login(request, user)
# 退出
logout(request)
```

> 保持登陆状态，即下次登录不用再重复输入账号、密码
>

# 六、模型类

### 1、模型类介绍

**字段类型**

| 类型             | 说明                                                         |
| :--------------- | :----------------------------------------------------------- |
| AutoField        | 自动增长的IntegerField，通常不用指定，不指定时Django会自动创建属性名为id的自动增长属性 |
| BooleanField     | 布尔字段，值为True或False                                    |
| NullBooleanField | 支持Null、True、False三种值                                  |
| CharField        | 字符串，参数max_length表示最大字符个数，必须设置             |
| TextField        | 大文本字段，一般超过4000个字符时使用                         |
| IntegerField     | 整数                                                         |
| DecimalField     | 十进制浮点数， 参数max_digits表示总位数， 参数decimal_places表示小数位数 |
| FloatField       | 浮点数                                                       |
| DateField        | 日期， 参数auto_now表示每次保存对象时，自动设置该字段为当前时间，用于"最后一次修改"的时间戳，它总是使用当前日期，默认为False； 参数auto_now_add表示当对象第一次被创建时自动设置当前时间，用于创建的时间戳，它总是使用当前日期，默认为False; 参数auto_now_add和auto_now是相互排斥的，组合将会发生错误 |
| TimeField        | 时间，参数同DateField                                        |
| DateTimeField    | 日期时间，参数同DateField                                    |
| FileField        | 上传文件字段                                                 |
| ImageField       | 继承于FileField，对上传的内容进行校验，确保是有效的图片      |

**可选参数** 

| 选项         | 说明                                                         |
| :----------- | :----------------------------------------------------------- |
| null         | 如果为True，表示允许为空，默认值是False                      |
| blank        | 如果为True，则该字段允许为空白，默认值是False                |
| db_column    | 字段的名称，如果未指定，则使用属性的名称                     |
| db_index     | 若值为True, 则在表中会为此字段创建索引，默认值是False        |
| default      | 默认                                                         |
| primary_key  | 若为True，则该字段会成为模型的主键字段，默认值是False，一般作为AutoField的选项使用 |
| unique       | 如果为True, 这个字段在表中必须有唯一值，默认值是False        |
| verbose_name | 主要是admin后台显示                                          |

**null是数据库范畴的概念，blank是表单验证范畴的**

**外键**

在设置外键时，需要通过**on_delete**选项指明主表删除数据时，对于外键引用表数据如何处理，在django.db.models中包含了可选常量：

- **CASCADE**级联，删除主表数据时连通一起删除外键表中数据
- **PROTECT**保护，通过抛出**ProtectedError**异常，来阻止删除主表中被外键应用的数据
- **SET_NULL**设置为NULL，仅在该字段null=True允许为null时可用
- **SET_DEFAULT**设置为默认值，仅在该字段设置了默认值时可用
- **SET()**设置为特定值或者调用特定方法
- **DO_NOTHING**不做任何操作，如果数据库前置指明级联性，此选项会抛出**IntegrityError**异常

### 2、定义模型类

**语法：属性=models.字段类型(选项)**

```python
from django.db import models

class BookInfo(models.Model):

    name = models.CharField(max_length=10)
    # 发布日期
    pub_date = models.DateField(verbose_name='发布日期', null=True)
    # 阅读量
    readcount = models.IntegerField(default=0, verbose_name='阅读量')
    # 评论量
    commentcount = models.IntegerField(default=0, verbose_name='评论量')
    # 逻辑删除
    is_delete = models.BooleanField(default=False, verbose_name='逻辑删除')

    class Meta:
        # 用这个参数，迁移的时候不会生成表
        abstract = True
        # 排序  查询数据集的时候会按照这个字段进行排序
        ordering = ['-update_time', '-id']   
        db_table = 'bookinfo'  # 指明数据库表名
        verbose_name = '图书'  # 在admin站点中显示的名称
        verbose_name_plural = verbose_name  # 显示的复数名称

    def __str__(self):
        """定义每个数据对象的显示信息"""
        return self.name


# 准备人物列表信息的模型类
class PeopleInfo(models.Model):
    GENDER_CHOICES = (
        (0, 'male'),
        (1, 'female')
    )
    name = models.CharField(max_length=20, verbose_name='名称')
    gender = models.SmallIntegerField(choices=GENDER_CHOICES, default=0, verbose_name='性别')
    description = models.CharField(max_length=200, null=True, verbose_name='描述信息')
    is_delete = models.BooleanField(default=False, verbose_name='逻辑删除')
    book = models.ForeignKey(BookInfo, on_delete=models.CASCADE, verbose_name='图书')  # 外键
    

# 富文本编辑器
from ckeditor_uploader.fields import RichTextUploadingField
from utils.validators import MediaUrlValidator

class News(BaseModel):
    """
    文章模型
    """
    # 标题
    title = models.CharField('标题',max_length=150,help_text='标题')
    # 摘要
    digest = models.CharField('摘要',max_length=200,help_text='摘要')

    content = RichTextUploadingField('内容',help_text='内容')

    # 点击量
    clicks = models.IntegerField('点击量',default=0,help_text='点击量')
    # url 图片的URL
    # image_url = models.URLField('图片url',default='',help_text='图片url')
```

> 基本步骤：写字段  >  str方法返回显示的字段  >  Meta类（指定表名、数据库名、排序等等方法）  

### 3、数据库操作

```python
# 默认objects，可通过如下改变
值 = Manager（）
```

**增：create**

**删：delete**

**改：update**

```python
BookInfo.objects.create(name = 'hello',pub_date = '2021-1-5')     # 增加
BookInfo.objects.filter(name="java").delete()     # 删除
BookInfo.objects.filter(name="hello").update(name="world")    # 修改
```

##### 1、基本查询

**get **查询单一对象，如果不存在会抛出**模型类.DoesNotExist**异常。

**all **查询多个结果。返回列表

**count **查询结果数量。

```python
>>> BookInfo.objects.get(id=1)
<BookInfo: 射雕英雄传>
>>> BookInfo.objects.all()
<QuerySet [<BookInfo: 射雕英雄传>, <BookInfo: 天龙八部>, <BookInfo: 笑傲江湖>, <BookInfo: 雪山飞狐>]>
>>> BookInfo.objects.count()
4
```

##### 2、过滤查询

实现SQL中的where功能，包括

- **filter**过滤出多个结果
- **exclude**排除掉符合条件剩下的结果
- **get**过滤单一结果

对于过滤条件的使用，上述三个方法相同，故仅以**filter**进行讲解。

**语法：属性名称__比较运算符=值 （两个下划线）**

| exact                                            | 判等                                                   |
| ------------------------------------------------ | ------------------------------------------------------ |
| contains                                         | 模糊查询，是否包含                                     |
| startswith                                       | 以...开头                                              |
| endswith                                         | 以...结尾                                              |
| isnull                                           | 空查询，是否为null                                     |
| in                                               | 范围查询，是否在范围内                                 |
| gt                                               | 比较查询，>                                            |
| gte                                              | >=                                                     |
| lt                                               | <                                                      |
| lte                                              | <=                                                     |
| year、month、day、week_day、hour、minute、second | 对日期时间类型的属性进行运算，日期格式必须是YYYY-MM-DD |

```python
BookInfo.objects.get(id__exact=1)      # 判等
BookInfo.objects.filter(name__contains='传')    # 是否包含传
BookInfo.objects.filter(name__startswith='天')   # 以天开头
BookInfo.objects.filter(name__isnull=True)   # 是否为null
BookInfo.objects.filter(id__in=[1,3，5])     # 是否在范围
BookInfo.objects.filter(id__gt=3)            # 是否大于
BookInfo.objects.filter(pub_date__gt='1990-1-1')    # 时间类型
```

##### 3、F和Q对象

```python
# 导包
from django.db.models import F,Q
```

**F对象作用：两个属性比较**

**语法：filter(字段名__运算符=F('字段名'))**

```python
# 查询阅读量大于等于评论量的图书。
BookInfo.objects.filter(readcount__gt=F('commentcount'))
<QuerySet [<BookInfo: 雪山飞狐>]>
```

**Q对象作用：如果需要实现逻辑或or的查询**

**语法：Q(属性名__运算符=值)**

Q对象运算符：&（and）、|（or）、~（not）

```python
# 查询阅读量大于20，或编号小于3的图书
BookInfo.objects.filter(Q(readcount__gt=20)|Q(id__lt=3))
<QuerySet [<BookInfo: 射雕英雄传>, <BookInfo: 天龙八部>, <BookInfo: 雪山飞狐>]>
# 例：查询编号不等于3的图书。
BookInfo.objects.filter(~Q(id=3))
<QuerySet [<BookInfo: 射雕英雄传>, <BookInfo: 天龙八部>, <BookInfo: 雪山飞狐>]>
```

##### 4、聚合函数和排序函数

**聚合函数语法：aggregate(聚合函数('字段名'))**

```python
from django.db.models import Sum,Avg,Max,Min,Count        # 导入聚合函数
# 查询图书的总阅读量。
BookInfo.objects.aggregate(Sum('readcount'))
# 返回的是字典形式： {'属性名__聚合类小写':值}
{'readcount__sum': 126}
```

**排序语法：order_by('字段名')，默认升序，降序在字段名前加-**

```python
# 降序
BookInfo.objects.all().order_by('-readcount')
```

##### 5、关联查询

**一对多语法：主表模型(实例对象).关联模型类名小写_set.查询方法**

```python
>>> from book.models import PeopleInfo
>>> book = BookInfo.objects.get(id=1) #1.查询书籍
>>> book.peopleinfo_set.all() #2.根据书籍关联人物信息
<QuerySet [<PeopleInfo: 郭靖>, <PeopleInfo: 黄蓉>, <PeopleInfo: 黄药师>, <PeopleInfo: 欧阳锋>, <PeopleInfo: 梅超风>]>
```

**多对一语法:从表模型(实例对象).外键**

```python
# 通过人物查询书籍信息( 已知 从表数据,关联查询主表数据)
# 查询人物为1的书籍信息
person = PeopleInfo.objects.get(id=1) 
person.book  
<BookInfo: 射雕英雄传>
```

多查询一数据语法：从表模型(实例对象).关联类属性_字段名

```python
# 通过人物查询书籍信息( 已知 从表数据,关联查询主表数据)
# 查询人物为1的书籍id
person = PeopleInfo.objects.get(id=1) 
person.book_id 
```

##### 6、关联过滤查询

**由多模型类条件查询一模型类数据**:

**语法：filter(关联模型类名小写___属性名条件__运算符=值)**

```python
# 查询图书，要求图书中人物的描述包含"八"
book = BookInfo.objects.filter(peopleinfo__description__contains='八')
<QuerySet [<BookInfo: 射雕英雄传>, <BookInfo: 天龙八部>]>
```

**由一模型类条件查询多模型类数据**:	

**语法：filter(一模型类关联属性名（外键）一模型类属性名条件运算符=值)**

```python
# 查询图书阅读量大于30的所有人物
people = PeopleInfo.objects.filter(book__readcount__gt=30)
<QuerySet [<PeopleInfo: 乔峰>, <PeopleInfo: 段誉>, <PeopleInfo: 虚竹>, <PeopleInfo: 王语嫣>, <PeopleInfo: 胡斐>, <PeopleIn
```

##### 7、查询集QuerySet

**查询集QuerySet概念：表示从数据库中获取的对象集合。**

当调用如下过滤器方法时，Django会返回查询集（而不是简单的列表）：

- all()：返回所有数据。
- filter()：返回满足条件的数据。
- exclude()：返回满足条件之外的数据。
- order_by()：对结果进行排序。

**判断某一个查询集中是否有数据**：

- exists()：判断查询集中是否有数据，如果有则返回True，没有则返回False。

##### 8、分页器

```python
# 导入分页类
from django.core.paginator import Paginator
# 查询数据
books = BookInfo.objects.all()
# 创建分页实例，参数一：查询集对象，参数二：每页的数量
paginator=Paginator(books,2)
# 获取指定页码的数据
page_skus = paginator.page(1)
# num_pages：获取总页数
total_page=paginator.num_pages
# 获取页面数据，get_page可以容错
news_info = paginator.get_page(page)
```

```python
class NewsListView(View):
    """
    新闻列表视图
    步骤：
    1、接收请求参数tag_id、page
    2、构造返回的数据
    3、使用分页器进行分页
    4、返回数据
    """
    def get(self,request):
        try:
            tag_id = int(request.GET.get('tag',0))
        except Exception as e:
            logger.error('标签错误\n{}'.format(e))
            tag_id = 0
        try:
            page = int(request.GET.get('page',0))
        except Exception as e:
            logger.error('页码错误\n{}'.format(e))
            page = 1

        # values() 接收可选的位置参数*fields，它指定SELECT 应该限制哪些字段。如果指定字段，每个字典将只包含指定的字段的键/值。如果没有指定字段，每个字典将包含数据库表中所有字段的键和值。
        # annotate 方法在底层调用了数据库的数据聚合函数
        news_queryset = News.objects.values('id','title','digest','image_url','update_time').annotate(tag_name=F('tag__name'),a=F('author__username'))
      
        # 过滤
        news = news_queryset.filter(is_delete=False,tag_id=tag_id) or news_queryset.filter(is_delete=False)
  
        # 调用分页器进行分页
        paginator = Paginator(news,constants.PER_PAGE_NEWS_COUNT)
        # 获取页面数据，get_page可以容错
        news_info = paginator.get_page(page)

        data = {
            # num_pages：总页数
            'total_pages':paginator.num_pages,
            'news':list(news_info)
        }
        return json_response(data=data)
```



##### 9、其它查询方法

**only:**

```python
# 指定获取当前这个列的属性  （字段），一般和filter一起使用
tags = Tag.objects.only('id','name').filter(is_delete=False)
```

**values(*fields)：**

values() 接收可选的位置参数*fields，它指定SELECT 应该限制哪些字段。如果指定字段，每个字典将只包含指定的字段的键/值。如果没有指定字段，每个字典将包含数据库表中所有字段的键和值。

```
>>> Blog.objects.values()
[{'id': 1, 'name': 'Beatles Blog', 'tagline': 'All the latest Beatles news.'}],
>>> Blog.objects.values('id', 'name')
[{'id': 1, 'name': 'Beatles Blog'}]
```

annotate语法：

annotate(随机名=F(关联外键名 +'双下划线' + 关联表的字段名))

```python
# annotate 方法在底层调用了数据库的数据聚合函数
news_queryset = News.objects.values('id','title','digest','image_url','update_time').annotate(tag_name=F('tag__name'),author=F('author__username'))
```

**select_related：**

语法：select_related(模型类外键).only(外键 + 双下划线 + 字段名)

```python
hot_news = HotNews.objects.select_related('news').only('news__title', 'news__image_url', 'news_id').filter(
        is_delete=False)
```

# 七、序列化器

```python
序列化：将程序中的一个数据结构类型转换为其他格式（字典、JSON、XML等），例如将Django中的模型类对象装换为JSON字符串
反序列化：：反之，将其他格式（字典、JSON、XML等）转换为程序中的数据，例如将JSON字符串转换为Django中的模型类对象。
```

### 1、DRF

```
安装：pip install djangorestframework

from rest_framework import serializers       # 导入序列化器
```

> 概念：将数据库的东西通过ORM的映射取出来，通过view和serializers文件绑定REST接口，当前端请求时，返回序列化好的json。
>
> 
>
> 注:需要在注册子应用处注册

### 2、创建序列化类

```python
from rest_framework import serializers       
from apps.books.models import BookInfo, PeopleInfo

方法一：继承ModelSerializer
class BookInfoModelSerializer(serializers.ModelSerializer):
    """
        图书的数据序列器（ModelSerializer)  专门做模型类的序列化
    """
    class Meta:
        # 指定使用的序列化的模型类
        model = BookInfo
        # 指定序列化 模型类中的属性（字段）
        # __all__：表示所有
        fields = "__all__"
        # 写入想要展示出来的字段
        # fields = ('name','readcount','commentcount')

        
方法二
class BookInfoSerializer(serializers.Serializer):
    """
    写一个为模型类提供一个序列化器   Serializer
    """
    id = serializers.IntegerField(label='ID',read_only=True)
    user = serializers.HiddenField(default=serializers.CurrentUserDefault())
    # 关联字段   PrimaryKeyRelatedField：返回序列化关联对象的id
    # book = serializers.PrimaryKeyRelatedField(label='图书',read_only=True)
    #  StringRelatedField：返回的是一个字符串的表达形式
    book = serializers.StringRelatedField(label='图书',read_only=True)
```

> 方法一：直接指定模型类
>
> 方法二：以序列化器方法重新写字段

| **字段**                | **说明**                                                     |
| ----------------------- | ------------------------------------------------------------ |
| **BooleanField**        | BooleanField()                                               |
| **CharField**           | CharField(max_length=None, min_length=None, allow_blank=False,<br/>trim_whitespace=True) |
| **EmailField**          | EmailField(max_length=None, min_length=None,<br/>allow_blank=False) |
| **RegexField**          | RegexField(regex, max_length=None, min_length=None,<br/>allow_blank=False) |
| **SlugField**           | SlugField(max*length=50, min_length=None, allow_blank=False) 正则字段，验证正则模式 [a-zA-Z0-9*-]+ |
| **URLField**            | URLField(max_length=200, min_length=None, allow_blank=False) |
| **UUIDField**           | UUIDField(format='hex_verbose')  format:  1) `'hex_verbose'` 如`"5ce0e9a5-5ffa-654b-cee0-1238041fb31a"`  2） `'hex'` 如 `"5ce0e9a55ffa654bcee01238041fb31a"`  3）`'int'` - 如: `"123456789012312313134124512351145145114"`  4）`'urn'` 如: `"urn:uuid:5ce0e9a5-5ffa-654b-cee0-1238041fb31a"` |
| **IPAddressField**      | IPAddressField(protocol='both', unpack_ipv4=False, **options) |
| **IntegerField**        | IntegerField(max_value=None, min_value=None)                 |
| **FloatField**          | FloatField(max_value=None, min_value=None)                   |
| **DecimalField**        | DecimalField(max_digits, decimal_places, coerce_to_string=None, max_value=None, min_value=None) max_digits: 最多位数 decimal_palces: 小数点位置 |
| **DateTimeField**       | DateTimeField(format=api_settings.DATETIME_FORMAT, input_formats=None) |
| **DateField**           | DateField(format=api_settings.DATE_FORMAT, input_formats=None) |
| **TimeField**           | TimeField(format=api_settings.TIME_FORMAT, input_formats=None) |
| **DurationField**       | DurationField()                                              |
| **ChoiceField**         | ChoiceField(choices) choices与Django的用法相同               |
| **MultipleChoiceField** | MultipleChoiceField(choices)                                 |
| **FileField**           | FileField(max_length=None, allow_empty_file=False, use_url=UPLOADED_FILES_USE_URL) |
| **ImageField**          | ImageField(max_length=None, allow_empty_file=False, use_url=UPLOADED_FILES_USE_URL) |
| **ListField**           | ListField(child=, min_length=None, max_length=None)          |
| **DictField**           | DictField(child=)                                            |

**选项参数：**

| **参数名称**        | **作用**                                 |
| ------------------- | ---------------------------------------- |
| **max_length**      | 最大长度                                 |
| **min_lenght**      | 最小长度                                 |
| **allow_blank**     | 是否允许为空                             |
| **trim_whitespace** | 是否截断空白字符                         |
| **max_value**       | 最小值                                   |
| **min_value**       | 最大值                                   |
| **style**           | 用于控制渲染器如何渲染字段的键值对的字典 |

> ```
> style：
> # 使用 <input type="password"> 作为输入。
> password = serializers.CharField(
>     style={'input_type': 'password'}
> )
> 
> # 使用单选输入而不是选择输入。
> color_channel = serializers.ChoiceField(
>     choices=['red', 'green', 'blue'],
>     style={'base_template': 'radio.html'}
> )
> ```
>

**通用参数：**

| **参数名称**       | **说明**                              |
| ------------------ | ------------------------------------- |
| **read_only**      | 仅用于序列化输出，默认False（只读）   |
| **write_only**     | 仅用于反序列化输入，默认False（只写） |
| **required**       | 反序列化时必填，默认True              |
| **default**        | 默认值                                |
| **allow_null**     | 允许为空，默认False                   |
| **validators**     | 使用的验证器                          |
| **error_messages** | 返回错误信息（字典）                  |
| **label**          | 显示字段名称                          |
| **help_text**      | 字段帮助提示信息                      |

### 3、校验

**1、validate_<field_name>验证**

对 <field_name> 字段进行验证，在序列化器中对单个字段追加额外的校验逻辑。

```python
# 英雄信息序列化器的定义
class hero_info_serializer(serializers.Serializer):
    # 设置英雄的序列化字段信息
   ......
    
    # 在序列化器中增加validate_<field_name>验证，用于验证英雄名称是否为 ‘无极剑圣’
    def validate_hero_name(self, value):  # value是接收从前端传过来的数据
        if '无极剑圣' not in value:
            raise serializers.ValidationError("这个英雄不是无极剑圣")
        return value
    
    class Meta:
        model = hero_info
        fields = '__all__'
```

**2、validate验证**

```python
# 英雄信息序列化器的定义
class hero_info_serializer(serializers.Serializer):
    # 设置英雄的序列化字段信息
    
    # 将英雄信息中的【使用次数】和【胜利次数】字段设置为必填
   ......
    
    # 记得注释掉validate_<field_name>验证，否则会导致validate验证不生效。
     
    # 在序列化器中新增 validate 验证，用于判断使用次数等于胜利次数的英雄
    def validate(self, attrs):
        usage_count = attrs['usage_count']
        victory_count = attrs['victory_count']
        if usage_count == victory_count:
            raise serializers.ValidationError('该英雄没有100%胜率')
        return attrs
    
    class Meta:
        model = hero_info
        fields = '__all__'
```

> attrs：字典数据

**3、validators验证**

在字段中添加validators选项参数，也可以补充验证行为。

```python
# 定义一个验证英雄价格的函数
def price_compare(value):
    if value < 6300:
        raise serializers.ValidationError('这个英雄身价太低')
    return value    
    
# 英雄信息序列化器的定义
class hero_info_serializers(serializers.ModelSerializer):
    # 设置英雄的序列化字段信息
    hero_name = serializers.CharField(label='英雄名称', max_length=10)  # 字符型字段
    
    # 在英雄价格字段的字段参数上添加 validators选线，用于验证该英雄的价格
    hero_price = serializers.FloatField(label='英雄价格', validators=
[price_compare])  
   ...
    
    # 记得注释掉其他验证，否则可能导致抛出的异常不对。
    class Meta:
        model = hero_info
        fields = '__all__'
```

### 4、序列化使用

```python
from book.models import BookInfo     # 导入模型类
from book.serializer import BookInfoSerializer       # 导入序列化类

book_qs = BookInfo.objects.all()
serializer = BookInfoSerializer(book_qs, many=True)
serializer.data
```

> 步骤：
>
> 1、创建模型类对象
> 2、实例序列化类，传入模型类对象
> 3、输出序列化后的数据，通过data属性可以获取序列化后的数据
> 4、如果是查询集QuerySet，添加many=True

### 5、反序列化的使用

```python
from booktest.serializers import BookInfoSerializer

data = {'pub_date':'2021-1-20'}
serializer = BookInfoSerializer(data=data)  # 序列化对象
# 校验出错后，会自动抛出错误信息
serializer.is_valid(raise_exception=True)
serializer.validated_data  # {}  获取反序列化校验后的数据还是字典

serializer.errors   # 验证失败后，通过errors来输出验证失败的错误提示
```

> 步骤：
>
> 1、构造数据
> 2、将要反序列化的数据传递给data构造参数
> 3、进行校验,is_valid方法
>
> 
>
> 注意：serializer = BookInfoSerializer(模型类对象，data=data)
>
> 传入两个参数，则为修改

### 6、数据的保存

在验证成功后，想基于validated_data完成数据对象的创建，可以通过重写create()和update()两个方法
来实现。

```python
# 英雄信息序列化器的定义
class hero_info_serializers(serializers.ModelSerializer):
    # 设置英雄的序列化字段信息 
   ...
    # 数据验证
   ...
    
    # 数据保存 - 当数据库中不存在新增的英雄数据
    def create(self, validated_data):
        hero = hero_info(**validated_data)  # 创建新增的英雄数据
        hero.save()     # 将创建的英雄数据保存到数据库中
        return hero 
    
    # 数据保存 - 当数据库中存在该数据
    def update(self, instance, validated_data):
        # 对验证通过后传递过来的字段进行逐个替换,没有传递的字段就使用原来的字段信息。
        instance.hero_name = validated_data.get('hero_name', instance.hero_name)
        instance.hero_price = validated_data.get('hero_price', 
instance.hero_price)
        instance.usage_count = validated_data.get('usage_count', 
instance.usage_count)
        instance.victory_count = validated_data.get('victory_count', 
instance.victory_count)
        instance.week_free = validated_data.get('week_free', instance.week_free)
        instance.release_date = validated_data.get('release_date', 
instance.release_date)
        instance.hero_image = validated_data.get('hero_image', 
instance.hero_image)
        instance.save()     # 将更新后的英雄信息保存到数据库中
        return instance
    
    class Meta:
        model = hero_info
        fields = '__all__'
```

# 八、DRF视图与路由

### 1、DRF的请求与响应

- 请求request

  常用属性

  1. data

     ```
     request.data 返回解析之后的请求体数据。
     类似于Django框架中的request.POST和 request.FILES属性，而且还提供如下特性：
     	1、包含了解析之后的文件和非文件数据
     	2、包含了对POST、PUT、PATCH请求方式解析后的数据
     利用了REST framework的parsers解析器，不仅支持表单类型数据，也支持JSON数据
     ```

  2. query_params

     ```python
     request.query_params 与Django框架的 request.GET 相同，只是更换了名称而已。
     ```

- 响应 Response

  ```python
  DRF框架提供了一个响应类Response，使用该类构造响应对象时，响应的具体数据内容会被 
  render 渲染成符合前端需求的类型。
  DRF框架提供了 Renderer 渲染器，用来根据请求头中的 Accept（接收数据类型声明）来自动转
  换响应数据到对应格式。如果前端请求中未进行Accept声明，则会采用默认方式处理响应数据，我们可
  以通过配置来修改默认响应格式。
  
  # 在settings文件中添加如下配置信息
  REST_FRAMEWORK = {
      'DEFAULT_RENDERER_CLASSES': (  # 默认响应渲染类
          'rest_framework.renderers.JSONRenderer',  # json渲染器
          'rest_framework.renderers.BrowsableAPIRenderer',  # 浏览API渲染器
     )
  }
  ```

  构造方式

  ```python
  from rest_framework.response import Response # 导入Response
  
  Response(data, status=None, template_name=None, headers=None, content_type=None)
  ```

  > 参数说明：
  >
  > data: 为响应准备的序列化处理后的数据；
  > status: 状态码，默认200；
  > template_name: 模板名称，如果使用 HTMLRenderer 时需指明；
  > headers: 用于存放响应头信息的字典；
  > content_type: 响应数据的 Content-Type ，通常无需传递，DRF会根据前端所需类型数据来设置
  > 该参数。
  >
  > 
  >
  > 常用属性：
  > 1、data ：传给response对象的序列化后，但尚未render处理的数据。
  > 2、status_code ：状态码的数字。
  > 3、content ：经过render处理后的响应数据。

### 2、DRF的视图种类

##### **APIView**

```python
from rest_framework.views import APIView # 导入APIView类
```

> APIView是DRF框架所提供的所有视图的基类，也被称为一级视图，它继承自Django的 View 父类。

**APIView与 View 的差别点：**

1. 传入到视图方法中的是DRF框架的 Request 对象，而不是Django框架的 HttpRequeset 对象；
2. 视图方法可以返回DRF框架的 Response 对象，视图会为响应数据设置（render）符合前端要求的
   格式；
3. 任何 APIException 异常都会被捕获到，并且处理成合适的响应信息；
4. 在进行dispatch()分发前，会对请求进行身份认证、权限检查、流量控制。

```python
例如：使用APIView实现英雄的增删改查
# 路由代码
from django.urls import path, re_path
urlpatterns = [
    path('heros/', views.drf_view.as_view()),
    re_path(r'^heros/(?P<pk>\d+)/$', views.hero_info_APIView.as_view()),
]
# 视图代码
from rest_framework import status
from APP06.models import hero_info
from rest_framework.views import APIView
from rest_framework.response import Response
from APP08.serializers import hero_info_serializers
# 不带pk值的英雄类视图
class hero_list_APIView(APIView):
    # 查询所有英雄信息
    def get(self, request):
        # 1、查询所有数据
        queryset = hero_info.objects.all()
        # 2、将查询到的数据进行序列化操作
        serializer = hero_info_serializer(instance=queryset, many=True)
        # 3、视图响应
        return Response(serializer.data)
    
    # 新增一条英雄信息
    def post(self, request):
        # 1、获取前端传入的请求体数据
        data = request.data
        # 2、将请求体数据进行反序列化
        serializer = hero_info_serializer(data=data)
        # 3、验证反序列化的数据
        serializer.is_valid(raise_exception=True)
        # 4、保存英雄信息到数据库中
        serializer.save()
        # 5、视图响应
        return Response(serializer.data, status=status.HTTP_201_CREATED)
    
# 带pk值的英雄类视图
class hero_info_APIView(APIView):
    # 查询单个英雄信息
    def get(self, request, pk):
        try:
            # 1、查询英雄信息
            hero = hero_info.objects.get(id=pk)
        except hero_info.DoesNotExist:
            # 2、没查到返回404
            return Response(status.HTTP_404_NOT_FOUND)
        # 3、将查询到的英雄数据进行序列化
        serializer = hero_info_serializer(instance=hero)
        # 4、数据响应
        return Response(serializer.data)
    # 修改英雄信息
    def put(self,request, pk):
        try:
            # 1、查询英雄信息
            hero = hero_info.objects.get(id=pk)
        except hero_info.DoesNotExist:
            # 2、没查到返回404
            return Response(status.HTTP_404_NOT_FOUND)
        # 3、将前端请求体传递过来的数据进行反序列化
        serializer = hero_info_serializer(instance=hero, data=request.data)
        # 4、对反序列化的数据进行验证
        serializer.is_valid(raise_exception=True)
        # 5、修改英雄数据,调用的是update不是create。
        serializer.save()
        # 6、数据响应
        return Response(serializer.data)
    # 删除英雄信息
    def delete(self, request, pk):
        try:
            # 1、查询英雄信息
            hero = hero_info.objects.get(id=pk)
        except hero_info.DoesNotExist:
            # 2、没查到返回404
            return Response(status.HTTP_404_NOT_FOUND)
        # 3、删除英雄信息
        hero.delete()
        # 4、数据响应
        return Response(status=status.HTTP_204_NO_CONTENT)
```

##### GenericAPIView

```python
from rest_framework.generics import GenericAPIView # 导入 GenericAPIView 类
```

> GenericAPIView 继承自 APIVIew ，也被称为二级视图，它在 APIView 的基础上增加了对于列表视图
> 和详情视图可能用到的通用支持方法。

```python
# 路由代码
from django.urls import path, re_path
urlpatterns = [
    # APIView
    # path('heros/', views.hero_list_APIView.as_view()),
    # re_path(r'^heros/(?P<pk>\d+)/$', views.hero_info_APIView.as_view()),
    # GenericAPIView
    path('heros/', views.hero_list_GenericAPIView.as_view()),
    re_path(r'^heros/(?P<pk>\d+)/$', views.hero_info_GenericAPIView.as_view()),
]
# 视图代码
from rest_framework import status
from APP06.models import hero_info
from rest_framework.response import Response
from rest_framework.generics import GenericAPIView
from APP08.serializers import hero_info_serializer
# GenericAPIView类 不带参数
class hero_list_GenericAPIView(GenericAPIView):
    # 指定查询集和序列化器
    queryset = hero_info.objects.all()
    serializer_class = hero_info_serializer
    # 查询所有英雄信息
    def get(self, request):
        hero_list = self.get_queryset()
        serializer = self.get_serializer(hero_list, many=True)
        return Response(serializer.data)
    # 新增一条英雄信息
    def post(self, request):
        data = request.data
        serializer = self.get_serializer(data=data)
        serializer.is_valid(raise_exception=True)
        serializer.save()
        return Response(serializer.data)
    
# GenericAPIView类 带参数
class hero_info_GenericAPIView(GenericAPIView):
    # 指定查询集和序列化器
    queryset = hero_info.objects.all()
    serializer_class = hero_info_serializer
    # 查询单个英雄信息
    def get(self, request, pk):
        try:
            hero = self.get_object()
        except hero_info.DoesNotExist:
            return Response(status.HTTP_404_NOT_FOUND)
        serializer = self.get_serializer(instance=hero)
        return Response(serializer.data)
    # 修改英雄信息
    def put(self, request, pk):
        try:
            hero = self.get_object()
        except hero_info.DoesNotExist:
            return Response(status.HTTP_404_NOT_FOUND)
        serializer = self.get_serializer(instance=hero, data=request.data)
        serializer.is_valid(raise_exception=True)
        serializer.save()
        return Response(serializer.data)
    # 删除英雄信息
    def delete(self, request, pk):
        try:
            hero = self.get_object()
        except hero_info.DoesNotExist:
            return Response(status.HTTP_404_NOT_FOUND)
        hero.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```

> 总结：
> 使用GenericAPIView类和APIView类没有太大的区别，主要是可以将查询集数据全局指定，这样在每个
> 请求方法直接从全局查询集的缓存中获取数据。通过降低与数据库的交互频率，从而减轻服务器的压
> 力。

##### Mixin扩展类

```
Mixin扩展类主要是为了简化增删改查代码所扩展出来的，使用时需要与 GenericAPIView 配合使用。
```

1. ListModelMixin

   ```
   使用 from rest_framework.mixins
   列表视图扩展类，提供 list(request, *args, **kwargs) 方法，快速实现列表视图，返回200状态
   码。
   该Mixin的list方法会对数据进行过滤和分页。
   ```

   > 示例：
   >
   > ```python
   > from rest_framework.mixins import ListModelMixin
   > from rest_framework.generics import GenericAPIView
   > # Mixin扩展类 不带参数
   > class hero_list_MixinView(ListModelMixin, GenericAPIView):
   >     # 指定查询集和序列化器
   >     queryset = hero_info.objects.all()
   >     serializer_class = hero_info_serializer
   >     # 查询所有英雄信息
   >     def get(self, request):
   >         return self.list(request)
   > ```

2. CreateModelMixin

   ```
   创建视图扩展类，提供 create(request, *args, **kwargs) 方法快速实现创建资源视图，返回201
   状态码。
   如果序列化器对前端发送的数据验证失败，返回400错误。
   ```

   > ```python
   > from rest_framework.generics import GenericAPIView
   > from rest_framework.mixins import ListModelMixin, CreateModelMixin
   > # Mixin扩展类 不带参数
   > class hero_list_MixinView(ListModelMixin, CreateModelMixin, GenericAPIView):
   >     # 指定查询集和序列化器
   >     queryset = hero_info.objects.all()
   >     serializer_class = hero_info_serializer
   >     # 查询所有英雄信息
   >  ......
   >     
   >  # 创建一个英雄信息
   >     def post(self, request):
   >         return self.create(request)
   > ```

3. RetrieveModelMixin

   ```
   详情视图扩展类，提供 retrieve(request, *args, **kwargs) 方法快速实现返回一个存在的数据对
   象。
   如果存在，返回200， 否则返回404。
   ```

   > ```python
   > from rest_framework.generics import GenericAPIView
   > from rest_framework.mixins import RetrieveModelMixin
   >     
   > # Mixin扩展类 带参数
   > class hero_info_MixinView(RetrieveModelMixin, GenericAPIView):
   >     # 指定查询集和序列化器
   >     queryset = hero_info.objects.all()
   >     serializer_class = hero_info_serializer
   >     # 查询单个英雄信息
   >     def get(self, request, pk):
   >         return self.retrieve(request)
   > ```

4. UpdateModelMixin

   ```
   更新视图扩展类，提供 update(request, *args, **kwargs) 方法，可以快速实现更新一个存在的数
   据对象。
   同时也提供 partial_update(request, *args, **kwargs) 方法，可以实现局部更新。
   成功返回200，序列化器校验数据失败时，返回400错误。
   ```

   > ```python
   > from rest_framework.generics import GenericAPIView
   > from rest_framework.mixins import RetrieveModelMixin, UpdateModelMixin
   > # Mixin扩展类 带参数
   > class hero_info_MixinView(RetrieveModelMixin, UpdateModelMixin, GenericAPIView):
   >     # 指定查询集和序列化器
   >     queryset = hero_info.objects.all()
   >     serializer_class = hero_info_serializer
   >  # 查询单个英雄信息
   >  ......
   >     # 修改英雄信息
   >     def put(self, request, pk):
   >         return self.update(request, pk)
   > ```

5. DestroyModelMixin

   ```
   删除视图扩展类，提供 destroy(request, *args, **kwargs) 方法，可以快速实现删除一个存在的
   数据对象。
   成功返回204，不存在返回404。
   ```

   > ```python
   > from rest_framework.generics import GenericAPIView
   > from rest_framework.mixins import RetrieveModelMixin, UpdateModelMixin, 
   > DestroyModelMixin
   > # Mixin扩展类 带参数
   > class hero_info_MixinView(RetrieveModelMixin, UpdateModelMixin, 
   > DestroyModelMixin, GenericAPIView):
   >     # 指定查询集和序列化器
   >     queryset = hero_info.objects.all()
   >     serializer_class = hero_info_serializers
   >     
   >  # 查询单个英雄信息
   >  ......
   >     # 删除英雄信息
   >     def delete(self, request, pk):
   >         return self.destroy(request, pk)
   > 
   > ```

##### 常用的三级视图

```
三级视图是基于 GenericAPIView 与 五个扩展类 所封装的子类，可以快速的实现数据的增删改查处
理。
```

1. ListAPIView

   ```python
   提供 get 方法
   继承自：GenericAPIView、ListModelMixin
   ```

   > ```python
   > from rest_framework.generics import ListAPIView
   > # 三级视图 不带参数
   > class hero_list_view(ListAPIView):
   >     # 指定查询集和序列化器
   >     queryset = hero_info.objects.all()
   >     serializer_class = hero_info_serializer
   > ```
   >
   > 注意：因为父类已经有get方法了，子类继承父类的 get 方法即可。

2. CreateAPIView

   ```
   提供 post 方法
   继承自： GenericAPIView、CreateModelMixin
   ```

   > ```python
   > from rest_framework.generics import ListAPIView, CreateAPIView
   > # 三级视图 不带参数
   > class hero_list_view(ListAPIView, CreateAPIView):
   >     # 指定查询集和序列化器
   >     queryset = hero_info.objects.all()
   >     serializer_class = hero_info_serializer
   > 
   > ```
   >
   > 注意：因为父类已经有post方法了，子类继承父类的post方法即可。

3. ListCreateAPIView

   ```
   提供 get，post 方法，二合一加强版
   继承自ListModelMixin，CreateModelMixin，GenericAPIView
   ```

   > ```python
   > from rest_framework.generics import ListCreateAPIView
   > # 三级视图 二合一加强版
   > class hero_list_pro_view(ListCreateAPIView):
   >     # 指定查询集和序列化器
   >     queryset = hero_info.objects.all()
   >     serializer_class = hero_info_serializers
   > 
   > ```
   >
   > 注意：因为父类已经有 get 和 post 方法了，子类直接继承父类的 get 和 post 方法即可。

4. RetrieveAPIView

   ```
   提供 get 方法
   继承自: GenericAPIView、RetrieveModelMixin
   ```

   > ```python
   > from rest_framework.generics import RetrieveAPIView
   > # 三级视图 带参数
   > class hero_info_view(RetrieveAPIView):
   >     # 指定查询集和序列化器
   >     queryset = hero_info.objects.all()
   >     serializer_class = hero_info_serializer
   > ```
   >
   > 注意：因为父类已经有get 方法了，子类继承父类的get 方法，所以子类不用写get 方法了

5. UpdateAPIView

   ```
   提供 put 和 patch 方法
   继承自：GenericAPIView、UpdateModelMixin
   ```

   > ```python
   > from rest_framework.generics import RetrieveAPIView, UpdateAPIView
   > 
   > # 三级视图 带参数
   > class hero_info_view(RetrieveAPIView, UpdateAPIView):
   >     # 指定查询集和序列化器
   >     queryset = hero_info.objects.all()
   >     serializer_class = hero_info_serializer
   > ```
   >
   > 注意：因为父类已经有put 和 patch 方法了，子类继承父类的方法，所以子类不用写put 和 patch 方
   > 法了

6. DestroyAPIView

   ```
   提供 delete 方法
   继承自：GenericAPIView、DestoryModelMixin
   ```

   > ```python
   > from rest_framework.generics import RetrieveAPIView, UpdateAPIView, 
   > DestroyAPIView
   > # 三级视图 带参数
   > class hero_info_view(RetrieveAPIView, UpdateAPIView, DestroyAPIView):
   >     # 指定查询集和序列化器
   >     queryset = hero_info.objects.all()
   >     serializer_class = hero_info_serializer
   > 
   > ```

7. RetrieveUpdateDestroyAPIView

   ```
   提供 get、put、patch、delete方法，三合一加强版
   继承自：GenericAPIView、RetrieveModelMixin、UpdateModelMixin、DestoryModelMixin
   ```

   > ```python
   > from rest_framework.generics import RetrieveUpdateDestroyAPIView
   > # 三级视图 带参数 三合一加强版
   > class hero_info_pro_view(RetrieveUpdateDestroyAPIView):
   >     # 指定查询集和序列化器
   >     queryset = hero_info.objects.all()
   >     serializer_class = hero_info_serializers
   > ```

### 3、DEF的视图集ViewSet

```
基于前面的各种视图，我们把它汇总在一起打包成一个视图集，使用视图集ViewSet，可以将一系
列逻辑相关的动作放到同一个类中。
```

- ModelViewSet

  ```
  它继承自GenericAPIVIew，同时也继承了ListModelMixin、RetrieveModelMixin、
  CreateModelMixin、UpdateModelMixin、DestoryModelMixin。
  ```

  > ```python
  > from rest_framework.viewsets import ModelViewSet
  > class hero_ViewSet(ModelViewSet):
  >     # 使用视图集
  >     queryset = hero_info.objects.all()
  >     serializer_class = hero_info_Serializer
  > ```

- 设置路由

  ```python
  from APP08 import views
  urlpatterns = [   
      # 视图集
      path('heros/', views.hero_ModelViewSet.as_view({
          'get': 'list',
          'post': 'create'
     })),
      re_path(r'^heros/(?P<pk>\d+)/$', views.hero_ModelViewSet.as_view({
          'get': 'retrieve',
          'put': 'update',
          'delete': 'destroy'
     }))
  ]
  ```

- **DEF的路由Routers（注册视图集）**

  ```python
  from rest_framework.routers import DefaultRouter 
  
  # 路由器只能结合视图集一起使用
  router = DefaultRouter()  # 可以处理视图的路由器
  router.register(r'heros', views.hero_ViewSet, base_name='heros')  # 向路由器中注册
  视图集
  urlpatterns += router.urls  # 将路由器中的所以路由信息追到到django的路由列表中
  ```

  > register(prefix, viewset, base_name)
  > 	prefix 该视图集的路由前缀
  > 	viewset 视图集
  > 	base_name 路由名称的前缀

# 九、DRF其它功能、

### 1、认证Authentication

- **可以在配置文件中配置全局默认的认证方案**

  ```python
  REST_FRAMEWORK = {
      'DEFAULT_AUTHENTICATION_CLASSES': (
          'rest_framework.authentication.BasicAuthentication',   # 基本认证
          'rest_framework.authentication.SessionAuthentication',  # session认证
     )
  }
  ```

- **也可以在每个视图中通过设置authentication_classess属性来设置**

  ```python
  from rest_framework.authentication import SessionAuthentication, 
  BasicAuthentication
  from rest_framework.views import APIView
  
  class ExampleView(APIView):
      authentication_classes = (SessionAuthentication, BasicAuthentication)
     ...
  ```

  > 认证失败会有两种可能的返回值：
  > 	401 Unauthorized 未认证
  > 	403 Permission Denied 权限被禁止

### 2、权限Permissions

- **可以在配置文件中设置默认的权限管理类**

  ```python
  REST_FRAMEWORK = {
      'DEFAULT_PERMISSION_CLASSES': (
          'rest_framework.permissions.IsAuthenticated',
     )
  }
  ```

  > 如果未指明，则采用如下默认配置:
  >
  > ```python
  > 'DEFAULT_PERMISSION_CLASSES': (
  >    'rest_framework.permissions.AllowAny',
  > )
  > ```

- **也可以在具体的视图中通过permission_classes属性来设置**

  ```python
  from rest_framework.permissions import IsAuthenticated
  from rest_framework.views import APIView
  class ExampleView(APIView):
      permission_classes = (IsAuthenticated,)
     ...
  ```

  > 提供的权限：
  > 	AllowAny 允许所有用户
  > 	IsAuthenticated 仅通过认证的用户
  > 	IsAdminUser 仅管理员用户
  > 	IsAuthenticatedOrReadOnly 认证的用户可以完全操作，否则只能get读取
  >
  > 
  >
  > 举例：
  >
  > ```python
  > from rest_framework.authentication import SessionAuthentication
  > from rest_framework.permissions import IsAuthenticated
  > from rest_framework.generics import RetrieveAPIView
  > 
  > class hero_info_View(RetrieveAPIView):
  >     queryset = hero_info.objects.all()
  >     serializer_class = hero_info_serializer
  >     authentication_classes = [SessionAuthentication]   # 设置认证
  >     permission_classes = [IsAuthenticated]             # 设置权限
  > 
  > ```

- **自定义权限**

  ```
  如需自定义权限，需继承rest_framework.permissions.BasePermission父类，并实现以下两个任何一
  个方法或全部
  
  	1、has_permission(self, request, view)
  是否可以访问视图， view表示当前视图对象
  	2、has_object_permission(self, request, view, obj)
  是否可以访问数据对象， view表示当前视图， obj为数据对象
  ```

  > 举例：
  >
  > ```python
  > # 控制对obj对象的访问权限
  > class MyPermission(BasePermission):
  >     def has_object_permission(self, request, view, obj):
  >        ......
  >         return False
  >     
  > class hero_info_ViewSet(ModelViewSet):
  >     queryset = hero_info.objects.all()
  >     serializer_class = hero_info_serializer
  >     permission_classes = [IsAuthenticated, MyPermission]
  > ```

### 3、限流Throttling

```
作用： 限流可以对接口访问的频次进行限制，用于减轻服务器压力。
```

- **在配置文件中**

  ```python
  # 使用 DEFAULT_THROTTLE_CLASSES 和 DEFAULT_THROTTLE_RATES 进行全局
  配置。
  
  REST_FRAMEWORK = {
      'DEFAULT_THROTTLE_CLASSES': (
          'rest_framework.throttling.AnonRateThrottle', # 匿名用户
          'rest_framework.throttling.UserRateThrottle' # 认证用户
     ),
      'DEFAULT_THROTTLE_RATES': {
          'anon': '100/day', # 匿名用户的限流标准
          'user': '1000/day' # 认证用户的限流标准
     }
  }
  ```

  > DEFAULT_THROTTLE_RATES 使用 second , minute , hour 或 day 来指明限流的时间周期。

- **视图中**

  ```python
  # 通过throttle_classess属性来配置
  
  from rest_framework.throttling import UserRateThrottle
  from rest_framework.views import APIView
  class ExampleView(APIView):
      throttle_classes = (UserRateThrottle,)
     ...
  ```

  > 可选限流类
  > 1、AnonRateThrottle
  > 限制所有匿名未认证用户，使用IP区分用户。
  > 使用 DEFAULT_THROTTLE_RATES['anon'] 来设置频次
  > 2、UserRateThrottle
  > 限制认证用户，使用User id 来区分。
  > 使用 DEFAULT_THROTTLE_RATES['user'] 来设置频次
  > 3、ScopedRateThrottle
  > 限制用户对于每个视图的访问频次，使用ip或user id。
  >
  > 
  >
  > ```python
  > REST_FRAMEWORK = {
  >     'DEFAULT_THROTTLE_CLASSES': (
  >         'rest_framework.throttling.ScopedRateThrottle',
  >    ),
  >     'DEFAULT_THROTTLE_RATES': {
  >         'contacts': '1000/day',
  >         'uploads': '20/day'
  >    }
  > }
  > ```

### 4、分页Pagination

- **在配置文件中设置全局的分页方式**

  ```python
  REST_FRAMEWORK = {
      'DEFAULT_PAGINATION_CLASS': 
  'rest_framework.pagination.PageNumberPagination',
      'PAGE_SIZE': 100  # 每页数目
  }
  ```

- **视图中**

  ```python
  # 自定义Pagination类，来为视图添加不同分页行为。在视图中通过 pagination_clas 属性来指
  明。
  
  # 定义分页类
  class LargeResultsSetPagination(PageNumberPagination):
      page_size = 1000
      page_size_query_param = 'page_size'
      max_page_size = 10000
      
  class hero_info_view(RetrieveAPIView):
      queryset = hero_info.objects.all()
      serializer_class = hero_info_serializer
      pagination_class = LargeResultsSetPagination    # 指定分页类
      
  
  注意：如果要在视图内关闭分页功能，只需在视图内设置
  pagination_class = None
  ```

- **可选分页器**

  1. PageNumberPagination

     ```python
     可以在子类中定义的属性：
         page_size 每页数目
         page_query_param 前端发送的页数关键字名，默认为"page"
         page_size_query_param 前端发送的每页数目关键字名，默认为None
         max_page_size 前端最多能设置的每页数量
     ```

     > 举例：
     >
     > ```python
     > from rest_framework.pagination import PageNumberPagination
     > 
     > class StandardPageNumberPagination(PageNumberPagination):
     >     page_size_query_param = 'page_size'
     >     max_page_size = 10
     >     
     > class hero_list_View(ListAPIView):
     >     queryset = hero_info.objects.all().order_by('id')
     >     serializer_class = hero_info_serializer
     >     pagination_class = StandardPageNumberPagination
     > ```
     >
     > 前端访问格式：127.0.0.1/heros/?page=1&page_size=2

  2. LimitOffsetPagination

     ```python
     可以在子类中定义的属性：
         default_limit 默认限制，默认值与 PAGE_SIZE 设置一致
         limit_query_param limit参数名，默认'limit'
         offset_query_param offset参数名，默认'offset'
         max_limit 最大limit限制，默认None
     ```

     > ```python
     > from rest_framework.pagination import LimitOffsetPagination
     > class hero_list_view(ListAPIView):
     >     queryset = hero_info.objects.all().order_by('id')
     >     serializer_class = hero_info_serializer
     >     pagination_class = LimitOffsetPagination
     > ```
     >
     > 前端访问格式：127.0.0.1:8000/heros/?offset=3&limit=2

### 5、异常处理Exceptions

REST framework提供了异常处理，我们可以自定义异常处理函数。

```python
from rest_framework.views import exception_handler

def custom_exception_handler(exc, context):
    # 先调用REST framework默认的异常处理方法获得标准错误响应对象
    response = exception_handler(exc, context)
    # 在此处补充自定义的异常处理
    if response is not None:
        response.data['status_code'] = response.status_code
    return response
```

在配置文件中声明自定义的异常处理

```python
REST_FRAMEWORK = {
    'EXCEPTION_HANDLER': 'my_project.my_app.utils.custom_exception_handler'
}
```

如果未声明，会采用默认的方式，如下

```python
REST_FRAMEWORK = {
    'EXCEPTION_HANDLER': 'rest_framework.views.exception_handler'
}
```

> 举例：
>
> ```python
> # 补充上处理关于数据库的异常
> from rest_framework.views import exception_handler as drf_exception_handler
> from rest_framework import status
> from django.db import DatabaseError
> 
> def exception_handler(exc, context):
>     response = drf_exception_handler(exc, context)
>     if response is None:
>         view = context['view']
>         if isinstance(exc, DatabaseError):
>             print('[%s]: %s' % (view, exc))
>             response = Response({'detail': '服务器内部错误'}, 
> status=status.HTTP_507_INSUFFICIENT_STORAGE)
>     return response
> ```
>
> REST framework定义的异常：
> 	APIException 所有异常的父类
> 	ParseError 解析错误
> 	AuthenticationFailed 认证失败
> 	NotAuthenticated 尚未认证
> 	PermissionDenied 权限决绝
> 	NotFound 未找到
> 	MethodNotAllowed 请求方式不支持
> 	NotAcceptable 要获取的数据格式不支持
> 	Throttled 超过限流次数
> 	ValidationError 校验失败

### 6、自动生成接口文档

DRF 框架可以自动帮助我们生成接口文档，方便用于查看对接，接口的文档会以路由访问网页的方式进
行呈现。

```python
# 安装
pip install coreapi 
```

- 设置接口文档访问路径

  ```python
  # 因为这个文档是通过路由访问网页的方式进行呈现的，所以我们需要在 主路由 中添加接口文档路径。
  from rest_framework.documentation import include_docs_urls # 导入
  
  urlpatterns = [
     ...
      path('docs/', include_docs_urls(title='Heros API')),
  ]
  ```

- 配置DEFAULT_SCHEMA_CLASS

  ```python
  # 在 REST_FRAMEWORK 中配置自动生成接口文档的指令。
  REST_FRAMEWORK = {
   ...
   # 自动生成接口文档
      'DEFAULT_SCHEMA_CLASS': 'rest_framework.schemas.coreapi.AutoSchema',
  }
  ```

- 查看接口文档

  ```
  浏览器访问 127.0.0.1:8000/docs/，即可看到自动生成的接口文档。
  ```

  > 注意事项：
  > 1、视图集ViewSet中的retrieve名称，在接口文档网站中叫做read
  > 2、参数的Description需要在模型类或序列化器类的字段中以help_text选项定义，如：
  >
  > ```python
  > class hero_info(models.Model):
  >    ...
  >     bread = models.IntegerField(default=0, verbose_name='阅读量', help_text='阅读
  > 量')
  >    ...
  > ```

# 十、模板标签

### 1、模板配置

```python
# 步骤
1.在django根目录下创建模板文件夹用来存放模板文件 （template）
2.settings.py中的TEMPLATES里的DIRS配置模板路径
3.使用模板,在文件夹下创建html文件
```

### 2、模板语法

语法：{{ 变量名}}

列表取值：{{ 变量名.下标}}

字典取值：{{ 变量名.键名}}

### 3、模板标签

##### 1、注释

```python
  1. <!-- 注释1 -->  html的注释语法,这个注释会在前端源代码中呈现出来
  2. {# 注释2 #}	DTL的注释语法
  3. {% comment %} 内容 {% endcoment %}	 comment标签之间的内容全部都会注释掉
   不管是{#  #}还是使用comment标签进行注释的时候，都不会在前端的源代码中呈现
```

##### 2、常用模板标签

```python
# if 标签
{% if num1 > 20 %}
{{ num1 }}
{% else  %}
no
{% endif  %}

# for 标签
{% for i in list %}
    <li>{{ i.name }}</li>
{% endfor %}

```

##### 3、自定义标签

```python
# 步骤：
1、在子应用中创建一个名为templatetags用于存放自定义标签的文件夹
2、创建用于定义自定义标签的py文件，以及一个__init__.py文件，该文件表示将所在目录视作一个可被导入的模块
3、导包from django.template import Library
4、 实例化一个Library对象
   	注意，register变量名千万不要变，不要用其他的名字，就用这个register
5、 定义实现自定义标签，以函数的形式
6、 将设计好的自定义标签进行注册
7、 在模板文件的开头将自定义标签引用到模板中

from django.template import Library
   
register = Library()        # 注意，register变量名千万不要变，不要用其他的名字，就用这个register
   
@register.simple_tag()		# 将自定义标签进行注册
def upper(value):
   # 将36e转换为36E之后输出在前端
   temp = value[-1]
   result = value[0:2] + temp.upper()
   return result

# 模板中
{% load 自己创建的用于设计自定义标签代码的py文件名 %}		
```

##### 4、过滤器

**语法：{{ 变量名 | 过滤器：可选参数 }}**

| length         | 返回变量的长度                                               |
| -------------- | ------------------------------------------------------------ |
| add            | 将add后的参数与变量值进行相加(只能进行整型的数据相加)        |
| safe           | 如果返回的数据中有前端语言可能进行转义的字符                 |
| filesizeformat | 以更易读的方式显示文件的大小                                 |
| date           | 根据给定格式对一个日期变量进行格式化。格式 **Y-m-d H:i:s**返回 **年-月-日 小时:分钟:秒** 的格式时间。 |

自定义过滤器：

```
步骤：

1. 创建一个文件夹用于存放自定义的过滤的定义代码的py文件，命名templatetags不能变，py文件名可自定义，register用于接收实例化的Library对象，并且register变量名也不能够改变，就为register
2. 自定义过滤器的代码
3. 将自定义过滤器注册到项目之中，注册时就用register来实现：@register.filter(name='liuquan')
```

### 4、模板继承

```python
模板继承和类的继承含义是一样的，主要是为了提高代码重用，减轻开发人员的工作量。

典型应用：网站的头部、尾部信息。
3.1、父模板

如果发现在多个模板中某些内容相同，那就应该把这段内容定义到父模板中。

标签block：用于在父模板中预留区域，留给子模板填充差异性的内容，名字不能相同。 为了更好的可读性，建议给endblock标签写上名字，这个名字与对应的block名字相同。父模板中也可以使用上下文中传递过来的数据。

{%block 名称%}
预留区域，可以编写默认内容，也可以没有默认内容
{%endblock  名称%}
3.2、子模板

标签extends：继承，写在子模板文件的第一行。

{% extends "父模板路径"%}
子模版不用填充父模版中的所有预留区域，如果子模版没有填充，则使用父模版定义的默认值。

填充父模板中指定名称的预留区域。

{%block 名称%}
实际填充内容
{{block.super}}用于获取父模板中block的内容
{%endblock 名称%}
```

# 十一、Admin

### 1、使用步骤

- 创建超级管理员

  ```
  python manage.py createsuperuser
  ```

  > 如果提示 django.db.utils.OperationalError: no such table: auth_user 错误：先如下执行
  >
  > ```python
  > python manage.py migrate # 先执行这个命令
  > python manage.py createsuperuser # 然后再创建超级管理员
  > python manage.py changepassword 用户名  # 如果想要修改密码可以执行（可选）
  > ```

- 访问

  ```
  1、http://127.0.0.1:8000/admin/   # 路由后添加admin
  2、输入前面创建的超级管理员号码 
  ```

### 2、配置

- **apps.py**

  ```python
  # 在apps.py 类中添加
  verbose_name = '英雄管理' 
  ```

  > 用于设置该子应用的别名，此名字在Django提供的Admin管理站点中会显示。

- **admin.py 注册模型类**

  ```python
  from django.contrib import admin
  导入需要注册的模型类
  
  # 第一种使用方式: 注册参数 (不推荐)
  class hero_info_Admin(admin.ModelAdmin):
      pass
  admin.site.register(模型类名, 模型类名_Admin)
  
  
  # 第二种使用方式: 使用装饰器 (推荐使用)
  @admin.register(模型类名)
  class 模型类名_Admin(admin.ModelAdmin):
      pass
  ```

### 3、方法

- **基础设置**

  ```python
  admin.site.site_header = '网站页头名'       
  admin.site.site_title = '页面标题名'
  admin.site.index_title = '首页标语'
  ```

- **通用设置**

  ```python
  @admin.register(模型类名)
  class 模型类名_Admin(admin.ModelAdmin):
      list_per_page = 100             # 设置每页显示的数据量
      actions_on_top=True   			# "操作选项"的位置，为True在顶部显示，默认为True。
      actions_on_bottom=False 	    # 底部显示的属性，为True在底部显示，默认为False。
      list_filter=[]        		    # 设置右侧过滤栏的字段
      search_fields=[]     	        # 设置搜索框的字段
      fields=[]             		    # 设置调整编辑页展示的字段
  ```

- **展示字段设置**

  ```python
  @admin.register(模型类名)
  class 模型类名_Admin(admin.ModelAdmin):
      # 展示模型类字段或定义的列方法
      list_display=[模型字段1,模型字段2,...]
  ```

  > 定义列方法：
  >
  > ```python
  > 1、模型类中
  > class hero_info(models.Model):
  >    ...
  >     # 定义方法作为列
  >     def win_rate(self):
  >         return self.victory_count / self.usage_count
  >     win_rate.short_description = "胜率"
  >     
  > 2、在admin.py
  > class hero_info_Admin(admin.ModelAdmin):
  >    ...
  >     # 显示列表中的列（字段）信息
  >     list_display = ['hero_name', 'hero_price', 'usage_count',
  >         'victory_count', 'week_free', 'release_date', 'win_rate'] # 增加win_rate方
  > 法
  > ```

- **分组显示**

  ```python
  fieldset = (
     ('组1标题', {'fields': ['字段1', '字段2']}),
     ('组2标题', {
          'fields': ['字段3', '字段4'],
          'classes': 'collapse'   # 折叠选项显示
     })
  )
  ```

- **关联对象**

  1. 以块的形式嵌入：

     ```python
     1\打开子应用中的 admin.py 文件，创建 hero_skill_StackInline 类。
     # 创建一个StackInline类，以块的形式嵌入到主表信息的编辑页面
     class hero_skill_StackInline(admin.StackedInline):
         model = hero_skill  # 被编辑的从表模型类
         extra = 0           # 附加编辑的数量，相当于新增数据的模板数量
         
     2\打开子应用中的 admin.py 文件，在 hero_info_Admin 类中添加如下代码：
     class hero_info_Admin(admin.ModelAdmin):
        ....
         # 在主表信息的编辑页面编辑从表信息
         inlines = [hero_skill_StackInline]
     ```

  2. 以表格的形式嵌入

     ```python
     1\打开子应用中的 admin.py 文件，创建 hero_skill_TabularInline 类。
     # 创建一个TabularInline类，以表格的形式嵌入到主表信息的编辑页面
     class hero_skill_TabularInline(admin.TabularInline):
         model = hero_skill # 被编辑的从表模型类
         extra = 0 # 附加编辑的数量，相当于新增数据的模板数量
     
     2\打开子应用中的 admin.py 文件，在 hero_info_Admin 类中添加如下代码：
     class hero_info_Admin(admin.ModelAdmin):
        ...
         # 在主表信息的编辑页面编辑从表信息
         inlines = [hero_skill_TabularInline]
     ```

- **图片上传与显示**

  1. 安装

     ```
     pip install Pillow
     ```

  2. 为模型类添加ImageField字段

     ```python
     class hero_info(models.Model):
        ...
        # upload_to：选项指明该字段的图片保存在MEDIA_ROOT目录中的哪个子目录下。
         hero_image = models.ImageField(upload_to='hero', verbose_name='英雄图片', 
     null=True)
     ```

  3. 增加字段

     ```python
     class hero_info_Admin(admin.ModelAdmin):
        ....
         # 显示字段信息
         fieldsets = (
            ('基础信息', {'fields': ['hero_name', 'hero_price', 'week_free', 
     'hero_image']}),
            ('综合信息', {
                 'fields': ['usage_count', 'victory_count', 'release_date'],
                 'classes': ('collapse',)   # 折叠选项显示
            })
        )
     ```

# 十二、forms表单

```
Django中的表单：
1、HTML中的表单：用来提交数据给服务器的,不管后台的服务器用的是Django还是PHP语言还是其他语言
2、Django中的表单：
      ①渲染表单模板
      ②表单验证数据是否合法
```

### 1、forms.py

**①app下新建forms.py文件**

| 用于表单验证常用的Field | 说明                                                         | 参数                                                         |
| ----------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| CharField               | 验证字符串                                                   | max_length：这个字段值的最大长度<br/>min_length：这个字段值的最小长度<br/>required：这个字段是否必须，默认是必须的<br/>error_messages：在某个条件验证失败的时候，给出错误信息 |
| EmailField              | 用来接收邮件，会自动验证邮件是否合法                         |                                                              |
| FloatField              | 用来接收浮点类型，并且如果验证通过后，会将这个字段的值转换为浮点类型 | max_value：最大的值<br/>min_value：最小的值                  |
| IntegerField            | 用来接收整形，并且验证通过后，会将这个字段的值转换为整形     | max_value：最大的值<br/>min_value：最小的值                  |
| URLField                | 用来接收url格式的字符串                                      |                                                              |
| ModelChoiceField        |                                                              | queryset： 查询数据库中的数据 <br>to_field_name=None:HTML中value的值对应的字段<br>limit_choices_to=None:ModelForm中对queryset二次筛选 |

**常用参数：**

| 参数           | 说明                                                       |
| -------------- | ---------------------------------------------------------- |
| label          | 指定标签                                                   |
| required       | 指定该字段是否为必传，布尔                                 |
| initial        | 初始默认值                                                 |
| help_text      | 帮助信息（在标签旁边显示）                                 |
| error_messages | （错误信息{‘required’:'不能为空'，‘invalid’：‘格式错误’}） |
| validators     | 自定义验证规则                                             |
| localize       | 是否支持本地化                                             |
| disabled       | 是否可以编辑                                               |
| widget         | HTML插件                                                   |

```python
from django import forms  # 导入forms包
 
# ①、定义一个表单
class MessageBoardForm(forms.Form):
    name = forms.CharField(label='名字',max_length=200,min_length=2)
    title = forms.CharField(label='标题',max_length=200,min_length=2,error_messages={"min_length":"长度不符合"})
    # widget 指定使用text类型
    content = forms.CharField(label='内容',widget=forms.Textarea)
    email = forms.EmailField(label='邮箱')
    reply = forms.BooleanField(required=False,label='回复')
    


from news.models import Tag, News
from .models import Menu
from user.models import User
class UserModelForm(forms.ModelForm):

    class Meta:
        # 指定模型类
        model = User
        # 展示的字段名
        fields = ['username', 'mobile', 'is_staff', 'is_superuser', 'is_active', 'groups']




class MenuModelForm(forms.ModelForm):
    parent = forms.ModelChoiceField(queryset=None,required=False,help_text='父菜单')


    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.fields['parent'].queryset = Menu.objects.filter(is_delete=False, is_visible=True, parent=None)

    class Meta:
        model = Menu
        fields = ['name', 'url', 'order', 'parent', 'icon', 'codename', 'is_visible']



# 导入权限类、分组类
from django.contrib.auth.models import Permission, Group
class GroupModelForm(forms.ModelForm):
    #ModelMultipleChoiceField 多选
    permissions = forms.ModelMultipleChoiceField(queryset=None, required=False,  help_text='权限', label='权限')  #label就是渲染到详情页中的 权限

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.fields['permissions'].queryset = Permission.objects.filter(menu__is_delete=False)

    class Meta:
        model = Group
        fields = ['name', 'permissions']


# 在admin/forms.py中创建如下表单

from ckeditor_uploader.widgets import CKEditorUploadingWidget

class NewsModelForm(forms.ModelForm):
    tag = forms.ModelChoiceField(queryset=None, required=False, help_text='分类', label='分类')
    content = forms.CharField(widget=CKEditorUploadingWidget(), label='内容')

    def __init__(self, *args, **kwargs):
        super().__init__(*args, **kwargs)
        self.fields['tag'].queryset = Tag.objects.filter(is_delete=False)

    class Meta:
        model = News
        fields = ['title', 'is_delete', 'digest', 'image_url', 'tag', 'content']
```

> 写forms字段

**validators参数用来指定验证器，进一步对数据进行过滤：**

**语法：validators=[validators.过滤验证器，message=验证失败后返回的话)]**

| 过滤验证器         | 说明               |
| ------------------ | ------------------ |
| MaxValueValidator  | 验证最大值         |
| MinValueValidator  | 验证最小值         |
| MinLengthValidator | 验证最小长度       |
| MaxLengthValidator | 验证最大长度       |
| EmailValidator     | 验证是否是邮箱格式 |
| URLValidator       | 验证是否是URL格式  |
| RegexValidator     | 正则表达式的验证器 |

```python
telephone = forms.CharField(
    validators=[validators.RegexValidator("1[345678]\d{9}",
                                          message='请输入正确格式的手机号码！')]
)
```

> forms字段中正则的使用

### 2、视图

```python
在使用GET请求的时候，我们传了一个form给模板，那么以后模板就可以使用form来生成一个表单的html代码；
在使用POST请求的时候，我们根据前端上传上来的数据，构建一个新的表单，这个表单是用来验证数据是否合法的；
如果数据都验证通过了，那么我们可以通过cleaned_data来获取相应的数据。在模板中渲染表单的HTML代码如下：

from course.forms import MessageBoarForm    # 导入forms.py 定义的表单类
from django.views import View

# ②、定义GET或者POST方法：根据是GET还是POST请求来做相应的操作
class IndexView(View):
    # 渲染模板
    def get(self,request):
        form = MessageBoarForm()
        return render(request,'index.html',context={'form':form})
    
    def post(self, request):
        # 实例类对象，传入返回的POST 请求数据
        form = MessageBoardForm(request.POST)
        # form.is_valid()：验证数据是否合法 
        if form.is_valid():
            # form.cleaned_data.get(类中定义的字段名)：获取表单数据
            title = form.cleaned_data.get('title')
            content = form.cleaned_data.get('content')
            email = form.cleaned_data.get('email')
            reply = form.cleaned_data.get('reply')
            form.save()
            return HttpResponse('success')
        
        else:
            # form.errors.get_json_data()：打印错误信息
            errno = form.errors.get_json_data()
            # {'telephone': [{'message': '请输入正确的手机号格式！', 'code': 'invalid'}]}
            return JsonResponse(data=errno,safe=False)
```

> 接收基本步骤：
>
> 1、form = MessageBoardForm(request.POST) 实例对象
>
> 2、form.is_valid() 校验数据
>
> 3、如果成功，form.save()保存
>
> 4、如果失败，可以使用form.errors.get_json_data()  打印错误信息

### 3、模板

```python
我们在最外面给了一个form标签，然后在里面使用了table标签来进行美化，
在使用form对象渲染的时候，使用的是table的方式，当然还可以使用ul的方式（as_ul），也可以使用p标签的方式（as_p），
并且在后面我们还加上了一个提交按钮。这样就可以生成一个表单了

③index.html文件内容如下：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="" method="post">
    <table>
        {{ form.as_table }}
        <tr>
            <td></td>
            <td><input type="submit" value="提交"></td>
        </tr>
    </table>
</form>
</body>
</html>
```

### 4、ChoiceField的区别

[![返回主页](https://www.cnblogs.com/skins/custom/images/logo.gif)[Django forms组件里的ChoiceField、ModelChoiceField和ModelMutipleChoiceField的区别](https://www.cnblogs.com/pungchur/p/11969844.html)



**阅读目录**

- **ChoiceField**
- **ModelChoiceField**
- **ModelMutipleChoiceField**

**阅读简要**

```
首先我们要明白Django forms组件里的ChoiceField、ModelChoiceField和ModelMutipleChoiceField是继承关系
```

 

![img](https://img2018.cnblogs.com/common/1414912/201912/1414912-20191202143202686-12486729.png)



## ChoiceField

```
1. Django forms组件中ChoiceField字段是对models里choice的渲染

2. choices作用:在数据库中用元组的第一项作为存储的值,在显示时,将元组的第二项作为显示的内容,便于前端使用下拉框

3. 用get_xxxx_display()显示第二项的值
```

![img](https://img2018.cnblogs.com/common/1414912/201912/1414912-20191202122024608-1037354832.png)

 

 

 

```
class Authors(models.Model):
    name = models.CharField("姓名", max_length=32)
    gender = models.SmallIntegerField(choices=((1, "男"), (2, "女")), default=1)
```

## ModelChoiceField

```
1. Django forms组件中ModelChoiceField字段是对models里Forekey的渲染

2. 在前端渲染为下拉菜单
```

![img](https://img2018.cnblogs.com/common/1414912/201912/1414912-20191202122033269-100102540.png)

```
class Book(models.Model):
    title = models.CharField("书名", max_length=32)
    publish = models.ForeignKey(to="Publish", on_delete=models.CASCADE, verbose_name="出版社")
```

## ModelMutipleChoiceField

```
1. Django forms组件中ModelMutipleChoiceField字段是对models里ManyToManyField的渲染
2. 在前端渲染为多选菜单
```

![img](https://img2018.cnblogs.com/common/1414912/201912/1414912-20191202122243232-1624033802.png)

models.py

```
class Authors(models.Model):
    name = models.CharField("姓名", max_length=32)
    age = models.IntegerField("年龄")
    gender = models.SmallIntegerField(choices=((1, "男"), (2, "女")), default=1)

    def __str__(self):
        return self.name

class Book(models.Model):
    title = models.CharField("书名", max_length=32)
    publish_time = models.DateField()
    publish = models.ForeignKey(to="Publish", on_delete=models.CASCADE, verbose_name="出版社")
    authors = models.ManyToManyField(to="Authors")

    def __str__(self):
        return self.title


class Publish(models.Model):
    name = models.CharField("出版社名", max_length=32)
    address = models.CharField("地址", max_length=32)

    def __str__(self):
        return self.name
```

forms.py

```
from django import forms


class AuthorForm(forms.Form):
    name = forms.CharField(label="姓名", max_length=32)
    age = forms.IntegerField(label="年龄")
    gender = forms.ChoiceField(choices=((1, "男"), (2, "女")))


class BookForm(forms.Form):
    title = forms.CharField(label="书名", max_length=32)
    publish_time = forms.DateField(label="发行时间")
    publish = forms.ModelChoiceField(label="出版社", queryset=Publish.objects.all())
    authors = forms.ModelMultipleChoiceField(label="作者", queryset=Authors.objects.all())
```

# 十三、搜索功能

### 1、search_indexes.py

**在apps下新建search_indexes.py 文件**

```python
from haystack import indexes    # 导入haystack中的indexes
from .models import News


class NewsIndex(indexes.SearchIndex, indexes.Indexable):
    """
    这个模型的作用类似于django的模型，它告诉haystack哪些数据会被
    放进查询返回的模型对象中，以及通过哪些字段进行索引和查询
    """
    # 这字段必须这么写，用来告诉haystack和搜索引擎要索引哪些字段
    # use_template：使用模板
    text = indexes.CharField(document=True, use_template=True)
    
    # indexes.CharField(model_attr=模型类中定义的字段名)
    # 模型字段,打包数据
    id = indexes.CharField(model_attr='id')
    title = indexes.CharField(model_attr='title')
    digest = indexes.CharField(model_attr='digest')
    content = indexes.CharField(model_attr='content')
    image_url = indexes.CharField(model_attr='image_url')

    def get_model(self):
        """
        返回建立索引的模型
        :return:
        """
        return 模型类名

    def index_queryset(self, using=None):
        """
        返回要建立索引的数据查询集
        :param using:
        :return:
        """
        # 这种写法遵从官方文档的指引
        return self.get_model().objects.filter(is_delete=False)
```

### 2、视图

```python
class NewsSearchView(SearchView):
    """
    新闻搜索视图
    """
    # 设置搜索模板文件
    template_name = 'news/search.html'

    # 重写get请求，如果请求参数q为空，返回模型News的热门新闻数据
    # 否则根据参数q搜索相关数据
    def get(self, request, *args, **kwargs):
        query = request.GET.get('q')
        if not query:
            # 显示热门新闻
            hot_news = HotNews.objects.select_related('news__tag').only('news__title', 'news__image_url', 'news_id',
                                                                        'news__tag__name').filter(
                is_delete=False).order_by('priority', '-news__clicks')
            paginator = Paginator(hot_news, settings.HAYSTACK_SEARCH_RESULTS_PER_PAGE)
            try:
                # 分页
                page = paginator.get_page(int(request.GET.get('page')))
            except Exception as e:
                page = paginator.get_page(1)
            # 返回数据
            return render(request, 'news/search.html', context={
                'page': page,
                'paginator': paginator,
                'query': query
            })
        
        else:
            # 搜索
            return super().get(request, *args, **kwargs)

    def get_context_data(self, *args, **kwargs):
        """
        在context中添加page变量
        :param args:
        :param kwargs:
        :return:
        """
        context = super().get_context_data(*args, **kwargs)
        if context['page_obj']:
            context['page'] = context['page_obj']
        return context
```

### 3、模板

**新建文件夹：templates  >   search  >  indexes  >  模型类名  >  模型类名_text.txt**   

```python
# 语法：{{ object.字段名 }}

{{ object.title }}
{{ object.digest }}
{{ object.content }}
{{ object.author.username }}
```

### 4、html文件

```html

{% extends 'base/base.html' %}
{% load static %}
{% load news_template_filters %}
{% block title %}新闻搜索{% endblock %}
{% block link %}
    <link rel="stylesheet" href="{% static 'css/news/search.css' %}">
{% endblock %}

{% block main_contain %}
    <!-- main-contain start  -->
    <div class="main-contain ">
        <!-- search-box start -->
        <div class="search-box">
            <form action="" style="display: inline-flex;">

                <input type="search" placeholder="请输入要搜索的内容" name="q" class="search-control">


                <input type="submit" value="搜索" class="search-btn">
            </form>
            <!-- 可以用浮动 垂直对齐 以及 flex  -->
        </div>
        <!-- search-box end -->
        <!-- content start -->
        <div class="content">
            {% if query %}
                <!-- search-list start -->
                <div class="search-result-list">
                    <h2 class="search-result-title">搜索结果 <span>{{ page.paginator.num_pages|default:0 }}</span> 页</h2>
                    <ul class="news-list">
                        {% load highlight %}
                        {% for news in page.object_list %}
                            <li class="news-item clearfix">
                                <a href="{% url 'news:news_detail' news.id %}" class="news-thumbnail"
                                   target="_blank"><img src="{{ news.image_url }}" alt=""></a>
                                <div class="news-content">
                                    <h4 class="news-title">
                                        <a href="{% url 'news:news_detail' news.id %}">{% highlight news.title with query %}</a>
                                    </h4>
                                    <p class="news-details">{{ news.digest }}</p>
                                    <div class="news-other">
                                        <span class="news-type">{{ news.object.tag.name }}</span>
                                        <span class="news-time">{{ news.object.update_time }}</span>
                                        <span class="news-author">{% highlight news.object.author.username with query %}</span>
                                    </div>
                                </div>

                            </li>
                        {% empty %}
                            <li class="news-item clearfix">
                                <p>没有找到你想要的找的内容.</p>
                            </li>
                        {% endfor %}
                    </ul>
                </div>

                <!-- search-list end -->
            {% else %}
                <!-- news-contain start -->

                <div class="news-contain">
                    <div class="hot-recommend-list">
                        <h2 class="hot-recommend-title">热门推荐</h2>
                        <ul class="news-list">
                            {% for hotnews in page %}
                                <li class="news-item clearfix">
                                    <a href="#" class="news-thumbnail">
                                        <img src="{{ hotnews.news.image_url }}">
                                    </a>
                                    <div class="news-content">
                                        <h4 class="news-title">
                                            <a href="{% url 'news:news_detail' hotnews.news_id %}">{{ hotnews.news.title }}</a>
                                        </h4>
                                        <p class="news-details">{{ hotnews.news.digest }}</p>
                                        <div class="news-other">
                                            <span class="news-type">{{ hotnews.news.tag.name }}</span>
                                            <span class="news-time">{{ hotnews.update_time }}</span>
                                            <span class="news-author">{{ hotnews.news.author.username }}</span>
                                        </div>
                                    </div>
                                </li>
                            {% endfor %}


                        </ul>
                    </div>
                </div>


                <!-- news-contain end -->
            {% endif %}
            <!-- Pagination start-->
            <div class="page-box" id="pages">
                <div class="pagebar" id="pageBar">
                    <a class="al">{{ page.paginator.count|default:0 }}条</a>
                    <!-- prev page start-->
                    {% if page.has_previous %}
                        {% if query %}
                            <a href="{% url 'news:news_search' %}?q={{ query }}&page={{ page.previous_page_number }}"
                               class="prev">上一页</a>
                        {% else %}
                            <a href="{% url 'news:news_search' %}?page={{ page.previous_page_number }}"
                               class="prev">上一页</a>
                        {% endif %}
                    {% endif %}
                    <!-- prev page end-->

                    <!-- page bar start-->
                    {% if page.has_previous or page.has_next %}
                        {% for n in page|page_bar %}
                            {% if query %}
                                {% if n == '...' %}
                                    <span class="point">{{ n }}</span>
                                {% else %}
                                    {% if n == page.number %}
                                        <span class="sel">{{ n }}</span>
                                    {% else %}
                                        <a href="{% url 'news:news_search' %}?page={{ n }}&q={{ query }}">{{ n }}</a>
                                    {% endif %}
                                {% endif %}
                            {% else %}
                                {% if n == '...' %}
                                    <span class="point">{{ n }}</span>
                                {% else %}
                                    {% if n == page.number %}
                                        <span class="sel">{{ n }}</span>
                                    {% else %}
                                        <a href="{% url 'news:news_search' %}?page={{ n }}">{{ n }}</a>
                                    {% endif %}
                                {% endif %}
                            {% endif %}
                        {% endfor %}
                    {% endif %}
                    <!-- page bar end-->

                    <!-- next page start-->
                    {% if page.has_next %}
                        {% if query %}
                            <a href="{% url 'news:news_search' %}?q={{ query }}&page={{ page.next_page_number }}"
                               class="prev">下一页</a>
                        {% else %}
                            <a href="{% url 'news:news_search' %}?page={{ page.next_page_number }}"
                               class="prev">下一页</a>
                        {% endif %}
                    {% endif %}
                    <!-- next page end-->


                </div>
            </div>
            <!-- Pagination end-->
        </div>
        <!-- content end -->
    </div>
    <!-- main-contain  end -->
{% endblock %}
```

