# P2_FastAPI+Vue全栈开发知识点总结

> JS 基础、FastAPI 入门、FastAPI 进阶、Pydantic、应用生命周期、HTTP 协议、跨域、Ubuntu/非 docker/Docker 部署、数据库配置 、Vue 前端

---

## 一、JavaScript 基础回顾（前端语言基础）

### 1.1 概述
- JS 是运行在浏览器端的脚本语言，由浏览器解释执行，负责网页与用户交互。
- 前端三大件：HTML（结构）、CSS（样式）、JS（行为）。

### 1.2 变量与数据类型
```javascript
// 弱类型语言：类型由值决定
var name = 'Tom';
var age = 18;
var isOK = true;

// 六种数据类型
typeof(10.1);          // number
typeof('abc');         // string
typeof(true);          // boolean
typeof(undefined);     // undefined
typeof(null);          // object（空对象）
typeof({a:1});         // object

// 作用域
var a = 12;            // 全局变量
function f(){ var b = 23; }   // 局部变量
```

### 1.3 函数与流程控制
```javascript
// 函数
function add(a, b){ return a + b; }
var r = add(3, 4);     // 7

// == vs === ：== 隐式转换，=== 值和类型都相等
if('12' == 12){ /* true */ }
if('12' === 12){ /* false */ }

// 数组
var arr = [1, 2, 3];
arr.push(4);           // 末尾添加
arr.pop();             // 末尾删除
arr.splice(0, 1);      // 删除第一项
```

### 1.4 对象与 DOM
```javascript
var person = { name: 'Tom', age: 25, sayName: function(){ alert(this.name); } };

// DOM 操作
document.getElementById('id');       // 获取元素
element.innerHTML = '<span>内容</span>';  // 读写内容
element.style.color = 'red';          // 样式
```

---

## 二、FastAPI 入门

### 2.1 FastAPI 简介
- FastAPI 是 Python 高性能 Web 框架，专门构建 API 服务。
- **核心优势**：原生异步支持（性能接近 Node.js/Go）、类型提示自动验证、自动生成 Swagger 交互式文档。

### 2.2 同步 vs 异步
```python
import time
import asyncio

@app.get("/sync")
def func_sync():                      # 同步：阻塞等待
    start = time.time()
    for i in range(10):
        time.sleep(1)
    return {"time": f'{time.time()-start:.2f}s'}   # 约10s

@app.get("/async")
async def func_async():               # 异步：I/O 等待时切换
    start = time.time()
    await asyncio.gather(*[asyncio.sleep(1) for _ in range(10)])
    return {"time": f'{time.time()-start:.2f}s'}   # 约1s
```
- 异步让服务器在等待 I/O 时切换执行其他任务，提升并发与资源利用率。

### 2.3 第一个 FastAPI 程序
```bash
pip install fastapi uvicorn
```
```python
from fastapi import FastAPI
app = FastAPI()

@app.get("/")
async def root():
    return {"message": "Hello World"}
```
```bash
uvicorn main:app --reload    # main:app 从 main.py 导入 app；--reload 改代码自动重启
```
- 接口地址：`http://127.0.0.1:8000`
- 交互式文档：`http://127.0.0.1:8000/docs`（Swagger UI，浏览器直接调用测试）

### 2.4 路由（Routing）
- 路由 = URL 地址与处理函数的映射关系，用装饰器定义。
```python
@app.get("/user/hello")
async def say_hello():
    return {"msg": "我正在学习 FastAPI ....."}
```

---

## 三、FastAPI 参数分类

| 参数类型 | 位置 | 作用 | 示例 | 方法 |
| --- | --- | --- | --- | --- |
| 路径参数 | URL 路径的一部分 | 指向唯一资源 | `/book/{id}` | GET |
| 查询参数 | `?` 后 `k1=v1&k2=v2` | 过滤、排序、分页 | `/news?skip=0&limit=10` | GET |
| 请求体 | HTTP 消息体中 | 创建/更新资源，携带 JSON | `{"username":"daqiu"}` | POST/PUT |

### 3.1 路径参数（Path）
```python
from fastapi import Path

# 原生类型注解
@app.get("/book/{id}")
async def get_book(id: int):
    return {"id": id, "title": f"这是第{id}本书"}

# Path 添加校验
@app.get("/book/{id}")
async def get_book(id: int = Path(..., gt=0, le=100, description="图书ID")):
    return {"id": id}

# pattern 正则校验
@app.get("/files/{file_code}")
async def get_file(file_code: str = Path(..., pattern=r"^[A-Z]{2}\d{4}$")):
    return {"file_code": file_code}
```
- Path 常用参数：`gt/ge`（大于/大于等于）、`lt/le`（小于/小于等于）、`min_length/max_length`、`description`、`...`（必填）。

