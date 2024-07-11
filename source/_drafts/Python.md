---
title: 《Python与数据科学》笔记
tags:
  - 笔记
categories: 学习笔记
mathjax: true
abbrlink: Note_Python
date: 2024-06-21 15:00:00
swiper_index:
description:
---
（正在施工中）

# 一、基础/语法
## 1.1. 标识符命名规则
- 由字母、数字、下划线组成
- 首字符必须是字母或下划线
- 大小写敏感
- 不能是Python关键词

## 1.2. 算符与算符优先级（从高到低）
[Python运算符优先级](https://docs.python.org/zh-cn/3/reference/expressions.html#operator-precedence)
- 小括号：()
- 索引/成员：obj[key], obj.attr
- 乘方：**
- 符号运算符：+, -, ~（按位取反）
- 乘除：*, @（矩阵乘法）, /（实数除法）, %, //（整数除法）
- 加减：+, -
- 移位：<<, >>
- 位运算：& > ^ > |
- 比较运算和成员检测：==, !=, >, <, >=, <=, is, is not, in, not in
- 布尔运算：not > and > or
- 条件表达式：x if c else y
- lambda表达式：lambda args: expression
- 赋值表达式：:=

# 二、控制结构
- Python用**缩进**控制代码的结构和从属关系
## 2.1. 条件语句 if-elif-else
```python
if condition1:
    statement1
elif condition2:
    statement2
else:
    statement3
```
## 2.2. for循环 for-else
- 遍历序列中的元素，具有else子句，当循环**正常结束**时执行
```python
for i in range(5):
    print(i)
else:
    prnt('done')
```
## 2.3. while循环 while-else
```python
while condition:
    statement1
else:
    statement2
```
## 2.4. break和continue
- break：跳出当前循环，不执行else
- continue：跳过当前循环的剩余语句，继续下一次循环
## 2.5. try-except-else-finally
- 作用：尝试执行可能出现异常的代码，并进行异常处理
- try语句块至少要有一个except或finally
```python
try:
    statement1 # 可能出现异常的代码
# 出现异常时，按except的顺序匹配异常类型，执行对应的处理语句
# 所有except和else中最多只有一个会被执行
except ValueError as ve: # ValueError异常处理
    statement2
except Exception as e: # 其他异常处理
    statement3 
# 没有触发except时执行
else:
    statement4 
# 无论是否有异常都会执行
finally:
    statement5 
```
## 2.6. pass
- 空语句，用于占位
## 2.7. 条件表达式
```python
x = ( 1 if condition else 0 )
```

# 三、函数和Lambda表达式
## 3.1. 函数
```python
def func(arg1, arg2: str='', *args, **kwargs):
    print(type(arg1), arg1)
    print(type(arg2), arg2)
    print(type(args), args)
    print(type(kwargs), kwargs)
    return None

func(1, 2, 3, 4, 5, key1='value1', key2='value2')
print()

args=[1,2,3,4,5,'value1','value2']
func(*args)
print()

args={
    'arg1': 1,
    'arg2': 2,
    'args': (3, 4, 5),
    'key1': 'value1',
    'key2': 'value2'
}
func(**args)
```

Out:
```
<class 'int'> 1
<class 'int'> 2
<class 'tuple'> (3, 4, 5)
<class 'dict'> {'key1': 'value1', 'key2': 'value2'}

<class 'int'> 1
<class 'int'> 2
<class 'tuple'> (3, 4, 5, 'value1', 'value2')
<class 'dict'> {}

<class 'int'> 1
<class 'int'> 2
<class 'tuple'> ()
<class 'dict'> {'args': (3, 4, 5), 'key1': 'value1', 'key2': 'value2'}
```
- 参数顺序：一般参数、带默认值的参数、可变参数`*args`、带关键字的可变参数`**kargs`
- 类型提示`arg2: str`：提示IDE参数的类型，但不强制要求，运行时不会检查
- 可变参数`*args`：接收任意数量的参数，类型为`tuple`
- 带关键字的可变参数`**kwargs`：接收任意数量的关键字参数，类型为`dict`
- 使用序列（`list`、`tuple`）传递参数，`*args`生效，接收多余参数，`**kwargs`不生效
- 使用字典（`dict`）传递参数，`**kwargs`生效，接收多余参数，`*args`不生效

## 3.2. Lambda表达式
- 具有函数的等价功能，用来定义简单的函数
- 可作为参数传递给map、filter、sort等函数
- 格式：lambda [参数]:[返回值的表达式]
```python
f = lambda x: (x**2 if x%2==0 else x**3, x+1)
print(f(3)) # (27, 4)
```

## 3.3. map函数
- 对序列中的每个元素执行函数，返回一个新的序列
- 要求函数具有1个参数
```python
ls = list(range(5))
ls = list(map(lambda x: x**2, ls)) # [0, 1, 4, 9, 16]
```

## 3.4. filter函数
- 对序列中的每个元素执行函数，返回一个新的序列，只保留返回值为True的元素
- 要求函数具有1个参数
```python
ls = list(range(5))
ls = list(filter(lambda x: x % 2 == 0, ls)) # [0, 2, 4]
```

## 3.5. reduce函数
- 按顺序取序列中的元素来执行函数，用结果和下一操作数继续执行函数，直到元素用完
- 要求函数具有2个参数
```python
from functools import reduce
ls = list(range(5))
ls = list(reduce(lambda x, y: x + y, ls)) # 10
```

# 四、数据结构
## 4.1. 列表list
- 可变的有序序列，元素之间用逗号分隔，用`[]`表示
```python
ls = [[1, 2, 3], 4, '5', [6, 7]] # 列表元素的类型可以不同
ls[index] # 索引，下标从0开始 
ls[0:-1:1] # 切片，默认step=1。step>0时，默认start=0，end=-1；step<0时反向切片，默认start=-1，end=0；setp不能为0
ls[::-1] # [[6, 7], '5', 4, [1, 2, 3]]: 反向切片，效果是列表逆序
ls.append(8) # 在列表末尾添加一个元素
ls.extend((9, 10)) # 使用序列在列表末尾添加多个元素
ls.insert(index, 0) # 在指定位置插入元素
ls.index(4, start, end) # 2: 返回元素4的索引，start和end是可选参数，表示查找区间。如果找不到会报ValueError
ls.count(4) # 1: 返回元素4的个数
ls.pop(-1) # (9, 10): 移除列表中指定位置的元素并将其返回。如果不指定位置，默认移除最后一个元素
ls.remove(4) # 移除列表中第一个出现的元素4，如果找不到会报ValueError
len(ls) # 7: 列表中元素的个数

ls = [x**2 for x in range(10) if x % 2] # [1, 9, 25, 49, 81]: 列表解析式
sum(ls) # 10: 列表中元素的算术和，要求列表中元素都支持实数“+”运算（不支持字符串拼接）

ls = ["lmsy","cc","z"]
max(ls) # z: 列表中元素的最大值，要求列表中元素之间都支持“>”运算（支持序列按字典序比较）
min(ls) # cc: 列表中元素的最小值，要求列表中元素之间都支持“<”运算（支持序列按字典序比较）

def mylen(s):
    return len(s)
ls.sort(key=mylen) # 将list按照字符串长度升序排序
sorted(ls, key=lambda x: len(x), reverse=True) # 不改变原list，返回list按照字符串长度降序排序后的结果
# key: 比较的关键词，默认为整体比较（序列按字典序），通过编写函数或lambda表达式实现自定义规则
# reverse: 默认w为False，表示升序；True表示降序
ls.reverse() # 将列表中的元素逆序
reversed(ls) # 不改变原list，返回列表中的元素逆序后的结果

"cc" in ls # True: 判断元素是否在列表中
"cc" not in ls # False: 判断元素是否不在列表中
[1, 2, 3]+[4, 5, 6] # [1, 2, 3, 4, 5, 6]: 列表拼接
[[1, 2, 3]]*2 # [[1, 2, 3], [1, 2, 3]]: 列表重复（元素引用赋值）
```

### 引用赋值、浅拷贝和深拷贝
```python
import copy
ls = [[1, 2, 3], 4, '5', [6, 7]]
ls1 = ls # 引用赋值（取别名）
ls2 = ls.copy() # 浅拷贝
ls3 = copy.deepcopy(ls) # 深拷贝

print(ls1 is ls) # True
print(ls2 is ls) # False
print(ls3 is ls) # False
print(ls2[0] is ls[0]) # True
print(ls3[0] is ls[0]) # False

ls[1] = 0 # 修改ls的元素
print(ls1) # [[1, 2, 3], 0, '5', [6, 7]]
print(ls2) # [[1, 2, 3], 4, '5', [6, 7]]
print(ls3) # [[1, 2, 3], 4, '5', [6, 7]]

ls[0][0] = 0 # 修改ls的list元素中的元素
print(ls1) # [[0, 2, 3], 0, '5', [6, 7]]
print(ls2) # [[0, 2, 3], 4, '5', [6, 7]]
print(ls3) # [[1, 2, 3], 4, '5', [6, 7]]
```

## 4.2. 元组tuple
- 不可变的有序序列，元素之间用逗号分隔，用`()`表示
```python
tp = 1, 2, 3, 4, 5 # 元组在赋值时可以省略括号
type(tp) # <class 'tuple'>
g = (x**2 for x in range(10) if x % 2) # 生成器表达式
next

# 索引和切片 √
# count, index √
# len, sum, max, min √
# sort, reverse × : 元组是不可变的，不能进行排序和逆序
# sorted, reversed √ : 返回一个list表示结果
# +, * √ : 返回一个新的tuple
# in, not in √
# copy ×, deepcopy × : 即使使用了deepcopy，也只是进行了引用赋值
```

## 4.3. 字符串str
- 不可变的有序字符序列，元素之间用逗号分隔，用`''`、`""`、`''' '''`、`""" """`表示
```python
s = 'heLLo, wOrlD!'
s.capitalize() # 'Hello, world!': 返回一个新的str，首字母大写，其余字母小写
s.lower() # 'hello, world!': 返回一个新的str，所有字母小写
s.upper() # 'HELLO, WORLD!': 返回一个新的str，所有字母大写

s.strip(None) # 'heLLo, wOrlD!': 返回一个新的str，去掉字符串两端的某些字符。None表示去掉空白字符
s.lstrip()
s.rstrip(' !,D') # 'heLLo, wOrl': 返回一个新的str，去掉字符串右端的某些字符。' !,D'表示去掉空格、感叹号、逗号和大写D

s.center(15,'*') # '*heLLo, wOrlD!*': 返回一个新的str，字符串居中，两侧填充字符，总长度为15
s.ljust(15,'*') # 'heLLo, wOrlD!**': 左对齐填充
s.rjust(15,'*') # '**heLLo, wOrlD!': 右对齐填充
s.zfill(15) # '000heLLo, wOrlD!': 右对齐填充0

s.replace(old, new, count=-1) # 'heLLo, world!': 返回一个新的str，将字符串中的old替换为new，从左往右最多替换count次。count默认为-1，表示不限制替换次数
s.split(sep = None, maxsplit = -1) # ['heLLo,', 'wOrlD!']: 返回一个list，表示以sep为分隔符，从左往右最多分割maxsplit次后的结果。sep默认为None，表示以空白字符为分隔符，maxsplit默认为-1，表示不限制分割次数
','.join(['heLLo', 'wOrlD']) # 'heLLo,wOrlD': 返回一个新的str，将序列中的元素以','连接起来

# 以下函数区间参数项可选
s.startwith('he', 0, -1) # True: 判断字符串是否以sub开头
s.endwith('zz', 0, -1) # False: 判断字符串是否以sub结尾
s.find('L', 0, -1) # 2: 返回sub在字符串中第一次出现的首字符下标，从左往右查找。如果sub不存在，返回-1
s.rfind('L', 0, -1) # 3: 返回sub在字符串中最后一次出现的首字符下标，从右往左查找。如果sub不存在，返回-1
s.index('L', 0, -1) # 2: 返回sub在字符串中第一次出现的首字符下标，从左往右查找。如果sub不存在，抛出ValueError异常
s.count('L', 0, -1) # 2: 返回sub在字符串中出现的次数，匹配之间不重叠

a=3.141592653589793
s = "pi = %8.2f" % (a) # 占位符
s = "{0:.2f} , {0:.2f}".format(3,4) # format
s = f"pi = {a:08.2f}" # f-string
# 占位符格式：[flags][width][.precision]type
# format格式：{[index]:[flags][width][.precision]type}
# type: f: float, d: int, s: string, u: unsigned, x: 16, o: 8
# flags: 
    # +: 正数显示正号, -: 左对齐, 0: 补0
    # #: 2进制b显示0b, 8进制o显示0o, 16进制x显示0x
    # 0<: 补0右对齐, 0>: 补0左对齐, 0^: 补0居中 （f-string支持）

# 具有元组的性质
```

## 4.4. 集合set
- 无序的不重复元素序列，元素之间用逗号分隔，用`{}`表示
- 具有可变集合`set`和不可变集合`frozenset`两种类型
```python
a={1,2,3,4,5}
b=set([2,1,1,1,3]) # [1,2,3]: 去重功能
c=a|b # 并集
a.union(b)
c=a&b # 交集
a.intersection(b)
c=a-b # 差集
a.difference(b)
c=a^b # 对称差集: (a|b)-(a&b)
a.symmetric_difference(b)
```

## 4.5. 字典dict
- 无序的键值对集合，键值对之间用逗号分隔，用`{key:value}`表示
```python
d = {'a':1, 'b':2, 'c':3}
d['a'] # 1: 获取键'a'对应的值
d.get('d', 0) # 0: 获取键'd'对应的值，如果键不存在，返回默认值0

# 以下返回值为序列型, 可转换为list
print(a.keys()) # dict_keys(['a', 'b', 'c']), a的key
print(a.values()) # dict_values([1, 2, 2]), a的value
print(a.items()) # dict_items([('a', 1), ('b', 2), ('c', 2)]), a的(key,value) 
```

# 五、NumPy
## 5.1. 数组
```python
import numpy as np

```