### Python 100 days

#### 字符串常见处理与常见数据结构

```python
str1 = 'hello, world!'
# 通过内置函数len计算字符串的长度
print(len(str1)) # 13
# 获得字符串首字母大写的拷贝
print(str1.capitalize()) # Hello, world!
# 获得字符串每个单词首字母大写的拷贝
print(str1.title()) # Hello, World!
# 获得字符串变大写后的拷贝
print(str1.upper()) # HELLO, WORLD!
# 从字符串中查找子串所在位置
print(str1.find('or')) # 8
print(str1.find('shit')) # -1
# 与find类似但找不到子串时会引发异常
# print(str1.index('or'))
# print(str1.index('shit'))
# 检查字符串是否以指定的字符串开头
print(str1.startswith('He')) # False
print(str1.startswith('hel')) # True
# 检查字符串是否以指定的字符串结尾
print(str1.endswith('!')) # True
# 将字符串以指定的宽度居中并在两侧填充指定的字符
print(str1.center(50, '*'))
# 将字符串以指定的宽度靠右放置左侧填充指定的字符
print(str1.rjust(50, ' '))
str2 = 'abc123456'
# 检查字符串是否由数字构成
print(str2.isdigit())  # False
# 检查字符串是否以字母构成
print(str2.isalpha())  # False
# 检查字符串是否以数字和字母构成
print(str2.isalnum())  # True
str3 = '  jackfrued@126.com '
print(str3)
# 获得字符串修剪左右两侧空格之后的拷贝
print(str3.strip())
#按照格式输出
a, b = 5, 10
print(f'{a} * {b} = {a * b}')
f'{emp.name}: {emp.get_salary():.2f}元'
# 先通过成员运算判断元素是否在列表中，如果存在就删除该元素
if 3 in list1:
	list1.remove(3)
if 1234 in list1:
    list1.remove(1234)
print(list1) # [1, 400, 5, 7, 100, 200, 1000, 2000]
# 从指定的位置删除元素
list1.pop(0)
list1.pop(len(list1) - 1)
print(list1) # [400, 5, 7, 100, 200, 1000]
# 清空列表元素
list1.clear()

list1 = ['orange', 'apple', 'zoo', 'internationalization', 'blueberry']
list2 = sorted(list1)
# sorted函数返回列表排序后的拷贝不会修改传入的列表
# 通过生成器可以获取到数据但它不占用额外的空间存储数据
# 每次需要数据的时候就通过内部的运算得到数据(需要花费额外的时间)

# 创建集合的字面量语法
set1 = {1, 2, 3, 3, 3, 2}
print(set1)
print('Length =', len(set1))
# 创建集合的构造器语法(面向对象部分会进行详细讲解)
set2 = set(range(1, 10))
set3 = set((1, 2, 3, 3, 2, 1))
print(set2, set3)
# 创建集合的推导式语法(推导式也可以用于推导集合)
set4 = {num for num in range(1, 100) if num % 3 == 0 or num % 5 == 0}
print(set4)

set1.add(4)
set1.add(5)
set2.update([11, 12])
set2.discard(5)
if 4 in set2:
    set2.remove(4)
print(set1, set2)
print(set3.pop())
print(set3)

# 集合的交集、并集、差集、对称差运算
print(set1 & set2)
# print(set1.intersection(set2))
print(set1 | set2)
# print(set1.union(set2))
print(set1 - set2)
# print(set1.difference(set2))
print(set1 ^ set2)
# print(set1.symmetric_difference(set2))
# 判断子集和超集
print(set2 <= set1)
# print(set2.issubset(set1))
print(set3 <= set1)
# print(set3.issubset(set1))
print(set1 >= set2)
# print(set1.issuperset(set2))
print(set1 >= set3)
# print(set1.issuperset(set3))
```

#### 面对对象编程基础

封装，继承，多态

#### 文件与异常

通过Python内置的`open`函数，我们可以指定文件名、操作模式、编码信息等来获得操作文件的对象，接下来就可以对文件进行读写操作了。这里所说的操作模式是指要打开什么样的文件（字符文件还是二进制文件）以及做什么样的操作（读、写还是追加），具体的如下表所示。

