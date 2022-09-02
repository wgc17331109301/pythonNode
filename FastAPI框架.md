# FastAPI

```python
# 安装
1、pip install fastapi
2、我们需要一个ASGI服务器，可以使用 Uvicorn 或 Hypercorn。
     pip install uvicorn
```

### 一、特点

- **1、快速**

  ```python
  性能极高，可与 NodeJS, Go 媲美。(得益于Starlette和Pydantic)。
  ```

  > Starlette 是一个轻量级 ASGI 框架/工具包。它非常适合用来构建高性能的 asyncio 服务，并支持 HTTP 和 WebSockets。
  >
  > 
  >
  > Pydantic 是一个使用Python类型提示来进行数据验证和设置管理的库。Pydantic定义数据应该如何使用纯Python规范用并进行验证。

- **2、简单易懂，易于上手**

  ```python
  1、设计的初衷就是易于学习和使用。不需要阅读复杂的文档。
  2、良好的编辑器支持。支持自动完成，可以花更少的时间用于调试。
  3、代码复用性强。
  4、方便的 API 调试，生成 API 文档。
  5、能提高开发人员两到三倍的开发速度。减少40%的人为出错几率。
  ```

### 二、示例

- **1、新建 main.py 文件**

  ```python
  from fastapi import FastAPI
  
  app = FastAPI()    # 实例
  
  # 定义get请求
  @app.get("/")
  def read_root():
      return {"Hello": "World"}
  
  
  # 路径参数*item_id*的值会直接作为实参*item_id*传递给函数read_item。
  @app.get("/items/{item_id}")
  # item_id：路径参数（必须为int类型）     q：参数（为str类型，可选）
  def read_item(item_id: int, q: str = None):
      return {"item_id": item_id, "q": q}
  ```

  > ```python
  > # 数据校验
  > 如果访问http://127.0.0.1:8000/items/foo，会报错
  > {
  >     "detail": [
  >         {
  >             "loc": [
  >                 "path",
  >                 "item_id"
  >             ],
  >             "msg": "value is not a valid integer",
  >             "type": "type_error.integer"
  >         }
  >     ]
  > }
  > 因为这里路径参数item_id的值是"foo"，无法转换成int。
  > ```
  >
  
- **2、运行命令**

  ```python
  uvicorn  main:app --reload
  ```

  > `关于命令`的解释:
  >
  > - `main`: 文件 `main.py` (Python "模块").
  > - `app`: `main.py` 创建的实例 `app = FastAPI()`.
  > - `--reload`: 代码有改动时服务会自动重启(仅适用于开发环境)

- **3、接口访问**

  ```python
  http://127.0.0.1:8000/items/5?q=somequery
  ```

- **4、交互式API文档**

  ```python
  访问以下两个地址，可获取自动生成的交互式API文档，并且当代码改动时文档会自动更新。方便我们的开发调试。
  
      1、http://127.0.0.1:8000/docs (基于 Swagger UI)
      2、http://127.0.0.1:8000/redoc (基于 ReDoc)
  ```

### 三、类型检查

```python
# 安装
pip install pydantic
```

- **1、使用**

  ```python
  from pydantic import ValidationError
  
  from datetime import datetime
  from typing import List
  from pydantic import BaseModel
  
  
  class User(BaseModel):
      id: int
      name = 'jack guo'
      signup_timestamp: datetime = None
      friends: List[int] = []
  ```

  > - id 要求必须为 int
  > - name 要求必须为 str, 且有默认值
  > - signup_timestamp 要求为 datetime, 默认值为 None
  > - friends 要求为 List，元素类型要求 int, 默认值为 []

- **2、使用类**

  ```python
  try:
      User(signup_timestamp='not datetime', friends=[1, 2, 3, 'not number'])
  except ValidationError as e:
      print(e.json())
  ```

  > `id` 没有默认值，按照预期会报缺失的异常
  >
  > `signup_timestamp` 被赋为非 datetime 类型值，按照预期会报异常
  >
  > `friends` 索引为 3 的元素被赋值为 str，按照预期也会报异常

- **3、执行代码**

  ```python
  [
    {
      "loc": [
        "id"
      ],
      "msg": "field required",
      "type": "value_error.missing"
    },
    {
      "loc": [
        "signup_timestamp"
      ],
      "msg": "invalid datetime format",
      "type": "value_error.datetime"
    },
    {
      "loc": [
        "friends",
        3
      ],
      "msg": "value is not a valid integer",
      "type": "type_error.integer"
    }
  ]
  ```

### 四、路径参数与请求参数

