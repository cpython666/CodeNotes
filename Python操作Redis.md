## **介绍**

`Redis`是一个开源的基于内存也可持久化的`Key-Value`数据库，采用`ANSI` C语言编写。它拥有丰富的数据结构，拥有事务功能，保证命令的原子性。由于是内存数据库，读写非常高速，可达`10w/s`的评率，所以一般应用于数据变化快、实时通讯、缓存等。但内存数据库通常要考虑机器的内存大小。

`Redis`有16个逻辑数据库（`db0-db15`），每个逻辑数据库项目是隔离的，默认使用`db0`数据库。若选择第2个数据库，通过命令 select 2 ，python中连接时可以指定数据库。

## **常用数据结构**

- `String`-字符串
- `List`-列表
- `Hash`-哈希
- `Set`-集合
- `ZSet`-有序集合
- `Bitmap`-位图

## python中安装redis模块包

```
pip install redis
```

## python连接redis

`redis` 提供两个类 `Redis` 和 `StrictRedis`, `StrictRedis` 用于实现大部分官方的命令，`Redis `是 `StrictRedis `的子类，用于向后兼用旧版本。
`redis` 取出的结果默认是字节，我们可以设定 `decode_responses=True` 改成字符串。

```python
import redis

host = 'localhost' # redis服务地址
port = 6379  # redis服务端口

r = redis.StrictRedis(host=host,port=port,db=0)
key = r.keys()
print(key)

r = redis.StrictRedis(host=host,port=port,db=0,decode_responses=True)
key = r.keys()
print(key)
#[b'proxies:universal']
#['proxies:universal']
```

**连接池**

redis 使用 connection pool 来管理对一个 redis server 的所有连接，避免每次建立、释放连接的开销。

默认，每个`Redis`实例都会维护一个自己的连接池。可以直接建立一个连接池，然后作为参数 `Redis`，这样就可以实现多个 `Redis `实例共享一个连接池。

```python
import redis

host = 'localhost' # redis服务地址
port = 6379  # redis服务端口

pool = redis.ConnectionPool(host=host,port=port,db=0,decode_responses=True)
r = redis.Redis(connection_pool=pool) # 或者用redis.StrictRedis

key = r.keys()
print(key)

#['proxies:universal']
```

## redis基本命令 - string

### set(name, value, ex=None, px=None, nx=False, xx=False)

在 `Redis` 中设置值，默认，不存在则创建，存在则修改。

参数：

ex - 过期时间（秒）
px - 过期时间（毫秒）
nx - 如果设置为True，则只有name不存在时，当前set操作才执行
xx - 如果设置为True，则只有name存在时，当前set操作才执行
ex - 过期时间（秒） 这里过期时间是3秒，3秒后，键 test 的值就变成None

```python
r.set('test','123456',ex=10)  # 设置键 test的值为123456，过期时间为3秒
print("test的值为：{}".format(r.get('test')))

time.sleep(10)
print("10秒后test的值为：{}".format(r.get('test')))

#test的值为：123456
#10秒后test的值为：None
```

**px - 过期时间（豪秒） 这里过期时间是3豪秒，3毫秒后，键foo的值就变成None**

```python
r.set('test','1234',px=3)  # 设置键 test 的值为1234，过期时间为3毫秒
print("test的值为：{}".format(r.get('test')))

time.sleep(0.003)
print("3毫秒后test的值为：{}".format(r.get('test')))

#test的值为：1234
#3毫秒后test的值为：None
```

**nx - 如果设置为True，则只有name不存在时，当前set操作才执行 （新建）**

```python
print("当前数据库中存在的键有：{}".format(r.keys()))
print("若test键不存在，则设置test的值为：123456，否则不执行")
r.set('test','123456',nx=True)  # 当test键不存在时，设置键 test 的值为123456
print("test的值为：{}".format(r.get('test')))

#当前数据库中存在的键有：['proxies:universal']
#若test键不存在，则设置test的值为：123456，否则不执行
#test的值为：123456
```

**xx - 如果设置为True，则只有name存在时，当前set操作才执行 （修改）**

```python
print("当前数据库中存在的键有：{}".format(r.keys()))
print("若test键存在，则修改test的值为：123456，否则不执行")
r.set('test','123456',xx=True)  # 当test键存在时，修改键 test 的值为123456
print("test的值为：{}".format(r.get('test')))

#当前数据库中存在的键有：['proxies:universal', 'test']
#若test键存在，则修改test的值为：123456，否则不执行
#test的值为：123456
```

### setnx(name, value)

设置值，只有name不存在时，执行设置操作（添加）

### setex(name, time, value)

设置值，time - 过期时间（数字秒 或 timedelta对象）

### psetex(name, time_ms, value)

设置值，time_ms - 过期时间（数字毫秒 或 timedelta对象）

### mset 和 mget

- `mset(*args, **kwargs)`：批量设置值
- `mget(keys, *args)`：批量获取值

```python
import redis
import time

host = 'localhost' # redis服务地址
port = 6379  # redis服务端口

pool = redis.ConnectionPool(host=host,port=port,db=0,decode_responses=True)
r = redis.Redis(connection_pool=pool) # 或者用redis.StrictRedis

print("当前数据库中存在的键有：{}".format(r.keys()))

print("设置键 k1和k2的值，分别为v1和v2")
r.mset({'k1': 'v1', 'k2': 'v2'})
print("获取键k1和k2的值")
print(r.mget("k1", "k2"))   # 一次取出多个键对应的值

#当前数据库中存在的键有：['proxies:universal', 'test']
#设置键 k1和k2的值，分别为v1和v2
#获取键k1和k2的值
#['v1', 'v2']
```

### `getset(name, value)`

设置新值并获取原来的值

```python
import redis
import time

host = '192.168.149.153' # redis服务地址
port = 6379  # redis服务端口

pool = redis.ConnectionPool(host=host,port=port,db=0,decode_responses=True)
r = redis.Redis(connection_pool=pool) # 或者用redis.StrictRedis

print("当前数据库中存在的键有：{}".format(r.keys()))

print("获取键test的值为：{}".format(r.get('test')))

result = r.getset('test','12345')  # 设置键test的新值为12345，并返回原test的值1234
print("键test的原值为：{}".format(result))
print("获取键test的当前的值为：{}".format(r.get('test')))

#当前数据库中存在的键有：['k2', 'proxies:universal', 'test', 'k1']
#获取键test的值为：123456
#键test的原值为：123456
#获取键test的当前的值为：12345
```

### `getrange(key, start, end)`

获取子序列（根据索引、字节获取）一个汉字占3个字节

参数：

- start - 起始位置（起始索引从0开始）
- end - 结束位置

