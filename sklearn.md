#### back-propagation and forward-propagation

forward propagate to get the output and compare it with the real value to get the error 

to minimize the error, you propagate backwards by finding the derivative of error with respect to each weight and then subtracting this value from the weight value 

### sklearn:

simple logistic regression:

clf = sklearn.linear_model.LogisticRegressionCV()
clf.fit(X.T, Y.T) # 数据集的行数是数据样本点数

####  the general methodology to build a Neural Network 

```
1. Define the neural network structure ( # of input units,  # of hidden units, etc). 
2. Initialize the model's parameters
3. Loop:
    - Implement forward propagation
    - Compute loss
    - Implement backward propagation to get the gradients
    - Update parameters (gradient descent)
```

