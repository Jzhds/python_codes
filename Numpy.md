# Numpy

[nd.array.shape](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.shape.html)

[numpy.dot](https://docs.scipy.org/doc/numpy/reference/generated/numpy.dot.html)

np.zeros((a,b))

predictions = (A2>0.5) (如果原来的数小于0.5，赋值为0，若果大于0.5，赋值为1)

np.flatnonzero :Return indices that are non-zero in the flattened version of a.

例子：

```python
>>> x = np.arange(-2, 3)
    >>> x
    array([-2, -1,  0,  1,  2])
    >>> np.flatnonzero(x)
    array([0, 1, 3, 4])

    Use the indices of the non-zero elements as an index array to extract
    these elements:

    >>> x.ravel()[np.flatnonzero(x)]
    array([-2, -1,  1,  2])
 np.flatnonzero(y_train == y)
```

np.random.choice():Generates a random sample from a given 1-D array

```python
choice(a, size=None, replace=True, p=None)

        Generates a random sample from a given 1-D array

                .. versionadded:: 1.7.0

        Parameters
        -----------
        a : 1-D array-like or int
            If an ndarray, a random sample is generated from its elements.
            If an int, the random sample is generated as if a were np.arange(a)
        size : int or tuple of ints, optional
            Output shape.  If the given shape is, e.g., ``(m, n, k)``, then
            ``m * n * k`` samples are drawn.  Default is None, in which case a
            single value is returned.
```