```python
import redis
import time

host = 'localhost' # redis服务地址
port = 6379  # redis服务端口

pool = redis.ConnectionPool(host=host,port=port,db=0,decode_responses=True)
r = redis.Redis(connection_pool=pool) # 或者用redis.StrictRedis

print("当前数据库中存在的键有：{}".format(r.keys()))
print("获取键test的值为：{}".format(r.get('test')))

result = r.getrange('test',0,1)  # 获取索引0到1的子序列
print("键test的0到1的子序列为：{}".format(result))

result = r.getrange('test',1,4)  # 获取索引1到4的子序列
print("键test的1到4的子序列为：{}".format(result))

#当前数据库中存在的键有：['test', 'k2', 'proxies:universal', 'k1']
#获取键test的值为：12345
#键test的0到1的子序列为：12
#键test的1到4的子序列为：2345
```

```python
import redis
import time

host = 'localhost' # redis服务地址
port = 6379  # redis服务端口

pool = redis.ConnectionPool(host=host,port=port,db=0,decode_responses=True)
r = redis.Redis(connection_pool=pool) # 或者用redis.StrictRedis

print("当前数据库中存在的键有：{}".format(r.keys()))
print("获取键name的值为：{}".format(r.get('name')))

result = r.getrange('name',0,2)  # 获取索引0到2的子序列
print("键name的0到2的子序列为：{}".format(result))

result = r.getrange('name',3,20)  # 获取索引3到20的子序列
print("键name的3到20的子序列为：{}".format(result))

#当前数据库中存在的键有：['test', 'k2', 'proxies:universal', 'name', 'k1']
#获取键name的值为：我们都是大帅哥
#键name的0到2的子序列为：我
#键name的3到20的子序列为：们都是大帅哥
```

***注意：中文一个汉字占3个字节，如果取0到1的子序列，则会报错（不到一个汉字）***

```python
r.getrange('name',1,2)

#UnicodeDecodeError: 'utf-8' codec can't decode byte 0x88 in position 0: invalid start byte
```

### setrange(name, offset, value)

修改字符串内容，从指定字符串索引开始向后替换（新值太长时，则向后添加）

参数：

- offset - 字符串的索引，字节（一个汉字三个字节）
- value - 要设置的值

长度需要匹配，操作不匹配报错，再获取值也会报错

```python
print("当前数据库中存在的键有：{}".format(r.keys()))
print("获取键name的值为：{}".format(r.get('name')))

print('将name的第一个字节开始的值改为“我”，占3个字节')
r.setrange('name',0,'我')  #
print("键name的值修改为：{}".format(r.get('name')))

#当前数据库中存在的键有：['test', 'k2', 'proxies:universal', 'name', 'k1']
#获取键name的值为：我们都是大帅哥
#将name的第一个字节开始的值改为“我”，占3个字节
#键name的值修改为：我们都是大帅哥
```

```python
r.setrange('name',0,123456)  #
print("键name的值修改为：{}".format(r.get('name')))

#键name的值修改为：123456都是大帅哥
```

```python
r.setrange('name',6,12)  
print("键name的值修改为：{}".format(r.get('name')))

#UnicodeDecodeError: 'utf-8' codec can't decode byte 0xbd in position 8: invalid start byte
```

### strlen(name)

返回name对应值的字节长度（一个汉字3个字节）

```python
print("获取键test的值为：{}".format(r.get('test')))
result = r.strlen('test')
print("键test的字节长度为：{}".format(result))

print("获取键name的值为：{}".format(r.get('name')))
result = r.strlen('name')
print("键name的字节长度为：{}".format(result))

#获取键test的值为：12345
#键test的字节长度为：5
#获取键name的值为：我是一个大帅哥
#键name的字节长度为：21
```

### `incr(key, amount=1)`

自增 key对应的值，当 key不存在时，则创建 key＝amount，否则，则自增。

参数：amount - 自增数（必须是整数）

```python
print("获取键num的值为：{}".format(r.get('num')))
print("获取键num1的值为：{}".format(r.get('num1')))

print("将键num的值增1")
r.incr('num',amount=1)
print("获取键num当前的值为：{}".format(r.get('num')))

r.incr('num1',amount=10)
print("获取键num1当前的值为：{}".format(r.get('num1')))

#获取键num的值为：None
#获取键num1的值为：None
#将键num的值增1
#获取键num当前的值为：1
#获取键num1当前的值为：10
```

### decr(key, amount=1)

自减 key对应的值，当 key不存在时，则创建 name＝amount，否则，则自减。

参数：amount - 自减数（整数)

### append(key, value)

在key对应的值后面追加内容

参数：value - 要追加的字符串

### 删除键：delete

## redis 基本命令 list

### 增加一个或多个元素，没有就新建

- lpush(name,values) ：在name对应的list中添加元素，每个新的元素都添加到列表的最左边
- rpush(name,values) ：在name对应的list中添加元素，每个新的元素都添加到列表的最右边

```python
r.delete('list1') # 删除键 list1

print("向list1中从左边依次添加 11 22 33")
r.lpush('list1',11,22,33)
print("list1的值为：{}".format(r.lrange('list1',0,2)))

print("向list1中从右边依次添加 44 55")
r.rpush('list1',44,55)
print("list1的值为：{}".format(r.lrange('list1',0,4)))

print('list1的长度为：{}'.format(r.llen('list1')))

'''
向list1中从左边依次添加 11 22 33
list1的值为：['33', '22', '11']
向list1中从右边依次添加 44 55
list1的值为：['33', '22', '11', '44', '55']
list1的长度为：5
'''
```

### 往已经有的name的列表中添加元素，没有的话无法创建

- lpushx(name, value)：往name列表的左边添加元素
- rpushx(name, value)：往name列表的右边添加元素

```python
r.delete('list1') # 删除键 list1

print("向list1中从左边依次添加 11 22 33:")
r.lpush('list1',11,22,33)
print("list1的值为：{}".format(r.lrange('list1',0,-1)))

print("向list1中左边添加 44:")
r.lpushx('list1',44)
print("list1的值为：{}".format(r.lrange('list1',0,-1)))

print("向list1中右边添加 55:")
r.rpushx('list1',55)
print("list1的值为：{}".format(r.lrange('list1',0,-1)))

'''
向list1中从左边依次添加 11 22 33:
list1的值为：['33', '22', '11']
向list1中左边添加 44:
list1的值为：['44', '33', '22', '11']
向list1中右边添加 55:
list1的值为：['44', '33', '22', '11', '55']
'''
```

### 新增（固定索引号位置插入元素）

linsert(name, where, refvalue, value))
在name对应的列表的某一个值前或后插入一个新值

参数：

name - redis的name
where - BEFORE或AFTER
refvalue - 标杆值，即：在它前后插入数据
value - 要插入的数据

```python
r.delete('list1') # 删除键 list1

print("向list1中从右边依次添加 11 22 33:")
r.rpush('list1',11,22,33)
print("list1的值为：{}".format(r.lrange('list1',0,-1)))

print("向list1中元素22的后面添加 44:")
r.linsert('list1','after',22,44)
print("list1的值为：{}".format(r.lrange('list1',0,-1)))

'''
向list1中从右边依次添加 11 22 33:
list1的值为：['11', '22', '33']
向list1中元素22的后面添加 44:
list1的值为：['11', '22', '44', '33']
'''
```

