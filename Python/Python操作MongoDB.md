# Python操作MongoDB

## MongoDB简介：

MongoDB是一种开源的文档型非关系型数据库，它不需要预定义数据结构，不需要固定的表结构，可以存储多种不同类型的数据，支持动态查询，通过使用JSON格式的BSON（Binary JSON）文档，使得存储数据非常灵活。MongoDB跨平台支持多种编程语言，包括C++、Python、Java等等，并且支持水平扩展，以及高可用性和自动容错能力。

以下是MongoDB的一些主要特点：

1. 非关系型： MongoDB是非关系型数据库，不需要遵循传统的表格形式来存储数据，用户可以自由构造数据集。
2. 面向文档型：MongoDB的每个记录都是一个文档对象，文档采用BSON(binary JSON)格式，BSON支持内嵌文档和列表类型等丰富的数据结构。
3. 动态数据模型：MongoDB不需要固定的数据结构，文档可以根据需要动态添加字段，删除字段。
4. 高可扩展性：MongoDB支持分布式存储，在多个服务器上进行数据复制、分片等操作，实现数据的高可用和高性能。
5. 强大的查询功能: MongoDB支持查询操作，可以根据条件查询文档，支持聚合、排序、分页等一系列功能。
6. 大数据量处理：MongoDB支持垂直扩展来处理大量数据集，如支持分片操作。
7. 免费、开源：MongoDB是免费、开源的数据库，用户可以自由地使用、分发、扩展或修改MongoDB。

MongoDB由于自身特点的优越性，在Web应用、移动应用、社交网络、实时数据和物联网应用等领域得到了广泛应用。

## 使用Python连接MongoDB

在Python中连接MongoDB通常需要使用第三方库，最常用的是pymongo库。下面是一个使用pymongo库连接MongoDB的简单示例：

```python
import pymongo

# 连接 MongoDB
client = pymongo.MongoClient("mongodb://localhost:27017/")

# 获取数据库实例
db = client["mydatabase"]

# 获取集合实例
collection = db["mycollection"]

# 插入一条文档
doc = {"name": "John", "age": 30}
res = collection.insert_one(doc)
print(res.inserted_id)

# 查询文档
query = {"name": "John"}
res = collection.find(query)
for doc in res:
    print(doc)
```

在上面的示例中：

1. 首先通过pymongo库中的MongoClient类创建了一个MongoDB客户端实例，指定了连接MongoDB服务器的地址和端口号。
2. 然后通过客户端实例获取了MongoDB实例和集合实例，使用这些实例来执行对数据库和集合的操作。
3. 最后插入一条文档并查询该文档。

在使用pymongo库操作MongoDB时，通常需要关注以下几个基本概念：

1. MongoClient类：用于连接和管理MongoDB服务器，通过该类可以获取MongoDB实例和集合实例。
2. MongoDatabase类：用于表示一个MongoDB数据库，可以通过客户端实例的索引方式获取MongoDB实例。
3. MongoCollection类：用于表示一个MongoDB集合，每个集合代表数据库中的一个表格，可以通过数据库实例的方法获取集合实例。
4. 文档（document）：MongoDB中的基本存储单元，类似于数据库中的记录或行。文档是一种类似于JSON的结构，可以有不同的字段和值。

## 什么是orm框架

ORM (Object-Relational Mapping) 是一种常见的软件开发技术，旨在将关系数据库中的数据与应用程序中的对象进行映射和同步。ORM框架是实现ORM技术的软件工具，它可以自动地生成SQL语句，将数据库表与应用程序中的对象互相转换，并提供更高级别的数据访问功能。

ORM框架主要用于简化与数据库的交互，特别是在复杂数据模型和大量数据的情况下。ORM框架通过使用面向对象编程的方式来处理数据库记录，提供了对数据库的高级别抽象，消除了很多手动编写SQL语句的需求。这些框架通常提供的功能包括：

1. 通过将实体类与数据库表进行映射，ORM框架可以自动创建或更新数据库表结构。
2. 自动处理数据类型转换、SQL语句的拼接和执行，从而简化了常见的数据库操作。
3. 提供了高级别的检索和查询功能，例如排序、分页和聚合查询等。
4. 提供事务管理，从而确保数据一致性和完整性。
5. 支持多种数据库，包括MySQL、PostgreSQL、Oracle和SQL Server等。