### 3.2 查询参数（Query）
```python
from fastapi import Query

@app.get("/user")
async def get_user(user_id: int = Query(..., ge=1, description="用户ID")):
    return {"user_id": user_id}
```

### 3.3 请求体参数（Body + Pydantic）
```python
from pydantic import BaseModel, Field

class Book(BaseModel):
    title: str = Field(..., min_length=2, max_length=20, description="书名")
    author: str = Field(..., min_length=2, max_length=10)
    publisher: str = Field(default="出版社")
    price: float = Field(..., gt=0, description="售价")

@app.post("/register")
async def register(user: User):
    return user
```
```bash
curl -X POST http://127.0.0.1:8000/register \
  -H "Content-Type: application/json" \
  -d '{"username": "daqiu", "password": "123456"}'
```

---

## 四、FastAPI 响应

### 4.1 常用响应类型
| 响应类 | 用途 | 示例 |
| --- | --- | --- |
| `JSONResponse` | 返回 JSON（默认） | `return {"key":"value"}` |
| `HTMLResponse` | 返回 HTML | `HTMLResponse("<h1>标题</h1>")` |
| `PlainTextResponse` | 返回纯文本 | `PlainTextResponse("text")` |
| `FileResponse` | 文件下载 | `FileResponse(path)` |
| `StreamingResponse` | 流式响应（生成器） | 大文件、实时数据 |
| `RedirectResponse` | 重定向 | `RedirectResponse(url)` |

### 4.2 设置响应类型两种方式
```python
from fastapi.responses import HTMLResponse, FileResponse

# 方式一：response_class
@app.get("/html", response_class=HTMLResponse)
async def get_html():
    return "<h1>这是标题</h1>"

# 方式二：直接返回响应对象
@app.get("/file")
async def get_file():
    return FileResponse("./files/1.jpeg")
```

### 4.3 response_model（自定义输出格式）
```python
from pydantic import BaseModel

class News(BaseModel):
    id: int
    title: str
    content: str

@app.get("/news/{id}", response_model=News)
async def get_news(id: int):
    return {
        "id": id, "title": f"这是第{id}条新闻",
        "content": "正文",
        "extra": "这个字段会被过滤掉"
    }
```
- 作用：数据验证与类型转换、生成文档响应结构、防泄露敏感字段。

### 4.4 异常处理
```python
from fastapi import HTTPException

@app.get('/news/{id}')
async def get_news(id: int):
    id_list = [1, 2, 3, 4, 5, 6]
    if id not in id_list:
        raise HTTPException(status_code=404, detail="当前id不存在")
    return {"id": id, "content": "新闻内容"}
```

---

## 五、中间件（Middleware）

### 5.1 概念
- 中间件是每次请求进入 FastAPI 应用时都会执行的函数：在请求到达路由函数**之前**运行一次，响应返回客户端**之前**再运行一次。
- 作用：记录日志、身份认证、跨域处理、设置响应头、性能监控。

### 5.2 定义中间件
```python
from fastapi import FastAPI, Request

app = FastAPI()

@app.middleware("http")
async def my_middleware(request: Request, call_next):
    print('中间件开始处理 -- start')      # 请求前逻辑
    response = await call_next(request)   # 调用下一中间件或路由函数
    print('中间件处理完成 -- end')        # 响应后逻辑
    return response
```

### 5.3 多个中间件执行顺序（自下而上）
```python
@app.middleware("http")
async def middleware_one(request, call_next):
    print("中间件1 请求前")
    response = await call_next(request)
    print("中间件1 响应后")
    return response

@app.middleware("http")
async def middleware_two(request, call_next):
    print("中间件2 请求前")
    response = await call_next(request)
    print("中间件2 响应后")
    return response
```
```
执行顺序：中间件2 请求前 → 中间件1 请求前 → 路由函数 → 中间件1 响应后 → 中间件2 响应后
```

---

## 六、依赖注入（Dependency Injection）

### 6.1 概念
- 依赖注入将可重用组件与业务逻辑分离，FastAPI 自动调用依赖项并把结果**注入**路径操作函数。
- **优点**：代码复用、解耦、易于测试（可替换模拟依赖）。

### 6.2 基本使用
```python
from fastapi import Depends

def common_parameters(q: str = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}

@app.get("/items/")
async def read_items(params: dict = Depends(common_parameters)):
    return params
```

### 6.3 典型场景
- 数据库会话管理、当前用户身份认证、请求参数预处理与校验、日志记录、权限检查。

---

## 七、ORM 与 SQLAlchemy 异步版

