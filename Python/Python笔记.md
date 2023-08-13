# Python笔记

## 编码声明

```python
# -*- coding: utf-8 -*-
```



> 下载python库一直失败，报hostname出错的问题
>
> 可能是因为你电脑开了代理



> 函数默认参数为对象时，只会加载一次
>
> selenium对象池获取对象时，只调用一次获取对象方法

## 基础

### 词频统计

```python
# 招聘日期
times=[]
word_dict = {}
for word in times:
    word=datetime.datetime.fromtimestamp(int(word)).strftime("%Y-%m-%d")
    if word not in word_dict:
        word_dict[word] = 1
    else:
        word_dict[word] += 1
sorted_word_dict = sorted(word_dict.items(), key=lambda x: x[0], reverse=False)
```

词频统计并显示词云图

```python
import jieba
from collections import Counter

words = jieba.cut(text) # 进行分词
stopwords=['一个','不是','这个','就是','什么','没有','为了','一下','一种','一股']
words=[i for i in words if i and len(i)>1 and i not in stopwords]
word_count = Counter(words) # 统计词频

print(word_count)
ls=word_count.most_common()
ls = filter(lambda x: '\u4e00' <= x[0] <= '\u9fa5', ls)
from pyecharts import options as opts
from pyecharts.charts import WordCloud
from pyecharts.globals import SymbolType


words = ls
c = (
    WordCloud()
    .add("", words, word_size_range=[20, 100], shape=SymbolType.DIAMOND)
    .set_global_opts(title_opts=opts.TitleOpts(title="5.评论词云分析-知乎"))
    .render("5.评论词云分析-知乎.html")
)
```



### 字典

```python
#创建空字典
dic1={}

dic2=dic()

#添加值
dic1[1]=1
dic1[2]=2

#清除值
del dic1[1]
dic1.clear()

#查询值
value=dict1["2"]

#有则返回值，无则插入默认值
dic1.setdefault(1,111)
#有则返回值，无则返回默认值
dic1.get(1,222)

dict1.keys()
dict1.values()
dict1.items

#下标索引
for i in enumerate(dict1):
    print(i)
```





## 常用库

### os

```python
#获取当前路径
os.path.abspath('.')
```

```
os.path.exists(path) 判断一个目录是否存在

os.makedirs(path) 多层创建目录

os.mkdir(path) 创建目录
```



### sys



当前python解释器路径

```
import sys
print(sys.executable)
```



```
import os
os.system('node script.js')
```

### json

```
python在使用json这个模块前，首先要导入json库：import json.

方法	描述
json.dumps()	将 Python 对象编码成 JSON 字符串
json.loads()	将已编码的 JSON 字符串解码为 Python 对象
json.dump()	将Python内置类型序列化为json对象后写入文件
json.load()	读取文件中json形式的字符串元素转化为Python类型
注意：不带s的是序列化到文件或者从文件反序列化，带s的都是内存操作不涉及持久化。
```

```
with open('article.json','w') as f:
    json.dump(articles,f)
```

```
with open('article.json','r') as f:
    articles=json.load(f)
```

### pyecharts

标题居中

```python
from pyecharts.charts import Line
from pyecharts import options as opts

# 创建一个 Line 实例
line_chart = Line()

# 设置图表标题并设置标题位置为居中
line_chart.set_global_opts(title_opts=opts.TitleOpts(title="图表标题", pos_left="center"))

# 添加数据
line_chart.add_xaxis(["a", "b", "c", "d", "e"])
line_chart.add_yaxis("系列1", [1, 3, 5, 7, 9])
line_chart.add_yaxis("系列2", [2, 4, 6, 8, 10])

# 渲染图表到 HTML 文件
line_chart.render("line_chart.html")
```

子标题居中

```python
bar = Bar()
bar.add_xaxis(['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'])
bar.add_yaxis('Category A', [820, 932, 901, 934, 1290, 1330, 1320])
bar.add_yaxis('Category B', [320, 332, 401, 334, 390, 330, 320])
bar.set_global_opts(
    title_opts=opts.TitleOpts(title="Bar Chart", 
                              subtitle="Example Subtitle",
                              title_textstyle_opts=opts.TextStyleOpts(font_size=24),
                              pos_left='center', 
                              subtext_style=opts.TextStyleOpts(font_size=18, text_align="center")),
)
```

图例显示在右方

要将饼形图的图例显示在右方，可以使用 Pyecharts 的 `set_global_opts()` 方法来设置全局参数 `legend_opts`。其中，`legend_opts` 是一个字典类型，用于设置图例相关的属性。可以通过指定 `orient` 参数的值来设置图例的排列方向。将 `orient` 设置为 ‘vertical’，可以让图例显示在右方。

下面是将图例显示在右方的代码示例：

```python
from pyecharts.charts import Pie
from pyecharts import options as opts

# 原始数据
raw_data = [('3', 465), ('经验不限', 464), ('1', 348), ('5', 168), ('1-3年', 55), ('3-5年', 52), ('5-7年', 6), ('10', 6), ('7年以上', 3), ('5-10年', 1)]

# 计算每个元素出现的频率
total = sum([count for _, count in raw_data])
data = [(name, count/total) for name, count in raw_data]

# 创建图表实例并添加数据
pie = (Pie()
       .add("", data)
       .set_series_opts(label_opts=opts.LabelOpts(formatter="{b}: {d:.2%}"))
       .set_global_opts(title_opts=opts.TitleOpts(title="数据占比"),
                        legend_opts=opts.LegendOpts(type_="scroll", orient="vertical", pos_right="5%", pos_top="20%"))
       )

# 渲染图表到 HTML 文件
pie.render("pie_chart.html")
```



在上述代码中，我们在 `set_global_opts()` 中使用了 `legend_opts` 来设置图例的相关属性。其中，`type_` 参数设置为 “scroll”，表示当图例过多时，图例可以通过滚动来浏览；`orient` 参数设置为 “vertical”，使图例竖直排列；`pos_right` 参数设置为 “5%”，将图例的位置设置到饼图右边，距离饼图页面右边界的距离为 5%；`pos_top` 参数设置为 “20%”，表示图例距离饼图页面顶部的距离为 20%。可以根据实际需求适当调整这些参数。

运行上述代码后，将生成一个带有竖直图例的饼图 HTML 文件，图例显示在了饼图的右侧。



输出为图片

```python
from pyecharts.charts import Line
from pyecharts import options as opts
from selenium import webdriver

# 创建一个 Line 实例
line_chart = Line()

# 添加数据
line_chart.add_xaxis(["a", "b", "c", "d", "e"])
line_chart.add_yaxis("系列1", [1, 3, 5, 7, 9])
line_chart.add_yaxis("系列2", [2, 4, 6, 8, 10])

# 设置图表标题并设置标题位置为居中
line_chart.set_global_opts(title_opts=opts.TitleOpts(title="图表标题", pos_left="center"))

# 渲染图表到 HTML 文件
line_chart.render("line_chart.html")

# 使用 selenium 打开 HTML 文件并截图为 PNG 图片
options = webdriver.ChromeOptions()
options.add_argument("--disable-dev-shm-usage")
options.add_argument("--no-sandbox")
options.add_argument("--disable-gpu")
options.add_argument("--headless")
driver = webdriver.Chrome(options=options)
driver.get("file:///path/to/line_chart.html")
driver.save_screenshot("line_chart.png")
driver.quit()
```

最高薪资最低薪资箱线图

```python
# 最低薪资和最高薪资数据
data = [minimumWage_list,maximumWage_list]

# 创建一个 Boxplot 实例
boxplot = Boxplot()

# 添加数据
boxplot.add_xaxis(["最低薪资", "最高薪资"])
boxplot.add_yaxis("", boxplot.prepare_data(data))

# 设置图表标题
boxplot.set_global_opts(title_opts={"text": "最低薪资与最高薪资分布箱线图"})

# 渲染图表到 HTML 文件
boxplot.render("招聘信息-月薪箱线图.html")
```









### pipreqs

在当前目录使用生成

```python
pipreqs ./ --encoding=utf8 --force
```

> `--encoding=utf8` ：为使用utf8编码
>
> `--force` ：强制执行，当 生成目录下的requirements.txt存在时覆盖 
>
> **.** /: 在哪个文件生成requirements.txt 文件

### fastapi

创建基本程序