| 操作模式 | 具体含义                         |
| -------- | -------------------------------- |
| `'r'`    | 读取 （默认）                    |
| `'w'`    | 写入（会先截断之前的内容）       |
| `'x'`    | 写入，如果文件已经存在会产生异常 |
| `'a'`    | 追加，将内容写入到已有文件的末尾 |
| `'b'`    | 二进制模式                       |
| `'t'`    | 文本模式（默认）                 |
| `'+'`    | 更新（既可以读又可以写）         |

**‘r’**：**只读**。该文件必须已存在。

**‘r+’**：**可读可写**。该文件必须已存在，写为追加在文件内容末尾。

**‘rb’**：表示以二进制方式读取文件。该文件必须已存在。

**‘w’**：**只写**。打开即默认创建一个新文件，如果文件已存在，则覆盖写（即文件内原始数据会被新写入的数据清空覆盖）。

**‘w+’**：**写读**。打开创建新文件并写入数据，如果文件已存在，则覆盖写。

**‘wb’**：表示以二进制写方式打开，只能写文件， 如果文件不存在，创建该文件；如果文件已存在，则覆盖写。

**‘a’**：**追加写**。若打开的是已有文件则直接对已有文件操作，若打开文件不存在则创建新文件，只能执行写（追加在后面），不能读。

**‘a+’**：**追加读写**。打开文件方式与写入方式和'a'一样，但是可以读。需注意的是你若刚用‘a+’打开一个文件，一般不能直接读取，因为此时光标已经是文件末尾，除非你把光标移动到初始位置或任意非末尾的位置。

代码示例：

```python
def main():
    f = None
    try:
        f = open('致橡树.txt', 'r', encoding='utf-8')
        print(f.read())
    except FileNotFoundError:
        print('无法打开指定的文件!')
    except LookupError:
        print('指定了未知的编码!')
    except UnicodeDecodeError:
        print('读取文件时解码错误!')
    else:
        f.close()


if __name__ == '__main__':
    main()
```

```python
def main():
    try:
        with open('致橡树.txt', 'r', encoding='utf-8') as f: #with 因有内置的__exit__函数可以自动释放资源
            print(f.read())
    except FileNotFoundError:
        print('无法打开指定的文件!')
    except LookupError:
        print('指定了未知的编码!')
    except UnicodeDecodeError:
        print('读取文件时解码错误!')


if __name__ == '__main__':
    main()
```

```python
#按行读取数据
def main():
    # 一次性读取整个文件内容
    with open('致橡树.txt', 'r', encoding='utf-8') as f:
        print(f.read())

    # 通过for-in循环逐行读取
    with open('致橡树.txt', mode='r') as f:
        for line in f:
            print(line, end='')
            time.sleep(0.5)
    print()

    # 读取文件按行读取到列表中
    with open('致橡树.txt') as f:
        lines = f.readlines()
    print(lines)
    

if __name__ == '__main__':
    main()
```

```python
#写文件
from math import sqrt


def is_prime(n):
    """判断素数的函数"""
    assert n > 0
    for factor in range(2, int(sqrt(n)) + 1):
        if n % factor == 0:
            return False
    return True if n != 1 else False


def main():
    filenames = ('a.txt', 'b.txt', 'c.txt')
    fs_list = []
    try:
        for filename in filenames:
            fs_list.append(open(filename, 'w', encoding='utf-8'))
        for number in range(1, 10000):
            if is_prime(number):
                if number < 100:
                    fs_list[0].write(str(number) + '\n')
                elif number < 1000:
                    fs_list[1].write(str(number) + '\n')
                else:
                    fs_list[2].write(str(number) + '\n')
    except IOError as ex:
        print(ex)
        print('写文件时发生错误!')
    finally:
        for fs in fs_list:
            fs.close()
    print('操作完成!')


if __name__ == '__main__':
    main()
```

