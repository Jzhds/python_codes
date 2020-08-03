# Fluent python 学习

### 字典与集合

#### 字典创建的方式：

```python
>>> a = dict(one=1, two=2, three=3)
>>> b = {'one': 1, 'two': 2, 'three': 3}
>>> c = dict(zip(['one', 'two', 'three'], [1, 2, 3]))
>>> d = dict([('two', 2), ('one', 1), ('three', 3)])
>>> e = dict({'three': 3, 'one': 1, 'two': 2})
```

#### 字典推导：

```python
country_code = {country: code for code, country in DIAL_CODES} # 从列表推导到字典

```

#### 字典的映射方法：

![1580347055345](C:\Users\92306\AppData\Roaming\Typora\typora-user-images\1580347055345.png)

![1580347090729](C:\Users\92306\AppData\Roaming\Typora\typora-user-images\1580347090729.png)

![1580347156831](C:\Users\92306\AppData\Roaming\Typora\typora-user-images\1580347156831.png)

#### setdefault:

```python
my_dict.setdefault(key, []).append(new_value) #插入新值，不需要判断新的key是否在已知的dict的keys中

```

#### 映射的弹性健查询

##### defaultdict： 处理找不到的键的一个选择 

在用户创建 defaultdict 对象的时候，就需要给它配置一个为找不到的键创造默认值的方法。 

构造方法：

```python
import collections
index = collections.defaultdict(list)#里面的list也可以是其他类型
```

在这个例子中，如果k不在Index的keys中，那么index[k]就会返回一个空列表，但是index.get(k)结果是none

##### 特殊方法：__missing__

可以通过自定义类来实现，比如原来的字典中的键全都是字符串，我们想要实现当输入键时能够自动把它转成字符串，这样就更方便了

```python
class StrKeyDict0(dict):

    def __missing__(self, key):

       if isinstance(key, str):
          raise KeyError(key)
       return self[str(key)]


    def get(self, key, default=None):
       try:
          return self[key]
       except KeyError:
          return default

    def __contains__(self, key):
        return key in self.keys() or str(key) in self.keys() #这是检查是字典否有对应的键，如果没有的话是不会调用missing方法的

m = StrKeyDict0({'1':'sd',
                 '2':'dfs',
                 '3':'dfsa'})

print(m[3])
```

##### collections.Counter:

这个映射类型会给键准备一个整数计数器。每次更新一个键的时候都会增加这个计数器。所以这个类型可以用来给可散列表对象计数，或者是当成多重集来用——多重集合就是集合里的元素可以出现不止一次。 Counter 实现了 + 和 - 运算符用来合并记录，还有像 most_common([n]) 这类很有用的方法。  

```python
ct = collections.Counter('abracadabra')
```

##### 子类化UserDict:

UserDict就是让用户用来继承的基类，会比dict更方便。UserDict 并不是 dict 的子类，但是 UserDict 有一个叫作data 的属性，是 dict 的实例，这个属性实际上是 UserDict 最终存储数据的地方。这样做的好处是，比起示例 3-7， UserDict 的子类就能在实现 __setitem__ 的时候避免不必要的递归，也可以让 __contains__ 里的代码更简洁。 

```python
class StrKeyDict(collections.UserDict):
      def __missing__(self, key):
          if isinstance(key, str):
              raise KeyError(key)
          return self[str(key)]
      def __contains__(self, key):
         return str(key) in self.data
      def __setitem__(self, key, item):
         self.data[str(key)] = item  #在赋值的时候就已经把所有的键处理成了字符型
```

##### 不可变映射类型：

types 模块中引入了一个封装类名叫 MappingProxyType。如果给这个类一个映射，它会返回一个只读的映射视图。虽然是个只读视图，但是它是动态的。这意味着如果对原映射做出了改动，我们通过这个视图可以观察到，但是无法通过这个视图对原映射做出修改。 