- **1、路径的顺序问题**

  ```python
  from fastapi import FastAPI
  
  app = FastAPI()
  
  
  @app.get("/users/me")
  async def read_user_me():
      return {"user_id": "the current user"}
  
  
  @app.get("/users/{user_id}")
  async def read_user(user_id: str):
      return {"user_id": user_id}
  ```

  >  这里路径 *`/users/me`* 应该在路径 *`/users/{user_id}`* 之前进行声明。否则，针对路径 `*/users/me* 的访问就会被匹配到 */users/{user_id}*，因为"me"会被认为是路径参数 *`user_id`* 的值。`

- **2、路径参数的预定义值**

  ```python
  # 使用Python Enum来声明路径参数的预定义值。
  from enum import Enum
  
  from fastapi import FastAPI
  
  
  # 这里，声明了一个类ModelName，它继承自str(这样限制了值的类型必须是字符串类型)和Enum。
  class ModelName(str, Enum):
      alexnet = "alexnet"
      resnet = "resnet"
      lenet = "lenet"
  
  
  app = FastAPI()
  
  
  @app.get("/model/{model_name}")
  async def get_model(model_name: ModelName):
      if model_name == ModelName.alexnet:
          return {"model_name": model_name}
      if model_name.value == "lenet":
          return {"model_name": model_name}
      return {"model_name": model_name}
  ```

  > 这里路径参数*model_name*的类型是我们定义的枚举类ModelName。

- **3、路径参数包含文件路径**

  ```python
  from fastapi import FastAPI
  
  app = FastAPI()
  
  
  @app.get("/files/{file_path:path}")
  # 这里:path指出了file_path可以匹配任何文件路径。
  async def read_file(file_path: str):
      return {"file_path": file_path}
  ```

- **4、路径操作的其他参数**

  ```python
  @app.post("/items/",
            response_model=Item, 
            tags=["items"],
            summary="Create an item",
            description="Create an item with all the information, name, description, price, tax and a set of unique tags",
            deprecated=True)
  async def create_item(*, item: Item):
      return item
  
  
  @app.get("/items/", tags=["items"])
  async def read_items():
      return [{"name": "Foo", "price": 42}]
  ```

  > tags：路径操作中添加其他参数，比如标签列表
  >
  > *summary*：概要
  >
  > description：描述
  >
  > *deprecated*：参数为True表示该路径操作已过期
  
- **5、请求参数**

  ```python
  from fastapi import FastAPI
  
  app = FastAPI()
  
  
  @app.get("/items/{item_id}")
  async def read_user_item(item_id: str, needy: str, skip: int = 0, limit: int = None):
      item = {"item_id": item_id, "needy": needy, "skip": skip, "limit": limit}
      return item
  ```

  > 如果函数里的参数不是路径参数的一部分，那么这样的参数就自动被解释为*请求参数*。
  >
  > 请求参数就是URL中问号('?')后面以'&'间隔开的键值对，它们是URL的一部分，并且参数类型都是字符串类型。
  >
  > ```python
  > 如果指定了参数有默认值的话，那么改参数就是可选的，如skip 、limit
  > 否则就是必选的，如 needy
  > ```

### 五、request body

```python
from fastapi import FastAPI
from pydantic import BaseModel


class Item(BaseModel):
    name: str
    description: str = None
    price: float
    tax: float = None


app = FastAPI()


@app.put("/items/{item_id}")
async def create_item(item_id: int, item: Item, q: str = None):
    result = {"item_id": item_id, **item.dict()}
    if q:
        result.update({"q": q})
    return result
```

> ```
> 函数参数按照如下的顺序进行识别匹配：
> 
> (1)、如果这个参数已经在路径中被声明过，那么它就是一个路径参数。
> 
> (2)、如果这个参数的类型是单类型的(如str、float、int、bool等)，那么它就是一个请求参数。
> 
> (3)、如果这个参数的类型是Pydantic数据模型，那么它就被认为是Request Body参数。
> ```

### 六、参数附加信息

##### 1、请求参数附加信息

```python
# 对请求参数附加信息的支持，FastAPI通过Query模块来实现。
from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
async def read_items(
    q: str = Query(
        None,
        alias="item-query",
        title="Query string",
        description="Query string for the items to search in the database that have a good match",
        min_length=3,
        max_length=50,
        regex="^fixedquery$",
        deprecated=True
    )
):
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

> #### 基于Query模块声明缺省值
>
> 可选参数声明
>
> ```
> q: str = Query(None)  # 等同于  q: str = None
> ```
>
> 缺省值参数声明
>
> ```
> q: str = Query("query")  # 等同于  q: str = "query"
> ```
>
> 必选参数声明
>
> ```
> q: str = Query(...)  # 等同于 q: str
> ```

**请求参数列表：**

```python
from typing import List
from fastapi import FastAPI, Query