```python
from Spider.logs import *
from Spider.config import *
from pydantic import BaseModel
from fastapi import FastAPI
import uvicorn

app = FastAPI(title="全网爬虫",
              description="这是一个网页爬虫的项目",
              docs_url="/docs",
              openapi_url="/openapi")

class Item(BaseModel):
    auth: str
    query: str
# @app.get('/')

@app.post('/search')
def search_(item:Item):
    if item.auth=='':
        return 1
    else:
        return 2

if __name__ == "__main__":
    uvicorn.run("__init__:app", host="0.0.0.0", port=5000)
```

挂载静态资源

```python
app.mount("/templates", StaticFiles(directory=os.path.join(os.path.dirname(__file__),'templates')), name="templates")
app.mount("/static", StaticFiles(directory=os.path.join(os.path.dirname(__file__),'static')), name="static")
```



### re

#### 匹配数字

```python
re.findall('(\d+)','5-10年')
```



```python
import re
ret = re.findall(r"\d+", "python = 9999, c = 7890, c++ = 12345")
print(ret)
```

#### 判断字符串是否含英文

```python
import re
def containEnglist(str_):
    '''判断字符串中是否含有英文'''
    return bool(re.search('[a-z]',str_))
containEnglist('弄完崇拜我北侧二百完成a')
```

#### 网页代码处理

```python
def clean_html(html: str) -> str:
    """清除html文本的html标记."""
    # 1.去除js，css，html描述，html标签
    cleaned_html = re.sub(
        r"(?is)<(script|style).*?>.*?(</\1>)|<!--(.*?)-->[\n]?|<(?s).*?>", "", html.strip()
    )

    # 2.处理空格和HTML实体zi'fu
    cleaned_html = re.sub(
        r"&nbsp;|  |\t|&.*?;[0-9]*&.*?;|&.*?;", "", cleaned_html
    )

    # 3.文本规范化
    # cleaned_html = ucd.normalize('NFKC', cleaned_html).replace(' ', '')

    return cleaned_html.strip()
```



1. ）这段代码使用了 Python 的 re 模块，对给定的 HTML 文本进行了正则表达式匹配和替换。

具体来说，该代码实现了以下正则表达式操作：

1. 针对脚本和样式标签以及其内容的匹配：

```python
(?is)<(script|style).*?>.*?(</\1>)
```



这个正则表达式匹配一个 `<script>` 或 `<style>` 标签，后面跟着零个或多个任意字符，然后匹配该标签的结束标记 `</script>` 或 `</style>`。其中 `(?is)` 是 re 模块对正则表达式的选项设置，`s` 表示在匹配时忽略换行符，`i` 表示不区分大小写。`\1` 是反向引用，它引用了前面的捕获组 `(script|style)` 中匹配的文本。

1. 对注释内容的匹配：

```python
<!--(.*?)-->[\n]?
```



这个正则表达式匹配 HTML 中的注释内容，其中 `(.*?)` 是一个非贪婪模式，表示匹配任何字符（点号）零个或多个，直到遇到闭合标记 `-->`。 `[\n]?` 匹配可选的换行符（如果有的话）。

1. 对标签的匹配：

```python
<(?s).*?>
```



这个正则表达式匹配所有的 HTML 标签，包括嵌套标签、不带属性的标签，以及其它密集的标签格式。`(?s)` 表示在匹配时不区分换行符和空格。

完整的代码：

```python
import re

html = '<p>Some text <script>alert("Hello");</script> surrounded by<br> whitespace   \t</p><!-- This is a comment -->'
cleaned_html = re.sub(r"(?is)<(script|style).*?>.*?(</\1>)|<!--(.*?)-->[\n]?|<(?s).*?>", "", html.strip())

print(cleaned_html) #输出：“Some text surrounded by whitespace”
```



在上面的示例中，我们将 HTML 文本传入 `re.sub()` 函数中，并使用正则表达式进行匹配和替换，最终删除了 HTML 文本中的脚本、样式、注释等元素，只留下纯文本。

2.）这段代码使用了正则表达式对 `cleaned_html` 中的字符串进行匹配替换。具体来说，该正则表达式匹配以下字符或字符串：

1. ` ` : 匹配 HTML 中的非断行空格。
2. ``: 匹配一个空格字符。
3. `\t` : 匹配一个 tab 字符。
4. `&.*?;[0-9]*&.*?;` : 匹配 HTML 中的特殊字符实体，包括它们的实体格式（`&...;`）。例如：` `, `©`。
5. `&.*?;` : 匹配 HTML 中的其他特殊字符实体。

`re.sub()` 方法的第一个参数是正则表达式，第二个参数是要替换匹配部分的新文本，第三个参数是被搜索和替换的原始文本。

这段代码的作用是将文本中的特殊字符和空格替换为一个空字符串，从而去除所有的空格和特殊字符，只剩下文本内容。替换后的文本将被返回。

以下是一个示例：

```python
import re

cleaned_html = "This is a    sample &nbsp; text.&copy; &lt;html&gt;"
cleaned_text = re.sub(r"&nbsp;|  |\t|&.*?;[0-9]*&.*?;|&.*?;", "", cleaned_html)

print(cleaned_text)  # 输出："Thisisasampletext."
```



在上面的示例中，该代码将输入字符串 `cleaned_html` 中的特殊字符和空格替换为一个空字符串，最终返回一个只有文本内容的字符串 `cleaned_text`。

3.）这段代码用于标准化 `cleaned_html` 中的文本，并移除其中的空格。具体来说，它包含两个步骤。

1. `ucd.normalize('NFKC', cleaned_html)` 用于将文本进行 Unicode 规范化，使得其中的字符能够以一致的方式表示。其中 `NFKC` 表示一个 Unicode 规范化形式，在这个规范形式下，文本中的字符被转换为其兼容表现形式并组合可能的字符。
2. `.replace(' ', '')` 用于删除文本中的所有空格（包括空格、制表符和换行符）。

以下是一个示例：

```
import unicodedata as ucd

cleaned_html = "This is\n a\t   test."
normalized_text = ucd.normalize('NFKC', cleaned_html).replace(' ', '')

print(normalized_text)  # 输出："Thisisa    test."
```



在上面的示例中，代码首先使用 `normalize()` 方法将文本进行 Unicode 规范化，然后使用 `replace()` 方法删除其中的空格。最终返回一个只包含字符和制表符的字符串 `normalized_text`。

### starlette 



### timeit

`timeit` 是 Python 标准库中用于测量代码执行时间的模块。该模块提供了 `timeit()` 函数，可以在代码执行时测量时间，并输出每次运行代码所用的时间、总时间、平均时间和标准偏差等统计信息。

通常，你可以使用 `timeit` 模块来测试程序中重复运行的瓶颈代码和算法在多个输入上的速度表现。以下是一个 `timeit` 模块的示例：

```python
import timeit

def test_function():
    # 这里是测试代码或函数
    pass

if __name__ == '__main__':
    # 测试代码块执行1000次
    result = timeit.timeit(test_function, number=1000)
    print(f"总共用时：{result} 秒")
```



在这个例子中，我们先定义了一个 `test_function()` 函数，它是我们要测试的代码或函数。然后，在主程序中，我们使用 `timeit.timeit()` 函数来测试运行 `test_function()` 函数 1000 次的总时间。默认情况下，`timeit.timeit()` 函数运行代码一百万次。我们可以通过 `number` 参数设置要运行的次数。

需要注意的是，`timeit.timeit()` 函数执行次数越多，产生的估计时间就越准确。然而，如果函数执行的时间很短，可能会出现估计时间偏差的情况，因为计时器的精度有限。在这种情况下，可以通过增加重复次数的数量来减少估计误差。

此外，`timeit` 模块还提供了其他用于计时的函数，比如 `default_timer()` 用于测量时间和 `repeat()` 用于反复测量相同的代码块以获得更准确的结果。

### time



### threading

Python中的multiprocessing和threading模块都可以用于实现多线程编程，但是它们之间有一些区别。

multiprocessing模块是Python中的一个多进程处理模块，它提供了与threading模块类似的API，但是它使用多个进程而不是多个线程来执行任务。这使得multiprocessing模块可以更好地利用多核CPU，从而提高程序的性能。另外，由于每个进程都有自己独立的内存空间，因此在使用multiprocessing模块时不需要担心线程安全问题。

相比之下，threading模块则是Python中的一个多线程处理模块。与multiprocessing模块不同，threading模块使用多个线程而不是多个进程来执行任务。由于线程共享同一进程的内存空间，因此在使用threading模块时需要注意线程安全问题。

