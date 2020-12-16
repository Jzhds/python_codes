### lPandas

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
df.loc[['A','C']]  #注意选择不连续的多行时，一定不要加后面的引号
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

long_data.index[long_data.T.isnull().all()] #搞出来全部值都是空值的行标签
rebased = px.apply(lambda x: x.loc[x.first_valid_index()]) #找到每列第一个非空的行的情况

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
# 获取某个index的信息
set(df_processed.index.get_level_values(0))
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
df.groupby('group')['value'].rank(ascending=False) #按照某一列进行聚合返回按照其他列进行排序的rank，这里代表的是降序
```

#### 排序与确定索引

```python
df.set_index(keys='',inplace = True)
df.sort_values(by="",ascending = False)#降序排列

```

#### Dataframe

#需要注意的是，list转化成dataframe的时候，和dict转化成dataframe不同，并不是有几个列表对应几列，而是对应的行数，在进行index和columns命名时，需要先转置

#### 添加新列

data['a'] = ''

#### 查看某一列第一个不是空值的对应列索引：

```python
for i in data.columns:
    print("{}:{}".format(i,data.index[data.loc[:,i].notnull()][0].strftime("%Y-%m-%d")))
```

#### 建立有列名的空数据框

```python
pd.DataFrame(columns = [""])
df.loc['a',:] = list1 # 添加新行
```

#### 查看dataframe中某列满足某个条件的index的情况

```python
player_data[player_data['age_start'] == min_enter_age].index
```

#### 去除重复列

```python
SP_500_data = SP_500_data.T.drop_duplicates().T
```

#### 按照某一列进行排序

```python
sort_values('val',ascending=False)
```

#### 两个数据框选择大的值进行填入，并用其中一个数据框填补缺失值

```python
df.where(df > df1, df1).fillna(df)
```

#### 返回按行的排序值

```python
df.rank(1, ascending=False, method='first')
```

#### 查看数据框各列的情况

```python
df2.dtypes
df.describe()
```

#### 选择

```python
df2[df2['E'].isin(['two', 'four'])]
pd.isna(df1) #看哪些位置里面是空值
```

#### apply

```python
df.apply(np.cumsum) #累加
df.apply(lambda x: x.max() - x.min())
```



temp_dataframe[y_data.isnull]  = np.nan

#### pivot table:转成分类的数据框

```python
pd.pivot_table(df, values='D', index=['A', 'B'], columns=['C'])
```

#### 转化成类别变量

```python
df["grade"] = df["raw_grade"].astype("category")
df["grade"].cat.categories = ["very good", "good", "very bad"]
```

```python
ts = ts.cumsum() #按列累加
```

#### 规定数据的类型

```python
s = pd.Series(['a', 2, np.nan], dtype="string")
#或者在series 或者 dataframe 创建后：
s = pd.Series(['a', 2, np.nan]).astype("string")
```

#### 对数据框执行字符串的一些操作

```python
s.str.lower() #需要添加str属性，对str属性进行操作
s2.str.split('_', expand=True) # 按照分隔符进行分割，并返回dataframe
```

```python
pat = r'[a-z]+'

In [55]: def repl(m):
   ....:     return m.group(0)[::-1] #group返回正则表达式匹配的值
   ....: 

In [56]: pd.Series(['foo 123', 'bar baz', np.nan],
   ....:           dtype="string").str.replace(pat, repl) #代替
```

https://pandas.pydata.org/docs/user_guide/text.html # 看到replace

#### shift()

shift() 取前面的数据，加上符号表示取后面的数据。

- 选择特定的列读入：usecols = [1,2]
- 重新设定参数：data.set_index("jk",inplace=True)
- index_col 不能取太大，一般取0，如果需要从某个特定区域开始读入的话，可以用usecols的方法，然后再重新设置Index

```

```