### 修改（指定索引号进行修改）

**r.lset(name, index, value)**

对name对应的list中的某一个索引位置重新赋值
参数：

- name - redis的name
- index - list的索引位置
- value - 要设置的值

```python
r.delete('list1') # 删除键 list1

print("向list1中从右边依次添加 11 22 33:")
r.rpush('list1',11,22,33)
print("list1的值为：{}".format(r.lrange('list1',0,-1)))

print("把list1中索引号为0的元素改为-11:")
r.lset('list1',0,-11)
print("list1的值为：{}".format(r.lrange('list1',0,-1)))

'''
向list1中从右边依次添加 11 22 33:
list1的值为：['11', '22', '33']
把list1中索引号为0的元素改为-11:
list1的值为：['-11', '22', '33']
'''
```

### 删除（指定值进行删除）

**r.lrem(name, num, value)**
在name对应的list中删除指定的值

参数：

name - redis的name
value - 要删除的值
num - num=0，删除列表中所有的指定值；
num=2 - 从前到后，删除2个, num=1,从前到后，删除左边第1个
num=-2 - 从后向前，删除2个

```python
r.delete('list1') # 删除键 list1

print("向list1中从右边依次添加 11 22 11 11 44 11 11:")
r.rpush('list1',11,22,11,11,44,11,11)
print("list1的值为：{}".format(r.lrange('list1',0,-1)))

print("把list1中的元素11删掉:")
r.lrem('list1',2,11)  # 从左边删掉2个元素 11
print("删除元素后，list1的值为：{}".format(r.lrange('list1',0,-1)))

'''
向list1中从右边依次添加 11 22 11 11 44 11 11:
list1的值为：['11', '22', '11', '11', '44', '11', '11']
把list1中的元素11删掉:
删除元素后，list1的值为：['22', '11', '11', '44', '11', '11']
'''
```

### 删除并返回

- lpop(name) : 在name对应的列表的左侧获取第一个元素并在列表中移除，返回删除的元素
- rpop(name) : 在name对应的列表的右侧获取第一个元素并在列表中移除，返回删除的元素

```python
r.delete('list1') # 删除键 list1

print("向list1中从右边依次添加 11 22 33 44 :")
r.rpush('list1',11,22,33,44)
print("list1的值为：{}".format(r.lrange('list1',0,-1)))

print("从左侧删除第一个元素，并返回删除的元素:")
result = r.lpop('list1')
print("删除的元素为：{}".format(result))
print("删除后list1的值为：{}".format(r.lrange('list1',0,-1)))

'''
向list1中从右边依次添加 11 22 33 44 :
list1的值为：['11', '22', '33', '44']
从左侧删除第一个元素，并返回删除的元素:
删除的元素为：11
删除后list1的值为：['22', '33', '44']
'''
```

### 除索引之外的值

**ltrim(name, start, end)**

在name对应的列表中移除没有在start-end索引之间的值

参数：

- name - redis的name
- start - 索引的起始位置
- end - 索引结束位置

```python
r.delete('list1') # 删除键 list1

print("向list1中从右边依次添加 11 22 33 44 55 :")
r.rpush('list1',11,22,33,44,55)
print("list1的值为：{}".format(r.lrange('list1',0,-1)))

print("删除索引 1-3之外的元素:")
r.ltrim('list1',1,3)
print("删除后list1的值为：{}".format(r.lrange('list1',0,-1)))

'''
向list1中从右边依次添加 11 22 33 44 55 :
list1的值为：['11', '22', '33', '44', '55']
删除索引 1-3之外的元素:
删除后list1的值为：['22', '33', '44']
'''
```

### 取值（根据索引号取值）

**lindex(name, index) ：在name对应的列表中根据索引获取列表元素**

```python
r.delete('list1') # 删除键 list1

print("向list1中从右边依次添加 11 22 33 44 55 :")
r.rpush('list1',11,22,33,44,55)
print("list1的值为：{}".format(r.lrange('list1',0,-1)))

print("索引为1的元素为：{}".format(r.lindex('list1',1)))

'''
向list1中从右边依次添加 11 22 33 44 55 :
list1的值为：['11', '22', '33', '44', '55']
索引为1的元素为：22
'''
```

### 移动 元素从一个列表移动到另外一个列表

rpoplpush(src, dst)
从一个列表取出最右边的元素，同时将其添加至另一个列表的最左边

参数：

- src - 要取数据的列表的 name
- dst - 要添加数据的列表的 name

### 移动 元素从一个列表移动到另外一个列表 可以设置超时

brpoplpush(src, dst, timeout=0)
从一个列表的右侧移除一个元素并将其添加到另一个列表的左侧

参数：

- src - 取出并要移除元素的列表对应的name
- dst - 要插入元素的列表对应的name
- timeout - 当src对应的列表中没有数据时，阻塞等待其有数据的超时时间（秒），0 表示永远阻塞

### 一次移除多个列表

blpop(keys, timeout)
将多个列表排列，按照从左到右去pop对应列表的元素

参数：

- keys - redis的name的集合
- timeout - 超时时间，当元素所有列表的元素获取完之后，阻塞等待列表内有数据的时间（秒）, 0 表示永远阻塞
- r.brpop(keys, timeout) 同 blpop，将多个列表排列,按照从右向左去移除各个列表内的元素

```python
r.delete('list1') # 删除键 list1
r.delete('list2') # 删除键 list2

r.lpush("list1", 1, 2, 3, 4, 5)
r.lpush("list2", 11, 22, 33, 44, 55)
while r.llen('list2'):
    r.blpop(["list1", "list2"], timeout=2)
    print(r.lrange("list1", 0, -1), r.lrange("list2", 0, -1))
    
'''
['4', '3', '2', '1'] ['55', '44', '33', '22', '11']
['3', '2', '1'] ['55', '44', '33', '22', '11']
['2', '1'] ['55', '44', '33', '22', '11']
['1'] ['55', '44', '33', '22', '11']
[] ['55', '44', '33', '22', '11']
[] ['44', '33', '22', '11']
[] ['33', '22', '11']
[] ['22', '11']
[] ['11']
[] []
'''
```

## redis基本命令 set

###  新增: sadd(name,values)

向集合中添加一个或多个元素

```python
r.delete('set1') # 删除键 set1
r.sadd('set1',1,2,3,4,5)

# 获取集合元素个数
num = r.scard('set1')
print('集合set1的元素个数为：{}'.format(num))

print("获取集合set1的元素：{}".format(r.smembers('set1')))

print("获取集合中所有的成员--元组形式:")
result = r.sscan('set1')
print(result)

# 同字符串的操作，用于增量迭代分批获取元素，避免内存消耗太大
print("获取集合中所有的成员--迭代器的方式:")
for i in r.sscan_iter("set1"):
    print(i)
    
'''
集合set1的元素个数为：5
获取集合set1的元素：{'3', '5', '1', '2', '4'}
获取集合中所有的成员--元组形式:
(0, ['1', '2', '3', '4', '5'])
获取集合中所有的成员--迭代器的方式:
1
2
3
4
5
'''
```