常用的ORM框架包括Hibernate、MyBatis、Spring Data JPA和Django ORM等。 各种ORM框架有其自己的特点和优点，需要根据项目需求和开发经验来选择合适的框架。

## Python操作MongoDB的ORM框架

在Python中操作MongoDB数据库可以使用ORM框架。ORM（Object-Relational Mapping）是一种编程技术，它将Python对象和MongoDB文档映射起来，使得可以通过Python代码来操作MongoDB数据库，避免直接在代码中写MongoDB的原生查询语句。

以下是几个Python操作MongoDB的ORM框架：

1. PyMongo

PyMongo是Python中最流行的MongoDB驱动程序之一，它提供了一个简单的API来操作MongoDB数据库。PyMongo不是一个完整的ORM框架，但它可以很方便地与Python对象进行交互，可以将Python对象转换成MongoDB文档，并将MongoDB文档转换成Python对象。

1. MongoEngine

MongoEngine是一个比较流行的MongoDB ORM框架。它提供了一个高层次的API，并支持复杂的查询，如聚合查询、嵌套查询等。使用MongoEngine，您可以定义Python类，将其映射到MongoDB的集合，并使用Python代码来查询和操作MongoDB数据。

1. Ming

Ming是一个Python编写的MongoDB ORM框架，它提供了大量的MongoDB特定的功能和查询API。Ming可以处理MongoDB数据库的基本操作，如插入、查询和更新，同时它也支持高级复杂查询。

1. Tortoise-ORM

Tortoise-ORM是一个异步的Python ORM框架，它支持MongoDB数据库。Tortoise-ORM具有简单易用的API，并支持复杂的查询，如聚合查询和嵌套查询。与其他ORM框架不同，Tortoise-ORM采用异步编程模式，可以更好地处理并发请求。

以上是一些操作MongoDB的ORM框架，它们都可以通过简单的配置来连接MongoDB数据库，并提供多种操作API。选择合适的ORM框架取决于您的项目需求和个人喜好。

## MongoEngine的使用

MongoEngine是一个Python ORM框架，专门用于与MongoDB数据库进行交互，并提供了更加Pythonic和便捷的方式来操作MongoDB数据库。MongoEngine旨在简化与MongoDB数据库的交互和管理，提供了更加友好的API来创建MongoDB文档（document）对象，并进行查询、更新、删除等操作。

在实际项目应用中，还可以使用MongoEngine提供的其他高级特性，例如聚合管道、嵌套文档、索引等。总的来说，MongoEngine提供了非常方便和易用的API来使用MongoDB数据库。

以下是MongoEngine的一些基本使用方法：

### 基本连接

首先需要安装MongoEngine，可以使用pip进行安装：

```python
pip install mongoengine
```



连接MongoDB：

```python
from mongoengine import connect

connect(db='mydb', host='localhost', port=27017)
```



### 定义MongoDB文档

MongoDB文档就是MongoDB中的一条记录，可以通过MongoEngine定义一个文档类来描述MongoDB文档的结构，并且文档类需要继承自`mongoengine.Document`。

```python
from mongoengine import Document, StringField, IntField

class User(Document):
    name = StringField(required=True)
    age = IntField()
```



上述代码定义了一个User类，它表示了一个MongoDB的文档数据集合的结构，包含了name和age两个字段，其中name字段必传。

### 插入数据

通过创建文档对象，可以将数据保存到数据库中。

```python
user = User(name='Tom', age=18)
user.save()
```



也可以使用下面的方式实现：

```python
User(name='Tom', age=18).save()
```



### 查询数据

查询可以使用MongoEngine提供的`queryset`对象来实现。

查询所有的数据：

```python
users = User.objects.all()
```



查询符合条件的数据：

```python
user = User.objects(name='Tom', age=18).first()
```



### 更新数据

对于已存在的文档，可以使用update方法来更新数据。