### 7.1 ORM 概念
- ORM（对象关系映射）：在面向对象语言和关系数据库间建立映射，通过操作对象而非写 SQL 与数据库交互。
- **优势**：减少重复 SQL、代码简洁、自动处理连接和事务、自动防 SQL 注入。
- **常见工具**：SQLAlchemy（功能最强/企业级）、Django ORM（上手快）、Tortoise ORM（全异步）。

### 7.2 ORM 使用流程
1. 创建异步数据库引擎 `create_async_engine`
2. 定义模型基类和模型类（继承 `DeclarativeBase`）
3. 创建数据库表 `run_sync(Base.metadata.create_all)`
4. 路由函数注入数据库会话依赖项 `Depends`
5. 执行增删改查

### 7.3 创建异步引擎
```python
from sqlalchemy.ext.asyncio import create_async_engine

ASYNC_DATABASE_URL = "mysql+aiomysql://root:123456@localhost:3306/fastapi_test?charset=utf8"

async_engine = create_async_engine(
    ASYNC_DATABASE_URL,
    echo=True,        # 输出 SQL 日志（调试）
    pool_size=10,     # 连接池持久连接数
    max_overflow=20   # 允许额外连接数
)
```

### 7.4 定义模型类
```python
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column
from sqlalchemy import String, DateTime, func
from datetime import datetime

class Base(DeclarativeBase):
    create_time: Mapped[datetime] = mapped_column(
        DateTime, insert_default=func.now(), default=datetime.now, comment="创建时间")
    update_time: Mapped[datetime] = mapped_column(
        DateTime, insert_default=func.now(), onupdate=func.now(),
        default=datetime.now, comment="修改时间")

class Book(Base):
    __tablename__ = "book"
    id: Mapped[int] = mapped_column(primary_key=True)
    bookname: Mapped[str] = mapped_column(String(255))
    author: Mapped[str] = mapped_column(String(255))
    price: Mapped[float] = mapped_column(default=0.0)
```

### 7.5 建表与会话依赖
```python
async def create_tables():
    async with async_engine.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)

@app.on_event("startup")
async def startup_event():
    await create_tables()

# 异步会话工厂 + 依赖项
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy.orm import sessionmaker

AsyncSessionLocal = sessionmaker(bind=async_engine, class_=AsyncSession, expire_on_commit=False)

async def get_database() -> AsyncSession:
    async with AsyncSessionLocal() as session:
        yield session
```

### 7.6 数据库 CRUD
```python
from sqlalchemy import select, func

# 查所有
@app.get("/books")
async def get_all_books(db: AsyncSession = Depends(get_database)):
    result = await db.execute(select(Book))
    return result.scalars().all()

# 按主键查
@app.get("/books/{book_id}")
async def get_book(book_id: int, db: AsyncSession = Depends(get_database)):
    book = await db.get(Book, book_id)
    if book is None:
        raise HTTPException(status_code=404, detail="Book not found")
    return book

# 条件查询：比较 / like 模糊 / 与非 / in
result = await db.execute(select(Book).where(Book.price > 100))
result = await db.execute(select(Book).where(Book.author.like("曹%")))  # %零个或多个 _单个
result = await db.execute(select(Book).where((Book.author == "曹雪芹") & (Book.price == 200)))
result = await db.execute(select(Book).where(Book.id.in_([1, 2, 3])))

# 聚合
c = (await db.execute(select(func.count(Book.id)))).scalar()
avg = (await db.execute(select(func.avg(Book.price)))).scalar()

# 分页：offset = (页码-1) × 每页数量
skip = (page - 1) * page_size
result = await db.execute(select(Book).offset(skip).limit(page_size))

# 新增
new_book = Book(bookname=data.bookname, author=data.author, price=data.price)
db.add(new_book); await db.commit(); await db.refresh(new_book)

# 更新
book = await db.get(Book, book_id)
if update_data.price is not None:
    book.price = update_data.price
await db.commit()

# 删除
await db.delete(book); await db.commit()
```

### 7.7 查询结果提取方法总结
| 方法 | 说明 | 示例 |
| --- | --- | --- |
| `scalars().all()` | 获取所有结果（列表） | `books = result.scalars().all()` |
| `scalars().first()` | 第一条（无则 None） | `book = result.scalars().first()` |
| `scalar_one_or_none()` | 一条，无则 None，多条抛异常 | `result.scalar_one_or_none()` |
| `scalar()` | 标量值（聚合） | `total = result.scalar()` |
| `db.get(Model, pk)` | 按主键获取单条 | `book = await db.get(Book, 1)` |

---

## 八、Pydantic 数据验证