### 差集

**sdiff(keys, *args)：在第一个name对应的集合中且不在其他name对应的集合的元素集合**

```python
r.delete('set1') # 删除键 set1
r.delete('set2') # 删除键 set2
r.sadd('set1',1,2,3,4,5)
r.sadd('set2',1,2,6,7,5)

print("集合set1：{}".format(r.smembers('set1')))
print("集合set2：{}".format(r.smembers('set2')))

print("在集合set1但是不在集合set2中：{}".format(r.sdiff("set1", "set2")))
print("在集合set2但是不在集合set1中：{}".format(r.sdiff("set2", "set1")))

'''
集合set1：{'3', '5', '1', '2', '4'}
集合set2：{'6', '7', '5', '1', '2'}
在集合set1但是不在集合set2中：{'3', '4'}
在集合set2但是不在集合set1中：{'7', '6'}
'''
```

**sdiffstore(dest, keys, *args) ：获取第一个name对应的集合中且不在其他name对应的集合，再将其新加入到dest对应的集合中**

```python
r.delete('set1') # 删除键 set1
r.delete('set2') # 删除键 set2
r.sadd('set1',1,2,3,4,5)
r.sadd('set2',1,2,6,7,5)

print("集合set1：{}".format(r.smembers('set1')))
print("集合set2：{}".format(r.smembers('set2')))

r.sdiffstore("set3", "set1", "set2")    # 在集合set1但是不在集合set2中的元素，存入set3中
print("集合set3：{}".format(r.smembers('set3')))

'''
集合set1：{'3', '5', '1', '2', '4'}
集合set2：{'6', '7', '5', '1', '2'}
集合set3：{'3', '4'}
'''
```

### 交集

**sinter(keys, *args)：获取多个name对应集合的交集**

**sinterstore**(dest, keys, *args)：获取多一个name对应集合的并集，再将其加入到dest对应的集合中

```python
r.delete('set1') # 删除键 set1
r.delete('set2') # 删除键 set2
r.delete('set3') # 删除键 set3

r.sadd('set1',1,2,3,4,5)
r.sadd('set2',1,2,6,7,5)

print("集合set1：{}".format(r.smembers('set1')))
print("集合set2：{}".format(r.smembers('set2')))

print('集合set1和set2的交集：{}'.format(r.sinter("set1", "set2")))

r.sinterstore("set3", "set1", "set2")   # 在集合set1和set2中的交集，存入set3中
print("交集集合set3：{}".format(r.smembers('set3')))

'''
集合set1：{'3', '5', '1', '2', '4'}
集合set2：{'6', '7', '5', '1', '2'}
集合set1和set2的交集：{'5', '1', '2'}
交集集合set3：{'5', '1', '2'}
'''
```

### 并集

**sunion**(keys, *args)：获取多个name对应的集合的并集

**sunionstore**(dest,keys, *args)：获取多一个name对应的集合的并集，并将结果保存到dest对应的集合中

### 判断是否是集合的成员 类似in

**sismember**(name, value)：检查value是否是name对应的集合的成员，结果为True和False

### 移动

**smove**(src, dst, value)：将某个成员从一个集合中移动到另外一个集合

```python
r.delete('set1') # 删除键 set1
r.delete('set2') # 删除键 set2

r.sadd('set1',1,2,3)
r.sadd('set2',1,2,4,5)

print("集合set1：{}".format(r.smembers('set1')))
print("集合set2：{}".format(r.smembers('set2')))

r.smove("set1", "set2", 3)

print("移动后集合set1：{}".format(r.smembers('set1')))
print("移动后集合set2：{}".format(r.smembers('set2')))

'''
集合set1：{'3', '1', '2'}
集合set2：{'5', '1', '2', '4'}
移动后集合set1：{'1', '2'}
移动后集合set2：{'3', '5', '1', '2', '4'}
'''
```

### 删除

**spop**(name)：从集合中随机删除一个元素，并返回

**srem**(name, values)：删除指定值

```python
r.delete('set1') # 删除键 set1

r.sadd('set1',1,2,3,4,5,6)

print("集合set1：{}".format(r.smembers('set1')))

result = r.spop("set1")
print("随机删除一个元素：{}".format(result))

print("删除后集合set1：{}".format(r.smembers('set1')))

print("删除集合set1的元素 6：{}".format(r.srem('set1',6)))
print("删除后集合set1：{}".format(r.smembers('set1')))

'''
集合set1：{'3', '6', '5', '1', '2', '4'}
随机删除一个元素：5
删除后集合set1：{'3', '6', '1', '2', '4'}
删除集合set1的元素 6：1
删除后集合set1：{'3', '1', '2', '4'}
'''
```

## redis基本命令 有序set

Set操作，Set集合就是不允许重复的列表，本身是无序的。

有序集合，在集合的基础上，为每元素排序；元素的排序需要根据另外一个值来进行比较，所以，对于有序集合，每一个元素有两个值，即：值和分数，分数专门用来做排序。

### 新增

**zadd**(name, {‘k1’:v1,‘k2’:v2})：在name对应的有序集合中添加元素

**zcard**(name)：获取name对应的有序集合元素的数量

```python
r.delete('zset1') # 删除键 zset1
r.delete('zset2') # 删除键 zset2

r.zadd('zset1', {'n1':11,'n2':22})
r.zadd("zset2", {'m1':22, 'm2':44})
print("zset1集合的长度：{}".format(r.zcard("zset1"))) # 集合长度
print("zset2集合的长度：{}".format(r.zcard("zset2"))) # 集合长度
print("获取有序集合中 zset1 的所有元素：")
print(r.zrange("zset1", 0, -1))
print("获取有序集合中 zset2 的所有元素和分数：")
print(r.zrange("zset2", 0, -1, withscores=True))

'''
zset1集合的长度：2
zset2集合的长度：2
获取有序集合中 zset1 的所有元素：
['n1', 'n2']
获取有序集合中 zset2 的所有元素和分数：
[('m1', 22.0), ('m2', 44.0)]
'''
```

### 获取有序集合的所有元素

r.zrange( name, start, end, desc=False, withscores=False, score_cast_func=float)

#### 按照索引范围获取name对应的有序集合的元素

参数：

- name - redis的name
- start - 有序集合索引起始位置（非分数）
- end - 有序集合索引结束位置（非分数）
- desc - 排序规则，默认按照分数从小到大排序
- withscores - 是否获取元素的分数，默认只获取元素的值
- score_cast_func - 对分数进行数据转换的函数

**从大到小排序**(同zrange，集合是从大到小排序的)
**zrevrange**(name, start, end, withscores=False, score_cast_func=float)