```python
# 查询并将Tom的年龄改为20
User.objects(name='Tom').update(age=20)
```



### 删除数据

删除文档可以使用delete方法。

```python
User.objects(name='Tom').delete()
```

### 设置某列值唯一

在MongoEngine中，可以使用`unique=True`参数设置模型中的字段值唯一。这将防止模型中指定的字段具有重复的值。例如，假设我们有一个名为`User`的模型，并希望确保其中的`email`字段值是唯一的，可以这样定义模型：

```python
from mongoengine import Document, StringField

class User(Document):
    email = StringField(max_length=100, required=True, unique=True)
    name = StringField(max_length=50, required=True)
    password = StringField(max_length=100, required=True)
```



在上述代码中，`unique=True`参数设置了`email`字段的值必须唯一，因此如果有两个或多个用户尝试使用相同的电子邮件地址进行注册，MongoEngine将引发重复键错误。

需要注意的是，MongoDB在使用唯一索引时会忽略空值，因此如果在模型中添加了`unique=True`参数，则必须确保该字段不为`Null`或空字符串。否则，多个空值将被视为相同值，引发重复键错误。在这种情况下，可以添加另一个属性，如`required=True`，以确保该字段不为空。

除了在字段级别上设置唯一性外，MongoEngine还可以在整个模型级别上设置唯一性。要在整个模型上设置唯一性，请使用`meta`属性并指定`unique`键。例如，如果我们希望确保`User`模型中的每个用户具有唯一的姓名和电子邮件地址，可以这样定义模型：

```python
from mongoengine import Document, StringField

class User(Document):
    email = StringField(max_length=100, required=True)
    name = StringField(max_length=50, required=True)

    meta = {
        'indexes': [
            {'fields': ['email'], 'unique': True},
            {'fields': ['name'], 'unique': True}
        ]
    }
```



在上述代码中，我们在模型的`meta`属性中指定了`indexes`键的值。该值是一个列表，其中包含多个字典，用于指定要在模型中创建的索引。每个字典都使用`fields`键指定要索引的字段，并使用`unique`键指定该字段是否应该具有唯一性。因此，我们定义了两个索引，一个索引用于`email`字段，另一个索引用于`name`字段，并将它们都设置为唯一。

总之，使用`unique=True`参数或在模型级别上使用`indexes`选项，可以轻松地确保MongoEngine模型中的字段或模型本身的值是唯一的。



### MongoEngine提供的嵌套文档

MongoEngine是一个Python的MongoDB 框架，提供了许多方便的操作MongoDB数据库的功能，其中之一就是支持嵌套文档。

嵌套文档是指在一个文档中嵌套了另一个文档，使得文档之间形成一定的层级结构，这样就可以更灵活地组织数据，同时也能够实现复杂的数据关系。

在MongoEngine中，定义一个嵌套文档和定义一个普通文档类似，只需要在字段上使用EmbeddedDocumentField类型即可。例如，定义一个嵌套在User文档中的Address文档：

```python
from mongoengine import Document, EmbeddedDocument, StringField, EmbeddedDocumentField

class Address(EmbeddedDocument):
    street = StringField(required=True)
    city = StringField(required=True)
    state = StringField(required=True)

class User(Document):
    name = StringField(required=True)
    address = EmbeddedDocumentField(Address)
```



可以看到，定义嵌套文档和普通文档的主要区别在于使用EmbeddedDocument类来定义嵌套文档，同时嵌套文档的定义需要在主文档中使用EmbeddedDocumentField类型来引用。

当使用MongoEngine进行数据库操作时，嵌套文档的操作与普通文档类似，只需要使用点号访问字段即可。例如，要获取一个用户的城市信息，可以如下面这样做：

```python
user = User.objects(name='Alice').first()
city = user.address.city
```



还可以使用嵌套文档实现数组形式的数据存储。例如，以下定义将在主文档中使用一个列表来嵌套存储多个Address文档：

```python
class User(Document):
    name = StringField(required=True)
    addresses = EmbeddedDocumentField(Address, multiple=True)
```



对于嵌套的列表类型，可以通过下标或迭代器访问每一个嵌套的文档，例如：