总的来说，如果你需要利用多核CPU来提高程序性能，并且不想担心线程安全问题，那么可以考虑使用multiprocessing模块。如果你只需要在单个CPU上执行任务，并且希望更轻量级地实现多线程编程，那么可以考虑使用threading模块。



### pymongo

```python
import pymongo
# 连接 MongoDB
client = pymongo.MongoClient('39.101.74', 27017, username='BigDatent', password='9', authSource='Bigntent')
# 获取数据库
db = client['BigData-Mountent']
# 获取集合实例
collection = db["mountain"]

l=[]

import json
with open('result.json','r',encoding='utf-8') as f:
    a=json.loads(f.read())

for i in a:
    l.append(i)

# res = collection.insert_one(doc)
res = collection.insert_many(l)
print(res.inserted_ids)
```



### logging

在 Python 中，可以使用 logging 模块来输出日志信息。logging 模块可以将日志输出到控制台、文件等位置，并可以控制日志的输出级别，十分灵活。

以下是输出日志到文件的示例代码：

```python
import logging

# 配置日志信息
logging.basicConfig(filename='1.log', level=logging.DEBUG, format='%(asctime)s %(levelname)s:%(message)s')

# 输出日志信息
logging.debug('This is a debug message')
logging.info('This is a info message')
logging.warning('This is a warning message')
logging.error('This is an error message')
logging.critical('This is a critical message')
```

在上述代码中，首先使用 logging.basicConfig() 方法来配置日志信息。这个方法定义了日志的输出位置、输出级别和格式。在这个例子中，我们将日志信息输出到文件 example.log 中，日志级别为 DEBUG，即输出所有级别的日志信息。输出格式采用了 asctime、levelname 和 message 三个变量，分别表示日志输出时间、日志级别和日志信息。

接下来，使用 logging.debug()、logging.info()、logging.warning()、logging.error() 和 logging.critical() 方法来输出不同级别的日志信息。

在运行这个程序后，日志信息会被输出到指定的文件 example.log 中。

注意：如果指定的文件不存在，则会自动创建。如果文件已经存在，则新的日志信息会被追加到文件的末尾。同时，需要注意写入权限问题，如果没有权限写入指定的目录或文件，则会抛出 IOError 异常。



### pandas

数据转DataFrame

```
a={
    '姓名':['张三','李四','王五'],
    '年龄':['18','15','22'],
    '身高':['186','175','156']
}
a_=pd.DataFrame(a)
a_.to_excel('人员信息.xlsx',index=False,header=None )
# 张三	18	186
# 李四	15	175
# 王五	22	156
```





```
查看某列是否有空值
data.loc[data['列名'].isnull()]

每一列缺失值进行统计
data.isnull().sum()

填充缺失值,用0填充
data.fina(0)

查看是否有空值
data.isnull().any()
```

#### 替换表头

```python
df = pd.read_csv('train.csv', names=['乘客ID','是否幸存','仓位等级','姓名','性别','年龄','兄弟姐妹个数','父母子女个数','船票信息','票价','客舱','登船港口'],index_col='乘客ID',header=0)
df.head()
```



#### 删除满足条件的行

```python
df = df.drop(df[(df.age < 25) | (df.age >= 30)].index)
```



#### 删除某一列

```python
#1.
del df['column-name']

#采用drop方法，有下面三种等价的表达式：
df,drop('num',axix=1),不改变内存，及输入df的时候，它还是显示原数据
df.drop('num',axix=1，inplace=True),改变内存，及输入df的时候，它显示改变后的数据
df_处理.drop(['热度值','热度单位','类别1','类别2'],axis=1,inplace=True)
df.drop([df.columns[[0,1]]],axis=1,inpalce=True)
```



```python
#列长度
pd['列名'].value_counts() 
#某列满足条件的列和
a=df[(floor<=df['年龄']) & (df['年龄']<=ceiling)]['信用卡负债'].sum()

#添加总分列
#按行求和，添加总分列
score["总分"] = score[['高数','英语','Python']].sum(axis =1)
#score["总分"] = score[['高数','英语','Python']].apply(lambda x:x.sum(),axis =1)
students['等第']=students.apply(lambda x:x.sum(),axis=0)

score
#添加行
score.loc['合计']=score[['高数','英语','Python','总分']].sum(axis=0)
score.loc['平均分']=score[['高数','英语','Python','总分']].mean(axis=0)
score.loc['标准差']=score[['高数','英语','Python','总分']].std(axis=0)
score
```

#### 根据某列的值判断新加列的值，如根据总分列决定及格不及格

```python
students['等第']=students['总分']
def f(s):
    if s>=400:
        return '优秀'
    elif s>=300:
        return '良好'
    elif s>=260:
        return '中'
    elif s>=200:
        return '及格'
    else:
        return '不及格'
students['等第']=students['等第'].apply(f)
```

#### 按照某列排序

```
students.sort_values('总分',inplace=True,ascending=False)
inplace为False时，不改变原df，
a's'c
```

#### reset_index()

```
df.index = range(len(df))  # 重置索引
```

| **drop**    | **布尔值，如果为False，则将替换的索引列添加到数据中。**   |
| ----------- | --------------------------------------------------------- |
| **inplace** | **布尔值，如果为True，则对原始 DataFrame 本身进行更改。** |

#### 提取数字并过滤

```python
df_清洗 = df_清洗.drop(df_清洗[df_清洗['字数'].str.replace('万字','').astype(float) < 500].index)
```





#### 将df的列顺序改变

要更改数据框中列的顺序，您可以使用 Pandas 中的 `reindex()` 方法。以下是一个简单的示例：

```python
import pandas as pd

# 创建一个示例数据框
data = {'A': [1, 2, 3], 'B': [4, 5, 6], 'C': [7, 8, 9]}
df = pd.DataFrame(data)

# 输出原始数据框
print(df)

# 声明一个新的列序列（按新序）
new_order = ['C', 'A', 'B']

# 使用 reindex() 方法重新排序列
df = df.reindex(columns=new_order)

# 输出重新排序后的数据框
print(df)
```



在这个例子中，首先我们创建了一个示例数据框，并将其输出以供查看。然后，我们声明一个新的列名称序列，并使用 `reindex()` 方法按照新的顺序重新排序了数据框的列。最后，我们输出了重新排序的数据框。

请注意，`reindex()` 方法不会更改原始数据框的顺序，而是返回一个重新排序的副本，因此您需要将其分配回原始数据框或将其分配给另一个新的数据框变量。

#### 将df某一列提取在前面

要将数据框中的某一列移动到第一列，可以使用 Pandas 中的 `reindex()` 方法和列索引。

以下是一个简单的例子：

```python
import pandas as pd

# 创建一个示例数据框
data = {'A': [1, 2, 3], 'B': [4, 5, 6], 'C': [7, 8, 9]}
df = pd.DataFrame(data)

# 输出原始数据框
print(df)

# 将 B 列移动到第一列
new_order = ['B'] + [col for col in df.columns if col != 'B']
df = df.reindex(columns=new_order)

# 输出移动后的数据框
print(df)
```

在这个例子中，首先创建了一个示例数据框，并将其输出以供查看。然后，我们声明一个新的列名称序列，其中 `B` 列是第一个元素，而其他列顺序不变。在新的顺序定义完成后，我们使用 `reindex()` 方法重新排序了数据框的列，并将其分配给原始数据框。最后，我们再次输出了数据框以供查看。

请注意， `reindex()` 方法不会更改原始数据框，而是返回一个重新排序的副本，因此您需要将其分配回原始数据框或将其分配给另一个新的数据框变量。

#### df添加索引列，从一开始



要将从 1 开始的索引列添加到 Pandas 数据框中，可以使用 Pandas 中的 `reset_index()` 方法。以下是一个简单的示例：

```python
import pandas as pd

# 创建一个示例数据框
data = {'A': [1, 2, 3], 'B': [4, 5, 6], 'C': [7, 8, 9]}
df = pd.DataFrame(data)

# 输出原始数据框
print(df)

# 使用 reset_index() 方法添加从 1 开始的索引列
df.reset_index(drop=False, inplace=True)
df['index'] = df['index'] + 1

# 重新命名索引列的名称
df = df.rename(columns={'index': 'new_index'})

# 输出新数据框
print(df)
```



在这个例子中，我们首先创建了一个示例数据框，并将其输出以供查看。然后，我们使用 `reset_index()` 方法添加了新的索引列，并将 `drop=False` 以保留原始索引列。在添加新索引列后，我们将新的索引列中的所有值加 1，以便从 1 开始。最后，我们使用 `rename()` 方法将新索引列重命名为 `new_index`。