```python
>>> from types import MappingProxyType
>>> d = {1:'A'}
>>> d_proxy = MappingProxyType(d)
>>> d_proxy
mappingproxy({1: 'A'})
>>> d_proxy[1] 
>>> d[2] = 'B'
>>> d_proxy ➌
mappingproxy({1: 'A', 2: 'B'})
>>> d_proxy[2]
'B'
```

>>> #### 集合论
>>>
>>> 集合中的元素必须是可散列的， set 类型本身是不可散列的，但是 frozenset 可以。 所以可以创建一个包含不同frozenset的set
>>>
>>> a | b 返回的是它们的合集， a & b 得到的是交集，而 a - b 得到的是差集 ,a^b得到的是对称差集，即是a或b中没有但对方集合中有的。这种中缀运算符需要两侧的被操作对象都是集合类型，但是其他的所
>>> 有方法则只要求所传入的参数是可迭代对象。例如，想求 4 个聚合类型 a、b、 c 和 d 的合集，可以用 a.union(b, c, d)，这里 a 必须是个 set，但是 b、c 和 d 则可以是任何类型的可迭代对象。 
>>>
>>> ```python
>>> found = len(set(needles) & set(haystack))
>>> # 另一种写法：
>>> found = len(set(needles).intersection(haystack))
>>> ```
>>>
>>> 如果要创建一个空集，你必须用不带任何参数的构造方法 set()。如果只是写成 {} 的形式，跟以前一样，你创建的其实是个空字典。 

![1580370220229](C:\Users\92306\AppData\Roaming\Typora\typora-user-images\1580370220229.png)

#### 文本和字节序列

```python
>>> s = 'café'
>>> len(s) # ➊
4
>>> b = s.encode('utf8') # ➋
>>> b
b'caf\xc3\xa9' # ➌
>>> len(b) # ➍
5
>>> b.decode('utf8') # ➎
'café'
```

>>> 把字节序列变成人类可读的文本字符串就是解码，而把字符串变成用于存储或传输的字节序列就是编码。 

```python
>>> cafe = bytes('café', encoding='utf_8') 
>>> cafe
b'caf\xc3\xa9'
>>> cafe[0] 
99
>>> cafe[:1] 
b'c'
>>> cafe_arr = bytearray(cafe)
>>> cafe_arr 
bytearray(b'caf\xc3\xa9')
>>> cafe_arr[-1:] 
bytearray(b'\xa9')
```

##### 识别编码方式的包：

统一字符编码侦测包 Chardet（https://pypi.python.org/pypi/chardet）就是这样工作的，它能识别所支持的 30 种编码。 Chardet 是一个 Python 库，可以在程序中使用，不过它也提供了命令行工具 chardetect。 

需要在多台设备中或多种场合下运行的代码，一定不能依赖默认编码。打开文件时始终应该明确传入 encoding= 参数，因为不同的设备使用的默认编码可能不同，有时隔一天也会发生变化。 

### 一等函数

可以把函数传给map函数，map 函数返回一个可迭代对象，里面的元素是把第一个参数（一个函数）应用到第二个参数（一个可迭代对象，这里是 range(11)）中各个元素上得到的结果。 

```python
def factorial(n):
    '''return n!'''
    if n<2:
        return 1
    return n*factorial(n-1)

m = factorial(5)
u = map(factorial, range(1,11))
print(list(u))
```

#### 高阶函数

接受函数为参数，或者把函数作为结果返回的函数是高阶函数（higher-order function） ，map函数就是其中一类

```python
>>> def reverse(word):
        return word[::-1] #返回反向排序
>>> reverse('testing')
'gnitset'
>>> sorted(fruits, key=reverse)
```

原本使用map和filter进行的工作基本上可以利用列表推导做到，更容易理解

```python
list(map(factorial,range(11)))
[factorial(n) for n in range(11)]
list(map(factorial,filter(lamda x:x%2,range(6))))
[factorial(n) for n in range(6) if n%2] #注意要先给出范围，然后再加限定条件，而不是先写限定条件
```