### 8.1 基础校验：模型与自动类型转换
```python
from pydantic import BaseModel
from typing import Optional
from datetime import datetime

class User(BaseModel):
    id: int                      # 必填，自动把字符串 "101" 转整数
    name: str
    email: str
    age: int
    is_active: bool = True       # 可选，默认 True
    signup_time: Optional[datetime] = None

data = {"id": "101", "name": "Alice", "email": "a@e.com", "age": "25"}
user = User(**data)
print(user.id)      # 101（自动转 int）
```
- 没有默认值的字段必填；用 `Optional` 或默认值则为可选；自动类型转换。

### 8.2 Field 字段级约束
```python
from pydantic import BaseModel, Field

class Product(BaseModel):
    name: str = Field(..., min_length=1, max_length=100, description="产品名称")
    price: float = Field(..., gt=0, description="价格必须为正数")
    quantity: int = Field(default=0, ge=0, le=10000)
    sku: str = Field(..., pattern=r'^[A-Z]{3}-\d{4}$')
```

### 8.3 自定义校验器
```python
from pydantic import BaseModel, field_validator, model_validator

class Registration(BaseModel):
    username: str
    password: str
    password_confirm: str

    @field_validator('username')         # 单字段校验
    @classmethod
    def username_alphanumeric(cls, v):
        if not v.isalnum():
            raise ValueError('用户名只能包含字母和数字')
        return v.lower()

    @model_validator(mode='after')        # 跨字段校验
    def passwords_match(self):
        if self.password != self.password_confirm:
            raise ValueError('两次输入的密码不一致')
        return self
```

### 8.4 嵌套模型
```python
from typing import List

class Address(BaseModel):
    city: str
    street: str

class Order(BaseModel):
    order_id: int
    address: Address            # 嵌套对象
    items: List[Address]        # 嵌套列表

order = Order(**{
    "order_id": 1,
    "address": {"city": "北京", "street": "朝阳路"},
    "items": [{"city": "上海", "street": "南京路"}]
})
print(order.address.city)       # 北京
```

### 8.5 Pydantic 校验流程总结
1. 定义模型（`BaseModel` + 类型提示基础检查）
2. 添加约束（`Field` 限制范围/长度）
3. 定制逻辑（`@field_validator` / `@model_validator`）
4. 实例化传入数据，自动解析清洗验证，错误抛 `ValidationError`

---

## 九、FastAPI 应用生命周期

### 9.1 生命周期事件
- `startup`：应用开始接收请求**之前**执行（初始化）。
- `shutdown`：应用停止运行**之前**执行（清理释放）。
- 类比咖啡店：开店准备 / 营业 / 打烊收尾。

### 9.2 on_event 经典用法
```python
@app.on_event("startup")
async def startup_event():
    print("应用正在启动...")
    # 连接数据库、加载配置、预热缓存

@app.on_event("shutdown")
async def shutdown_event():
    print("应用正在关闭...")
    # 关闭数据库、保存日志、释放资源
```
- 可多次注册同一事件，按注册顺序执行；推荐统一用 `async def`。
- **避坑**：`on_event` 只在顶层 `FastAPI()` 实例上生效，`APIRouter` 上无效。

### 9.3 lifespan 方式（官方推荐）
```python
from contextlib import asynccontextmanager
from fastapi import FastAPI

@asynccontextmanager
async def lifespan(app: FastAPI):
    # --- 启动阶段 ---
    print("正在初始化资源...")
    db_connection = "Connected!"
    app.state.db = db_connection       # 存到 app.state，避免全局变量
    yield                               # 应用在这里开始运行
    # --- 关闭阶段 ---
    print("正在清理资源...")
    print("资源清理完成")

app = FastAPI(lifespan=lifespan)

@app.get("/status")
async def get_status():
    return {"db_status": app.state.db}
```
- `yield` 前代码启动时执行，`yield` 后关闭时执行；用 `app.state` 共享资源。

### 9.4 on_event vs lifespan
| 特性 | on_event（旧） | lifespan（新） |
| --- | --- | --- |
| 逻辑组织 | 启动/关闭分离 | 集中，清晰 |
| 变量共享 | 依赖全局变量，易出错 | 通过 `app.state` 安全 |
| 官方推荐 | 已弃用 | **强烈推荐** |

---

## 十、HTTP 协议

### 10.1 HTTP 概述
- HTTP（超文本传输协议）：Web 服务器与客户端传输 HTML、图片、JSON 的规则。
- 特点：应用层协议（基于 TCP/IP）、**无状态**（每个请求独立）、可扩展。
- 通信流程：建 TCP 连接（80/http，443/https）→ 发请求 → 收响应 → 关闭或 Keep-Alive 复用。

### 10.2 请求报文结构（四部分）
```
GET /index.html HTTP/1.1            ← 请求行（方法 + URL + 版本）
Host: www.example.com               ← 请求头
User-Agent: Mozilla/5.0
Accept: text/html
                                    ← 空行（CRLF \r\n）
（GET 一般无请求体）
```

