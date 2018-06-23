## 数组中的最高排名数
### 题目level
* 6kyu

### 题目描述
* 写一个方法highestRank（arr）（或clojure中的最高级别），它返回给定输入数组（或ISeq）中最频繁的数字。 如果最频繁的号码有一个平局，则返回最大号码。

* 举例

```python
arr = [12, 10, 8, 12, 7, 6, 4, 10, 12];
highestRank(arr) //=> returns 12

arr = [12, 10, 8, 12, 7, 6, 4, 10, 12, 10];
highestRank(arr) //=> returns 12

arr = [12, 10, 8, 8, 3, 3, 3, 3, 2, 4, 10, 12, 10];
highestRank(arr) //=> returns 3
```

### 解题思路
* 1.将每个元素的出现次数放入字典s中
* 2.对字典s进行排序，找到出现次数最多的次数是多少。对
* 3.针对所有都出现了最多次数的键值放入一个新字典s_f
* 4.对字典s_f根据key排序(顺序)，取出现次数最高并且值最大的元素。


### 解题代码
* 我的代码
```python
import operator
def highest_rank(arr):
    s={}
    s_f={}
    print(set(arr))
    for i in set(arr):
        s[i]=arr.count(i)
    sorted_x = sorted(s.items(), key=operator.itemgetter(1))[-1]
    for k,v in s.items():
        if v==sorted_x[1]:
            s_f[k]=v
    sorted_y=sorted(s_f.items(), key=operator.itemgetter(0))[-1]
    return sorted_y[0]
```

* 优秀代码
```python
from collections import Counter

def highest_rank(arr):
    if arr:
        c = Counter(arr)
        m = max(c.values())
        return max(k for k,v in c.items() if v==m)
```

### 题目反思
#### 我的代码反思
* 1.字典排序
  * 排序依据为value:
  ```python
  sorted_x = sorted(s.items(), key=operator.itemgetter(1))
  ```
  * 排序依据为key:
  ```python
  sorted_y=sorted(s.items(), key=operator.itemgetter(0))
  ```
* 2.即使知道我的方法繁琐，依然不知道怎么化繁而简。

#### 优秀代码反思
##### 优秀代码思路
* 1.先将arr里面的元素根据出现次数，用Counter转换为字典
* 2.获取字典的value最大值
* 3.筛选出字典value=最大值value的键值对，选出key最大的元素。
* 4.按照优秀代码的思路，我对自己的代码进行了一些更改，还算比较满意😁:

```python
import operator
def highest_rank(arr):
    s={}
    for i in set(arr):
        s[i]=arr.count(i)
    m = max(s.values())
    return max(k for k, v in s.items() if v == m)
```


##### 新学知识点
**from collections import Counter**

计数器（Counter）是一个容器，用来跟踪值出现了多少次。

计数器支持三种形式的初始化。构造函数可以调用序列，包含key和计数的字典，或使用关键字参数。
* 1.1 创建类的4种方法
```python
from collections import Counter
Counter()
Counter(['a', 'b', 'c', 'a', 'b', 'b'])
Counter({'a':2, 'b':3, 'c':1})
Counter(a=2, b=3, c=1)
#输出结果
Counter()
Counter({'b': 3, 'a': 2, 'c': 1})
Counter({'b': 3, 'a': 2, 'c': 1})
Counter({'b': 3, 'a': 2, 'c': 1})
```
注意key的出现顺序是根据计数的从大到小

* 1.2 计数值的访问与缺失的键

当所访问的键不存在时，返回0，而不是KeyError；否则返回它的计数。

```python
c=Counter("abcdefabcd")
c["a"]
c["g"]
#输出结果
2
0
```
* 1.3 计数器的更新（update和subtract）

可以使用一个iterable对象或者另一个Counter对象来更新键值。

计数器的更新包括增加和减少两种。其中，增加使用update()方法

```python
c=Counter('which')
c.update('witch')
c['h']
#输出结果
3
```

减少则使用subtract()方法：

```python
c = Counter('which')
c.subtract('witch')  # 使用另一个iterable对象更新
c['h']
#输出结果
1
```
* 1.4 键的删除
当计数值为0时，并不意味着元素被删除，删除元素应当使用del。
```python
c = Counter("abcdcba")
c
#输出结果
Counter({'a': 2, 'c': 2, 'b': 2, 'd': 1})

c["b"] = 0
c
#输出结果
Counter({'a': 2, 'c': 2, 'd': 1, 'b': 0})

del c["a"]
c
#输出结果
Counter({'c': 2, 'b': 2, 'd': 1})
```
* 1.5 elements()

返回一个迭代器。元素被重复了多少次，在该迭代器中就包含多少个该元素。元素排列无确定顺序，个数小于1的元素不被包含。

```python
c = Counter(a=4, b=2, c=0, d=-2)
list(c.elements())
#输出结果
['a', 'a', 'a', 'a', 'b', 'b']
```

* 1.6 most_common([n])

返回一个TopN列表。如果n没有被指定，则返回所有元素。当多个元素计数值相同时，排列是无确定顺序的。

```python
c = Counter('abracadabra')
c.most_common()
#输出结果
[('a', 5), ('r', 2), ('b', 2), ('c', 1), ('d', 1)]

c.most_common(3)
#输出结果
[('a', 5), ('r', 2), ('b', 2)]
```