```python
r.delete('zset1') # 删除键 zset1
r.delete('zset2') # 删除键 zset2

r.zadd('zset1', {'n1':11,'n2':22,'n3':10,'n4':33})
r.zadd("zset2", {'m1':22, 'm2':44, 'm3':30, 'm4':10, 'm5':55})

print("获取有序集合中 zset1 的所有元素：")
print(r.zrevrange("zset1", 0, -1))
print("获取有序集合中 zset2 的所有元素和分数：")
print(r.zrevrange("zset2", 0, -1, withscores=True))  # 分数倒序

'''
获取有序集合中 zset1 的所有元素：
['n4', 'n2', 'n1', 'n3']
获取有序集合中 zset2 的所有元素和分数：
[('m5', 55.0), ('m2', 44.0), ('m3', 30.0), ('m1', 22.0), ('m4', 10.0)]
'''
```

#### 按照分数范围获取name对应的有序集合的元素

**zrangebyscore**(name, min, max, start=None, num=None, withscores=False, score_cast_func=float)

```python
r.delete('zset1') # 删除键 zset1

r.zadd("zset1", {'m1':22, 'm2':44, 'm3':30, 'm4':10, 'm5':55})

print("获取有序集合zset1中，分数在10-30的元素和分数：")
print(r.zrangebyscore("zset1", 10,30,withscores=True))  # 分数从小到大

获取有序集合zset1中，分数在10-30的元素和分数：
[('m4', 10.0), ('m1', 22.0), ('m3', 30.0)]

'''
获取有序集合zset1中，分数在10-30的元素和分数：
[('m4', 10.0), ('m1', 22.0), ('m3', 30.0)]
'''
```

#### 按照分数范围获取有序集合的元素并排序（默认从大到小排序）

**zrevrangebyscore**(name, max, min, start=None, num=None, withscores=False, score_cast_func=float)

```python
r.delete('zset1') # 删除键 zset1

r.zadd("zset1", {'m1':22, 'm2':44, 'm3':30, 'm4':10, 'm5':55})

print("获取有序集合zset1中，分数在30-50的元素和分数：")
print(r.zrevrangebyscore("zset1", 50,30,withscores=True))  # 分数从大到小

#获取有序集合zset1中，分数在30-50的元素和分数：
#[('m2', 44.0), ('m3', 30.0)]
```

#### 获取所有元素–默认按照分数顺序排序

**zscan**(name, cursor=0, match=None, count=None, score_cast_func=float)

```python
r.delete('zset1') # 删除键 zset1

r.zadd("zset1", {'m1':22, 'm2':44, 'm3':30, 'm4':10, 'm5':55})

print("获取有序集合zset1的所有元素和分数，以元组形式返回：")
print(r.zscan("zset1"))  # 分数从小到大

'''
获取有序集合zset1的所有元素和分数，以元组形式返回：
(0, [('m4', 10.0), ('m1', 22.0), ('m3', 30.0), ('m2', 44.0), ('m5', 55.0)])
'''
```

#### 获取所有元素–迭代器

**zscan_iter**(name, match=None, count=None,score_cast_func=float)

```python
for i in r.zscan_iter("zset3"): # 遍历迭代器
    print(i)
```

### 统计个数

**zcount**(name, min, max)：获取name对应的有序集合中分数 在 [min,max] 之间的个数

```python
r.delete('zset1') # 删除键 zset1

r.zadd("zset1", {'m1':22, 'm2':44, 'm3':30, 'm4':10, 'm5':55})

print(r.zrange("zset1", 0, -1, withscores=True))
print(r.zcount("zset1", 11, 50))

'''
[('m4', 10.0), ('m1', 22.0), ('m3', 30.0), ('m2', 44.0), ('m5', 55.0)]
3
'''
```

### 自增

**zincrby**(name, amount, value)：自增name对应的有序集合的 value 对应的分z数

```python
r.delete('zset1') # 删除键 zset1

r.zadd("zset1", {'m1':22, 'm2':44, 'm3':30, 'm4':10, 'm5':55})

print(r.zrange("zset1", 0, -1, withscores=True))

r.zincrby("zset1", 2, 'm2')    # 每次将m2的分数自增2
print(r.zrange("zset1", 0, -1, withscores=True))

'''
[('m4', 10.0), ('m1', 22.0), ('m3', 30.0), ('m2', 44.0), ('m5', 55.0)]
[('m4', 10.0), ('m1', 22.0), ('m3', 30.0), ('m2', 46.0), ('m5', 55.0)]
'''
```

### 获取值的索引号

#### zrank(name, value)：获取某个值在 name对应的有序集合中的索引（从 0 开始）

#### zrevrank(name, value)，获取某个值在 name对应的有序集合中的索引（从大到小排序）

```python
r.delete('zset1') # 删除键 zset1

r.zadd("zset1", {'m1':22, 'm2':44, 'm3':30, 'm4':10, 'm5':55})

print(r.zrange("zset1", 0, -1, withscores=True))

print('m2的索引号是3,这里按照分数顺序（从小到大）:{}'.format(r.zrank("zset1", "m2")))
print('m2的索引号是1,这里安照分数倒序（从大到小）:{}'.format(r.zrevrank("zset1", "m2")))

'''
[('m4', 10.0), ('m1', 22.0), ('m3', 30.0), ('m2', 44.0), ('m5', 55.0)]
m2的索引号是3,这里按照分数顺序（从小到大）:3
m2的索引号是1,这里安照分数倒序（从大到小）:1
'''
```

### 删除

#### zrem(name, values)：删除name对应的有序集合中值是values的成员

```py
r.delete('zset1') # 删除键 zset1

r.zadd("zset1", {'m1':22, 'm2':44, 'm3':30, 'm4':10, 'm5':55})

print("有序集合zset1：")
print(r.zrange("zset1", 0, -1, withscores=True))

r.zrem('zset1','m2') # 删除 m2 元素
print("删除m2元素后，有序集合zset1：")
print(r.zrange("zset1", 0, -1, withscores=True))

'''
有序集合zset1：
[('m4', 10.0), ('m1', 22.0), ('m3', 30.0), ('m2', 44.0), ('m5', 55.0)]
删除m2元素后，有序集合zset1：
[('m4', 10.0), ('m1', 22.0), ('m3', 30.0), ('m5', 55.0)]
'''
```

#### zremrangebyrank(name, min, max)：根据排行范围删除，按照索引号来删除

```py
r.delete('zset1') # 删除键 zset1

r.zadd("zset1", {'m1':22, 'm2':44, 'm3':30, 'm4':10, 'm5':55})

print("有序集合zset1：")
print(r.zrange("zset1", 0, -1, withscores=True))

r.zremrangebyrank("zset1", 1, 3)  # 删除有序集合中的索引号是1到3的元素
print("删除元素后，有序集合zset1：")
print(r.zrange("zset1", 0, -1, withscores=True))

'''
有序集合zset1：
[('m4', 10.0), ('m1', 22.0), ('m3', 30.0), ('m2', 44.0), ('m5', 55.0)]
删除元素后，有序集合zset1：
[('m4', 10.0), ('m5', 55.0)]
'''
```