```python
user = User.objects(name='Alice').first()

# 访问第一个地址信息
first_address = user.addresses[0]

# 遍历所有地址信息
for address in user.addresses:
    print(address.city)
```



在MongoDB中，嵌套文档的存储方式与关系型数据库的外键不同，嵌套文档是完全嵌套在主文档中的，因此适用于小型、高度相关的数据和文档频繁被一次性读取或更新的场景。同时，也需要注意使用嵌套文档时，嵌套层数不要过深，否则容易导致查询性能下降。

### MongoEngine提供的聚合管道

MongoEngine是Python的一个MongoDB ODM库，它提供了一些用于聚合数据的工具。其中最常用的工具之一就是聚合管道（Aggregation Pipeline）。聚合管道是MongoDB中一种强大的聚合工具，允许您对文档进行基于多个阶段的处理，以生成分析结果。MongoEngine中的聚合管道利用MongoDB原生的聚合操作实现，可以实现复杂的聚合操作，例如分组、过滤、排序和限制等。

MongoEngine的聚合管道API类似于MongoDB聚合管道，它允许您使用流水线的方式组合多个操作步骤来处理数据。以下是MongoEngine提供的一些主要聚合管道：

1. $project：选择文档的子集，只返回指定字段。可以对选择的字段进行处理、重新命名或排除。
2. $match：对输入文档进行筛选，仅输出符合条件的文档。可以使用各种查询操作符构建复杂的条件。
3. $group：将文档分组到一个集合中，并对每个集合执行聚合操作。
4. $sort：将文档按指定字段进行排序。
5. $limit：限制输出文档的数量。
6. $skip：跳过指定数量的文档，以便进行分页操作。
7. $unwind：将由数组或嵌套文档组成的字段进行拆分和展开。
8. $lookup：实现左外连接操作，将一个集合中的文档与另一个集合中的文档进行关联。

通过使用这些操作，可以构建复杂的流水线来处理MongoDB中的文档，同时得出有用的分析结果。MongoEngine聚合管道提供了一个方便而强大的方式来使这些文档转换成有意义的数据。

以下是一些可以使用MongoEngine聚合管道执行的示例：

1. 计算特定字段的总数：

```python
from mongoengine import connect, Document, IntField
from mongoengine.aggregation import Aggregate

connect('test')

class Person(Document):
    age = IntField()

# 统计年龄总和
pipeline = [
    {'$group': {'_id': None, 'totalAge': {'$sum': '$age'}}},
    {'$project': {'_id': 0, 'totalAge': 1}}
]

result = Person.aggregate(*pipeline)
for doc in result:
    print(doc.totalAge)
```



1. 按字段分组并计算每组的平均值：

```python
from mongoengine import connect, Document, StringField, IntField
from mongoengine.aggregation import Aggregate

connect('test')

class Person(Document):
    name = StringField()
    age = IntField()

# 计算每个名字的平均年龄
pipeline = [
    {'$group': {'_id': '$name', 'avgAge': {'$avg': '$age'}}},
    {'$project': {'_id': 0, 'name': '$_id', 'avgAge': 1}}
]

result = Person.aggregate(*pipeline)
for doc in result:
    print(doc.name, doc.avgAge)
```



1. 对数据进行排序和筛选后取前5个：

```python
from mongoengine import connect, Document, StringField, IntField
from mongoengine.aggregation import Aggregate

connect('test')

class Person(Document):
    name = StringField()
    age = IntField()

# 获取年龄最大的5个人的名字和年龄
pipeline = [
    {'$sort': {'age': -1}},
    {'$limit': 5},
    {'$project': {'_id': 0, 'name': 1, 'age': 1}}
]

result = Person.aggregate(*pipeline)
for doc in result:
    print(doc.name, doc.age)
```



聚合管道功能使得从MongoDB数据库中提取和处理数据变得更加方便和高效。使用MongoEngine的聚合管道，开发人员可以轻松地执行复杂的聚合操作，而不需要编写大量的代码来实现这些操作。



### 更多查询

1.使用时先声明一个继承自MongoEngine.Document的类