all(iterable)
如果 iterable 的每个元素都是真值，返回 True； all([]) 返回 True。
any(iterable)
只要 iterable 中有元素是真值，就返回 True； any([]) 返回 False。 

#### 可调用对象

除了用户定义的函数，调用运算符（即 ()）还可以应用到其他对象上。如果想判断对象能否调用，可以使用内置的 callable() 函数。  

可调用的对象包括：用户定义的函数，内置函数，内置方法，方法，类，类的实例（如果类定义了 __call__ 方法，那么它的实例可以作为函数调用），生成器函数（使用 yield 关键字的函数或方法。调用生成器函数返回的是生成器对象 ）

```python
import random
class BingoCage:
      def __init__(self, items):
           self._items = list(items)
           random.shuffle(self._items) #随机排序
      def pick(self):
          try:
             return self._items.pop()
          except IndexError:
             raise LookupError('pick from empty BingoCage')
      def __call__(self):
             return self.pick()

u = BingoCage(range(3))
m=u.pick()
l=u() #u因为类中有call属性，所以也可以调用
```

#### 函数内省

可以利用dir查看函数都有哪些属性

与用户定义的常规类一样，函数使用 __dict__ 属性存储赋予它的用户属性。

*和**用来展开迭代对象

```python
def tag(name, *content, cls=None, **attrs):
     """生成一个或多个HTML标签"""
     if cls is not None:
        attrs['class'] = cls
     if attrs:
        attr_str = ''.join(' %s="%s"' % (attr, value)
                      for attr, value
                      in sorted(attrs.items()))
     else:
        attr_str = ''
     if content:
       return '\n'.join('<%s%s>%s</%s>' %(name, attr_str, c, name) for c in content)
     else:
       return '<%s%s />' % (name, attr_str)


print(tag('br') ) #传入单个定位参数，生成一个指定名称的空标签。


print(tag('p', 'hello')) 

print(tag('p', 'hello', 'world')) #第一个参数后面的任意个参数会被 *content 捕获，存入一个元组。

print(tag('p', 'hello', id=33))
'<p id="33">hello</p>'
print(tag('p', 'hello', 'world', cls='sidebar')) #cls 参数只能作为关键字参数传入


print(tag(content='testing', name="img")) # 调用 tag 函数时，即便第一个定位参数也能作为关键字参数传入。

my_tag = {'name': 'img', 'title': 'Sunset Boulevard','src': 'sunset.jpg', 'cls': 'framed'} 
print(tag(**my_tag)) # 在 my_tag 前面加上 **，字典中的所有元素作为单个参数传入，同名键会绑定到对应的具名参数上，余下的则被 **attrs 捕获。
```

定义函数时若想指定仅限关键字参数，要把它们放到前面有 * 的参数后面。如果不想支持数量不定的定位参数，但是想支持仅限关键字参数，在签名中放一个 * ，如：

```python
def mul(a,*,b):
    return(a*b)
```

#### 获取关于参数的信息

有点复杂，以后有需要的话再看

#### 函数注解

函数声明中的各个参数可以在 : 之后增加注解表达式。 如果参数有默认值，注解放在参数名和 = 号之间。如果想注解返回值，在 ) 和函数声明末尾的 : 之间添加 -> 和一个表达式。那个表达式可以是任何类型。注解中最常用的类型是类（如 str 或 int）和字符串（如'int > 0'）。 

```python
def clip(text:str, max_len:'int >0'=80) -> str:
    """再max_len前面或者后面的第一个空格处截断文本"""
    end = None
    if len(text) > max_len:
        space_before = text.rfind(' ', 0, max_len) #Return the highest index in S where substring sub is found,找不到的话会返回-1
        if space_before >= 0:
            end = space_before
        else:
            space_after = text.rfind(' ', max_len)
            if space_after >= 0:
                end = space_after
    if end is None:
        end = len(text)
    return text[:end+1].rstrip() #Return a copy of the string with trailing whitespace removed.

m = clip('kjlfskdjf ijlk',5)
```

