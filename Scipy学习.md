### Scipy学习

#### 1.介绍

scipy是在numpy基础上编写的库，用于科学计算

#### 2.file io package

```python
import numpy as np
from scipy import io as sio

array1 = np.zeros((4, 5))
sio.savemat('example_mat', {'ar': array1})
data = sio.loadmat('example_mat', struct_as_record=True)
print(data['ar'])
```

#### 3.special function 

包含很多数值计算的功能



```python
#立方根
from scipy.special import *
m = cbrt(27)
#排列组合
#感觉它的组合函数有问题
m = perm(5,2,exact=True)
#Log Sum Exponential computes the log of sum exponential input element.
logsumexp(x) 


```

#### 4.linalg

用于矩阵计算

```python
from scipy import linalg
import numpy as np
#define square matrix
two_d_array = np.array([ [4,5], [3,2] ])
#pass values to det() function
linalg.det( two_d_array )#计算行列式
scipy.linalg.inv()#计算逆矩阵
#特征值与特征向量
arr = np.array([[5,4],[6,3]])
#pass value into function
eg_val, eg_vect = linalg.eig(arr)
#get eigenvalues
print(eg_val)
#get eigenvectors
print(eg_vect)
```

#### 5.Optimization and Fit in SciPy – scipy.optimize

```python
from scipy import optimize
def func1(x):
    return x**2 + 10*x
optimize.fmin_bfgs(func1,0)
optimize.basinhopping(func1, 0)
```

