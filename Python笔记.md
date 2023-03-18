# Python笔记

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



### re

```
import re
ret = re.findall(r"\d+", "python = 9999, c = 7890, c++ = 12345")
print(ret)
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

### requests

```python
import requests

url='https://img9.doubanio.com/view/photo/m_ratio_poster/public/p2879947754.jpg'
text=requests.get(url).content
with open(f'./douban/{url.split("/")[-1]}', 'wb') as f:
    f.write(text)
```

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



自动检查安装更新webdriver代码

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

```
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

