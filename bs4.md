# 使用Python的BeautifulSoup库解析网页内容

安装

```python
pip install beautifulsoup4
```

导入

```python
from bs4 import BeautifulSoup
```

实例化

```python
soup = BeautifulSoup(html_content, 'html.parser')
soup = BeautifulSoup(html_content, 'lxml')
```



```python
pythonCopy codetitle = soup.find('h1').text
paragraphs = soup.find_all('p')
```

- 使用标签名来获取结点： 
  - soup.标签名
- 使用标签名来获取结点标签名【这个重点是name，主要用于非标签名式筛选时，获取结果的标签名】： 
  - soup.标签.name
- 使用标签名来获取结点属性： 
  - soup.标签.attrs【获取全部属性】
  - soup.标签.attrs[属性名]【获取指定属性】
  - soup.标签[属性名]【获取指定属性】
  - soup.标签.get(属性名)
- 使用标签名来获取结点的文本内容： 
  - soup.标签.text
  - soup.标签.string
  - soup.标签.get_text()

提取所有页面链接

from bs4 import BeautifulSoup
import requests
from urllib.parse import urlparse

def extract_links_from_html(url):
    response = requests.get(url)
    soup = BeautifulSoup(response.content, 'html.parser')
    links = []

```python
for link in soup.find_all('a'):
    href = link.get('href')
    if href:
        parsed_url = urlparse(href)
        if parsed_url.scheme == '' and parsed_url.netloc == '':
            # 相对链接，拼接成绝对链接
            absolute_url = urlparse(url)
            href = absolute_url.scheme + '://' + absolute_url.netloc + href
        links.append(href)

return links
```

## 去除css，svg，js

```python
def remove_css(html):
    soup = BeautifulSoup(html, 'html.parser')

    # 删除<style>标签
    for style_tag in soup('style'):
        style_tag.decompose()

    # 删除<link>标签
    for link_tag in soup('link'):
        link_tag.decompose()

    # 删除<symbol>标签
    for symbol_tag in soup('symbol'):
        symbol_tag.decompose()

    # 删除<script>标签
    for script_tag in soup('script'):
        script_tag.decompose()
        
    # 删除注释
    comments = soup.find_all(string=lambda text: isinstance(text, Comment))
    for comment in comments:
        comment.extract()

    return str(soup)
```

## find与select

`find()` 和 `select()` 是 Beautiful Soup 库中用于在 HTML 或 XML 文档中查找元素的方法。它们之间的主要区别在于使用的参数类型和查找的方式。

`find()` 方法接受一个标签名和/或属性来查找单个元素，并返回第一个匹配的结果。它可以使用标签名或属性来进行精确匹配，也可以使用正则表达式进行模糊匹配。



```python
# 使用标签名查找元素
soup.find('div')

# 使用属性名和属性值进行查找
soup.find('div', class_='example-class')
soup.find('div', attrs={'class': 'example-class'})

# 使用正则表达式进行模糊匹配
import re
soup.find(re.compile('^h\d\$'))
```

`select()` 方法使用 CSS 选择器语法来查找元素，并返回所有匹配的结果作为列表。它可以根据元素的标签名、类名、id等属性进行选择，并支持层次关系的选择。

```python
# 使用标签名查找元素
soup.select('div')

# 使用类名进行查找
soup.select('.example-class')

# 使用id进行查找
soup.select('#example-id')

# 使用属性名和属性值进行查找
soup.select('div[class="example-class"]')

# 使用层次关系进行选择
soup.select('div .example-class')
```

总结一下，`find()` 使用灵活的参数类型（标签名、属性、正则表达式），返回第一个匹配的结果，更适用于简单的查找；而 `select()` 使用 CSS 选择器语法，返回所有匹配的结果作为列表，更适用于复杂的查找。根据具体情况，您可以选择合适的方法来查找和处理元素。