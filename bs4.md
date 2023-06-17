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