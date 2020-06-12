### python 杂记

[collections: 可以理解成定义一个简单的类](https://www.liaoxuefeng.com/wiki/897692888725344/973805065315456)

```python
>>> from collections import namedtuple
>>> Point = namedtuple('Point', ['x', 'y'])
>>> p = Point(1, 2)
>>> p.x
1
>>> p.y
2
```

 `namedtuple`是一个函数，它用来创建一个自定义的`tuple`对象，并且规定了`tuple`元素的个数，并可以用属性而不是索引来引用`tuple`的某个元素。 

[the difference between __repr__ and __str__](https://stackoverflow.com/questions/1436703/difference-between-str-and-repr)

### python 内置序列类型概览：

容器序列：

list、tuple、和collections.deque这些序列能存放不同类型的数据

扁平序列：

str,bytes,butearray,memoryview和array.array 这类序列只能容纳一种类型

容器序列存放的是它们所包含的任意类型的对象的引用，而扁平序列存放的是值而不是引用

元组的每个元素都存放了记录中一个字段的数据，外加这个字段的位置

#### 创建具名元组：（namedtuple)

1. 创建一个具名元组需要两个参数，一个是类名，另一个是类的各个字段的名字。后者可
   以是由数个字符串组成的可迭代对象，或者是由空格分隔开的字段名组成的字符串。
2. 存放在对应字段里的数据要以一串参数的形式传入到构造函数中（注意，元组的构造函
   数却只接受单一的可迭代对象）。
3. 你可以通过字段名或者位置来获取一个字段的信息。
4. 具名元组还有一些自己专有的属性。比如_fields 类属性、类方法_make(iterable) 和实例方法_asdict()。

```python
from collections import namedtuple

City = namedtuple('City', 'Name continent population')
tokyo = City('Tokyo',  'Asia', 35)
m = City._fields
delhi_data = ('Beijing',  'Asia', 35)
delhi = City._make(delhi_data) # 进行赋值
deldict = delhi._asdict() # 注意要有（）
deldict
for key, value in deldict.items(): # items后面要加（）
    print(key + ':' + str(value))
```

#### 列表和元组支持的具体操作：

![1577089148715](C:\Users\yingcheng_intern2\AppData\Roaming\Typora\typora-user-images\1577089148715.png)

![1577089129887](C:\Users\yingcheng_intern2\AppData\Roaming\Typora\typora-user-images\1577089129887.png)

#### Slice

切片区间长度 = stop - start

s[a: b:c] 的形式对s 在a 和b 之间以c 为间隔取值。c 的值还可以为负，负值意味着反向取值。

```

```

#### 清华镜像

pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pyspider

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple torch==1.5.0+cu92 torchvision==0.6.0+cu92 -f https://download.pytorch.org/whl/torch_stable.html
pip install torch==1.5.0+cu92 torchvision==0.6.0+cu92 -f https://download.pytorch.org/whl/torch_stable.html
```

数据框合并

```python
m1 = pd.merge(pct_change_50_dataframe,product1,left_on=pct_change_50_dataframe.index,right_on=product1.index)
合并后会产生一个key_0的一列
```

日期规范格式

```python
times = list(map(lambda x : x.strftime('%Y-%m-%d'),product1.index)) 
product1.index = times
```

```python

```

注意在读取xlsx中某个sheet时，也是从0开始计数的

dataframe重新命名列名

```python
m1.rename(columns={"key_0":"date"},inplace=True)
```

```python
a = a.dropna() #去除空值后记得重新赋值
```



回归：为了更好地查看回归结果，通常用sm，而不是sklearn

```python
import statsmodels.api as sm
x = sm.add_constant(x,prepend=False) #回归中添加常数
model = sm.OLS(y,x).fit()
print(model.summary())
```

list 不能取间隔的元素切片

排序用list.sort()

merge一定要写merge的条件

read_excel里面可以指定use_columns

数据框反向可以在后面添加.iloc[::-1]