请注意，`reset_index()` 方法需要在索引列之前添加一个新的整数列，以便在保留原始索引的情况下添加一个新的索引列。如果 `drop=True`，则原始索引列将被删除。在本例中，我们选择保留原始索引列并添加一个新的索引列。最后请注意：使用`inplace=True`直接在原数据帧上操作，使代码更简洁。

### matplotlib

show前savefig,不然是白色

```
import matplotlib.pyplot as plt

plt.rcParams['font.family'] = ['sans-serif']
plt.rcParams['font.sans-serif'] = ['SimHei']

x = range(2,26,2)
y = [15,13,14.5,17,20,25,26,26,27,22,18,15]
 
# 绘图
plt.plot(x,y)
# 显示
plt.show()
```

设置x轴间隔

```
from matplotlib.pyplot import MultipleLocator

x_spliter=MultipleLocator(3)
ax=plt.gca()
ax.xaxis.set_major_locator(x_spliter)
```

### collections

`collections` 是 Python 标准库中一个非常实用的模块，提供了许多集合数据类型的实现，包括 OrderedDict、defaultdict、Counter 等，也有一些辅助函数。

以下是 `collections` 中一些常用的数据类型和函数：

1. `defaultdict`：一个字典类型，用于创建默认值为指定类型的字典。当使用一个不存在的键时，会返回该类型的默认值，而不是引发 KeyError 异常。
2. `OrderedDict`：从 Python 3.7 起，在普通字典中也保留了插入顺序，但之前的版本可以通过使用 `OrderedDict` 来创建有序字典。
3. `Counter`：计数器，用于统计可哈希对象中每个元素出现的次数。返回一个字典类型，键为原始元素，值为元素出现的次数。
4. `deque`：双端队列，实现了在两端快速地插入和删除操作，比列表的插入和删除操作效率更高。
5. `namedtuple`：具有命名字段的元组，可以使用字段名来访问元组的元素，比使用索引访问更方便，也更易读。
6. `ChainMap`：将多个字典或映射（例如命名元组）合并成单个映射。 查找时会先从第一个映射中查找，如果没找到再到第二个映射中查找？，以此类推。

函数：

1. `Counter.elements()` 返回计数器中的所有元素，以列表形式返回。
2. `Counter.most_common([n])` 返回计数器中出现频率最高的 n 个元素和它们出现的次数。
3. `deque.rotate(n)` 将 deque 中所有元素向右移动 n 个位置。当 n 为正数时，队列最右边的 n 个元素会被移到队列最左边；当 n 为负数时，队列最左边的 n 个元素会被移到队列最右边。

除了上述常用的数据类型和函数之外，`collections` 中还有一些其他实用的数据类型和函数，可以参考 Python 官方文档中的 `collections` 模块部分。



### requests

```python
import requests

url='https://img9.doubanio.com/view/photo/m_ratio_poster/public/p2879947754.jpg'
text=requests.get(url).content
with open(f'./douban/{url.split("/")[-1]}', 'wb') as f:
    f.write(text)
```

> 将 字 典 中 的 " h t t p s " 和 " h t t p " 全 部 大 写 了 ， 尝 试 之 后 确 实 可 行 了 
>
> - 注意字典proxy中，对于每个value无论key是HTTP还是HTTPS，都用HTTP开头！只有key用HTTPS！
> - 如果requests想要爬取的网站是https:// ，那么一定一定需要在requests里加上verify = False这句话


对于`requests`库返回的响应对象 `r`，以下是相关属性和方法的解释：

1. `r.content`: 这是响应内容的原始字节码。它表示网页的原始数据，以字节形式存储。
2. `r.encoding`: 这是根据响应头部和内容自动推测的网页编码。`requests`库会尝试根据`Content-Type`头部中的编码信息来确定编码。你可以通过访问 `r.encoding` 来获取当前编码。
3. `r.text`: 这是使用 `r.content` 和 `r.encoding` 进行解码后的字符串。通过访问 `r.text`，你可以获取到解码后的文本内容，即以字符串形式表示的网页内容。
4. `decode()`: 这是用于将字节码解码为字符串的方法。当你有字节码数据时，可以使用 `decode()` 方法来指定相应的编码进行解码。例如，`r.content.decode('utf-8')` 将以 UTF-8 编码将字节码解码为字符串。

请注意，`r.content` 返回的是字节码，而 `r.text` 返回的是已解码的字符串。当你使用 `r.text` 时，`requests`库会自动根据 `r.encoding` 属性进行解码操作，以提供方便的文本访问接口。如果你使用 `r.content`，则需要手动进行解码。

另外，需要注意的是，`r.encoding` 并不总是准确的。有些网页可能未正确指定编码，或者 `requests` 库的自动检测可能会失败。在这些情况下，你可能需要根据实际情况自行推测编码或使用其他库（如 `chardet`）进行编码检测。



### chardet

练习爬虫的许多小伙伴，在爬取网页时，肯定遇到过页面乱码的情况，其实是网页编码没有成功配对。

虽然在HTML页面中有charset标签，可以查看，或者一种一种编码地试，大概率也能不难地实现。那如果有第三方库，帮助我们检测网页编码，岂不美哉！于是就有了这篇文章“主角”的登场：chardet

> chardet.detect() : 函数接受一个参数，一个非unicode字符串。它返回一个字典，其中包含自动检测到的字符编码和从0到1的可信度级别。
>
> 
>
> 返回的内容有三个：
> encoding：表示字符编码方式。
> confidence：表示可信度，也可以理解为检测的概率。
> language：语言。
>
> 
>
> 这里检测的结果返回的是字典，而我们需要的是encoding的内容，即
>
> chardet.detect()['encoding']
>
> 
>
> r.encoding = chardet.detect(r.content)['encoding']

```python
def get_page(url):
    r=requests.get(url,headers=get_headers(),timeout=TIMEOUT)
    if r.status_code == 200:
        r = r.content.decode(chardet.detect(r.content)["encoding"])
        return r
    else:
        raise Exception(f"请求失败{r.text}")
```



### execjs

```python
import execjs
# 输出引擎版本
print(execjs.get().name)

# 读取js文件代码
# with open('1.js','r',encoding='utf-8') as f:
#     jscode=f.read()

print(execjs.eval("'red yellow blue'.split(' ')"))

ctx = execjs.compile("""
     function add(x, y) {
        return x + y;
   }
""")
print(ctx.call("add", 1, 2))
```

### heapq

Python中的`heapq`是一个堆队列算法模块，提供一些基本的堆操作函数。堆是一种特殊的树形数据结构，它满足堆的特性：对于每个节点，父节点的键值总是小于或等于任何一个子节点的键值。这里的“小于等于”可以是任意一种可比较的关系，常见的有小于号、大于号等等。堆通常用来实现优先队列，能够很快地找到优先级最高的元素。

heapq模块提供以下常用的函数：

1. `heapq.heappush(heap, item)`：往堆heap中插入一个元素item，保证heap仍然是一个最小堆。
2. `heapq.heappop(heap)`：从堆heap中弹出最小的元素，该元素在heap中被移除。
3. `heapq.heappushpop(heap, item)`：推入元素item到heap中，并且返回堆中最小值。
4. `heapq.heapreplace(heap, item)`：将堆中最小值替换成item，并返回堆中原来的最小值。堆大小不变。
5. `heapq.nlargest(n, iterable, key=None)`：返回最大的n个元素，iterable表示可迭代对象，如列表，元组等；key表示排序的关键字，和sorted中的key类似。
6. `heapq.nsmallest(n, iterable, key=None)`：返回最小的n个元素，用法同nlargest。
7. `heapq.heapify(x)`：将列表x转换为堆。

示例：

```python
import heapq

# 1. 初始化一个堆列表
heap = []

# 2. 往堆中插入元素，维持堆的特性
heapq.heappush(heap, 3)
heapq.heappush(heap, 1)
heapq.heappush(heap, 2)

# 3. 弹出堆中最小的元素，3将被移除
print(heapq.heappop(heap))  # 1

# 4. 往堆中插入一个元素，并弹出堆中最小的元素
print(heapq.heappushpop(heap, 4))  # 2

# 5. 堆中元素替换，原堆中最小的元素是2，现在被替换成了5
print(heapq.heapreplace(heap, 5))  # 2

# 6. 返回堆中最大的n个元素
print(heapq.nlargest(2, heap))  # [5, 4]

# 7. 返回堆中最小的n个元素
print(heapq.nsmallest(2, heap))  # [3, 4]
```

### pytextrank