#### 支持函数式编程的包

#### operator 和 functools（没有仔细看）

### 使用一等函数实现设计模式

假如一个网店制定了下述折扣规则。
• 有 1000 或以上积分的顾客，每个订单享 5% 折扣。
• 同一订单中，单个商品的数量达到 20 个或以上，享 10% 折扣。 

• 订单中的不同商品达到 10 个或以上，享 7% 折扣。
简单起见，我们假定一个订单一次只能享用一个折扣。 

```python
from collections import namedtuple
Customer = namedtuple('Customer', 'name fidelity')

class LineItem:
    def __init__(self, product, quantity, price):
        self.product = product
        self.quantity = quantity
        self.price = price

    def total(self):
        return self.price*self.quantity

class Order:

    def __init__(self, customer, cart, promotion=None):
        self.customer = customer
        self.cart = list(cart)
        self.promotion = promotion

    def total(self):
        if not hasattr(self, '__total'): # Return whether the object has an attribute with the given name.
            self.__total =sum(item.total() for item in self.cart)
        return self.__total

    def due(self):
        if self.promotion is None:
            discount = 0
        else:
            discount = self.promotion(self)
        return self.total() - discount

    def __repr__(self):
        fmt = '<Order total:{:.2f} due: {:.2f}>'
        return fmt.format(self.total(), self.due()) # 固定格式

def fidelity_promo(order):
    return order.total() * .05 if order.customer.fidelity >= 1000 else 0

def bulk_item_promo(order):
    discount = 0
    for item in order.cart:
        if item.quantity >= 20:
            discount += item.total()*.1
    return discount

def large_order_promo(order):
    distinct_items = {item.product for item in order.cart}
    if len(distinct_items) >= 10:
        return order.total *.07
    return 0

promos = [fidelity_promo, bulk_item_promo, large_order_promo]
def best_promo(order):
    """选择可用的最佳折扣"""
    return max(promo(order) for promo in promos)
joe = Customer('John Doe', 0)
ann = Customer('Ann Smith', 1100)
cart = [LineItem('banana', 4, .5),
LineItem('apple', 10, 1.5),
LineItem('watermellon', 5, 5.0)]
m = Order(ann, cart, best_promo)
m
```

#### 函数装饰器与闭包

函数装饰器用于在源码中“标记”函数，以某种方式增强函数的行为。 

##### 装饰器基础知识

装饰器是一个可以调用的对象，参数是被装饰的函数

实例代码：

```python
@decorate
def target():
    print("target()")

def target():
    print("target()")
    
target = decorate(target)

# 两种表达方式相同
```

装饰器的作用： 通常是把被装饰的函数改成其他函数

##### 装饰器何时运行

通常是函数定义后立即执行，在调用模块时立即执行，被装饰的函数只有调用时才会生效

##### 装饰器的实际应用，用于之前策略的改进

```python
promotion = [] # 把所有的策略都存进promotion列表中，避免会遗漏新策略

def promotion(func):
    promotion.append(func)
    return func

@promotion
def fidelity_promo(order):
    return order.total() * .05 if order.customer.fidelity >= 1000 else 0
@promotion
def bulk_item_promo(order):
    discount = 0
    for item in order.cart:
        if item.quantity >= 20:
            discount += item.total()*.1
    return discount
@promotion
def large_order_promo(order):
    distinct_items = {item.product for item in order.cart}
    if len(distinct_items) >= 10:
        return order.total *.07
    return 0
      
    
```

#### 变量作用域规则

python不要求声明变量，但会把在函数中进行赋值的变量当作局部变量

如果想要把某个变量当成全局变量：需要在前面加上global

##### 闭包

闭包指延伸了作用域的函数，其中包含函数定义体中引用、但是不在定义体中定义的非全局变量。 

实例代码：

