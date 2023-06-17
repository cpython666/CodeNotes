# FastAPI手册

## FastAPI基础

### 创建一个app并启动

```python
from fastapi import FastAPI
import uvicorn

app = FastAPI(title="网络检索",
              description="这是一个网络检索的项目",
              docs_url="/docs",
              openapi_url="/openapi")
 
if __name__ == "__main__":
    uvicorn.run("__init__:app", host="0.0.0.0", port=6004)
```

### 挂载静态资源

```python
from fastapi.staticfiles import StaticFiles

app.mount("/templates",StaticFiles(directory=os.path.join(os.path.dirname(__file__),'templates')),name="templates")
app.mount("/static",StaticFiles(directory=os.path.join(os.path.dirname(__file__),'static')), name="static")
```

### 添加蓝图

```python
from fastapi import APIRouter

router = APIRouter()

@router.get("/pagecount")
def get_page_count_():
    return get_page_count()

app.include_router(router,prefix="/api")
```

### post请求体传参：

```python
from pydantic import BaseModel
from fastapi import FastAPI

class Item(BaseModel):
    auth: str
    query: str
    
@app.post('/search')
def search_(item:Item):
    print(item.auth)
```