### 10.3 请求方法
- `GET` 获取资源、`POST` 提交数据、`PUT` 替换、`DELETE` 删除、`HEAD` 只取响应头、`OPTIONS` 询问支持方法。

### 10.4 常见请求头
- `Host`（域名端口，HTTP/1.1 必填）、`User-Agent`（客户端类型）、`Accept`（响应格式）、`Content-Type`（请求体格式：表单/JSON/multipart）、`Cookie`、`Referer`。

### 10.5 响应报文结构
```
HTTP/1.1 200 OK                     ← 状态行（版本 + 状态码 + 短语）
Content-Type: text/html             ← 响应头
Content-Length: 1024
Set-Cookie: sessionId=abc123
                                    ← 空行
<html><body>...</body></html>       ← 响应体
```

### 10.6 状态码分类（口诀）
- **2xx** 成功：200 OK、201 Created
- **3xx** 重定向：301 永久、302 临时、304 使用缓存
- **4xx** 客户端错：400 语法错误、401 需认证、403 无权限、404 不存在
- **5xx** 服务器锅：500 内部错误、502 网关错误、503 不可用

### 10.7 无状态与解决方案
- 无状态：服务器不保留两次请求之间信息，每个请求都是"陌生人"。
- 解决方案：Cookie（客户端存）、Session（服务端存，Cookie 中 sessionId 关联）、Token/JWT（客户端存加密令牌，放 Authorization 头）。

### 10.8 HTTP 版本演进
| 版本 | 年份 | 特点 |
| --- | --- | --- |
| HTTP/0.9 | 1991 | 只有 GET，无头无状态 |
| HTTP/1.0 | 1996 | 加 POST/HEAD、状态码，每次请求新建连接 |
| HTTP/1.1 | 1997 | Keep-Alive、管道化、Host 头，至今最广 |
| HTTP/2 | 2015 | 二进制分帧、多路复用、服务器推送，解决队头阻塞 |
| HTTP/3 | 2022 | 基于 UDP 的 QUIC，减少延迟抗丢包 |

### 10.9 HTTPS
- HTTPS = HTTP + TLS/SSL；增加加密（防窃听）、完整性校验（防篡改）、身份认证（证书，防冒充）。
- 流程：验证服务器证书（CA 签发）→ 协商对称密钥 → 加密传输。

### 10.10 调试工具
- 浏览器 F12 → Network；`curl -v`、`curl -X POST -H ... -d ...`；Wireshark/Fiddler/Charles；Postman/HTTPie。

---

## 十一、跨域（CORS）

### 11.1 什么是跨域
- 浏览器安全策略（同源策略）禁止网页向**不同源**服务器发起请求。
- 源 = 协议 + 域名 + 端口，三者相同才同源。
- 例：`http://a.com` → `http://b.com/api`（域名不同）、`http://a.com:8080` → `:3000`（端口不同）均跨域。
- 报错实质：请求已发到服务器并返回，但浏览器拦截数据交给 JS。

### 11.2 FastAPI 解决跨域（CORSMiddleware）
```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

origins = [
    "http://localhost:5500",    # 前端开发地址
    "http://127.0.0.1:5500",
    "http://localhost:3000",
]

app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,      # 允许的源
    allow_credentials=True,     # 是否允许携带 cookie
    allow_methods=["*"],        # 允许的方法
    allow_headers=["*"],        # 允许的请求头
)

@app.get("/data")
def get_data():
    return {"message": "Hello, World!"}
```
- 关键点：`allow_origins` 必须包含前端源；生产不要 `allow_origins=["*"]` + `allow_credentials=True`（浏览器拒绝）；非简单请求会先发 OPTIONS 预检，中间件自动处理。

---

## 十二、Vue 前端知识点（扩充）

> 以下为按用户要求扩充的 Vue 前端核心知识。P2 全栈项目前端基于 Vue 3 + Element Plus，建议掌握到能独立开发管理后台的水平。

### 12.1 Vue 概述
- Vue 是一套用于构建用户界面的渐进式 JavaScript 框架，核心是**数据驱动视图 + 组件化**。
- 版本：Vue 2（Options API）已停止维护；**Vue 3**（Composition API + `<script setup>`）为当前主流。
- 引入方式：
```html
<!-- 方式一：CDN 引入（简单练习） -->
<script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>

<!-- 方式二：脚手架（正式项目，推荐） -->
npm create vue@latest        # Vue 3 + Vite
```