#### zremrangebyscore(name, min, max)：根据分数范围删除

### 获取值对应的分数

```py
r.delete('zset1') # 删除键 zset1

r.zadd("zset1", {'m1':22, 'm2':44, 'm3':30, 'm4':10, 'm5':55})

print("有序集合zset1：")
print(r.zrange("zset1", 0, -1, withscores=True))

print('获取元素m4对应的分数:{}'.format(r.zscore("zset1", "m4")))
```

## redis 基本命令 hash

### 单个增加–修改(单个取出)–没有就新增，有的话就修改

**hset**(name, key, value)
name对应的hash中设置一个键值对（不存在，则创建；否则，修改）

参数：

- name - redis的name
- key - name对应的hash中的key
- value - name对应的hash中的value
- 注：hsetnx(name, key, value) 当name对应的hash中不存在当前key时则创建（相当于添加）

```py
r.delete('zset1') # 删除键 zset1

r.hset("hash1", "k1", "v1")
r.hset("hash1", "k2", "v2")
print(r.hkeys("hash1")) # 取hash中所有的key
print(r.hget("hash1", "k1"))    # 单个取hash的key对应的值
print(r.hmget("hash1", "k1", "k2")) # 多个取hash的key对应的值

'''
['k1', 'k2']
v1
['v1', 'v2']
'''
```

