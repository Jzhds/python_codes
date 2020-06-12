### Pandas

```python
import pandas as pd
s = pd.Series(np.random.randint(0,150,size = 100),index = np.arange(10,110),dtype=np.int16,name = 'Python')
s #在规定了index之后，就不能直接用相对位置来进行索引了，需要用已有的index 
比如 s[1]是错误的
s[::2]
s[::-1]  #反向
s.loc[10]
s.iloc[1]#可以用相对位置索引

df = pd.DataFrame(data = np.random.randint(0,150,size= (10,3)),index=list('ABCDEFHIJK'),columns=['Python','En','Math']) #注意要把size传上去

df
df['A':'D'] #显示从A行到D行的数据
df['A']#则报错，很神奇
df["Python"]
df["Python":"Math"] #只显示对应的列名
df.loc[['A','C']] 
df.iloc[0]


```

#### 检查是否有空值

```python
df.isnull().any() #对于每一列检查是否有空值，返回布尔值
for i in range(50):
    # 行索引
    index = np.random.randint(100,200,size =1)[0]

    cols = df.columns

    # 列索引
    col = np.random.choice(cols) #随机选择一个列表内的元素

    df.loc[index,col] = None
df.isnull().sum() #把每一列有多少个控制列举出来
#空值填补
df.fillna(value = 1) #用固定值填补
df1 = df.fillna(df.mean()) #用列均值填补,注意需要重新赋值一下，不然还是用空值的
df2 = df1.astype(np.int16) #重新规范一下类型，避免出现过多位小数
# 中位数填充
df4 = df.fillna(df.median())
df['Python'].unique() #独有的值，会包括空值
df['Python'].value_counts() #返回各个值对应的个数

#搞出来每一列中出现频率最高的值
fre = []
for col in df.columns:
    fre.append(df.col.value_counts().index[0])
sefre = pd.Series(fre,index = df.columns)
df.dropna() #有空值的行直接删除

```

#### 多层索引

```python
# 多层索引，两层，三层以上（规则一样）
s2 = pd.Series(np.random.randint(0,150,size = 6),index = pd.MultiIndex.from_product([['张三','李四','王五'],['期中','期末']]))
s2

df = pd.DataFrame(np.random.randint(0,150,size = (6,3)),columns=['Python','En','Math'],index =pd.MultiIndex.from_product([['张三','李四','王五'],['期中','期末']]) )

df
# 先获取列后获取行
df3['Python']['张三']['期中']
```

#### 多层索引计算

```python
# 多层列索引
df = DataFrame(np.random.randint(0,150,size = (6,6)),index = list('ABCDEF'),
               columns=pd.MultiIndex.from_product([['Python','En','Math'],['期中','期末']]))
df.mean().round(1)#保留一位小数
df.mean(axis = 1,level = 0) #1表示列，这就表示只对列的第一个层级求平均值
# 从列变成行
df2 = df.stack(level = 1)
df2
# 从行变成列
df2.unstack(level= 0 )
```

#### concat 和merge

```python
pd.concat([df1,df2],axis = 1) #沿着哪一个维度进行合并
df4.merge(df5,left_index=True,right_index=True)
```

#### 分组聚合

```python
df = pd.DataFrame({'Hand': ['right', 'left', 'left', 'right', 'right', 'right', 'right', 'right', 'left', 'right'],
                    'Smoke': ['yes', 'yes', 'no', 'no', 'yes', 'no', 'no', 'no', 'no', 'yes'],
                    'sex': ['male', 'female', 'female', 'male', 'male', 'male', 'female', 'female', 'male', 'female'],
                    'weight': [80, 50, 48, 75, 68, 100, 40, 90, 88, 76],
                    'IQ': [100, 120, 90, 130, 140, 80, 94, 110, 100, 160]})
 df
weight_mean = df.groupby('sex')[['weight','IQ']].mean()
#或者用apply表示
df.groupby(by = ['Hand'])[['weight']].apply(np.mean).round(1)
df2 = df.groupby(by = ['Hand'])[['weight']].transform(np.mean).round(1) #用分类的平均值进行转化替代
df2 = df2.add_suffix('_mean') #在名称上加上后缀
data = df.groupby(by = ['Hand'])['IQ','weight']
data.agg(['max','mean']).round(1) #agg的作用应该是对每一个都执行后面的函数，需要注意要加上引号，应该相当于apply的功能感觉
data.agg({'IQ':'max','weight':'mean'}).round(1)
pop2.drop(labels = 'abbreviation',axis = 1,inplace=True)

```

#### 排序与确定索引

```python
df.set_index(keys='',inplace = True)
df.sort_values(by="",ascending = False)#降序排列

```

#### Dataframe

#需要注意的是，list转化成dataframe的时候，和dict转化成dataframe不同，并不是有几个列表对应几列，而是对应的行数，在进行index和columns命名时，需要先转置