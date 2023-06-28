## 初始化

```python
from selenium import webdriver
import os
current_path = os.path.dirname(os.path.abspath(__file__))
driver = webdriver.Chrome(executable_path=os.path.join(current_path,'chromedriver.exe'))
```

## 添加配置

```python
from selenium.webdriver.chrome.options import Options
import os
# 获取当前文件路径
current_path = os.path.abspath(__file__)
chrome_options = Options()
# chrome_options.add_experimental_option('excludeSwitches', ['enable-automation'])
# chrome_options.add_experimental_option('useAutomationExtension', False)
# # 禁用字体
# chrome_options.add_argument("--disable-remote-fonts")
#
# # 禁止css
# # chrome_options.add_argument("--disable-extensions")
# # chrome_options.add_argument("--disable-gpu")
# # chrome_options.add_argument("--disable-infobars")
# # chrome_options.add_argument("--disable-dev-shm-usage")
# # chrome_options.add_argument("--disable-browser-side-navigation")
# # chrome_options.add_argument("--disable-software-rasterizer")
# # chrome_options.add_argument("--disable-default-apps")
# # chrome_options.add_argument("--disable-translate")
# # chrome_options.add_argument("--disable-webgl")
# # chrome_options.add_argument("--blink-settings=imagesEnabled=false")
#
# prefs = {
#     # 禁止加载js
#     'profile.default_content_setting_values': {
#         'images': 2,
#         'javascript': 2
#     }
# }
# chrome_options.add_experimental_option('prefs', prefs)
#
# # 禁止加载图片，加快页面载入速度
# chrome_options.add_argument('--blink-settings=imagesEnabled=false')
# # 禁用语言和设备检测功能，允许访问全部内容
# chrome_options.add_argument('--disable-blink-features=AutomationControlled')
if seleniumHeadless:#True
    # # 设置浏览器窗口大小，避免被服务器识别为无头浏览器
    # chrome_options.add_argument('--window-size=1920,1080')
    # 设置无头模式，不启动图形化界面
    chrome_options.add_argument('--headless')

# chrome_options.add_argument("--disable-blink-features")
# chrome_options.add_argument("--disable-blink-features=AutomationControlled")
```

## 完全防检测

```python
with open(os.path.join(current_path,'stealth.min.js')) as f:
    js = f.read()

driver.execute_cdp_cmd("Page.addScriptToEvaluateOnNewDocument", {
    "source": js
})
```

## 按键和By类

```python
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys

```

## 页面滚动到底部

```python
driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
```

