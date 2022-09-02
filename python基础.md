[风变python基础知识库](https://docs.forchange.cn/docs/zdkyBjyGvoUybwA6)

# 基础概念：

### 1、变量

格式：变量名 (标识符） = 变量值

规定：

​	1、只能由数字，字母，_（下划线）组成

​	2、不能以数字开头

​	3、不能是关键字

​	4、严格区分大小写

### 2、数值类型

```
int整型   ：任意大小的整数
float浮点型： 通俗一点来讲就是小数
bool 布尔型：不能被随便定义，有固定写法
布尔值只有两个：一个为True（真），一个为False（假）
complex 复数型 ：它其实就是一个数学概念，用于数学计算，包含实部和虚部
固定写法  a+bj   a------实部     b-------虚部       j-------虚部
```

### 3、条件判断

比较运算符： "!="、">"、"<"、">="、"<="、"=="

布尔类型:用来表示条件是否成立，True 和 False，真和假

单分支if语句：一个 if 语句包含有五个要素：
        ① 关键词"if"；② "条件"；③ 英文冒号":"；④ 缩进；⑤ 代码块

多分支结构 if...elif...else... 语句
    条件判断语句里除了 if 和 else 语句外，还有一个 elif 语句，是 else if 的缩写。必须与 if 连用，实现分支判断【如果… 就…；如果… 就…】；

# 一、数据类型

**注：只有同类型的数据才能进行拼接**

### 1、字符串str

**常用方法：**

| 参数                               | 说明                                           | 示例                          |
| ---------------------------------- | ---------------------------------------------- | ----------------------------- |
| encode(encoding)                   | 将其他编码的字符转换成Unicode编码，默认'utf-8' | str.encoding('utf-8')         |
| decode()                           | 将Unicode编码转换成其他编码的字符串            | str.decode()                  |
| find                               | 检测字符是否包含在字符串中，返回下标           | str.find('九')                |
| replace(旧内容，新内容，替换次数） | 替换，默认全部替换                             | str.replace('九','9')         |
| split                              | 分割：指定分割符来切字符串                     | str.split('九')               |
| capitallize                        | 第一个字符大写                                 | str.capitallize()             |
| startswith                         | 是否以某字符开头，是的话返回为True             | str.startswith('九')          |
| endswith                           | 是否以某字符结束，是的话返回为True             | str.endswith('使')            |
| lower                              | 大写字符转为小写                               | str.lower()                   |
| upper                              | 小写转大写                                     | str.upper()                   |
| strip                              | 去除两边空格                                   | str.strip()                   |
| splitlines                         | 返回字符串列表,keepends默认False不返回换行符   | str.splitlines(keepends=True) |

### 2、列表list

**切片：list[start,end,步长]，包前不包后原则**

**2、1 添加元素**

| **append** | **整体添加**                         | **list.append(数据)**           |
| ---------- | ------------------------------------ | ------------------------------- |
| **extend** | **分散添加，将数据逐一添加到列表中** | **list.extend('九歌老师')**     |
| **insert** | **在指定下标前输入元素**             | **list.insret(下标位置，数据)** |

**2、2 修改元素**

通过下标修改：list[下标] = 值

**2、5 排序**

| sort        | 默认False升序          | list.sort(reverse=False) |
| ----------- | ---------------------- | ------------------------ |
| **reverse** | **将列表倒置，反过来** | **list.reverse()**       |

**2、6 列表推导式**

基本写法：[表达式  for 变量 in  列表]

```python
[list.append('e') for i in range(11)]
```

### 3、元组tuple

**元组属于不可变的数据类型，即一经创建就不可修改。**

**注：创建元组时，若只有一个元素，末尾要加逗号（，）**

### 4、字典dict

**字典中的键具备唯一性，值可以重复**

**语法：dict = {key1: value1, key2: value2, ... , keyn: valuen}**

**4、1 字典取值**

| dict[键名]                | 键不存在会报错                                         |
| ------------------------- | ------------------------------------------------------ |
| **dict.get(键名,默认值)** | **若键名不存在，返回默认值，如果没有默认值，返回None** |

**4、2 字典的基本操作**

| dict[键名] = 值       | 修改元素                               |
| --------------------- | -------------------------------------- |
| **dict[新键名] = 值** | **添加元素**                           |
| **del dict[键名]**    | **删除指定元素**                       |
| **clear**             | **清空整个字典**                       |
| **dict.pop(键名)**    | **删除指定键值对，不存在的话就会报错** |

**4、3 其它操作**

| **items**  | **返回字典里面包含所有键值对(元组)的列表** |
| ---------- | ------------------------------------------ |
| **keys**   | **返回字典里面包含所有键名的列表**         |
| **values** | **返回字典里面包含所有值的列表**           |

### 5、集合set

**格式：集合名 = {元素1,元素2...}**

**注意：集合具有唯一性和无序性**

**常用方法：**

| **add**     | **一次只能添加一个元素**                     | set.add(数据)              |
| :---------- | -------------------------------------------- | -------------------------- |
| **update**  | **把传入的元素拆分，一个个放到集合中，无序** | **set.update(可迭代对象)** |
| **discard** | **删除指定元素**                             | **set.discard(数据)**      |

```python
# 创建空集合
s1 = set()
```

### **6、类型转换**

| **int**       | **如果字符串中有数字和正负号以外的字符就会报错** |
| ------------- | ------------------------------------------------ |
| **float**     | **转换为一个小数**                               |
| **str**       | **转换为字符串**                                 |
| **eval(str)** | **用来执行一个字符串表达式，并返回表达式的值**   |
| **list**      | **将一个可迭代对象转换成列表**                   |
| **tuple**     | **将一个可迭代对象转换成元组**                   |
| **set**       | **先去重再转换成集合**                           |
| **dict**      | **列表转字典**                                   |
| **&**         | **交集，共有的部分**                             |
| **\|**        | **并集，所有的都放在一起**                       |

**可变对象：变量对应的值可以修改，但是内存地址保存不变，常见的可变类型：list，dict，set**

**不可变对象：变量对应的值不能被修改，常见类型：数值类型，字符串，元组**

### **7、深浅拷贝**

```python
import copy
# 浅拷贝，数据半共享
copy.copy(data)
# 深拷贝，完全不共享
copy.deepcopy(data) 
# 查看内存地址
id(data)
```

### **8、可迭代对象方法**

**常用方法：**

| 参数  | 说明                                                     | 示例             |
| ----- | -------------------------------------------------------- | ---------------- |
| index | 检测字符是否包含在可迭代对象中（返回下标），没找到会报错 | list.index(str)  |
| count | 返回出现的次数                                           | list.count(str)  |
| join  | 连接可迭代对象                                           | '+' . join(对象) |

**查找：**

| 参数   | 说明                 | 示例            |
| ------ | -------------------- | --------------- |
| in     | 如果存在返回True     | str  in  list   |
| not in | 如果不存在返回为True | str  not   list |

**删除数据（用于列表和集合）：**

| 参数   | 说明                                 | 示例         |
| ------ | ------------------------------------ | ------------ |
| pop    | 删除最后一个元素，并且返回该元素的值 | list.pop()   |
| del    | 清除变量（列表可以指定删除下标）     | del  变量    |
| clear  | 清空数据                             | list.clear() |
| remove | 选择删除的数据，没有就报错           |              |

### **9、格式化输出**

**9、1 占位符**

| **%s** | **字符串** |
| ------ | ---------- |
| **%d** | **整数**   |
| **%f** | **浮点数** |

```python
print('姓名:%s,年龄：%d,身高：%f' %('九歌',18,175.5))
```

**9、2 格式化f**

```python
age = 18
print(f'年龄是{age}')
```

**9、3 format**

```python
print('{0}最喜欢{1}'.format('老王','打游戏'))
```

### **10、转义字符**

| **\n**  | **换行符**         |
| ------- | ------------------ |
| **\r**  | **回车**           |
| **\\**  | **反斜杠符号**     |
| **\\'** | **单引号**         |
| **\\"** | **双引号**         |
| **\t**  | **横向制表符 tab** |

# **二、循环**

### **1、while循环**

**条件满足时做的事**

```python
# 无限循环
while True:
	print('无限循环')
```

**例子：**

```python
# 九九乘法表
# a  ------列       b------行         实际上就是a * b
a = 1     #设置列数的初始值为1
while a <= 9 : #一共九列   外循环
    b = 1      #设置行数的初始值为1
    while b <=  a  :   #  内循环
        print(f'{a} * {b} = {a * b}',end = '\t')   # 设置end的作用：末尾不换行
        b += 1
    print( )    # 括号里面空的作用-------换行
    a += 1
```

### **2、for循环**

**可以完成循环的功能，依次取出对象中的元素**

**语法：for    临时变量   in  可迭代对象：**

**range函数：range（开始，结束） 默认从0开始，包前不包后**

### **3、 其它**

**退出当前循环：continue，立即结束循环：break**

# **三、函数**

**概念：通过把代码打包起来使用，函数能有效减少重复性代码。**

```python
# 基本格式
def 函数名():         #函数名要符合标识符规定，最好见名知意，看到函数名就知道里面是什么功能
	函数体
   
# 调用函数
函数名()
```

**return:**

```python
1、return是返回值，当输入参数给函数，函数就会返回一个值，事实上每个函数都会有返回值。
2、如果不是立即要对函数返回值做操作，可以用return语句保留返回值。
3、若需要返回多个值，在return后用逗号隔开，此时返回的数据类型是一个元组。
```

### **1、参数与返回值**

**1、1 必备参数**

```python
def funa(name,name2):        # 写几个就要传几个参数，不可多也不可少
    print(name,name2)
funa('bingbing','suus')
```

**1、2 默认参数**

```python
# 设置了默认值，没有传值就根据默认值来执行代码，传了值根据传入的值执行代码
# 非默认参数要放到默认参数的前面，否则就会报语法错误
def funb(name = 'bingbing'):
	print(name)
funb()
```

**1、3 可变参数**

```python
# 传入的值数量可以改变，可以传多个，也可以不传
def funa(*args):           # *代表可以一个或多个，也可以们没有
    print(args)
funa('冰冰','苏苏',18)      # 可变参数接受形式为元组形式
```

**1、4 关键字参数**

```python
def funa(**kwargs):
    print(kwargs)
funa()               # 空字典
funa(name = 'bingbing',age = 18)     # 要传关键字参数，采用键等于值的形式
```

**1、5 返回值**

```python
# 函数执行结束后，最后给调用者一个结果
def funa():
    num = 20
    return num,'bingbing'    # 返回多个值是以元组的形式返回，return下面的代码不会执行，后面什么都没有，返回None
print(funa())
```

**附：函数嵌套**

```python
# def study():      # 外层函数
#     print('晚上在学习')
#     def eat():     # 内层函数
#         print('学习完之后再去干饭')
#     eat()
# study()
```

### **2、函数进阶**

| **global**   | **声明全局变量，可对多个进行声明** |
| ------------ | ---------------------------------- |
| **nonlocal** | **用来声明外层的局部变量**         |

**查看所有的内置函数**

```python
import builtins
print(dir(builtins))
```

**累积函数：作用：先把对象中的前两个元素取出，计算出一个值然后保存，接下来把这个值当作第三个元素进行计算**

```python
from functools import reduce

li = [1,2,3,4]
def add(x,y):        # 函数必须要是有两个参数的函数，对象必须是可迭代对象
    return x + y
res = reduce(add,li)
print(res)
```

**拆包：对于函数中的多个返回数据，去掉元组，列表或者字典直接获取里面数据的过程**

```python
tu = (1,2,3,4)
a,b,c,d = tu
print(a,b,c,d)

a,*b = tu
print(a)    # 1
print(b)    # [2,3,4]
```

### **3、函数总结**

##### **1、匿名函数**

**匿名函数：函数名 = lambda   形参 ：返回值** 

**调用 ：结果 =函数名（实参）**

**说明：冒号前是参数，可以有多个，用逗号隔开，冒号右边是返回值。**

```python
a = lambda x,y:x*y
print(a(3,4))      # 12
```

##### **2、sorted 排序函数**

**语法：sorted()**

**说明：主要用于对可迭代的对象进行排序操作**

| **参数**     | **说明**                             |
| ------------ | ------------------------------------ |
| **iterable** | **可迭代参数**                       |
| **key**      | **可迭代对象中用于比较排序的元素**   |
| **reverse**  | **用于设置排序规则，默认 False升序** |

```python
# 定义列表
students_age = [['john', 15], ['jane', 12], ['dave', 10]]

# 按年龄从大到小排序
result = sorted(students_age, key=lambda x: x[1],reverse = True)
print(result)
```

##### **3、enumerate 函数**

**语法：enumerate()**

**说明：用于遍历可迭代对象(如列表、元组或字符串)，在获取对象数据的同时，还能获取到对应计数的值**

| **参数**     | **说明**                              |
| ------------ | ------------------------------------- |
| **iterable** | **可迭代对象**                        |
| **start**    | **用于传入计数值，默认从 0 开始计数** |

```python
total_rows = [['陈洁', '销售七组', 10393, 815, 2993, 971, 1833, 889, 1128, 8629], ['刘波', '销售七组', 10133, 1496, 2667, 774, 1924, 315, 1142, 8318]]

# 对排序后的数据列表添加序号
# 使用两个参数进行接收，count：排序数字   row：遍历的数据
for count, row in enumerate(total_rows,start=1):
    print(count,row)
```

##### **4、数字计算函数**

**常用的函数：abs、sum、min、max等，只能对数字进行计算**

**语法：sum(可迭代对象)**

```python
print(sum[1,2,3])        # 6
```

##### **5、zip 拉链函数**

**语法：zip(iterables)**

**说明：函数将对象作为参数，将里面对应的元素打包成一个个元组，当最短的可迭代对象被输出时，该迭代器完成**

```python
x = [1,2,3,4]
y = ['张','三','李','四']
z = ['牛','鬼','蛇']

print(zip(x,y,z))               # 返回的<zip object at 0x00000171F363E7C8>对象
print(list(zip(x,y,z)))         # [(1, '张', '牛'), (2, '三', '鬼'), (3, '李', '蛇')]
for i in zip(x,y,z):
    print(i)              # (1, '张', '牛')
                          # (2, '三', '鬼')
                          # (3, '李', '蛇')
    
    
# *的作用是把上面生成的迭代器又再次拉链式的组合了该迭代器中的元素
print(list(zip(*zip(x,y,z))))     # [(1, 2, 3), ('张', '三', '李'), ('牛', '鬼', '蛇')]
```

##### **6、map 映射函数**

**语法：map(函数，可迭代对象)**

**说明：将对象中每一个元素来进行映射，分别执行函数**

```python
x = [1,2,3,4]

方法一：
def func(x):
    return x+5
print(map(func,x))     # <map object at 0x0000011EBFCDE7C8>
print(list(map(func,x)))          # [6, 7, 8, 9]

方法二：与匿名函数连用
print(list(map(lambda x:x+5,x)))    # [6,7,8,9]
```

### **4、递归函数**

**一个函数在内部不调用其他的函数，而是调用它本身**

```python
# 满足的条件：
1.必须有一个明确的结束条件
2.每进行更深一层的递归时，问题规模相比上次递归都要有所减少
3.相邻两次重复之间有紧密的联系

def funa(n):    # n = 4
    if n == 1:
        return 1
    return n + funa(n - 1)    # return 4+3+2+1
print(funa(4))
```

**斐波那契数列：这个数列从第三项开始，每一项都等于前两项之和**

```python
def fib(n):
    if n <= 1:
        return n
    return fib(n - 1) + fib(n-2)

print(fib(3))
```

# **四、异常与错误**

### **1、异常**

```python
# 捕获异常（异常处理）
# 语法格式一：
try：
  不确定能否正确运行的代码
except:    异常类型（可写可不写）
    出现异常现象的处理代码
except Exception as e:       # 万能异常，可以捕获任意异常
    print(e)
    print("遇到异常时执行的代码。")
else:
    print(没有捕获到异常执行的代码)
finally:          # 表示无论是否检测到异常都会执行的代码
    try代码块结束后运行的代码
```

**自定义异常类**

```python
class InputError(Exception):
    # 自定义异常类型的初始化
    def __init__(self,val):
        self.val = val

    # 返回异常类对象的描述信息
    def __str__(self):
        return f'{self.val} is invalid input'

try:
    raise InputError('骚操作')    # 抛出这个异常

except InputError as e:
    print('error是：',e)
```

### **2、报错类型**

**一、语法错误SyntaxError：**

```python
    第一种：SyntaxError: invalid syntax（无效语法）
    第二种：SyntaxError: invalid character in identifier（标识符中有无效字符）
    第三种：SyntaxError: EOL while scanning string literal（检查到不完整的字符串）
```

**二、缩进错误IndentationError：**

```python
    第一种：IndentationError: expected an indented block（需要缩进的代码块）
    第二种：IndentationError: unindent does not match any outer indentation level（缩进内容不匹配任何一个层级）
```

**三、类型错误TypeError：**

```python
    第一种：TypeError: unsupported operand type(s) for …（不支持的运算）
    第二种：TypeError: can only concatenate str (not "int") to str （只能用字符串拼接字符串）
    第三种：TypeError: 'xxx' object is not iterable（对象不可被迭代）
```

**其它错误：**

| **ModuleNotFoundError** | **未找到模块错误** |
| ----------------------- | ------------------ |
| **AttributeError**      | **属性错误**       |
| **FileNotFoundError**   | **文件未找到错误** |
| **UnicodeDecodeError**  | **编码格式错误**   |
| **IndexError**          | **索引错误**       |

# **五、包与闭包**

### **1、包**

**包含有_init_.py的文件夹，import导入包时，首先执行_init_.py文件的代码**

### **2、闭包**

**条件：**

```python
1.函数中嵌套一个函数  ---函数嵌套
2.内层嵌套函数对外部作用域有一个非全局变量的引用---内层函数用到了外层函数的变量
3.外层函数的返回值是内层函数的函数名 ---外层函数return了内层函数名
```

```python
def outer(m):
    n = 10
    def inner():
        print(n + m)   # 2.内层函数用到了外层函数的局部变量
    return inner        # 3.外层函数return了内层  函数名！！！
ot = outer(3)
ot()
```

### **3、回调函数**

```python
# 回调函数就是一个参数，把函数作为了一个参数传到了另外一个函数中
def test1(test):
    test()     # test()  相当于 调用了test2()
    print('这是test1')
def test2():
    print('test2')
test1(test2)
test2()
```

# **六、文件读写与模块**

### **1、常用方法**

| **read**      | **可以一次性读取文件的所有内容**                             |
| ------------- | ------------------------------------------------------------ |
| **write**     | **将指定内容写入文件**                                       |
| **close**     | **关闭文件**                                                 |
| **name**      | **返回文件的名称**                                           |
| **mode**      | **返回文件的访问模式**                                       |
| **closed**    | **查看文件是否关闭，关闭就返回True**                         |
| **readline**  | **一次读取一行内容，方法执行完，会把文件指针移到到下一行，准备再次读取** |
| **readlines** | **按照行的方式把文件内容一次性读取，返回是一个列表，每一行的数据就是一个元素** |

```python
# file：指定文件路径   mode：打开方式    encoding：指定编码格式
f = open(file='xxx.txt',mode='r',encoding='utf-8')
# 关闭
f.close()
```

### **2、with方法，自动关闭**

**访问模式：**

| **r**  | **只读模式，默认**                            |
| ------ | --------------------------------------------- |
| **w**  | **只写模式  ：不存在则创建，存在则重写**      |
| **a**  | **追加模式 ：不存在则创建，存在则只追加内容** |
| **+**  | **表示可以同时读写某个文件**                  |
| **r+** | **可读写文件，文件不存在抛出异常**            |
| **w+** | **先写再读**                                  |

```python
with open('path.txt','w',encoding='utf-8')as f:
	f.write('新年快乐！')
```

![img](C:\Users\王国成\Desktop\笔记\前端\images\3J4HXL9B1596180612019.png)

### **3、模块**

**模块：是一个 Python 文件，一个写好了代码、功能齐全的 Python 文件。**

**库：**

​	**1、标准库：python自带的**

​	**2、第三方库：第三方提供，需要下载**

```python
引入模块：import 模块名/库名
导入模块中的类语法：from 模块名 import 类名/方法名
```

# **七、面向对象**

### **1、装饰器**

**在不改变项目原有代码的基础上增加一些额外的功能**

**1、1 标准装饰器**

```python
def send():
    print('发送信息给冰冰：勤奋打工人')

def outer(fn):      # 外函数

    def inner():    # 内函数
        print('登录...')
        fn()
    return inner
ot = outer(send)     # 调用内函数
ot()
```

**1、2 语法糖**

```python
def outer(fn):   # fn等价于send
    def inner():
        fn()
        print('小明光速找到了对象')
    return inner
# 被装饰的函数
@outer
def send():
    print('小明快去在线征婚')
send()
```

### **2、类**

```python
# 三要素
1.类名      --   要遵循大驼峰命名法，最好是见名知意
2.属性  ： 对象的特征描述
3.方法  ：  对象具有的行为
```

**示例：**

```python
class Cat():
    def __init__(self):
        self.color = '棕色'
    # 定义方法
	def run(slef):
		print('我在跑步')
        
# 调用类
cat = Cat()
# 调用类方法与属性
cat.run()
cat.color
```

**2、1 创建类**

```python
lass Washier():
    height = 800         # 类属性：就是类所拥有的属性
    
	def Wash(self):      # 实例方法：由对象来调用，至少一个self参数，执行实例方法的时候，自动将调用该方法
        print('方法中的self：',self)     # self代表当前调用方法的对象
wa = Washier()     # 实例化一个对象
print(wa)          #  注意：打印实例化对象的时候，默认显示出对象在内存中的地址
wa.Wash()
```

**2、2 init方法**

```python
# 通常用来做属性初始化或赋值操作
# 注意：类被实例化的时候会自动执行__init__方法，也就是说实例化的时候会自动调用
class Test():
    def __init__(self,name,age,weight):
        self.name = name
        self.age = age
        self.weight = weight
        
te = Test('bingbing',18,45)
te.play()
```

**2、3 析构函数   del方法**

```python
# 当一个对象在内存中销毁前，就会自动调用__del__方法
# class Person:
#     def __init__(self):
#         print('我是__init__方法')
#     def __del__(self):
#         print('我是__del__，有我说明被销毁了')
# p = Person()
# del p
# print('我是最后一句话')
# print(124)
# 正常运行时不会调用__del__()
```

**2、4 增删改查：类名.属性名**

**2、5 私有属性/方法**

```python
# xxx：普通变量/方法，如果是类中定义的属性，则类对象可以在任何地方使用
# _xxx:单下划线开头，声明私有属性/方法，如果定义在类中，外部可以使用，子类也可以继承但是在另一个py文件中通过from xxx import * 导入时无法导入,这种命名主要是为了避免和python关键字冲突而采用的命名方法
# _xxx:双下划线开头，隐藏属性/方法，如果定义在类中，无法在外部直接使用，子类也不会继承，要访问只能通过间接的方式
# 在另一个py文件中通过from xxx import * 导入时无法导入，这种命名一般是python中的魔法属性或魔法方法，都是有特殊含义和特殊功能的

class Person:
    name = 'bingbing'    # 类属性
    __age = 18           # 隐藏属性
    _sex = '女'          # 私有属性
    def fn(self):
        print(Person.__age)
pe = Person()
print(pe.name)
pe.fn()
# print(pe.__age)
print(pe._sex)
```

**2、6 new方法**

```python
# 执行步骤
#     一个对象的实例化过程；先执行new方法，如果没有new方法，默认调用object里面的new方法，返回一个实例对象
#     然后再调用init方法，对这个对象进行初始化

class Human(object):
    def __init__(self,name):
        self.name = name    # 实例属性
        print('这是init中的self：',self)
        print('名字是：',self.name)

    def __new__(cls, *args, **kwargs):
        print('这是new中的cls:',cls)
        print('这是new方法')
        return super().__new__(cls)

print('这是类：',Human)
hu = Human('老王')
print('这是实例化的对象：',hu)

hu2 = Human('山不转水转')
print('这是第二次实例化的对象',hu2)
```

### **3、继承**

**语法：class 类名（父类名），可继承多个**

```python
class Person:         
    def eat(self):
        print('我会干饭')
 
# 继承
class Man(Person):    
    pass 
```

### **4、重写**

**概念：**

```python
重写指在子类中定义与父类同名的方法
1.覆盖父类方法
在子类中定义一个跟父类同名的方法，会调用子类重写的方法
2.对父类方法进行扩展
继承父类方法的同时子类可以增加自己的功能
```

**实现的方法：**

```python
# 1、父类名.方法名（self）
# 2、super().方法名     --- 推荐使用
# super是一个特殊的类，super（）是使用super创建出来的对象，可以调用父类的方法
# 3、super(子类名,self).方法名

class Person():
    def drive(self):
        print('骑自行车')
class Son(Person):
    def drive(self):
        super(Son,self).drive()
        # Person.drive(self)
        print('时代发展了，小汽车走起')
son = Son()
son.drive()
```

### **5、静态方法**

```python
# 没有self/cls参数限制        
                 # 注意 ：静态方法和类无关，可以将其转换成普通函数使用   
class 类名:
    @staticmethod
    def 方法名(形参):
        方法体
类名.方法名()
对象名.方法名()

class Person(object):
    @staticmethod
    def study(name):
        print(f'{name}会学习')
Person.study('小明')
pe = Person()
pe.study('susu')
```

### **6、单例模式（了解）**

```python
单例设计流程
1、定义一个类属性，初始值是None，用于记录单例对象的引用
2、重写new方法
3、进行判断，如果类属性是None，把new方法返回的对象引用保存进去
4、返回类属性中记录的对象引用
```

**通过new实现**

```python
class Eaxm(object):
    # 记录第一个被创建的对象引用
    ins = None   # 类属性

    def __init__(self):
        print('init方法')

    def __new__(cls, *args, **kwargs):
        # 1、判断类属性是否为空
        if cls.ins == None:
            # 调用父类的new方法，为第一个对象分配空间
            cls.ins = object.__new__(cls)

        return cls.ins
        
# 单例模式：每一次实例化所创建的实例都是同一个，内存地址都一样
a1 = Eaxm()
print(a1)
a2 = Eaxm()
print(a2)
a3 = Eaxm()
print(a3)
```

**通过装饰器实现**

```python
def outer(fn):        #  fn是被修饰的类名
    # 创建一个字典来保存类的实例对象
    ins = { }
    def inner():
        # 先判断这个类有没有对象
        if fn not in ins:
            # 创建一个对象，保存在字典中
            ins[fn] = fn()      # fn是类名，在ins中是键；fn()是实例化的对象在ins中是值；

        # 将实例对象返回
        return ins[fn]

    return inner

@outer          # 装饰器修饰的是Test类
class Test(object):
    pass

te = Test()
print(te)
te2 = Test()
print(te2)
```

# **八、线程**

```python
# 导入线程类
from threading import Thread
```

### **1、多线程**

**步骤：**

​	**1、创建子线程： Thread()** 

​	**2、守护线程   setDaemon()**

​	**3、开启线程   start()** 

​	**4、阻塞主线程  join()**

```python
线程类Thread参数：
        target: 执行的任务名
        args: 以元组的形式给执行任务传参
        kwargs： 以字典的形式给执行任务传参
```

```python
def funa():
    print('2020')
    time.sleep(2)
    print('结束了')

def funb():
    print('2021')
    time.sleep(2)
    print('开始了')

if __name__ == '__main__':
    # 1、创建子线程
    t1 = threading.Thread(target=funa)     # 任务名后不要加括号！！
    t2 = threading.Thread(target=funb)

    # 3、守护线程：主线程执行完了，子线程也会跟着结束,要放在start前面
    t1.setDaemon(True)
    t2.setDaemon(True)

    # 2、开启子线程
    t1.start()
    t2.start()

    # 4、join  阻塞主线程 ： 暂停的作用，等添加了join的子线程执行完，主线程才继续执行，要放在start后面
    t1.join()
    t2.join()

    # 5、修改名字
    t1.setName('线程一')
    t2.setName('线程二')

    # 6、获取线程名字
    print(t1.getName())
    print(t2.getName())
    print('这是主线程，程序的最后一行')
```

### **2、互斥锁**

**概念：保证多个线程访问共享数据不会出现数据错误问题：保证同一时刻只能有一个线程去操作**

**使用方法：threading模块里面定义了lock这个函数，通过调用这个函数可以获取到一把互斥锁**

**注意：这两个方法必须成对出现，acquire必须有release后才能 acquire()，不然会造成死锁**

```python
from threading import Thread,Lock
a = 0

# 1、创建全局互斥锁
lock = Lock()

# 循环一次给全局变量a加1
def jia():
    lock.acquire()         # 2、加锁
    for i in range(b):
        global a        # global 声明全局变量
        a += 1
    print('第一次:',a)
    lock.release()      # 3、 解锁


if __name__ == '__main__':
    first = Thread(target=jia)
    # 创建子线程
    first.start()
```

# **九、进程与协程**

### **1、进程概念**

**进程： 一个程序运行起来后，代码+ 用到的资源称之为进程，是操作系统分配资源的基本单位**

**进程的状态：**

​	**1、 就绪态： 正在执行CPU执行**

​	**2、 执行态： CPU正在执行其功能**

​	**3、 等待态： 等待某些条件满足** 

### **2、常用方法**

| **start()**    | **开启子进程**                                        |
| -------------- | ----------------------------------------------------- |
| **is_alive()** | **判断子进程是否还活着，存活返回True，否则返回False** |
| **join**       | **主进程等待子进程执行完**                            |

### **3、常用属性**

| **name** | **当前进程的别名**   |
| -------- | -------------------- |
| **pid**  | **当前进程的进程号** |

### **4、使用**

```python
# 导入模块
from multiprocessing import Process
import os

def one(name):
    print(f'{name}}子进程id{os.getpid()},父进程id{os.getppid()}')

def two(name2):
    print(f'{name2}子进程id{os.getpid()},父进程id{os.getppid()}')

if __name__ == '__main__':
    # 创建子进程
    p1 = Process(target=one,name='小白',args=('九歌',))    # 修改子进程方法一
    p2 = Process(target=two,args=('王康',))

    # 开启
    p1.start()
    p2.start()

    print(f'主进程id:{os.getpid()},父进程id:{os.getppid()}')
    # 查看进程命令：tasklist  (进入到命令提示符，输入tasklist后回车，找到pycharm64.exe)

    p2.name = '小额'        # 修改子进程名方法二

    print('p1的子进程名是：', p1.name)
    print('p2的子进程名是：', p2.name)

    # 查看子进程号
    print('p1的子进程号是：', p1.pid)
    print('p2的子进程号是：', p2.pid)

    # 查看进程是否还活着
    print('p1的状态是：', p1.is_alive())
    print('21的状态是：', p2.is_alive())
```

### **5、进程间的通信**

| **q.put()**   | **入队，即放入数据**           |
| ------------- | ------------------------------ |
| **q.get()**   | **出队，取出数据**             |
| **q.empty()** | **如果队列为空，返回True**     |
| **q.qsize()** | **返回当前队列包含的消息数量** |
| **q.full()**  | **如果队列满了，返回True**     |

```python
# 进程里面使用队列，直接使用进程模块自带的Queue
from multiprocessing import Process,Queue
import time

li = ['包子','面包','油条','河粉']
# 写入数据
def wdata(q1):
    for i in li:
        print(f'将早餐:{i}放进去')
        q1.put(i)
        time.sleep(0.2)

# 读取数据
def rdata(q2):
    # 只要还有消息，就一直取出来
    while True:
        #  判断是否为空，队列为空的话，就停止循环
        if q2.empty():       # 如果队列为空，返回True
            break
        # 不为空，就取出
        else:
            print('顾客从队列中获取到：', q2.get())


if __name__ == '__main__':
    # 创建队列对象
    q = Queue()        # 省略里面的参数，没有大小限制
    p1 = Process(target=wdata,args=(q,))
    p2 = Process(target=rdata,args=(q,))

    p1.start()
    p1.join()       # 等待队列运行完
    p2.start()
```

### **6、进程池**

```python
from multiprocessing import Pool
import time

def work(a):
    print('我们在上课')
    time.sleep(2)
    return a * 3

if __name__ == '__main__':
    # 定义一个进程池,最大进程数是3
    p = Pool(3)

    li = []
    for i in range(6):
        '''p.apply_async(调用的目标，传递的值)
           异步运行，根据进程池中有的进程数，每次最多三个子进程在异步执行
           执行完一个就释放一个进程，这个进程就可以去接受新的任务
        
           异步：进程不需要一直等下去，而是继续执行下面的操作，不管其他进程的状态     '''

        res = p.apply_async(work,args=(i,))

        li.append(res)


    # 关闭进程池
    p.close()
    # 等待p进程池中所有子进程执行完，必须放在close方法后面
    p.join()

    # 使用get来获取异步非阻塞的结果
    for i in li:
        print(i.get())
```

### **7、协程**

**概念 ： 是一个用C实现的协程模块，通过设置switch()来实现任意函数之间的切换**

**安装命令：pip install greenlet** 

```python
3、4  通过greenlet实现任务切换                   '''

# 导入模块
from greenlet import greenlet

def eat():
    print('开始吃夜宵')
    g2.switch()    # 切换到g2中运行
    print('吃饱了')

def study():
    print('开始学习了')
    print('学完协程了')
    g1.switch()

# 实例化一个协程对象        greenlet(任务名)
g1 = greenlet(eat)
g2 = greenlet(study)

# print
开始吃夜宵
开始学习了
学完协程了
吃饱了
```

### **8、gevent使用**

**创建协程对象：gevent.spawn(函数名)**

```python
# join: 阻塞，等待某个协程执行完毕
# joinall： 参数是一个协程对象列表，会等待所有的协程都执行完毕再退出

import gevent

def work(name):
    for i in range(3):
        gevent.sleep(1)
        print(f'函数名是：{name},i的值是：{i}')

gevent.joinall([gevent.spawn(work,'小白'),
                gevent.spawn(work,'大黑')])

# print
函数名是：小白,i的值是：0
函数名是：大黑,i的值是：0
函数名是：小白,i的值是：1
函数名是：大黑,i的值是：1
```

# **十、迭代器与生成器**

### **1、生成器**

**生成器函数：python中，使用了yield的函数被称为生成器（generator）**

**生成器表达式：类似于列表推导式**

```python
# 把列表推导式[]改成（）
li2 = (i*10 for i in range(3))   # 生成器表达式
print(li2)            # <generator object <genexpr> at 0x000001613D468B48>
```

​	**1、普通函数：返回值用return； 生成器函数使用yidls语句**

​	**2、yield语句一次返回一个结果，在每个结果中间，挂起函数，以便下次从它离开的地方继续执行**

​	**3、yield效果使函数中断，并保存中断的状态**

```python
#   生成器函数
def funb():
    print('----开始了----')
    yield 'a'     # 返回一个a，并暂停函数
    yield 'b'
    yield 'c'

fb = funb()     # 调用生成器函数，返回的是一个生成器对象
print(fb)
print(next(fb))
print(next(fb))
print(next(fb))
print(next(fb))

for i in fb:
    print(i)
```

### **2、迭代器**

**2、1 概念**

**可迭代对象：**

​	**1、对象实现了 iter 方法**

​	**2、iter 方法返回了迭代器对象**

**for 循环工作原理：**	

​	**1、在内部对可迭代对象调用iter 方法，获取到迭代器对象**

​	**2、再一次次的通过迭代器对象调用next方法获取迭代结果**

### **2、迭代器的使用**

**方法一：**

```python
li = ['迷糊', '海绵宝宝', '小白', '小额']
# 创造迭代器对象
name_li = iter(li)    # 1.通过iter()将函数变为迭代器
print(name_li)        # <list_iterator object at 0x000002BF32A7D9C8>

print(next(name_li))    # 2. 将获取到的迭代器对象不断地使用next（）函数获取下一条数据
print(next(name_li))
print(next(name_li))
print(next(name_li))    #  3. 取完元素后，再使用引发stopIteration异常
```

**方法二：**

```python
it =li. __iter__()
print(it. __next__())
```

**总结：**

​	**1、iter()调用该对象的 iter 方法，并把 iter 方法的返回结果作为自己的返回值**

​	**2、再使用next()函数来调用 next 方法**

​	**3、取完元素后， iter 方法将引发stopIteration异常**

### **3、可迭代对象和迭代器**

| **可迭代对象** | **iterable** | **凡是可以for循环的都属于可迭代对象** |
| -------------- | ------------ | ------------------------------------- |
| **迭代器**     | **iterator** | **凡是可以next  的都是迭代器**        |

```python
from collections.abc import Iterable,Iterator

name = '所以爱会消失对吗'
# 判断是否为可迭代对象和迭代器
print(isinstance(name,Iterable))      # True
print(isinstance(name,Iterator))      # False

name2 = iter(name)
print(isinstance(name2,Iterable))    # True
print(isinstance(name2,Iterator))    # True

# 迭代器对象一定是可迭代对象，而可迭代对象不一定是迭代器对象
```

**总结：**

​	**1、可迭代对象可以通过方法变成迭代器对象**

​	**2、如果一个对象拥有iter方法，是可迭代对象：如果一个对象拥有next方法，是迭代器对象**

​	**3、定义可迭代器对象，必须实现iter方法：定义迭代器，必须实现iter方法和next方法**

### **4、自定义迭代器类**

**条件：**

​	**1、有iter方法：返回迭代器对象本身(返回实例对象本身即可，self)**

​	**2、有next方法：返回容器的下一个元素或抛出stopIteration异常**

```python
# 自定义迭代器类
class Test2:
    def __iter__(self):       # 该方法返回的是迭代器对象
        self.n = 1
        return self           # 返回self，表示实例对象本身是自己的迭代器对象

    def __next__(self):
        self.n += 1
        return self.n

te2 = Test2()
print(te2)              # te2跟my的地址是一样的
print(next(te2))        # te2使用next方法报错是因为没有调用iter方法，里面的self.n实例属性没有生成
my = iter(te2)          # 调用__iter__ 方法
print(next(my))

for i in te2 :
    if i <= 10:
        print(i)
    else:
        break
```