```python
class Averager():
    def __init__(self):
        self.series = []
    def __call__(self, new_value):
        self.series.append(new_value)
        total = sum(self.series)
        return total/len(self.series)

avg = Averager()
avg(10)
avg(11)

# 利用闭包和高阶函数实现

def make_averager():
    series = [] # 和下面的嵌套函数构成了一个闭包
    def average(new_value): 
        series.append(new_value) # series在这里作为自由变量
        total = sum(series)
        return total/len(series)
    return average
avg = make_averager()
avg(10)
avg(11)
```

对于函数中默认会把进行赋值的变量当作局部变量，对于一些不想当成局部变量，但又不是全局变量的变量可以用nonlocal进行声明

实例代码：用于计算函数运行的时间

```python
import time
import functools
def clock(func):
    @functools.wraps(func)
    def clocked(*args, **kwargs):
        t0 = time.time()
        result = func(*args, **kwargs)
        elapsed = time.time() - t0
        name = func.__name__
        arg_lst = []
        if args:
           arg_lst.append(', '.join(repr(arg) for arg in args))
        if kwargs:
           pairs = ['%s=%r' % (k, w) for k, w in sorted(kwargs.items())]
           arg_lst.append(', '.join(pairs))
        arg_str = ', '.join(arg_lst)
        print('[%0.8fs] %s(%s) -> %r ' % (elapsed, name, arg_str, result))
        return result
    return clocked

 
```

装饰器的通常特性：和被装饰的函数传入相同的参数，除了返回被装饰的函数的结果以外，也会返回一些额外值

##### 标准库中的装饰器

Python 内置了三个用于装饰方法的函数： property、 classmethod 和 staticmethod。 

使用@functools.lru_cache()  做备忘：可以把耗时最近利用的函数结果保存起来，避免重复计算，加括号的原因，是因为自己有自己的参数设置

单分派泛函数：使用 @singledispatch （from functools import singledispatch)装饰的普通函数会变成泛函数（generic function）：根据第一个参数的类型，以不同方式执行相同操作的一组函数。 (PS:这个没有太看懂，有需要再看吧)

#### 对象引用、可变性和垃圾回收

变量不是 盒子，而是标识，所以一个值可以有很多标识

==比较的是值，而is比较的是标识

每个变量都包括三个方面：标识，类型和值

函数，类的默认值不要用空的可变类型，如有需要，应该用none，否则可能出现共用的情况

把参数赋值给实例变量时一定要注意：这样会给参数赋给一个别名，如果不确定是否要对参数进行更改，尽量用创建副本的形式，或者用深复制

```python
import copy
a = [1,2,3]
b = list(a)
c =  copy.deepcopy(a)
print([id(a),id(b),id(c)])
```

#### 符合python风格的对象

repr():是针对开发者的，直接引用该变量时会显示

str():是针对用户的，写print时候会显示

```python
from array import array
import math


class Vector2d:
    typecode = 'd' # 是类属性

    def __init__(self, x, y):
        self.x = float(x)
        self.y = float(y)

    def __iter__(self):  # 变成可迭代对象
        return (i for i in (self.x, self.y))

    def __repr__(self):
        class_name = type(self).__name__
        return '{}({!r}, {!r})'.format(class_name, *self)

    def __str__(self):
        return str(tuple(self))

    def __bytes__(self):
        return (bytes([ord(self.typecode)]) +
                bytes(array(self.typecode, self)))

    def __eq__(self, other):
        return tuple(self) == tuple(other)

    def __abs__(self):
        return math.hypot(self.x, self.y)

    def __bool__(self):
        return bool(abs(self))

if __name__ == '__main__':
    a = Vector2d(1, 2)
    print(repr(a))

```

##### classmethod 和 staticmethod

classmethod是针对类的装饰器，staticmethod是针对静态方法或者说是函数的装饰器

##### 格式化显示

格式化显示可以有两种方法：format ()和 str.format()

format_spec是格式说明符， format(str, format_spec) ,或者是 str.format()中字符串{}中：后面的格式

