---
layout: post
title:  "Back to basics - Python"
date:  2021-01-30 12:50:00
categories: python
---
# Python Cheetsheet
### Numbers - int, float, complex
```python
year = 2021
type(year) # int
pi = 3.14
type(pi) # 3.14

# conversion
int(3.5) # 3
float(3) # 3.0

# some math op
float_num = 5 / 2 # returns a float
floored_div = 5 // 2 # returns a int
squared = 2 ** 2
mod = 5 % 2

# get divisor and modulas
div, mod = divmod(5, 2)

complex_num = 3 + 2j # (3 + 2j)
type(complex_num) # complex

# some functions
abs(-25)
round(2.5)
```

### Strings and Bytes
```python
# string can use single/double or docblock quote
name = "Python" + 'Python' + """Python"""
type(name) # str
binary = b"Python"
type(binary) # bytes
name == binary # false

# codepoint to str
chr(65) # A
type(chr(65)) # str
chr(240) # ð

# str functions
"python".upper() # PYTHON
"python".capitalize() # Python
" python ".strip() # python
("hello" "world") # helloworld
"hello"[0] # h
"hello world".split() # list [hello, world]
# python string are immutable!

# string/array slicing
"hello"[0] # h
"hello"[-1] # o
"hello"[:] # hello
"hello"[1:] # ello
"hello"[:-1] # hell 

# str formatting
"{lang} is awesome".format(lang="Python") # Python is awesome
lang = "Python"
f"{lang} is awesome" # Python is awesome

# strings are enumerable
upper = ""
for c in "hello":
    upper += c.upper()

# list comprehension on string
"".join([c.upper() for c in "hello"])

# byte conversion
"hello".encode() # b"hello"
"ð".encode() # conver into bytes b'\xc3\xb0'
len("ð") # 1
len("ð".encode()) # 2
```
### Collection - list, tuple, set, dict
```python
# list aka dynamic array
evens = [2, 4, 6]
evens.append(8) # [2, 4, 6, 8]
evens.remove(2) # [4, 6, 8]
evens.index(6) # 1
evens.extend([8, 10]) # [4, 6, 8, 8, 10]
evens.sort() # sort asc order
sorted(evens, reverse=True) # sort desc order

# looping a list
for val in range(5):
    print(val)

# looping list with index
for i, val in enumerate(range(5)):
    print(i, val)

# 2d array, 2x3 dimension
grid = [[1,2,3], [4,5,6]]
[0] * 3 # [0, 0, 0]
[[0, 1]] * 3 # [[0, 1], [0, 1], [0, 1]]

# jagged array
jagged = [[1], [1,2], [1,2,3]]

# tuple - immutable
immutable_list = tuple(2, 4, 6)

# set
distinct = set("AABBCCDD") # {'A', 'B', 'C', 'D'}

# conversion
a_list = list(range(5))
a_tup = tuple(a_list)
a_set = set(a_list)

# dict - map
hash_map = {"state": "GA", "post_code": 30342}
hash_map.keys() # dict_keys(['state', 'post_code'])
hash_map.values() # dict_values(['GA', 30342])
hash_map.get('state') # GA
hash_map.get('unknown_key', 'default_val') # default_val
hash_map.setdefault('foo', 'bar')
hash_map['foo'] = 'baz'

# looping a dict
for k, v in hash_map.items():
    print(k, v)
```

# List comprehension
```python
[x**2 for x in range(5)] # [0, 1, 4, 9, 16]
# set
{x**2 for x in range(5)} #  {0, 1, 4, 9, 16}
[key for key in hash_map.keys()] # ['state', 'post_code']
[val for val in hash_map.values()] # ['GA', 30342]
[item for item in hash_map.items()] # [('state', 'GA'), ('post_code', 30342)]
# dict
{key: key ** 2 for key in range(5)} # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```
# Dates - datetime, time, timedelta
```python
from datetime import date, datetime, timedelta
import time

# just the day
today = date.today()
# specific day 
dob = date(2000,1,1)
dob.isoformat() # '2000-01-01'
delta_days = today - dob # days
dt = datetime.fromisoformat('2011-11-04') # only python >= 3.7

# date diff using timedelta
yesterday = today - timedelta(days=1)
tomorrow = today + timedelta(days=1)

# datetime, it uses timezon
today = datetime.today()

# timestamp
timestamp = time.time()
today = date.fromtimestamp(timestamp)

# date formatting
today.strftime("%Y-%m-%d %H:%M:%S") # 2021-01-30 16:01:20
```
# Functional programming - itertools, functools
```python
# generator expressions - surrounded by parenthesis ()
iter = (line.strip() for line in ["hello world", "foo bar"])

# map, filter
list(map(lambda x: x.upper(), ["hello", "world"])) # ['HELLO', 'WORLD']
list(filter(lambda x: x % 2 == 0, range(10))) # [0, 2, 4, 6, 8]
# any, all
any([0,0,1]) # True
all([1,1,1]) # True

# itertools module
from itertools import count, cycle, repeat, takewhile, accumulate, groupby
import operator
# infinite iterator
# start counting from 100, 200, 300 ...
for i in count(100, 100):
    if i >= 1000:
        break
    print(i)

# takewhile 
list(takewhile(lambda x: x < 1000, count(100, 100)))

# cycle
for val in cycle("ABC"):
    print(val)

# repeat some numbers
repeat("ABC") # repeat infinitely
repeat("ABC", 3) # repeat 3 times

# running sum
list(accumulate(range(10)))

# group by - iterms need to be sorted!
{k: len(list(g)) for k, g in groupby('AABBBCCDEEE')} # {'A': 2, 'B': 3, 'C': 2, 'D': 1, 'E': 3}

# functools module - reduce
from functools import reduce
import operator
# sum up, init 0
reduce(operator.add, range(10), 0)
```