### 12.2 数据绑定（核心概念）
- **双向数据绑定**：`v-model`，视图与数据同步更新。
- **数据渲染**：`{{ }}` 插值、`v-text`、`v-html`。
```vue
<template>
  <div>
    <input v-model="message" placeholder="请输入" />
    <p>{{ message }}</p>            <!-- 文本插值 -->
    <p v-text="message"></p>        <!-- 等价 -->
    <p v-html="rawHtml"></p>        <!-- 渲染 HTML（慎用，防 XSS） -->
  </div>
</template>

<script setup>
import { ref } from 'vue'
const message = ref('你好，Vue')     // ref 创建响应式数据，模板中自动解包
const rawHtml = ref('<b>加粗内容</b>')
</script>
```

### 12.3 条件与列表渲染
```vue
<template>
  <div>
    <p v-if="score >= 90">优秀</p>
    <p v-else-if="score >= 60">及格</p>
    <p v-else>不及格</p>

    <ul>
      <li v-for="(item, index) in list" :key="item.id">
        {{ index }} - {{ item.name }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const score = ref(85)
const list = ref([
  { id: 1, name: '苹果' },
  { id: 2, name: '香蕉' }
])
</script>
```
- `v-for` 必须绑定 `:key`（唯一标识，帮助 Vue 高效复用 DOM）。

### 12.4 事件处理
```vue
<template>
  <button @click="addCount">点击 +1（当前 {{ count }}）</button>
  <input @keyup.enter="onEnter" />
</template>

<script setup>
import { ref } from 'vue'
const count = ref(0)
function addCount() { count.value++ }       // script 中要用 .value
function onEnter(e) { console.log(e.target.value) }
</script>
```
- `@click`、`@keyup.enter`、`@submit` 等；事件修饰符 `.stop`、`.prevent` 常用。

### 12.5 计算属性与侦听器
```vue
<template>
  <p>姓：{{ firstName }}，名：{{ lastName }}，全名：{{ fullName }}</p>
  <p>数量：{{ count }}，翻倍：{{ doubleCount }}</p>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
const firstName = ref('张')
const lastName = ref('三')
const count = ref(5)

// computed：依赖数据变化时自动重新计算（有缓存）
const fullName = computed(() => firstName.value + lastName.value)
const doubleCount = computed(() => count.value * 2)

// watch：侦听数据变化做副作用
watch(count, (newVal, oldVal) => {
  console.log(`count 从 ${oldVal} 变为 ${newVal}`)
})
</script>
```

### 12.6 组件化开发
- 组件是可复用的独立模块，分**父组件**和**子组件**。
- 父传子用 **props**，子传父用 **emit 事件**。
```vue
<!-- 子组件 Child.vue -->
<template>
  <div>
    <p>子组件收到：{{ title }}</p>
    <button @click="emit('update', '来自子组件的消息')">通知父组件</button>
  </div>
</template>

<script setup>
const props = defineProps({ title: { type: String, default: '' } })
const emit = defineEmits(['update'])
</script>
```
```vue
<!-- 父组件 App.vue -->
<template>
  <Child title="你好，我是父组件" @update="handleUpdate" />
</template>

<script setup>
import Child from './Child.vue'
function handleUpdate(msg) { console.log('父组件收到：', msg) }
</script>
```

### 12.7 路由（Vue Router）
- 管理页面跳转，实现单页应用（SPA）。
```bash
npm install vue-router@4
```
```javascript
// router/index.js
import { createRouter, createWebHistory } from 'vue-router'
import Home from '../views/Home.vue'
import About from '../views/About.vue'

const routes = [
  { path: '/', name: 'home', component: Home },
  { path: '/about', name: 'about', component: About },
]

const router = createRouter({
  history: createWebHistory(),
  routes
})

export default router
```
```vue
<template>
  <router-link to="/">首页</router-link>
  <router-link to="/about">关于</router-link>
  <router-view />          <!-- 路由组件渲染出口 -->
</template>
```
- 传参：路径参数 `/user/:id` → `route.params.id`；编程式导航 `router.push('/about')`。
- 路由守卫 `beforeEach` 可用于登录鉴权。

### 12.8 状态管理（Pinia）
- 管理全局共享状态（登录用户、购物车等），替代 Vuex（Vue 3 推荐 Pinia）。
```bash
npm install pinia
```
```javascript
// store/user.js
import { defineStore } from 'pinia'
import { ref } from 'vue'

export const useUserStore = defineStore('user', () => {
  const token = ref('')
  const setToken = (t) => { token.value = t }
  return { token, setToken }
})
```
```vue
<script setup>
import { useUserStore } from '../store/user'
const userStore = useUserStore()
userStore.setToken('abc123')
console.log(userStore.token)
</script>
```