```python
rate = 1
format(rate, '.2f')
'jfskdjf is {rate:.2f}'.format(rate=rate) # 代换字段中的 'rate' 子串是字段名称，与格式说明符无关，但是它决定把 .format() 的哪个参数传给代换字段。
m = 900009
print(isinstance(m, int))
j = "select * from table JK{}"
print(j.format(m))
k = "select * from table JK{code}"
print(k.format(code = m))
print("select * from table %s"%m
```

b 和 x 分别表示二进制和十六进制的 int 类型， f 表示小数形式的 float 类型，而 % 表示百分数形式： 

##### python的私有属性和“受保护”的属性

为了避免写类继承的时候，新的命名属性覆盖原来的类属性，可以把该属性前面加上两个下划线，后面加上一个或者不加下划线，这样该属性就变成了一个私有属性。在利用__dict__查看实例属性时，会返回__类名_属性名，这样写类继承时也不会覆盖掉

```python
class Vector2d:
    typecode = 'd' # 是类属性

    def __init__(self, x, y):
        self.__x = float(x)
        self.__y = float(y)

    @property
    def x(self):
        return self.__x

    @property
    def y(self):
        return self.__y
 
 
class Vector2d1(Vector2d):
    typecode = 'd' # 是类属性

    def x(self, m):
        self.x = m
        return m
d = Vector2d(1, 2)
print(d.__dict__)
d = Vector2d1(1, 2)
d.x(3)
print(d.__dict__)

{'_Vector2d__x': 1.0, '_Vector2d__y': 2.0}
1.0
{'_Vector2d__x': 1.0, '_Vector2d__y': 2.0, 'x': 3}
    

```

#### 协议

##### 协议定义

在面向对象编程中，协议是非正式的接口，只在文档中定义，而不在代码中定义。如序列定义只需实现__len__和____getitem____两种方法

接口，协议和抽象基类：接口实现特定角色的方法集合，抽象基类则是代码中明确了需要实现哪些具体的方法，协议是非正式的接口，只在文档中定义，而不在代码中定义。

继承抽象基类非常简单，只需要实现了对应的方法

接口是

可变序列类型需要实现：setitem

标准库中的抽象基类一般都在collections.abc中

#### 绝对路径

要用/，而不是windows中默认的\

或者在正常的路径前面加上r

#### numpy

1.np.newaxis:给现有的array增加一个维度

```python
a = np.arrange(5) #a形状是（5，）
a_new = a[:np.newaxis] #a_new的形状是（5，1）
b = np.arrange(3)
a_new + b #就会得到一个5*3的array,对于a_new中的每一个元素，都会依次加上b中的值
```

2.np.logspace：np.logspace(2.0, 3.0, num=4, base=2.0) 产生以base函数为底，指数项是从start到end的长度为num的序列

```python
>>> y = np.linspace(start, stop, num=num, endpoint=endpoint) #等价于一下两个函数
    ... # doctest: +SKIP
    >>> power(base, y).astype(dtype)
    ... # doctest: +SKIP
```



#### pandas

1.加入空白的新列：

  data['new'] = ''

data['new']=np.nan

#### plot

1.

```python
import matplotlib.pyplot as plt
import seaborn as sns
```

2.matplotlib 画散点图：

```python
plt.scatter(x,y,c='red')(c代表颜色)
```

3.sns画分类的图：

```python
sns.catplot(x=,y=,data=,hue=) (hue是分类依靠的类名)
```

#### sklearn学习

##### ols

```python
import matplotlib.pyplot as plt
import numpy as np
from sklearn import datasets, linear_model
from sklearn.metrics import mean_squared_error, r2_score
#划分训练集和测试集
regr = linear_model.LinearRegression()

regr.fit(diabetes_X_train, diabetes_y_train)
diabetes_y_pred = regr.predict(diabetes_X_test) #得到预测值
print('Coefficients: \n', regr.coef_)
# The mean squared error
print('Mean squared error: %.2f'
      % mean_squared_error(diabetes_y_test, diabetes_y_pred)) #得到均方误差
# The coefficient of determination: 1 is perfect prediction
print('Coefficient of determination: %.2f'
      % r2_score(diabetes_y_test, diabetes_y_pred)) #得到R^2

# Plot outputs
plt.scatter(diabetes_X_test, diabetes_y_test,  color='black')
plt.plot(diabetes_X_test, diabetes_y_pred, color='blue', linewidth=3)

plt.xticks(())
plt.yticks(())
plt.show()

```