`pytextrank`是在Python中使用TextRank算法的一个包，用于从文本中提取关键词和摘要。TextRank是一种基于图的排序算法，用于在文本中发现重要的单词和短语。它在PageRank算法（一种用于在web页面上评估页面的重要性）的基础上进行开发，并在许多自然语言处理任务中得到了广泛应用。TextRank使用迭代方式计算单词的重要性，将单词表示为图中的节点，并将共现单词之间的边权值分配为单词之间的相似性。重要性得分被视为节点中心性分数。

`pytextrank`包提供了以下功能：

1. 关键词提取：从文本中提取关键词，其中关键词按其重要性得分排序。
2. 摘要提取：从文本中提取摘要，其中摘要是文本中预定义字数的最重要文本段落。
3. 短语提取：从文本中提取短语，其中短语是高度相关且频繁出现的单词组合。
4. 总结和摘录：提供可读格式的可编程摘要。

`pytextrank`的工作原理：

1. 将文本划分为句子，每个句子都是一个节点。
2. 计算句子之间的相似性，将其用权重表示为图中的边。
3. 通过迭代计算排名函数以确定每个单词（节点）的重要性得分，其中排名函数基于PageRank算法。
4. 基于单词（节点）的得分，提取关键字、短语和摘要。

下面是一个使用`pytextrank`从文本中提取关键词的示例：

```python
import pytextrank
import spacy

nlp = spacy.load("en_core_web_sm")
tr = pytextrank.TextRank()
nlp.add_pipe(tr.PipelineComponent, name="textrank", last=True)

text = "TextRank is a graph-based ranking model for text processing. It uses a modification of the PageRank algorithm to create a weighted graph of words or phrases extracted from a text document and calculate a score for each word or phrase based on its significance in the text."
doc = nlp(text)

# 输出关键词及其权重得分
for phrase in doc._.textrank.summary(limit_phrases=15, limit_sentences=5):
    print(phrase.text)
```



输出结果可能为：

```
TextRank algorithm
graph-based ranking model
weighted graph
phrases extracted
score
PageRank algorithm
text processing
significant
words
modification
document
significance
calculate
model
```



以上是一个简单的使用`pytextrank`的示例，可以看到，`pytextrank`提取的关键词具有很好的语义连贯性和主题相关性，从文本中提取关键词和摘要成为自然语言处理中非常常见和重要的任务，`pytextrank`提供了一种简单而实用的方法。

### warning

`warnings`模块是Python中用于处理警告信息的一个标准库。在Python程序中，当某些代码存在潜在问题或者错误时，会生成警告信息，这些警告信息通常不会停止程序的执行，但是可以提示开发者注意一些问题。`warnings`模块提供了一些功能，可以控制和处理警告信息。

主要的函数有:

1. `warnings.warn(msg, WarningType=UserWarning, stacklevel=1)`：向警告日志中添加一条警告信息，参数`msg`表示警告信息字符串，`WarningType`为警告类型，默认为`UserWarning`，`stacklevel`表示警告信息的打印级别，默认为`1`。
2. `warnings.filterwarnings(action, category=Warning, message='', module='', lineno=0, append=False)`：用于控制警告信息的输出，可以过滤或者忽略某些警告信息。`action`可以为以下几种之一：
   - `error`：将警告信息转为异常，警告信息将被抛出并终止程序的执行。
   - `ignore`：忽略指定类型的警告信息，不会对程序执行造成任何影响。
   - `always`：无条件输出所有警告信息，即使已经有相同的警告信息输出过。
   - `default`：打印第一次出现的指定类型的警告信息，而不管重复出现多少次。
   - `module`：与`message`配合使用，可以控制只输出指定模块或文件中特定警告信息。
3. `warnings.showwarning(message, category, filename, lineno, file=None, line=None)`：定义了警告信息的默认输出格式。

示例：

```python
import warnings

# 取消打印DeprecationWarning警告信息
warnings.filterwarnings("ignore",category=DeprecationWarning)

def foo():
    # 提示函数将在未来版本中被删除
    warnings.warn("Function 'foo' is deprecated.", DeprecationWarning)

foo()
```



在上面的示例中，我们用过 `filterwarnings()` 函数取消了打印`DeprecationWarning`的警告信息，因此程序不会打印任何警告信息。结果是程序退出时返回值为`None`。如果把 `filterwarnings()` 函数的参数改为 `warnings.filterwarnings('error')`，程序将抛出一个`DeprecationWarning`的异常，终止程序的执行。

需要注意的是，警告信息常常意味着代码的不完善性和潜在的问题，建议确保代码的完整性并尽量减少警告信息的产生。如果警告信息不会影响程序的执行，可以使用`filterwarnings()`函数进行忽略或过滤。





### selenium

```python
script = '''object.defineProperty(navigator,'webdriver',{undefinedget: () => undefined})'''
#execute_cdp_cmd用来执行chrome开发这个工具命令
driver.execute_cdp_cmd("page.addscriptToEvaluateonNewDocument",{"source": script})
```

```javascript
window.navigator.webdriver
```

#### 完全隐藏

```python
from selenium import webdriver
import os
current_path = os.path.dirname(os.path.abspath(__file__))
driver = webdriver.Chrome(                 executable_path=os.path.join(current_path,'chromedriver.exe'))

with open(os.path.join(self.current_path,'stealth.min.js')) as f:
    js = f.read()

    driver.execute_cdp_cmd("Page.addScriptToEvaluateOnNewDocument", {
        "source": js
    })
    driver.get(url)
```



#### 快速开始

```python
import requests
import random
from lxml import etree
import string
from time import sleep
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.keys import Keys

page_list=[]
# keywords=['chatgpt']
keywords=['chatgpt','人工智能']
base_url='https://www.baidu.com/'

service=Service('../chromedriver.exe')

bro=webdriver.Chrome(service=service)
# url_f=open('result_url.csv','a',encoding='utf-8')
f=open('result.csv','a',encoding='utf-8')

for keyword in keywords:
    urls = []
    bro.get(base_url)
    sleep(0.5)
    input=bro.find_element(By.XPATH,'//*[@id="kw"]')
    input.send_keys(keyword)
    input.send_keys(Keys.ENTER)
```





```python
1.webdriver提供的8种页面元素定位方法：
    id/name/class name/tag name/link text/partial link text/xpath/css selector
    其中python对应的8种方法：
    find_element_by_id()                       如： find_element_by_id("kw")  
    find_element_by_name()                     如： find_element_by_name("wd")
    find_element_by_class_name()               如： find_element_by_class_name("s_ipt") 
    find_element_by_tag_name()                 如： find_element_by_tag_name("input") 
    find_element_by_link_text()                如：find_element_by_link_text(u"新闻")    
    find_element_by_partial_link_text()        如：find_element_by_partial_link_text(u"一个很长的") 
    find_element_by_xpath()                    如： find_element_by_xpath(" .//*[@id='kw']")  
    find_element_by_css_selector()             如： find_element_by_css_selector("#kw")
```

实用的：

```
点击链接文字
web.find_element_by_link_text('电视剧').click()

当前页面源码
text=web.page_source
```

鼠标滚轮

```python
js="window.scrollTo(100,450);"
web.execute_script(js)
```





无头浏览器

```python
from selenium.webdriver import DesiredCapabilities


# capabilities = DesiredCapabilities.CHROME.copy()
# capabilities['acceptSslCerts'] = True
# capabilities['acceptInsecureCerts'] = True
#
#
#
# from selenium.webdriver.chrome.options import Options  # 导入浏览器内核设置，主要是为了设置无头（headless）模式
# chrome_options = Options()
# chrome_options.add_argument('--headless')  # 设置Chrome为无头模式
# chrome_options.add_argument("no-sandbox")
# chrome_options.add_argument("disable-dev-shm-usage")


#在driver中加入desired_capabilities参数
# web = webdriver.Chrome(options=chrome_options,desired_capabilities=capabilities)
```

#### 禁用加载

在 Selenium 中，可以通过设置 ChromeOptions 参数来禁止加载字体文件。

以下是一个示例：

```python
options.add_argument("--disable-remote-fonts")
```