### 12.9 Axios 请求后端（对接 FastAPI）
- axios 是常用 HTTP 库，配合 Vue 调用后端接口。
```bash
npm install axios
```
```javascript
// api/request.js
import axios from 'axios'

const request = axios.create({
  baseURL: 'http://localhost:8000/api/v1',   // 指向 FastAPI 后端
  timeout: 5000
})

// 请求拦截器：自动携带 token
request.interceptors.request.use(config => {
  const token = localStorage.getItem('token')
  if (token) config.headers.Authorization = `Bearer ${token}`
  return config
})

// 响应拦截器：统一处理错误
request.interceptors.response.use(
  res => res.data,
  err => {
    if (err.response?.status === 401) { /* 跳转登录 */ }
    return Promise.reject(err)
  }
)

export default request
```
```vue
<script setup>
import request from '../api/request'
import { ref, onMounted } from 'vue'

const books = ref([])
async function getBooks() {
  const data = await request.get('/books')
  books.value = data
}
onMounted(getBooks)          // 页面加载时请求
</script>
```

### 12.10 生命周期钩子（Vue 3）
| 钩子 | 时机 |
| --- | --- |
| `onMounted` | 组件挂载后（发请求） |
| `onUpdated` | 数据更新后 |
| `onBeforeUnmount` | 卸载前（清理定时器） |
```vue
<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'
const timer = ref(null)
onMounted(() => { timer.value = setInterval(() => console.log('tick'), 1000) })
onBeforeUnmount(() => clearInterval(timer.value))
</script>
```

### 12.11 与 Element Plus 配合（管理后台常用）
```bash
npm install element-plus
```
```javascript
// main.js 完整引入
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
app.use(ElementPlus)
```
```vue
<template>
  <el-table :data="books" border>
    <el-table-column prop="bookname" label="书名" />
    <el-table-column prop="author" label="作者" />
    <el-table-column prop="price" label="价格" />
    <el-table-column label="操作">
      <template #default="{ row }">
        <el-button type="danger" size="small" @click="removeBook(row.id)">删除</el-button>
      </template>
    </el-table-column>
  </el-table>
</template>
```

### 12.12 前端构建部署（对接 Nginx）
```bash
npm run build        # 生成 dist 静态文件夹
```
- 把 `dist` 上传到服务器，由 Nginx 托管；`/api/` 请求反向代理到 FastAPI 后端。

---

## 十三、项目部署

### 13.1 方案一：非 Docker（直接部署）
```bash
# 1. 安装 MySQL
sudo apt update && sudo apt install mysql-server -y
sudo systemctl start mysql && sudo systemctl enable mysql

# 2. 设置 root 密码 + 建库
sudo mysql -u root
#   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
#   FLUSH PRIVILEGES;
mysql -u root -p123456 -e "CREATE DATABASE qidian_ai CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"

# 3. 导入表结构
mysql -u root -p123456 qidian_ai < /home/zhao/backend/test_data.sql

# 4. Python 虚拟环境 + 依赖（国内源加速）
sudo apt install python3-venv python3-pip libmysqlclient-dev -y
cd /home/zhao/backend
python3 -m venv venv && source venv/bin/activate
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

# 5. 修改 .env 数据库连接
nano /home/zhao/backend/.env
#   DATABASE_URL=mysql+pymysql://root:123456@localhost:3306/qidian_ai

# 6. 启动后端
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
curl http://127.0.0.1:8000/api/v1/audit/categories?page=1&page_size=20

# 7. 安装 Node + 构建前端
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
cd /home/zhao/frontend && npm install && npm run build   # 生成 dist

# 8. 安装配置 Nginx
sudo apt install nginx -y
sudo nano /etc/nginx/sites-available/qidian
#   server {
#     listen 80;
#     server_name 192.168.0.109;
#     location / { root /home/zhao/frontend/dist; try_files $uri $uri/ /index.html; }
#     location /api/ { proxy_pass http://127.0.0.1:8000/api/v1/; proxy_set_header Host $host; }
#   }
sudo ln -s /etc/nginx/sites-available/qidian /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t && sudo systemctl restart nginx
sudo ufw allow 80/tcp
```

### 13.2 方案二：Docker Compose 一键编排
**本机改数据库地址**：把 `DATABASE_URL` 中的 `localhost` 改为 `db:3306`（Docker 内部数据库代号）。

**创建 Dockerfile**
```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY . /app
RUN pip install --no-cache-dir -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

**创建 docker-compose.yml**
```yaml
version: '3'
services:
  db:
    image: mysql:8.0
    container_name: my_mysql_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 123456
      MYSQL_DATABASE: qidian_ai
    ports:
      - "3307:3306"
    volumes:
      - mysql_data:/var/lib/mysql
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 10s
      timeout: 5s
      retries: 5

  web:
    build: .
    container_name: my_fastapi_app
    restart: always
    ports:
      - "8000:8000"
    environment:
      DATABASE_URL: mysql+aiomysql://root:123456@db:3306/qidian_ai
    depends_on:
      db:
        condition: service_healthy