##### ridge regression and classification

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn import linear_model

# X is the 10x10 Hilbert matrix
X = 1. / (np.arange(1, 11) + np.arange(0, 10)[:, np.newaxis])
y = np.ones(10)

# #############################################################################
# Compute paths

n_alphas = 200
alphas = np.logspace(-10, -2, n_alphas)

coefs = []
for a in alphas:
    ridge = linear_model.Ridge(alpha=a, fit_intercept=False)
    ridge.fit(X, y)
    coefs.append(ridge.coef_)
y_predict = ridge.predict(X)
print("Mean squared error is {}".format(mean_squared_error(y,y_predict)))
# #############################################################################
# Display results

ax = plt.gca()

ax.plot(alphas, coefs)
ax.set_xscale('log')
ax.set_xlim(ax.get_xlim()[::-1])  # reverse axis
plt.xlabel('alpha')
plt.ylabel('weights')
plt.title('Ridge coefficients as a function of the regularization')
plt.axis('tight')
plt.show()

```

### 继承的优缺点

#### 有多个同级的超类都有同名函数

可以在调用时规定使用哪一个超类的方法

```python
class A:
    def ping(self):
        print("Ping",self)
class B:
    def ping(self):
        print("ping",self)
class C(A,B):
d = C()
B.ping(d) #即表示用B中的方法进行操作
or
class C(A,B):
    def ping(self):
        A.ping(self)#而不是super().ping(self)
  

```

_mro_属性会自动规定超类函数的使用顺序，谁在前面就先使用谁的

总的来说多重继承没有什么太大的缺点，需要注意的是，当要继承str,dict,list这些超类时，这些由于是用C语言写的，所以一些相应的子类中的功能可能不会覆盖超类中的功能，可以考虑把UserDict,UserString,UserList作为要继承的超类

#### 运算符重载

~运算符代表：-（x+1) 对整数取反

##### Counter用法：返回后面可迭代对象的出现次数的统计的字典

```python
from collections import Counter
a = Counter("abcdfsdfa")

```

```python
def __add__(self, other):
     pairs = itertools.zip_longest(self, other, fillvalue=0.0) 
     return Vector(a + b for a, b in pairs) 
```

itertools.zip_longest在这里的作用：把self和other展开成可迭代对象，并把比较短的序列中的值用0代替。

常见的运算符：

![image-20200729084723874](https://i.loli.net/2020/07/29/tosORTdQgxKYnIu.png)

![image-20200729084848040](https://i.loli.net/2020/07/29/4NloLHAReJGcTqy.png)

#### 可迭代的对象，迭代器与生成器

检查序列a是不是迭代器的最好方法：

```python
isinstance(a,abc.Iterator)
```

可迭代的对象与迭代器的区别：

可迭代对象只需实现

```python
__iter__
```

功能,而迭代器除了该功能以外，还要实现next功能

生成器函数和普通函数的区别：生成器函数有yield

#### 上下文管理器和else

for + else: 当for 循环被完整执行，即没有遇到break时，执行else语句

try + else:当try 中没有遇到异常值是，执行else语句

else 在if 和 循环中不同的含义：在if中 表示如果不是之前的情况时执行，循环中是循环都被执行后执行esle语句

with 和上下文管理器：实现了上下文管理器中的 enter 和 exit内置功能

以读写文件为例：

```python
with open('a.txt') as fp:
     src = fp.read(60)
fp
```

在with使用结束后，可以 调用fp ，但不可以对fp再执行I/O操作 。