在类中声明一些属性，相当于创建一个用来保存数据的数据结构，即数据已类似数据结构的形式存入数据库中，通常把这样的一些类都存放在一个脚本中，作为应用的Model模块

```py
from mongoengine import *
connect('mydb', host='localhost', port=27017)
import datetime
class Users(Document):
    name = StringField(required=True, max_length=200)
    age = IntField(required=True)

users = Users.objects.all() #返回所有的文档对象列表
for u in users:
    print("name:",u.name,",age:",u.age)
```



2.保存文档

required：设置必须；

default：如果没有其他值给出使用指定的默认值

unique：确保集合中没有其他document有此字段的值相同

choices：确保该字段的值等于数组中的给定值之一

```py
from mongoengine import *
connect('mydb', host='localhost', port=27017)
import datetime
class Users(Document):
    name = StringField(required=True, max_length=200)
    age = IntField(required=True)
user1 = Users(
    name='jack',
    age= 21
)
user1.save()   
print(user1.name)
user1.name = 'jack2'
user1.save()       
print(user1.name)
```

3.查询10=<年龄<30的，按姓名排列

```py
from mongoengine import *
connect('mydb', host='localhost', port=27017)
import datetime
class Users(Document):
    name = StringField(required=True, max_length=200)
    age = IntField(required=True)
user_search = Users.objects(age__gte=10, age__lt=30).order_by('name')
for u in user_search:
    print("name:",u.name,",age:",u.age)
```

查询10=<年龄<30的，按姓名倒序

```py
from mongoengine import *
connect('mydb', host='localhost', port=27017)
import datetime
class Users(Document):
    name = StringField(required=True, max_length=200)
    age = IntField(required=True)
user_search = Users.objects(age__gte=10, age__lt=30).order_by('-name')
for u in user_search:
    print("name:",u.name,",age:",u.age)
```

查询name=jack2

```py
from mongoengine import *
connect('mydb', host='localhost', port=27017)
import datetime
class Users(Document):
    name = StringField(required=True, max_length=200)
    age = IntField(required=True)

tmp = Users.objects(name="jack2")
for u in tmp:
    print("name:",u.name,",age:",u.age)
```



4.修改name=jack2 的age加1

```py
from mongoengine import *
connect('mydb', host='localhost', port=27017)
import datetime
class Users(Document):
    name = StringField(required=True, max_length=200)
    age = IntField(required=True)
tmp = Users.objects(name="jack3").update(inc__age=1)
tmp = Users.objects(name="jack3")
for u in tmp:
    print("name:",u.name,",age:",u.age)
```


修改name=jack的age设为66

```python
from mongoengine import *
connect('mydb', host='localhost', port=27017)
import datetime
class Users(Document):
    name = StringField(required=True, max_length=200)
    age = IntField(required=True)

tmp = Users.objects(name="jack").update(set__age=66)
tmp = Users.objects(name="jack")
for u in tmp:
    print("name:",u.name,",age:",u.age)
```

高级查询
例如有时候你需要将约束条件进行与，或的操作。你可以使用mongoengine提供的 Q 类来实现，一个 Q 类代表了一个查询的一部分，里面的参数设置与你查询document的时候相同。建立一个复杂查询的时候，你需要用 & 或 | 操作符将 Q 对象连结起来，例子如下：

Post.objects(Q(name=“jack”) | Q(age=66))

查询相关操作符

```python
ne – 不等于
lt – 小于
lte – 小于等于
gt – 大于
gte – 大于等于
not – 使其检查的反面，需要使用在其他操作符之前(e.g. Q(age__not__mod=5))
in – 值在list里面
nin – 值不在list里面
mod – value % x == y
all – list里面所有的值
size – 这个array的大小
exists – 存在这个值
#一下操作符在需要进行正则检查的时候是比较快捷的方法：
exact – 字符串型字段完全匹配这个值
iexact – 字符串型字段完全匹配这个值（大小写敏感）
contains – 字符串字段包含这个值
icontains –字符串字段包含这个值（大小写敏感）
startswith – 字符串字段由这个值开头
istartswith –字符串字段由这个值开头（大小写敏感）
endswith – 字符串字段由这个值结尾
iendswith –字符串字段由这个值结尾（大小写敏感）
match – 使你可以使用一整个document与数组进行匹配查询list
```