```python
from selenium import webdriver

options = webdriver.ChromeOptions()
options.add_argument("--disable-extensions")
options.add_argument("--disable-gpu")
options.add_argument("--disable-infobars")
options.add_argument('--disable-dev-shm-usage')
options.add_argument('--no-sandbox')
options.add_argument("--headless")
options.add_argument("--disable-default-apps")
options.add_argument("--disable-features=site-per-process")
options.add_argument("--disable-popup-blocking")
options.add_argument("--disable-translate")
#禁用浏览器的同源策略
options.add_argument("--disable-web-security")
#指定 Chrome 在本地开启一个远程调试端口
options.add_argument("--remote-debugging-port=9222")
options.add_experimental_option("prefs", {"profile.managed_default_content_settings.images": 2,
                                          "profile.managed_default_content_settings.stylesheet": 2,
                                          "profile.managed_default_content_settings.cookies": 2,
                                          "profile.managed_default_content_settings.javascript": 1,
                                          "profile.managed_default_content_settings.plugins": 2,
                                          "profile.managed_default_content_settings.popups": 2,
                                          "profile.managed_default_content_settings.geolocation": 2,
                                          "profile.managed_default_content_settings.notifications": 2,
                                          "download.default_directory": "~/Downloads",
                                          "download.prompt_for_download": False,
                                          "download.directory_upgrade": True,
                                          "safebrowsing.enabled": False})
prefs = {"download.default_directory": "~/Downloads",
         "download.prompt_for_download": False,
         "download.directory_upgrade": True,
         "safebrowsing.enabled": False}
options.add_experimental_option("prefs", prefs)

# 禁止加载字体文件
options.add_experimental_option("prefs", {"profile.default_content_settings": {"fonts": {"enabled": False}}})

driver = webdriver.Chrome(chrome_options=options)
```



在这个示例中，我们首先创建了一个 `ChromeOptions` 对象，并设置了一些 Chrome 浏览器选项。然后，我们使用 `add_experimental_option` 方法添加了一个名为 `prefs` 的字典，其中包含了禁止加载字体文件的配置信息。

最后，我们创建了一个 Chrome WebDriver 对象，并将 `ChromeOptions` 对象传递给它作为参数，启动 Chrome 浏览器。

在请求网页时，Chrome 浏览器将不会加载字体文件，可以提高页面加载速度。





#### 自动检查安装更新webdriver代码

```python
import os
import winreg
import zipfile

import requests

def get_Chrome_version():
    key = winreg.OpenKey(winreg.HKEY_CURRENT_USER, r'Software\Google\Chrome\BLBeacon')
    version, types = winreg.QueryValueEx(key, 'version')
    return version


def get_server_chrome_versions():
    '''return all versions list'''
    versionList = []
    url = "https://registry.npmmirror.com/-/binary/chromedriver/"
    rep = requests.get(url).json()
    for item in rep:
        versionList.append(item["name"])
    return versionList


def download_driver(download_url):
    '''下载文件'''
    file = requests.get(download_url)
    with open("chromedriver.zip", 'wb') as zip_file:  # 保存文件到脚本所在目录
        zip_file.write(file.content)
        print('下载成功')


def get_version(file_path):
    '''查询系统内的Chromedriver版本'''
    outstd2 = os.popen(file_path + 'chromedriver --version').read()
    if outstd2 is None or len(outstd2) == 0:
        return '1'
    return outstd2.split(' ')[1]


def unzip_driver(path):
    '''解压Chromedriver压缩包到指定目录'''
    f = zipfile.ZipFile("chromedriver.zip", 'r')
    for file in f.namelist():
        f.extract(file, path)


def check_update_chromedriver(file_path):
    url = 'http://npm.taobao.org/mirrors/chromedriver/'
    chromeVersion = get_Chrome_version()
    chrome_main_version = int(chromeVersion.split(".")[0])  # chrome主版本号
    driver_main_version = ''
    if os.path.exists(os.path.join(file_path, "chromedriver.exe")):
        driverVersion = get_version(file_path)
        driver_main_version = int(driverVersion.split(".")[0])  # chromedriver主版本号
        return
    download_url = ""
    if driver_main_version != chrome_main_version:
        print("chromedriver版本与chrome浏览器不兼容，更新中>>>")
        versionList = get_server_chrome_versions()
        if chromeVersion in versionList:
            download_url = f"{url}{chromeVersion}/chromedriver_win32.zip"
        else:
            for version in versionList:
                if version.startswith(str(chrome_main_version)):
                    download_url = f"{url}{version}/chromedriver_win32.zip"
                    break
            if download_url == "":
                print("暂无法找到与chrome兼容的chromedriver版本，请在http://npm.taobao.org/mirrors/chromedriver/ 核实。")

        download_driver(download_url=download_url)
        path = file_path
        unzip_driver(path)
        os.remove("chromedriver.zip")
        print('更新后的Chromedriver版本为：', get_version(file_path))
    else:
        print("chromedriver版本与chrome浏览器相兼容，无需更新chromedriver版本！")
    return os.path.join(file_path, "chromedriver.exe")

driver_path=check_update_chromedriver('./')
```



### lxml

> etree主要函数：
>
> - etree.HTML()：解析字符串格式的HTML文档对象，将字符串参数变为Element对象，以便使用xpath()等方法；
>
> - etree.tostring()：将Element对象转换成字符串；
>
> - > ```python
>   > 只转换文本
>   > text = etree.tounicode(text, method="text")
>   > #html标签也转换
>   > text = etree.tounicode(text)
>   > ```
>
> - etree.fromstring()：将字符串转换成Element对象。

标签属性值

a/@href

//div[contains(@class, 'demo') and contains(@class, 'other')]



### tushare



### jieba

#### 添加自定义词

```python
import jieba
jieba.add_word("自定义词1")
jieba.add_word("自定义词2")

上面制作需要我们手动一个个添加，当自定义词较多时，我们可以用下面的方法
import jieba
jieba.load_userdict(file_name) 

file_name 为文件类对象或自定义词典的路径，词典格式和 dict.txt 一样，一个词占一行；每一行分三部分：词语、词频（可省略）、词性（可省略），用空格隔开，顺序不可颠倒。file_name 若为路径或二进制方式打开的文件，则文件必须为 UTF-8 编码。
```

#### 删除自定义词

```
import jieba
jieba.del_word("自定义词1")
```











```
支持四种分词模式：

精确模式
全模式
搜索引擎模式
paddle模式
```
lcut 将返回的对象转化为list对象返回

```
def cut(self, sentence, cut_all=False, HMM=True, use_paddle=False):
# sentence: 需要分词的字符串;
# cut_all: 参数用来控制是否采用全模式；
# HMM: 参数用来控制是否使用 HMM 模型;
# use_paddle: 参数用来控制是否使用paddle模式下的分词模式，paddle模式采用延迟加载方式，通过enable_paddle接口安装paddlepaddle-tiny
```

```python
1）精准模式（默认）:
试图将句子最精确地切开，适合文本分析

seg_list = jieba.cut("我来到北京清华大学", cut_all=False)
print("精准模式: " + "/ ".join(seg_list))  # 精确模式

#    精准模式: 我/ 来到/ 北京/ 清华大学
```

```
2）全模式:
把句子中所有的可以成词的词语都扫描出来, 速度非常快，但是不能解决歧义；

seg_list = jieba.cut("我来到北京清华大学", cut_all=True)
print("全模式: " + "/ ".join(seg_list))  # 全模式

-----output-----

全模式: 我/ 来到/ 北京/ 清华/ 清华大学/ 华大/ 大学
```

```
3）paddle模式
利用PaddlePaddle深度学习框架，训练序列标注（双向GRU）网络模型实现分词。同时支持词性标注。
paddle模式使用需安装paddlepaddle-tiny，pip install paddlepaddle-tiny==1.6.1。
目前paddle模式支持jieba v0.40及以上版本。
jieba v0.40以下版本，请升级jieba，pip installjieba --upgrade。 PaddlePaddle官网

import jieba

通过enable_paddle接口安装paddlepaddle-tiny，并且import相关代码；

jieba.enable_paddle()  # 初次使用可以自动安装并导入代码
seg_list = jieba.cut(str, use_paddle=True)
print('Paddle模式: ' + '/'.join(list(seg_list)))

-----output-----

Paddle enabled successfully......
Paddle模式: 我/来到/北京清华大学
```

2，jieba.cut_for_search 和 jieba.lcut_for_search
搜索引擎模式
在精确模式的基础上，对长词再次切分，提高召回率，适合用于搜索引擎分词

seg_list = jieba.cut_for_search("小明硕士毕业于中国科学院计算所，后在日本京都大学深造")  # 搜索引擎模式
print(", ".join(seg_list))

-----output-----

小明, 硕士, 毕业, 于, 中国, 科学, 学院, 科学院, 中国科学院, 计算, 计算所, ，, 后, 在, 日本, 京都, 大学, 日本京都大学, 深造