> > 此方法被弃用
>
> Redis Hmset 命令用于同时将多个 field-value (字段-值)对设置到[哈希表](https://so.csdn.net/so/search?q=哈希表&spm=1001.2101.3001.7020)中。
>
> 此命令会覆盖哈希表中已存在的字段。
>
> 如果哈希表不存在，会创建一个空哈希表，并执行 HMSET 操作。





### 取出所有键值对

#### hgetall(name)：获取name对应hash的所有键值

```py
r.delete('hash1') # 删除键 hash1

r.hset("hash1", "k1","v1")
r.hset("hash1", "k2","v2")
r.hset("hash1", "k3","v3")

print(r.hgetall("hash1"))

'''
print(r.hgetall("hash1"))
'''
```

### 其他常用命令

| 命令               | 描述                             |
| ------------------ | -------------------------------- |
| hlen(name)         | 获取name对应的hash中键值对的个数 |
| hkeys(name)        | 得到所有的keys                   |
| hvals(name)        | 得到所有的value                  |
| hexists(name, key) | 判断成员是否存在                 |
| hdel(name,*keys)   | 删除键值对                       |

### 自增自减整数(将key对应的value–整数 自增1或者2，或者别的整数 负数就是自减)

hincrby(name, key, amount=1)
自增name对应的hash中的指定key的值，不存在则创建key=amount

参数：

name - redis中的name
key - hash对应的key
amount - 自增数（整数）

### 自增自减浮点数(将key对应的value–浮点数 自增1.0或者2.0，或者别的浮点数 负数就是自减)

hincrbyfloat(name, key, amount=1.0)
自增name对应的hash中的指定key的值，不存在则创建key=amount

参数：

- name - redis中的name
- key - hash对应的key
- amount，自增数（浮点数）
- 自增 name 对应的 hash 中的指定 key 的值，不存在则创建 key=amount

### 取值查看–分片读取

hscan(name, cursor=0, match=None, count=None)
增量式迭代获取，对于数据大的数据非常有用，hscan可以实现分片的获取数据，并非一次性将数据全部获取完，从而放置内存被撑爆

参数：

- name - redis的name
- cursor - 游标（基于游标分批取获取数据）
- match - 匹配指定key，默认None 表示所有的key
- count - 每次分片最少获取个数，默认None表示采用Redis的默认分片个数

### hscan_iter(name, match=None, count=None)

利用yield封装hscan创建生成器，实现分批去redis中获取数据

参数：

- match - 匹配指定key，默认None 表示所有的key
- count - 每次分片最少获取个数，默认None表示采用Redis的默认分片个数

## 管道（pipeline）

redis默认在执行每次请求都会创建（连接池申请连接）和断开（归还连接池）一次连接操作，如果想要在一次请求中指定多个命令，则可以使用pipline实现一次请求指定多个命令，并且默认情况下一次pipline 是原子性操作。

管道（pipeline）是redis在提供单个请求中缓冲多条服务器命令的基类的子类。它通过减少服务器-客户端之间反复的TCP数据库包，从而大大提高了执行批量命令的功能

```py
pipe = r.pipeline() # 创建一个管道

pipe.set('name', 'jack')
pipe.set('role', 'sbhh')
pipe.sadd('faz', 'baz')
pipe.incr('num')    # 如果num不存在则vaule为1，如果存在，则value自增1
pipe.execute()

print(r.get("name"))
print(r.get("role"))
print(r.get("num"))

'''
jack
sbhh
2
'''
```

**管道的命令可以写在一起：**

```py
pipe.set('hello', 'redis').sadd('faz', 'baz').incr('num').execute()
print(r.get("name"))
print(r.get("role"))
print(r.get("num"))

'''
jack
sbhh
4
'''
```

## 其它常用操作

操作	描述

1. delete(key)	根据删除redis中的任意数据类型的key
2. exists(name)	检测redis的name是否存在，存在就是True，False不存在
3. expire(name ,time)	为某个redis的某个name设置超时时间
4. rename(src, dst)	对redis的name重命名
5. randomkey()	随机获取一个redis的name（不删除）
6. type(name)	获取name对应值的类型
7. dbsize()	当前redis包含多少条数据
8. save()	检查点"操作，将数据写回磁盘。保存时阻塞
9. flushdb()	清空redis中的所有数据

## 常用命令 LINUX

### Redis 键(key) 命令

| 命令                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [Redis Type 命令](https://www.redis.net.cn/order/3543.html)  | 返回 key 所储存的值的类型。                                  |
| [Redis PEXPIREAT 命令](https://www.redis.net.cn/order/3533.html) | 设置 key 的过期时间亿以毫秒计。                              |
| [Redis PEXPIREAT 命令](https://www.redis.net.cn/order/3534.html) | 设置 key 过期时间的时间戳(unix timestamp) 以毫秒计           |
| [Redis Rename 命令](https://www.redis.net.cn/order/3541.html) | 修改 key 的名称                                              |
| [Redis PERSIST 命令](https://www.redis.net.cn/order/3537.html) | 移除 key 的过期时间，key 将持久保持。                        |
| [Redis Move 命令](https://www.redis.net.cn/order/3536.html)  | 将当前数据库的 key 移动到给定的数据库 db 当中。              |
| [Redis RANDOMKEY 命令](https://www.redis.net.cn/order/3540.html) | 从当前数据库中随机返回一个 key 。                            |
| [Redis Dump 命令](https://www.redis.net.cn/order/3529.html)  | 序列化给定 key ，并返回被序列化的值。                        |
| [Redis TTL 命令](https://www.redis.net.cn/order/3539.html)   | 以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)。 |
| [Redis Expire 命令](https://www.redis.net.cn/order/3531.html) | seconds 为给定 key 设置过期时间。                            |
| [Redis DEL 命令](https://www.redis.net.cn/order/3528.html)   | 该命令用于在 key 存在是删除 key。                            |
| [Redis Pttl 命令](https://www.redis.net.cn/order/3538.html)  | 以毫秒为单位返回 key 的剩余的过期时间。                      |
| [Redis Renamenx 命令](https://www.redis.net.cn/order/3542.html) | 仅当 newkey 不存在时，将 key 改名为 newkey 。                |
| [Redis EXISTS 命令](https://www.redis.net.cn/order/3530.html) | 检查给定 key 是否存在。                                      |
| [Redis Expireat 命令](https://www.redis.net.cn/order/3532.html) | EXPIREAT 的作用和 EXPIRE 类似，都用于为 key 设置过期时间。 不同在于 EXPIREAT 命令接受的时间参数是 UNIX 时间戳(unix timestamp)。 |
| [Redis Keys 命令](https://www.redis.net.cn/order/3535.html)  | 查找所有符合给定模式( pattern)的 key 。                      |

### Redis 字符串(String) 命令

| 命令                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [Redis Setnx 命令](https://www.redis.net.cn/order/3552.html) | 只有在 key 不存在时设置 key 的值。                           |
| [Redis Getrange 命令](https://www.redis.net.cn/order/3546.html) | 返回 key 中字符串值的子字符                                  |
| [Redis Mset 命令](https://www.redis.net.cn/order/3555.html)  | 同时设置一个或多个 key-value 对。                            |
| [Redis Setex 命令](https://www.redis.net.cn/order/3551.html) | 将值 value 关联到 key ，并将 key 的过期时间设为 seconds (以秒为单位)。 |
| [Redis SET 命令](https://www.redis.net.cn/order/3544.html)   | 设置指定 key 的值                                            |
| [Redis Get 命令](https://www.redis.net.cn/order/3545.html)   | 获取指定 key 的值。                                          |
| [Redis Getbit 命令](https://www.redis.net.cn/order/3548.html) | 对 key 所储存的字符串值，获取指定偏移量上的位(bit)。         |
| [Redis Setbit 命令](https://www.redis.net.cn/order/3550.html) | 对 key 所储存的字符串值，设置或清除指定偏移量上的位(bit)。   |
| [Redis Decr 命令](https://www.redis.net.cn/order/3561.html)  | 将 key 中储存的数字值减一。                                  |
| [Redis Decrby 命令](https://www.redis.net.cn/order/3562.html) | key 所储存的值减去给定的减量值（decrement） 。               |
| [Redis Strlen 命令](https://www.redis.net.cn/order/3554.html) | 返回 key 所储存的字符串值的长度。                            |
| [Redis Msetnx 命令](https://www.redis.net.cn/order/3556.html) | 同时设置一个或多个 key-value 对，当且仅当所有给定 key 都不存在。 |
| [Redis Incrby 命令](https://www.redis.net.cn/order/3559.html) | 将 key 所储存的值加上给定的增量值（increment） 。            |
| [Redis Incrbyfloat 命令](https://www.redis.net.cn/order/3560.html) | 将 key 所储存的值加上给定的浮点增量值（increment） 。        |
| [Redis Setrange 命令](https://www.redis.net.cn/order/3553.html) | 用 value 参数覆写给定 key 所储存的字符串值，从偏移量 offset 开始。 |
| [Redis Psetex 命令](https://www.redis.net.cn/order/3557.html) | 这个命令和 SETEX 命令相似，但它以毫秒为单位设置 key 的生存时间，而不是像 SETEX 命令那样，以秒为单位。 |
| [Redis Append 命令](https://www.redis.net.cn/order/3563.html) | 如果 key 已经存在并且是一个字符串， APPEND 命令将 value 追加到 key 原来的值的末尾。 |
| [Redis Getset 命令](https://www.redis.net.cn/order/3547.html) | 将给定 key 的值设为 value ，并返回 key 的旧值(old value)。   |
| [Redis Mget 命令](https://www.redis.net.cn/order/3549.html)  | 获取所有(一个或多个)给定 key 的值。                          |
| [Redis Incr 命令](https://www.redis.net.cn/order/3558.html)  | 将 key 中储存的数字值增一。                                  |

### Redis 哈希(Hash) 命令

| 命令                                                         | 描述                                                     |
| :----------------------------------------------------------- | :------------------------------------------------------- |
| [Redis Hmset 命令](https://www.redis.net.cn/order/3573.html) | 同时将多个 field-value (域-值)对设置到哈希表 key 中。    |
| [Redis Hmget 命令](https://www.redis.net.cn/order/3572.html) | 获取所有给定字段的值                                     |
| [Redis Hset 命令](https://www.redis.net.cn/order/3574.html)  | 将哈希表 key 中的字段 field 的值设为 value 。            |
| [Redis Hgetall 命令](https://www.redis.net.cn/order/3567.html) | 获取在哈希表中指定 key 的所有字段和值                    |
| [Redis Hget 命令](https://www.redis.net.cn/order/3566.html)  | 获取存储在哈希表中指定字段的值/td>                       |
| [Redis Hexists 命令](https://www.redis.net.cn/order/3565.html) | 查看哈希表 key 中，指定的字段是否存在。                  |
| [Redis Hincrby 命令](https://www.redis.net.cn/order/3568.html) | 为哈希表 key 中的指定字段的整数值加上增量 increment 。   |
| [Redis Hlen 命令](https://www.redis.net.cn/order/3571.html)  | 获取哈希表中字段的数量                                   |
| [Redis Hdel 命令](https://www.redis.net.cn/order/3564.html)  | 删除一个或多个哈希表字段                                 |
| [Redis Hvals 命令](https://www.redis.net.cn/order/3576.html) | 获取哈希表中所有值                                       |
| [Redis Hincrbyfloat 命令](https://www.redis.net.cn/order/3569.html) | 为哈希表 key 中的指定字段的浮点数值加上增量 increment 。 |
| [Redis Hkeys 命令](https://www.redis.net.cn/order/3570.html) | 获取所有哈希表中的字段                                   |
| [Redis Hsetnx 命令](https://www.redis.net.cn/order/3575.html) | 只有在字段 field 不存在时，设置哈希表字段的值。          |

### Redis 列表(List) 命令

| 命令                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [Redis Lindex 命令](https://www.redis.net.cn/order/3580.html) | 通过索引获取列表中的元素                                     |
| [Redis Rpush 命令](https://www.redis.net.cn/order/3592.html) | 在列表中添加一个或多个值                                     |
| [Redis Lrange 命令](https://www.redis.net.cn/order/3586.html) | 获取列表指定范围内的元素                                     |
| [Redis Rpoplpush 命令](https://www.redis.net.cn/order/3591.html) | 移除列表的最后一个元素，并将该元素添加到另一个列表并返回     |
| [Redis Blpop 命令](https://www.redis.net.cn/order/3577.html) | 移出并获取列表的第一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| [Redis Brpop 命令](https://www.redis.net.cn/order/3578.html) | 移出并获取列表的最后一个元素， 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| [Redis Brpoplpush 命令](https://www.redis.net.cn/order/3579.html) | 从列表中弹出一个值，将弹出的元素插入到另外一个列表中并返回它； 如果列表没有元素会阻塞列表直到等待超时或发现可弹出元素为止。 |
| [Redis Lrem 命令](https://www.redis.net.cn/order/3587.html)  | 移除列表元素                                                 |
| [Redis Llen 命令](https://www.redis.net.cn/order/3582.html)  | 获取列表长度                                                 |
| [Redis Ltrim 命令](https://www.redis.net.cn/order/3589.html) | 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。 |
| [Redis Lpop 命令](https://www.redis.net.cn/order/3583.html)  | 移出并获取列表的第一个元素                                   |
| [Redis Lpushx 命令](https://www.redis.net.cn/order/3585.html) | 将一个或多个值插入到已存在的列表头部                         |
| [Redis Linsert 命令](https://www.redis.net.cn/order/3581.html) | 在列表的元素前或者后插入元素                                 |
| [Redis Rpop 命令](https://www.redis.net.cn/order/3590.html)  | 移除并获取列表最后一个元素                                   |
| [Redis Lset 命令](https://www.redis.net.cn/order/3588.html)  | 通过索引设置列表元素的值                                     |
| [Redis Lpush 命令](https://www.redis.net.cn/order/3584.html) | 将一个或多个值插入到列表头部                                 |
| [Redis Rpushx 命令](https://www.redis.net.cn/order/3593.html) | 为已存在的列表添加值                                         |

### Redis 集合(Set) 命令

| 命令                                                         | 描述                                                |
| :----------------------------------------------------------- | :-------------------------------------------------- |
| [Redis Sunion 命令](https://www.redis.net.cn/order/3606.html) | 返回所有给定集合的并集                              |
| [Redis Scard 命令](https://www.redis.net.cn/order/3595.html) | 获取集合的成员数                                    |
| [Redis Srandmember 命令](https://www.redis.net.cn/order/3604.html) | 返回集合中一个或多个随机数                          |
| [Redis Smembers 命令](https://www.redis.net.cn/order/3601.html) | 返回集合中的所有成员                                |
| [Redis Sinter 命令](https://www.redis.net.cn/order/3598.html) | 返回给定所有集合的交集                              |
| [Redis Srem 命令](https://www.redis.net.cn/order/3605.html)  | 移除集合中一个或多个成员                            |
| [Redis Smove 命令](https://www.redis.net.cn/order/3602.html) | 将 member 元素从 source 集合移动到 destination 集合 |
| [Redis Sadd 命令](https://www.redis.net.cn/order/3594.html)  | 向集合添加一个或多个成员                            |
| [Redis Sismember 命令](https://www.redis.net.cn/order/3600.html) | 判断 member 元素是否是集合 key 的成员               |
| [Redis Sdiffstore 命令](https://www.redis.net.cn/order/3597.html) | 返回给定所有集合的差集并存储在 destination 中       |
| [Redis Sdiff 命令](https://www.redis.net.cn/order/3596.html) | 返回给定所有集合的差集                              |
| [Redis Sscan 命令](https://www.redis.net.cn/order/3608.html) | 迭代集合中的元素                                    |
| [Redis Sinterstore 命令](https://www.redis.net.cn/order/3599.html) | 返回给定所有集合的交集并存储在 destination 中       |
| [Redis Sunionstore 命令](https://www.redis.net.cn/order/3607.html) | 所有给定集合的并集存储在 destination 集合中         |
| [Redis Spop 命令](https://www.redis.net.cn/order/3603.html)  | 移除并返回集合中的一个随机元素                      |

### Redis 有序集合(sorted set) 命令

| 命令                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [Redis Zrevrank 命令](https://www.redis.net.cn/order/3625.html) | 返回有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序 |
| [Redis Zlexcount 命令](https://www.redis.net.cn/order/3614.html) | 在有序集合中计算指定字典区间内成员数量                       |
| [Redis Zunionstore 命令](https://www.redis.net.cn/order/3627.html) | 计算给定的一个或多个有序集的并集，并存储在新的 key 中        |
| [Redis Zremrangebyrank 命令](https://www.redis.net.cn/order/3621.html) | 移除有序集合中给定的排名区间的所有成员                       |
| [Redis Zcard 命令](https://www.redis.net.cn/order/3610.html) | 获取有序集合的成员数                                         |
| [Redis Zrem 命令](https://www.redis.net.cn/order/3619.html)  | 移除有序集合中的一个或多个成员                               |
| [Redis Zinterstore 命令](https://www.redis.net.cn/order/3613.html) | 计算给定的一个或多个有序集的交集并将结果集存储在新的有序集合 key 中 |
| [Redis Zrank 命令](https://www.redis.net.cn/order/3618.html) | 返回有序集合中指定成员的索引                                 |
| [Redis Zincrby 命令](https://www.redis.net.cn/order/3612.html) | 有序集合中对指定成员的分数加上增量 increment                 |
| [Redis Zrangebyscore 命令](https://www.redis.net.cn/order/3617.html) | 通过分数返回有序集合指定区间内的成员                         |
| [Redis Zrangebylex 命令](https://www.redis.net.cn/order/3616.html) | 通过字典区间返回有序集合的成员                               |
| [Redis Zscore 命令](https://www.redis.net.cn/order/3626.html) | 返回有序集中，成员的分数值                                   |
| [Redis Zremrangebyscore 命令](https://www.redis.net.cn/order/3622.html) | 移除有序集合中给定的分数区间的所有成员                       |
| [Redis Zscan 命令](https://www.redis.net.cn/order/3628.html) | 迭代有序集合中的元素（包括元素成员和元素分值）               |
| [Redis Zrevrangebyscore 命令](https://www.redis.net.cn/order/3624.html) | 返回有序集中指定分数区间内的成员，分数从高到低排序           |
| [Redis Zremrangebylex 命令](https://www.redis.net.cn/order/3620.html) | 移除有序集合中给定的字典区间的所有成员                       |
| [Redis Zrevrange 命令](https://www.redis.net.cn/order/3623.html) | 返回有序集中指定区间内的成员，通过索引，分数从高到底         |
| [Redis Zrange 命令](https://www.redis.net.cn/order/3615.html) | 通过索引区间返回有序集合成指定区间内的成员                   |
| [Redis Zcount 命令](https://www.redis.net.cn/order/3611.html) | 计算在有序集合中指定区间分数的成员数                         |
| [Redis Zadd 命令](https://www.redis.net.cn/order/3609.html)  | 向有序集合添加一个或多个成员，或者更新已存在成员的分数       |