## 新建一个用户表

```python
from mongoengine import connect,Document,IntField,StringField,DateTimeField,UUIDField,EmailField
import uuid
import datetime

# connect(db='user', host='localhost', port=27017)
connect(db='Bt', username='Bent', password='9', host='309', port=27017, authSource='Bint')

class User(Document):
    username = StringField(required=True,unique=True)
    password = StringField(required=True)
    email = EmailField(sparse=True,unique=True)
    name = StringField(default='Unknown')
    birthday = DateTimeField()
    gender = StringField(choices=('Male', 'Female', 'Unknown'),default='Unknown')
    phone_number = StringField(default='Unknown')
    address = StringField(default='Unknown')
    nationality = StringField(default='China')
    registered_time = DateTimeField(default=datetime.datetime.now())
    last_login_time = DateTimeField(default=datetime.datetime.now())
    avatar = StringField(default='https://img2.baidu.com/it/u=3618236253,1028428296&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=500')
    account_status = StringField(choices=('Enabled', 'Disabled'),default='Enabled')
    account_type = StringField(choices=('Normal', 'Admin'),default='Normal')

# 创建一个用户对象并保存到MongoDB
user = User(username='Alic11111zaea', password='password123')
user.save()
```

## mongoengine查找url以jpg结尾的数据

要使用MongoEngine查找数据库中URL以jpg结尾的数据，可以使用正则表达式来匹配URL，并使用MongoEngine的`icontains`运算符进行不区分大小写的模糊查询。

以下是一个使用MongoEngine查找数据库中URL以jpg结尾的数据的示例：

```python
from mongoengine import *

connect("mydatabase")

class Image(Document):
    url = StringField(required=True)

images = Image.objects(url__iregex=r"\.jpg$")  # 使用正则表达式查找以jpg结尾的URL
for image in images:
    print(image.url)
```



在这个例子中，我们定义了一个名为`Image`的模型类，其中包含一个名为`url`的字符串属性。我们使用`__iregex`运算符对`url`属性进行不区分大小写的正则表达式匹配，以查找URL以`.jpg`结尾的文档。最后，我们在for循环中打印出找到的每个文档的`url`属性。

请注意，正则表达式的`$`字符匹配字符串的结尾部分。

另外，MongoEngine还提供了`__iendswith`运算符，这个运算符用于快速匹配字符串的结尾部分。在这个例子中，使用`__iendswith`运算符可以直接查找URL以`.jpg`结尾的文档，如下所示：

```python
images = Image.objects(url__iendswith=".jpg")
```



希望这个解释能够帮助你使用MongoEngine查找数据库中URL以jpg结尾的数据。

## 输出一个mongoengine对象的字符串形式

```python
person = Person(name="John Smith", age=30)
person.save()

print(person.to_json())
```

## 添加索引

在使用MongoEngine连接MongoDB时，你可以通过在模型定义中设置索引来优化查询性能。MongoEngine提供了`index`参数，用于指定要为哪些字段创建索引。

以下是一些示例，展示如何在MongoEngine模型中设置索引：

```python
pythonCopy codefrom mongoengine import connect, Document, StringField, IntField

# 连接MongoDB数据库
connect('your_database_name')

# 定义模型
class MyModel(Document):
    field1 = StringField(required=True)
    field2 = IntField()
    field3 = StringField()

    # 设置索引
    meta = {
        'indexes': [
            'field1',  # 单字段索引
            ('field2', 'field3'),  # 复合索引
        ]
    }
```

在上述示例中，我们定义了一个名为`MyModel`的模型，其中包含了几个字段`field1`、`field2`和`field3`。通过在模型的`meta`属性中设置`indexes`，我们可以指定要创建的索引。

- `field1`字段上的索引是一个简单的单字段索引。在查询中涉及到`field1`字段时，这个索引可以提高查询性能。
- `('field2', 'field3')`字段上的索引是一个复合索引，包含了两个字段。复合索引可以提高涉及到这两个字段的查询性能。

