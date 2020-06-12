# python 字符串的一些常用操作

```python
import string
from string import Template
if __name__ == '__main__':
    li = "gaolinhang"
    li = li[:-2] + "k"
    a = "that is %d %s" %(1, "bird")
    printf(a)
    m = 3.14
    n = "%s" %m  # %s这个符号可以把所有的对象形态都转化成字符串
    s = "immediately"
    s1 = "ll".join(s.split("mm"))
    s2 = s1.replace("mm", "ll")
    s = 'The quick brown fox jumped over the lazy dog.'
    s3 = string.capwords(s)
    s4 = s.swapcase()  # 更改所有字母的大小写
    print(s4)
    intab = "abcd"
    outtab = "1234"
    str_trantab = str.maketrans(intab, outtab)  # 用于创建字符映射的转换表，第一个参数是字符串，是要转换的字符，第二个参数也是字符串，表示转换的目标
    test_str = "csdn blog:http://blog.csdn.net/wirelessqa"
    test_str1 = test_str.translate(str_trantab)
    p = Template("$x,glorious $x!")
    p1 = p.substitute(x="slurm")
    print(p1)
    str ='121212343245421'
    str1 = str.strip('12')  # 只能删除开始或者结尾的字符
    x = input('Enter an integer:')  # 输入字符串
    y = eval(input('Enter an integer:'))  # 输入数
    print("123", "456", sep='-')
    print(235, end="end")
    print(3423, end="end\n")
```