3，jieba.Tokenizer(dictionary=DEFAULT_DICT)
新建自定义分词器，可用于同时使用不同词典。jieba.dt 为默认分词器，所有全局分词相关函数都是该分词器的映射。

import jieba

test_sent = "永和服装饰品有限公司"
result = jieba.tokenize(test_sent) ##Tokenize：返回词语在原文的起始位置
print(result)
for tk in result:

print ("word %s\t\t start: %d \t\t end:%d" % (tk[0],tk[1],tk[2])    )

​    print (tk)
​    

-----output-----

<generator object Tokenizer.tokenize at 0x7f6b68a69d58>
('永和', 0, 2)
('服装', 2, 4)
('饰品', 4, 6)
('有限公司', 6, 10)





### openpyxl

写文件

```python
from openpyxl import Workbook

wb = Workbook()   #创建excel

ws1 = wb.create_sheet(index=0)  # 默认最后一个
ws1.cell(row=1, column=1, value='序号')
ws1.cell(row=1, column=2, value='视频名称')

ws1.cell(row=1, column=3, value='时间')

wb.save('bilibili.xlsx')
```

读文件

```python
from openpyxl import load_workbook
#加载Excel
workbook_object = load_workbook(filename='case_data.xlsx')  #将case_data.xlsx文件与Python文件放在同一级目录下，如果不在同一级目录，需要添加路径
#获取表单名称
names= workbook_object.sheetnames  #获取表单的名称返回list
#获取表单对象
#方法一
shett_object=workbook_object['login']
#方法二
shett_object1=workbook_object.worksheets[0]  #获取的表单返回的是list，所以可以通过索引取值
#获取单元格
#方法一
cell_object=shett_object['A1']  #获取得是单元格的对象“A1”，并非"A1"的值
print(cell_object.value)  #通过对象.value 可以获取"A1"的值（单元格内容）
#方法二
cell_object1=shett_object.cell(1,1)  #获取的是第一行第一列的值
print(cell_object1.value)
workbook_object.close()  #读完表后关闭Excel
```

封装成一个类，接口测试框架中可以直接调用此方法

```python
from openpyxl import load_workbook


class HandleExcel:
    def __init__(self, file_name, sheet_name):
        self.workbook_object = load_workbook(filename=file_name)
        self.sheet_object = self.workbook_object[sheet_name]

    def get_excel_tese_case(self):
        cases_list = []

        datas = list(self.sheet_object.iter_rows(values_only=True))  # 获取Excel表中的所有数据，按行显示，先是第一行的内容

        # 将Excel表中的数据拼成字典

        case_title = datas[0]  # 获取表头
        case_datas = datas[1:]  # 获取表数据
        for case in case_datas:
            result = dict(zip(case_title, case))
            cases_list.append(result)
        self.close_file()
        print(cases_list)
        return cases_list
        print(result)

    def close_file(self):
        self.workbook_object.close()


if __name__ == '__main__':
    cl = HandleExcel(file_name='case_data.xlsx', sheet_name='login')
    cl.get_excel_tese_case()
```



### unicodedata

unicode编码标准化

```python
import unicodedata as ucd

# import unicodedata

# 将 Unicode 字符串转换为 NFC 标准形式
# s = ucd.normalize('NFC', ' \xa0')
# print(s)

a=' \xa0'
a= ucd.normalize('NFKC', a)

print(a.replace(' ', ''))
```

### WorldCould

```python
# 1.读取文本内容，并用jieba库中的cut()函数进行分词
import jieba
report = open('信托行业报告.txt','r').read()
words = jieba.cut(report)

# 2.通过for循环语句提取列表words中长度大于等于4个字的词
report_words = []
for word in words: #将长度大于等于4个字的词放入列表
    if len(word) >= 4:
        report_words.append(word)
#print(report_words)

# 3.获得打印输出高频词的出现次数
from collections import Counter
result = Counter(report_words).most_common(50) #取最多的50组
#print(result)

# 4.绘制词云图
from wordcloud import WordCloud #导入相关库
content = ' '.join(report_words) #把列表转换为字符串
wc = WordCloud(font_path='simhei.ttf',#字体文件路径（这里为黑体）
               background_color='white',#背景颜色（这里为白色）
                width=1000,#宽度
                height=600,#高度
                 ).generate(content) #绘制词云图
wc.to_file('词云图.png') #导出成PNG格式图片（使用相对路径）

```



### PyQt5

#### 实时刷新页面

QApplication.processEvents()

```python
from PyQt5.Qt import *
import time

class Window(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("QSlider的学习")
        self.resize(500, 500)
        self.setup_ui()

    def setup_ui(self):

        self.sd=QTextEdit(self)
        self.sd.move(20,100)
        bt=QPushButton('1111',self)
        bt.clicked.connect(self.ap)

    def ap(self):
        for i in range(10):
            self.sd.append('123')
            time.sleep(1)
            QApplication.processEvents()

if __name__ == '__main__':
    import sys

    app = QApplication(sys.argv)

    window = Window()
    window.show()

    sys.exit(app.exec_())
```



hello，word

```python
from PyQt5.Qt import *
import sys

app=QApplication(sys.argv)

window=QWidget()
window.setWindowTitle('呦西')

window.resize(500,500)
window.move(400,200)

label=QLabel(window)
label.setText('hello word')
label.move(200,200)

window.show()
sys.exit(app.exec_())
```











### pyinstaller

```python
图标尺寸无要求

#有控制台 一个文件 图标
pyinstaller -c -F x.py -i favo.ico 

使用-c参数的作用是将打包生成的可执行文件以控制台应用程序的形式运行，而不是以图形界面应用程序的形式运行。这意味着程序将在控制台窗口中打印输出，并且可以接收控制台的输入。

通过在命令行中添加-c参数，可以告诉PyInstaller生成一个控制台程序，即使原始Python脚本是一个图形界面的应用程序。

-F 表示生成单个可执行文件

-w 表示去掉控制台窗口，这在GUI界面时非常有用。不过如果是命令行程序的话那就把这个选项删除吧！

-p 表示你自己自定义需要加载的类路径，一般情况下用不到

-i 表示可执行文件的图标
```

### urllib

`urllib`是Python标准库中的一个模块，包含了许多处理URL的方法，它提供了以下功能：

1. 发送 HTTP/HTTPS 请求：`urllib.request.urlopen()`

   `urlopen()`是`urllib.request`模块中的一个方法，用于发送HTTP/HTTPS请求并获取响应内容。它支持GET、POST、PUT等HTTP方法，并可以通过设置请求头、请求体等参数来定制请求。

   ```python
   import urllib.request
   
   url = 'https://www.example.com'
   response = urllib.request.urlopen(url);
   
   print(response.status)
   print(response.getheader('Content-Type'))
   print(response.read())
   ```

   

2. 解析 URL：`urllib.parse.urlparse()`

   `urlparse()`可以将一个URL解析成6个部分：协议、主机名、端口、路径、查询参数和片段，并返回一个包含这些部分的`ParseResult`对象。

   ```python
   from urllib.parse import urlparse
   
   url = 'https://example.com:8080/path/to/page?param=value#fragment'
   result = urlparse(url)
   
   print(result.scheme)
   print(result.netloc)
   print(result.path)
   print(result.query)
   print(result.fragment)
   ```

   

3. 编码 URL 参数：`urllib.parse.urlencode()`

   `urlencode()`可以将一个字典类型的数据编码成URL参数格式，并返回一个字符串。它通常用于构建GET请求中的查询参数。

   ```python
   from urllib.parse import urlencode
   
   query_params = {
       'page': 1,
       'size': 10,
       'q': 'keyword'
   }
   encoded_params = urlencode(query_params)
   
   print(encoded_params)
   ```

   

4. 编码和解码 URL 路径：`urllib.parse.quote()`和`urllib.parse.unquote()`

   `quote()`和`unquote()`可以分别将字符串编码成URL路径安全的格式和解码已编码的URL路径。

   ```python
   from urllib.parse import quote, unquote
   
   path = '/path/to/page with spaces'
   encoded_path = quote(path)
   
   print(encoded_path)
   
   decoded_path = unquote(encoded_path)
   print(decoded_path)
   ```

   

总之，`urllib`提供了许多有用的方法来处理URL和发送HTTP/HTTPS请求。这些方法在Web开发中非常有用。（请注意：自Python 3版本起，`urllib`分为了4个子模块，分别是`urllib.request`、`urllib.parse`、`urllib.error`和`urllib.robotparser`。）