```python
#读取json文件
import json
def main():
    mydict = {
        'name': '骆昊',
        'age': 38,
        'qq': 957658,
        'friends': ['王大锤', '白元芳'],
        'cars': [
            {'brand': 'BYD', 'max_speed': 180},
            {'brand': 'Audi', 'max_speed': 280},
            {'brand': 'Benz', 'max_speed': 320}
        ]
    }
    try:
        with open('data.json', 'w', encoding='utf-8') as fs:
            json.dump(mydict, fs)
    except IOError as e:
        print(e)
    print('保存数据完成!')


if __name__ == '__main__':
    main()
```

##### json的重要函数

- `dump` - 将Python对象按照JSON格式序列化到文件中
- `dumps` - 将Python对象处理成JSON格式的字符串
- `load` - 将文件中的JSON数据反序列化成对象
- `loads` - 将字符串的内容反序列化成Python对象

序列化：把程序中的数据结构转化成文件

反序列化：把文件中的数据转化成程序中的数据结构

#### 字符串与正则表达式

##### 常见表达：

|              |                                  |                  |                                                              |
| ------------ | -------------------------------- | ---------------- | ------------------------------------------------------------ |
| 符号         | 解释                             | 示例             | 说明                                                         |
| .            | 匹配任意字符                     | b.t              | 可以匹配bat / but / b#t / b1t等                              |
| \w           | 匹配字母/数字/下划线             | b\wt             | 可以匹配bat / b1t / b_t等 但不能匹配b#t                      |
| \s           | 匹配空白字符（包括\r、\n、\t等） | love\syou        | 可以匹配love you                                             |
| \d           | 匹配数字                         | \d\d             | 可以匹配01 / 23 / 99等                                       |
| \b           | 匹配单词的边界                   | \bThe\b          |                                                              |
| ^            | 匹配字符串的开始                 | ^The             | 可以匹配The开头的字符串                                      |
| $            | 匹配字符串的结束                 | .exe$            | 可以匹配.exe结尾的字符串                                     |
| \W           | 匹配非字母/数字/下划线           | b\Wt             | 可以匹配b#t / b@t等 但不能匹配but / b1t / b_t等              |
| \S           | 匹配非空白字符                   | love\Syou        | 可以匹配love#you等 但不能匹配love you                        |
| \D           | 匹配非数字                       | \d\D             | 可以匹配9a / 3# / 0F等                                       |
| \B           | 匹配非单词边界                   | \Bio\B           |                                                              |
| []           | 匹配来自字符集的任意单一字符     | [aeiou]          | 可以匹配任一元音字母字符                                     |
| [^]          | 匹配不在字符集中的任意单一字符   | [^aeiou]         | 可以匹配任一非元音字母字符                                   |
| *            | 匹配0次或多次                    | \w*              |                                                              |
| +            | 匹配1次或多次                    | \w+              |                                                              |
| ?            | 匹配0次或1次                     | \w?              |                                                              |
| {N}          | 匹配N次                          | \w{3}            |                                                              |
| {M,}         | 匹配至少M次                      | \w{3,}           |                                                              |
| {M,N}        | 匹配至少M次至多N次               | \w{3,6}          |                                                              |
| \            |                                  | 分支             | foo\                                                         |
| (?#)         | 注释                             |                  |                                                              |
| (exp)        | 匹配exp并捕获到自动命名的组中    |                  |                                                              |
| (?<name>exp) | 匹配exp并捕获到名为name的组中    |                  |                                                              |
| (?:exp)      | 匹配exp但是不捕获匹配的文本      |                  |                                                              |
| (?=exp)      | 匹配exp前面的位置                | \b\w+(?=ing)     | 可以匹配I'm dancing中的danc                                  |
| (?<=exp)     | 匹配exp后面的位置                | (?<=\bdanc)\w+\b | 可以匹配I love dancing and reading中的第一个ing              |
| (?!exp)      | 匹配后面不是exp的位置            |                  |                                                              |
| (?<!exp)     | 匹配前面不是exp的位置            |                  |                                                              |
| *?           | 重复任意次，但尽可能少重复       | a.*b a.*?b       | 将正则表达式应用于aabab，前者会匹配整个字符串aabab，后者会匹配aab和ab两个字符串 |
| +?           | 重复1次或多次，但尽可能少重复    |                  |                                                              |
| ??           | 重复0次或1次，但尽可能少重复     |                  |                                                              |
| {M,N}?       | 重复M到N次，但尽可能少重复       |                  |                                                              |
| {M,}?        | 重复M次以上，但尽可能少重复      |                  |                                                              |

| compile(pattern, flags=0)                    | 编译正则表达式返回正则表达式对象                             |
| -------------------------------------------- | ------------------------------------------------------------ |
| match(pattern, string, flags=0)              | 用正则表达式匹配字符串 成功返回匹配对象 否则返回None         |
| search(pattern, string, flags=0)             | 搜索字符串中第一次出现正则表达式的模式 成功返回匹配对象 否则返回None |
| split(pattern, string, maxsplit=0, flags=0)  | 用正则表达式指定的模式分隔符拆分字符串 返回列表              |
| sub(pattern, repl, string, count=0, flags=0) | 用指定的字符串替换原字符串中与正则表达式匹配的模式 可以用count指定替换的次数 |
| fullmatch(pattern, string, flags=0)          | match函数的完全匹配（从字符串开头到结尾）版本                |
| findall(pattern, string, flags=0)            | 查找字符串所有与正则表达式匹配的模式 返回字符串的列表        |
| finditer(pattern, string, flags=0)           | 查找字符串所有与正则表达式匹配的模式 返回一个迭代器          |
| purge()                                      | 清除隐式编译的正则表达式的缓存                               |
| re.I / re.IGNORECASE                         | 忽略大小写匹配标记                                           |
| re.M / re.MULTILINE                          | 多行匹配标记                                                 |

```python
#替换不良内容
def main():
    sentence = '你丫是傻叉吗? 我操你大爷的. Fuck you.'
    purified = re.sub('[操肏艹]|fuck|shit|傻[比屄逼叉缺吊屌]|煞笔',
                      '*', sentence, flags=re.IGNORECASE)
    print(purified)  # 你丫是*吗? 我*你大爷的. * you.
```

group 返回正则表达式匹配的字符串

#### 线程与进程

进程是操作系统中执行的一个程序，操作系统按照进程合理分配资源。

一个进程还可以拥有多个并发的执行线索，简单的说就是拥有多个可以获得CPU调度的执行单元，这就是所谓的线程。

Python实现并发编程主要有3种方式：多进程、多线程、多进程+多线程。

```python
#多进程实例
from multiprocessing import Process
from os import getpid
from random import randint
from time import time, sleep


def download_task(filename):
    print('启动下载进程，进程号[%d].' % getpid())
    print('开始下载%s...' % filename)
    time_to_download = randint(5, 10)
    sleep(time_to_download)
    print('%s下载完成! 耗费了%d秒' % (filename, time_to_download))


def main():
    start = time()
    p1 = Process(target=download_task, args=('Python从入门到住院.pdf', ))
    p1.start()
    p2 = Process(target=download_task, args=('Peking Hot.avi', ))
    p2.start()
    p1.join()
    p2.join()
    end = time()
    print('总共耗费了%.2f秒.' % (end - start))


if __name__ == '__main__':
    main()
```

```python
#多进程示例代码：
from multiprocessing import Process, Queue
from random import randint
from time import time


def task_handler(curr_list, result_queue):
    total = 0
    for number in curr_list:
        total += number
    result_queue.put(total)


def main():
    processes = []
    number_list = [x for x in range(1, 100000001)]
    result_queue = Queue()
    index = 0
    # 启动8个进程将数据切片后进行运算
    for _ in range(8):
        p = Process(target=task_handler,
                    args=(number_list[index:index + 12500000], result_queue))
        index += 12500000
        processes.append(p)
        p.start()
    # 开始记录所有进程执行完成花费的时间
    start = time()
    for p in processes:
        p.join()
    # 合并执行结果
    total = 0
    while not result_queue.empty():
        total += result_queue.get()
    print(total)
    end = time()
    print('Execution time: ', (end - start), 's', sep='')


if __name__ == '__main__':
    main()
```

异步I/O操作 -  用asyncio模块

**异步I/O**是计算机操作系统对[输入输出](https://zh.wikipedia.org/wiki/输入输出)的一种处理方式：发起I/O请求的线程不等I/O操作完成，就继续执行随后的代码，I/O结果用其他方式通知发起I/O请求的程序。

异步处理

```python
async def wget(host):
    print('wget %s...' % host)
    connect = asyncio.open_connection(host, 80)
    # 异步方式等待连接结果
    reader, writer = await connect
    header = 'GET / HTTP/1.0\r\nHost: %s\r\n\r\n' % host
    writer.write(header.encode('utf-8'))
    # 异步I/O方式执行写操作
    await writer.drain()
    while True:
        # 异步I/O方式执行读操作
        line = await reader.readline()
        if line == b'\r\n':
            break
        print('%s header > %s' % (host, line.decode('utf-8').rstrip()))
    writer.close()


loop = asyncio.get_event_loop()
# 通过生成式语法创建一个装了三个协程的列表
hosts_list = ['www.sina.com.cn', 'www.sohu.com', 'www.163.com']
tasks = [wget(host) for host in hosts_list]
# 下面的方法将异步I/O操作放入EventLoop直到执行完毕
loop.run_until_complete(asyncio.wait(tasks))
loop.close()
```

```python

#多线程操作示例
from random import randint
from time import time, sleep
import threading


class DownloadTask(threading.Thread):

    def __init__(self, filename):
        super().__init__()
        self._filename = filename

    def run(self):
        print('开始下载%s...' % self._filename)
        time_to_download = randint(5, 10)
        print('剩余时间%d秒.' % time_to_download)
        sleep(time_to_download)
        print('%s下载完成!' % self._filename)


def main():
    start = time()
    # 将多个下载任务放到多个线程中执行
    # 通过自定义的线程类创建线程对象 线程启动后会回调执行run方法
    thread1 = DownloadTask('Python从入门到住院.pdf')
    thread1.start()
    thread2 = DownloadTask('Peking Hot.avi')
    thread2.start()
    thread1.join()
    thread2.join()
    end = time()
    print('总共耗费了%.3f秒' % (end - start))
```



```python
#为了保证多线程进行时对公用数据的操作都能执行，需要加上锁
from threading import Thread, Lock
from time import sleep


class Account(object):

    def __init__(self):
        self._balance = 0
        self._lock = Lock()

    def deposit(self, money):
        # 先获取锁才能执行后续的代码
        self._lock.acquire()
        try:
            new_balance = self._balance + money
            sleep(0.01)
            self._balance = new_balance
        finally:
            # 这段代码放在finally中保证释放锁的操作一定要执行
            self._lock.release()

    @property
    def balance(self):
        return self._balance


class AddMoneyThread(Thread):

    def __init__(self, account, money):
        super().__init__()
        self._account = account
        self._money = money

    def run(self):
        self._account.deposit(self._money)


def main():
    account = Account()
    threads = []
    # 创建100个存款的线程向同一个账户中存钱
    for _ in range(100):
        t = AddMoneyThread(account, 1)
        threads.append(t)
        t.start()
    # 等所有存款的线程都执行完毕∫
    for t in threads:
        t.join()
    print('账户余额为: ￥%d元' % account.balance)


if __name__ == '__main__':
    main()
```

#### 网络编程入门与网络应用开发

TCP协议/IP协议分为四层：网络接口层、网络层、传输层、应用层

![image-20200818092847162](C:\Users\92306\AppData\Roaming\Typora\typora-user-images\image-20200818092847162.png)wb

```Python
from time import time
from threading import Thread

import requests


# 继承Thread类创建自定义的线程类
class DownloadHanlder(Thread):

    def __init__(self, url):
        super().__init__()
        self.url = url

    def run(self):
        filename = self.url[self.url.rfind('/') + 1:] # rfind找到后面的匹配符最后一次出现的位置
        resp = requests.get(self.url)
        with open('/Users/Hao/' + filename, 'wb') as f: # wb代表以二进制格式写数据
            f.write(resp.content)


def main():
    # 通过requests模块的get函数获取网络资源
    # 下面的代码中使用了天行数据接口提供的网络API
    # 要使用该数据接口需要在天行数据的网站上注册
    # 然后用自己的Key替换掉下面代码的中APIKey即可
    resp = requests.get(
        'http://api.tianapi.com/meinv/?key=APIKey&num=10')
    # 将服务器返回的JSON格式的数据解析为字典
    data_model = resp.json()
    for mm_dict in data_model['newslist']:
        url = mm_dict['picUrl']
        # 通过多线程的方式实现图片下载
        DownloadHanlder(url).start()


if __name__ == '__main__':
    main()
```

TCP套接字就是使用TCP协议提供的传输服务来实现网络通信的编程接口。

#### heapq 模块（堆排序）

```python
import heapq

list1 = [34, 25, 12, 99, 87, 63, 58, 78, 88, 92]
list2 = [
    {'name': 'IBM', 'shares': 100, 'price': 91.1},
    {'name': 'AAPL', 'shares': 50, 'price': 543.22},
    {'name': 'FB', 'shares': 200, 'price': 21.09},
    {'name': 'HPQ', 'shares': 35, 'price': 31.75},
    {'name': 'YHOO', 'shares': 45, 'price': 16.35},
    {'name': 'ACME', 'shares': 75, 'price': 115.65}
]
print(heapq.nlargest(3, list1))
print(heapq.nsmallest(3, list1))
print(heapq.nlargest(2, list2, key=lambda x: x['price']))
print(heapq.nlargest(2, list2, key=lambda x: x['shares']))
```

#### collections 模块

```python
找出序列中出现次数最多的元素
from collections import Counter

words = [
    'look', 'into', 'my', 'eyes', 'look', 'into', 'my', 'eyes',
    'the', 'eyes', 'the', 'eyes', 'the', 'eyes', 'not', 'around',
    'the', 'eyes', "don't", 'look', 'around', 'the', 'eyes',
    'look', 'into', 'my', 'eyes', "you're", 'under'
]
counter = Counter(words)
print(counter.most_common(3))
```

#### 数据结构与算法

暂时没细看

#### 元编程和元类

对象是通过类创建的，类是通过元类创建的，元类提供了创建类的元信息。所有的类都直接或间接的继承自`object`，所有的元类都直接或间接的继承自`type`。

#### 面向对象设计原则

- 单一职责原则 （**S**RP）- 一个类只做该做的事情（类的设计要高内聚）
- 开闭原则 （**O**CP）- 软件实体应该对扩展开发对修改关闭
- 依赖倒转原则（DIP）- 面向抽象编程（在弱类型语言中已经被弱化）
- 里氏替换原则（**L**SP） - 任何时候可以用子类对象替换掉父类对象
- 接口隔离原则（**I**SP）- 接口要小而专不要大而全（Python中没有接口的概念）
- 合成聚合复用原则（CARP） - 优先使用强关联关系而不是继承关系复用代码
- 最少知识原则（迪米特法则，Lo**D**）- 不要给没有必然联系的对象发消息

### 迭代器和生成器

迭代器就是实现了迭代器协议的对象，__iter__和__next__魔术方法就是迭代器协议。

```python
#以斐波那契数列为例：
class Fib(object):
    """迭代器"""
    
    def __init__(self, num):
        self.num = num
        self.a, self.b = 0, 1
        self.idx = 0
   
    def __iter__(self):
        return self

    def __next__(self):
        if self.idx < self.num:
            self.a, self.b = self.b, self.a + self.b
            self.idx += 1
            return self.a
        raise StopIteration()
```

生成器是语法简化的迭代器

```Python
def fib(num):
    """生成器"""
    a, b = 0, 1
    for _ in range(num):
        a, b = b, a + b
        yield a
```

协程：协程不是进程或线程，其执行过程更类似于子例程，或者说不带返回值的[函数调用](https://baike.baidu.com/item/函数调用/4127405)，协程简单的说就是可以相互协作的子程序。