volumes:
  mysql_data:
```

**一键启动**
```bash
sudo docker-compose up -d      # 或 sudo docker compose up -d
# 访问 http://你的IP:8000/api/v1/docs
```

### 13.3 部署要点与排查
- **Nginx 反向代理**：前端静态文件由 Nginx 托管，`/api/` 请求转发到后端 `127.0.0.1:8000`。
- 生产环境用 **systemd** 注册后端服务（开机自启、自动重启），避免终端关闭即停。
- 排查：`sudo systemctl status mysql`、`netstat -tlnp | grep 8000`、`sudo ufw status`、`/var/log/nginx/error.log`、检查 `.env` 密码。
- 数据库连接：`.env` 用 `mysql+aiomysql://`（异步）；普通同步可用 `mysql+pymysql://`。
- SQL 导入：`mysql -u root -p qidian_ai < test_data.sql`；数据库必须已存在；遇错加 `--force` 继续；登录后可用 `SOURCE 路径`。

---

## 十四、完整示例：图书管理 API（FastAPI + SQLAlchemy）

```python
from fastapi import FastAPI, Depends, HTTPException
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy import select, func
from pydantic import BaseModel
from typing import Optional, List

app = FastAPI()

class BookCreate(BaseModel):
    bookname: str
    author: str
    price: float

class BookUpdate(BaseModel):
    bookname: Optional[str] = None
    author: Optional[str] = None
    price: Optional[float] = None

class BookOut(BaseModel):
    id: int
    bookname: str
    author: str
    price: float

@app.post("/books", response_model=BookOut)
async def create_book(data: BookCreate, db: AsyncSession = Depends(get_database)):
    new_book = Book(**data.dict())
    db.add(new_book)
    await db.commit()
    await db.refresh(new_book)
    return new_book

@app.get("/books", response_model=List[BookOut])
async def list_books(skip: int = 0, limit: int = 100, db: AsyncSession = Depends(get_database)):
    result = await db.execute(select(Book).offset(skip).limit(limit))
    return result.scalars().all()

@app.get("/books/{book_id}", response_model=BookOut)
async def get_book(book_id: int, db: AsyncSession = Depends(get_database)):
    book = await db.get(Book, book_id)
    if not book:
        raise HTTPException(status_code=404, detail="Book not found")
    return book

@app.put("/books/{book_id}", response_model=BookOut)
async def update_book(book_id: int, data: BookUpdate, db: AsyncSession = Depends(get_database)):
    book = await db.get(Book, book_id)
    if not book:
        raise HTTPException(status_code=404, detail="Book not found")
    for field, value in data.dict(exclude_unset=True).items():
        setattr(book, field, value)
    await db.commit()
    await db.refresh(book)
    return book

@app.delete("/books/{book_id}")
async def delete_book(book_id: int, db: AsyncSession = Depends(get_database)):
    book = await db.get(Book, book_id)
    if not book:
        raise HTTPException(status_code=404, detail="Book not found")
    await db.delete(book)
    await db.commit()
    return {"message": "deleted"}
```

---

## 十五、学习路线建议

1. **课程主线**：JS 基础（前端语言）→ FastAPI 入门（异步/路由/参数/响应/异常）→ FastAPI 进阶（中间件/依赖注入/ORM）→ Pydantic（数据验证）→ 生命周期 → HTTP 协议 → 跨域 → 部署（非 docker / Docker）。**Vue 前端为重要扩充**：数据绑定 → 组件化 → 路由 → 状态管理 → axios 对接后端 → Element Plus。
2. **重点难点**：
   - 异步 vs 同步（asyncio.gather）；
   - 参数三分类（路径/查询/请求体）+ Field 校验；
   - `response_model` 输出过滤；
   - 中间件执行顺序（自下而上）；
   - 依赖注入 `Depends` 复用会话；
   - SQLAlchemy 异步 CRUD 与结果提取方法（scalars/scalar_one_or_none）；
   - Pydantic `field_validator` / `model_validator` / 嵌套模型；
   - `on_event` vs `lifespan`；
   - HTTP 报文结构与状态码、跨域原理；
   - Vue 双向绑定、props/emit、路由、axios 拦截器。
3. **全栈贯通项目**：以「图书管理后台」为例——FastAPI 提供增删改查 API → CORS 解决跨域 → Vue + Element Plus 表格展示 → axios 请求 → 部署到 Ubuntu（Nginx 托管前端 + 反代后端）。
4. **衔接下一阶段**：可进阶到 FastAPI 高级（WebSocket、BackgroundTasks、JWT 鉴权）、Pydantic 进阶、Vue 生态（Vite、TypeScript、Vuex→Pinia、UI 组件库）、DevOps（Docker、CI/CD、systemd）。