app = FastAPI()


@app.get("/items/")
# 设置参数列表的缺省值
async def read_items(q: List[str] = Query(["foo", "bar"])):
    query_items = {"q": q}
    return query_items
```

> FastAPI基于Query模块可以支持请求参数列表，例如请求参数q可以在URL中出现多次：
>
> ```
> http://localhost:8000/items/?q=foo&q=bar
> ```

##### 2、路径参数附加信息

```python
from fastapi import FastAPI, Path

app = FastAPI()


@app.get("/items/{item_id}")
async def read_items(
    q: str, item_id: int = Path(..., title="The ID of the item to get")
):
    results = {"item_id": item_id}
    if q:
        results.update({"q": q})
    return results
```

> #### 注意，路径参数在URL里是必选的，因此Path的第一个参数是...，即使你传递了None或其他缺省值，也不会影响参数的必选性。

##### 3、Pydantic模型添加附加信息

```python
from fastapi import Body, FastAPI
# Field：为模型添加附加信息
from pydantic import BaseModel, Field

app = FastAPI()


class Item(BaseModel):
    name: str
    description: str = Field(None, title="The description of the item", max_length=300)
    price: float = Field(..., gt=0, description="The price must be greater than zero")
    tax: float = None


@app.put("/items/{item_id}")
async def update_item(*, item_id: int, item: Item = Body(..., embed=True)):
    results = {"item_id": item_id, "item": item}
    return results
```



##### 4、附加信息

```python
# 用于字符串的附加信息：
min_length：最小长度
max_length：最大长度
regex：正则表达式
q: str = Query(None, max_length=50)  # 限制q的最大长度不超过50
# 主要用于自动化文档的附加信息：
title：参数标题
description：参数描述信息
deprecated：表示参数即将过期
# 特殊附加信息：
alias：参数别名
# 对数字类型参数校验：
gt： 大于(greater than)
ge： 大于或等于(greater than or equal)
lt： 小于(less than)
le： 小于或等于( less than or equal)
```

##### 5、参数顺序问题

```
1、不带缺省值的参数应放在前面
2、可以通过传递 * 作为第一个参数，就解决了上面的参数顺序问题。
```

### 七、Pydantic嵌套模型

- #### 模型的属性可以是数据集合类型

```python
from typing import List,Set,Dict

class Item(BaseModel):
    tags: Set[str] = set()
    tags: List[str] = []
        
app = FastAPI()


@app.post("/index-weights/")
async def create_index_weights(weights: Dict[int, float]):
    return weights
```

> 我们也可以直接定义一个字典类型的body，通常指定了键和值的数据类型。

- #### 嵌套模型

  ```python
  from typing import List, Set
  from fastapi import FastAPI
  from pydantic import BaseModel, HttpUrl
  
  app = FastAPI()
  
  
  class Image(BaseModel):
      url: HttpUrl
      name: str
  
  
  class Item(BaseModel):
      name: str
      description: str = None
      price: float
      tax: float = None
      tags: Set[str] = []
      images: List[Image] = None
  
  
  @app.put("/items/{item_id}")
  async def update_item(*, item_id: int, item: Item):
      results = {"item_id": item_id, "item": item}
      return results
  ```

  > 格式：
  >
  > ```python
  > {
  >     "name": "Foo",
  >     "description": "The pretender",
  >     "price": 42.0,
  >     "tax": 3.2,
  >     "tags": [
  >         "rock",
  >         "metal",
  >         "bar"
  >     ],
  >     "images": [
  >         {
  >             "url": "http://example.com/baz.jpg",
  >             "name": "The Foo live"
  >         },
  >         {
  >             "url": "http://example.com/dave.jpg",
  >             "name": "The Baz"
  >         }
  >     ]
  > }
  > ```

- #### 深层嵌套模型

  ```python
  from typing import List, Set
  
  from fastapi import FastAPI
  from pydantic import BaseModel, HttpUrl
  
  app = FastAPI()
  
  
  class Image(BaseModel):
      url: HttpUrl
      name: str
  
  
  class Item(BaseModel):
      name: str
      description: str = None
      price: float
      tax: float = None
      tags: Set[str] = []
      images: List[Image] = None
  
  
  class Offer(BaseModel):
      name: str
      description: str = None
      price: float
      items: List[Item]
  
  
  @app.post("/offers/")
  async def create_offer(*, offer: Offer):
      return offer
  ```

  