设置索引后，MongoEngine会在数据库中创建对应的索引，以优化查询性能。索引可以加速查询，并提高数据的读取效率。

请注意，索引的使用应根据具体的查询模式和数据访问模式进行优化。在设置索引时，需要考虑到查询的频率、字段的选择性、查询条件的复杂度等因素。

通过合理地设置索引，可以大大提高查询性能。在设计数据模型时，根据实际的查询需求和数据访问模式，选择合适的字段来创建索引，以获得更好的性能。

## 根据timestamp字段查询近几日数据量

```python
def get_daily_count(x):
    current_date = datetime.now().replace(hour=0, minute=0, second=0, microsecond=0)
    # 构建聚合查询 pipeline
    pipeline = [
        {"$match": {
            "timestamp": {
                "$gte": current_date - timedelta(days=x-1),
                "$lt": current_date + timedelta(days=1)
            }
        }},
        {"$group": {
            "_id": {
                "year": {"$year": "$timestamp"},
                "month": {"$month": "$timestamp"},
                "day": {"$dayOfMonth": "$timestamp"}
            },
            "count": {"$sum": 1}
        }},
        {"$project": {
            "_id": 0,
            "year": "$_id.year",
            "month": "$_id.month",
            "day": "$_id.day",
            "count": 1
        }},
        {"$sort": {"year": 1, "month": 1, "day": 1}}
    ]
    result = list(Page.objects.aggregate(*pipeline))
    date_range = [(current_date - timedelta(days=i)).date() for i in range(x-1, -1, -1)]
    date_dict = {date: 0 for date in date_range}

    for entry in result:
        date = datetime(entry["year"], entry["month"], entry["day"]).date()
        count = entry["count"]
        date_dict[date] = count
    return [(str(date),cnt) for date,cnt in date_dict.items()]
```

## 切换集合，获取数据等

```python
#获取要操作的集合名
collection_name=get_collection_name(url,type=type)
print(collection_name)
#切换集合
collection=Link().switch_collection(collection_name)
print(collection)
#huo'qu'ji
docs=collection._get_collection().find({})
for document in docs:
    # 处理每个文档
    print(document)
```

```python
# try:
#     Link(url=url, visited=visited).save()
#     print(f'{url}请求成功，保存完毕')
# except:
#     Link.objects(url=url).update(visited=visited)
#     print(f'{url}请求成功，更新完毕')
```

```python
db=Test()._get_db()
cols=db.list_collection_names()
```

对于MongoEngine库而言，直接使用`_get_db()`和`_get_collection()`方法获取的集合和通过`Test().switch_collections()`方法获取的集合对象是不同的。

`_get_db()`和`_get_collection()`方法是MongoEngine库中的内部方法，用于获取数据库和集合对象。这两个方法返回的是MongoDB原生驱动（如PyMongo）中的对象，而不是MongoEngine库中提供的`Document`类的实例。

而`Test()`对象和`switch_collections()`方法是在自定义的类中实现的。根据`Test()`类和其中的`switch_collections()`方法的具体实现，`switch_collections()`方法可能是通过MongoEngine库的API，通过其他方法获取集合对象。

因此，直接使用`_get_db()`和`_get_collection()`方法获取的集合与通过`Test().switch_collections()`方法获取的集合对象很可能是不同的。要确定两者是否相同，需要查看`Test()`类和其中的`switch_collections()`方法的具体实现。

获取数据库获取集合再插入数据：

```python
db=Test()._get_db()['test']
a=db.insert_one({'url':111})
```

对象换集合：

```python
db=Test().switch_collection('111')._get_collection()
a=db.insert_one({'url':111})
```

## 遍历集合所有数据

```python
collection = db['mycollection']

# 查询_idzui'xiao一条数据
data = collection.find_one({}, sort=[("_id", pymongo.ASCENDING)])
while data:
    # 处理数据
    print(data)

    # 查询下一条数据
    data = collection.find_one({"_id": {"$gt": data["_id"]}})
```

更新

```python
filter = {"name": "John"}  # 指定要更新的文档条件
update = {"$set": {"age": 30}}  # 更新的字段和值
result = collection.update_one(filter, update)
```
