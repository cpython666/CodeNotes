# Python笔记

> 下载python库一直失败，报hostname出错的问题
>
> 可能是因为你电脑开了代理

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



### re

匹配数字

```python
re.findall('(\d+)','5-10年')
```



```
import re
ret = re.findall(r"\d+", "python = 9999, c = 7890, c++ = 12345")
print(ret)
```

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







#### 删除某一列

```python
#1.
del df['column-name']

#采用drop方法，有下面三种等价的表达式：
df,drop('num',axix=1),不改变内存，及输入df的时候，它还是显示原数据
df.drop('num',axix=1，inplace=True),改变内存，及输入df的时候，它显示改变后的数据
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



### matplotlib

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



### selenium

快速开始

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





### WorldCould

```
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


-F 表示生成单个可执行文件

-w 表示去掉控制台窗口，这在GUI界面时非常有用。不过如果是命令行程序的话那就把这个选项删除吧！

-p 表示你自己自定义需要加载的类路径，一般情况下用不到

-i 表示可执行文件的图标
```



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