## 写文件

```
f=open('1.txt','a')
```



```
字符       意义
'r'       文本读取（默认）
'w'       文本写入，并先清空文件（慎用），文件不存在则创建
'x'       文本写，排它性创建，如果文件已存在则失败
'a'	      文本写，如果文件存在则在末尾追加，不存在则创建

'b'	    二进制模式，例如：'rb'表示二进制读
't'	    文本模式（默认），例如：rt 一般省略 t
'+'	    读取与写入，例如：'r+' 表示同时读写

r 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。
rb 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。
r+ 打开一个文件用于读写。文件指针将会放在文件的开头。
rb+ 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。
w 打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。
wb 以二进制格式打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。
w+ 打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。
wb+ 以二进制格式打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。
a 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。
ab 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。
a+ 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。
ab+ 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。
```



## 列表生成式与自定义函数

```python
len2=[10000000 if i==0.0 else i for i in len1]

exp_list=[i if i !='不限' and i!=0 else '经验不限' for i in exp_list]

# 只保留汉字
seg_list = filter(lambda x: '\u4e00' <= x <= '\u9fa5', seg_list)

detail_list['虫子编号']=detail_list['虫子编号'].apply(lambda row:156 if row==11 else row)
detail_list=pd.read_csv('result2.csv',usecols=[0,1,2])
```

## append()添加元素作为一个整体，extend分别添加

## url编码

```python
 import urllib.parse
>>> # url编码
...
>>> en_url = urllib.parse.quote("hello 世界!"))
>>> print(en_url)
hello%20%E4%B8%96%E7%95%8C%21

 # url解码
...
 de_url = urllib.parse.unquote(en_url)
 print(de_url)
hello 世界!
```

## 网页编码

```
resres.content
res=str(res,'utf-8')
```

## socket

server

```python
import socket 
s=socket.socket(socket.AF_INET, socket.SOCK_STREAM)             # 创建socket对象
s.bind(('127.0.0.1',4323))                                      # 绑定地址
s.listen(5)                                                     # 建立5个监听
while True:
    conn,addr= s.accept()                                       # 等待客户端连接
    print('欢迎{}'.format(addr))                              #打印访问的用户信息
    while True:
        data=conn.recv(1024) 
        dt=data.decode('utf-8')                                 #接收一个1024字节的数据 
        print('收到：',dt)
        aa=input('服务器发出：') 
        if aa=='quit':
            conn.close()                                        #关闭来自客户端的连接
            s.close()                                           #关闭服务器端连接
        else:
            conn.send(aa.encode('utf-8'))
```



client

```python
import socket   
import sys
c=socket.socket()                                           # 创建socket对象
c.connect(('127.0.0.1',4323))                                #建立连接
while True:
    ab=input('客户端发出：')
    if ab=='quit':
        c.close()                                               #关闭客户端连接
        sys.exit(0)
    else:
        c.send(ab.encode('utf-8'))                               #发送数据
        data=c.recv(1024)                                        #接收一个1024字节的数据
        print('收到：',data.decode('utf-8'))
```







server

```python
import socket  # 导入socket库
from time import ctime
import json
import time

HOST = ''
PORT = 9001
ADDR = (HOST, PORT)
BUFFSIZE = 1024  # 定义一次从socket缓冲区最多读入1024个字节
MAX_LISTEN = 5  # 表示最多能接受的等待连接的客户端的个数


def tcpServer():  # TCP服务
    # with socket.socket() as s:
    with socket.socket(socket.AF_INET,
                       socket.SOCK_STREAM) as s:  # AF_INET表示socket网络层使用IP协议，SOCK_STREAM表示socket传输层使用tcp协议
        # 绑定服务器地址和端口
        s.bind(ADDR)
        # 启动服务监听
        s.listen(MAX_LISTEN)
        print('等待用户接入……')
        while True:
            # 等待客户端连接请求,获取connSock
            conn, addr = s.accept()
            print('警告，远端客户:{} 接入系统！！！'.format(addr))
            # with conn:
            while True:
                print('接收请求信息……')
                # 接收请求信息
                data = conn.recv(BUFFSIZE)  # 读取的数据一定是bytes类型，需要解码成字符串类型
                if not data:
                    break
                info = data.decode()
                # print('data=%s' % data)
                print(f'接收数据：{info}')

                # 发送请求数据
                conn.send(f'服务端接收到信息{info}'.encode())
                print('发送返回完毕！！！')
            conn.close()
            s.close()


# 创建UDP服务
def udpServer():
    # 创建UPD服务端套接字
    with socket.socket(socket.AF_INET, socket.SOCK_DGRAM) as s:
        # 绑定地址和端口
        s.bind(ADDR)
        # 等待接收信息
        while True:
            print('UDP服务启动，准备接收数据……')
            # 接收数据和客户端请求地址
            data, address = s.recvfrom(BUFFSIZE)

            if not data:
                break
            info = data.decode()
            print(f'接收请求信息：{info}')

            s.sendto(b'i am udp,i got it', address)

        s.close()


if __name__ == '__main__':

    while True:
        choice = input('input choice t-tcp or u-udp:')

        if choice != 't' and choice != 'u':
            print('please input t or u,ok?')
            continue

        if choice == 't':
            print('execute tcpsever')
            tcpServer()
        else:
            print('execute udpsever')
            udpServer()
```

client

```python
import socket

from time import ctime

HOST = 'localhost'
PORT = 9001
ADDR = (HOST, PORT)
BUFFSIZE = 1024


def tcpClient():
    # 创建客户套接字
    with socket.socket(family=socket.AF_INET, type=socket.SOCK_STREAM) as s:
        # 尝试连接服务器
        s.connect(ADDR)
        print('连接服务成功！！')
        # 通信循环
        while True:
            inData = input('pleace input something:')
            if inData == 'q':
                break
            # 发送数据到服务器
            # inData = '[{}]:{}'.format(ctime(), inData)

            s.send(inData.encode())
            print('发送成功！')

            # 接收返回数据
            outData = s.recv(BUFFSIZE)
            print(f'返回数据信息：{outData}')

        # 关闭客户端套接字
        s.close()


def udpClient():
    # 创建客户端套接字
    with socket.socket(socket.AF_INET, socket.SOCK_DGRAM) as s:
        while True:
            # 发送信息到服务器
            data = input('please input message to server or input \'quit\':')
            if data == 'quit':
                break
            data = '[{}]:{}'.format(ctime(), data)

            s.sendto(data.encode('utf-8'), ADDR)

            print('send success')

            # 接收服务端返回信息
            recvData, addrs = s.recvfrom(BUFFSIZE)
            info = recvData.decode()
            print(f'recv message : {info}')

        # 关闭套接字
        s.close()


if __name__ == '__main__':

    while True:
        choice = input('input choice t-tcp or u-udp or q-quit:')
        if choice == 'q':
            break
        if choice != 't' and choice != 'u':
            print('please input t or u,ok?')
            continue

        if choice == 't':
            print('execute tcpsever')
            tcpClient()
        else:
            print('execute udpsever')
            udpClient()
```







# 爬虫

## 哔哩哔哩

弹幕接口

```
https://api.bilibili.com/x/v1/dm/list.so?oid=911120913
```

1)根据aid找到cid，cid等于上面弹幕的oid

```
https://api.bilibili.com/x/web-interface/view?aid=941279875
https://api.bilibili.com/x/web-interface/view?aid=46101743
```

2)bvid找cid（推荐）

```
1.bvid获取cid: https://api.bilibili.com/x/player/pagelist?bvid=(bvid,要带上开头的BV!) 
2.bvid和cid获取视频播放列表 https://api.bilibili.com/x/player/playurl?cid=(cid)&qn=(qn)&bvid=(bvid,要带上开头的BV!) 
3.用bvid和cid获取aid: https://api.bilibili.com/x/web-interface/view?cid=(cid)&bvid=(bvid,要带上开头的BV!) 在json的["data"]["aid"]里 
这应该是全网第一个破解了bilibili的BV号api的文章把(好像有点水啊) 
```



## 一些函数

```python
def filter_urls(answers_list,black_list=None):
    if black_list is None:
        # 黑名单放置被墙的网站,避免超时请求
        black_list = ["google.com","zh.wikipedia.org"]
        filtered_urls=[]
        for answer in answers_list:
            if all(domain not in answer['url'] for domain in black_list) and answer['url'].split(".")[-1] != "pdf":
                filtered_urls.append(answer)
                return filtered_urls
```
