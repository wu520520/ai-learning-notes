# P1_Web全栈开发基础知识点总结

> 涵盖 **14 个模块**：闭包装饰器、HTML/CSS、HTTP、JavaScript、jQuery、Linux 基础命令、Linux 高级命令、miniweb、多任务、MySQL 基础、MySQL 高级、MySQL 查询、网络编程、正则表达式。**每个知识点均配有可直接运行/执行的代码示例**

---

## 一、HTML 与 CSS（网页结构与样式）

### 1.1 前端三大件
| 技术 | 职责 |
| --- | --- |
| HTML | 负责网页**结构**（骨架） |
| CSS | 负责网页**样式**（美化 + 布局） |
| JavaScript | 负责网页**行为**（交互效果） |

### 1.2 HTML 基础
- **定义**：HTML（HyperText Mark-up Language）超文本标记语言，是开发网页的语言。
- **超文本两层含义**：可包含图片/视频/音频；可跳转到其它网页。
- **基本结构**：
```html
<!DOCTYPE html>            <!-- 文档声明：指定 HTML5 版本 -->
<html>
    <head>
        <meta charset="UTF-8">
        <title>网页标题</title>   <!-- head 设置标题/编码/引入css js -->
    </head>
    <body>
          网页显示内容              <!-- body 编写显示内容 -->
    </body>
</html>
```
- **标签分类**：
  - 双标签（闭合标签）：`<h1></h1>`、`<div></div>`、`<p></p>`，可嵌套和承载内容
  - 单标签（空标签）：`<br>`、`<img>`、`<hr>`
  - 建议小写；`<img src="..." alt="...">`、`<a href="...">` 为带属性标签

### 1.3 常用标签
```html
<!-- 标题 -->
<h1>一级标题</h1>
<h2>二级标题</h2>
<!-- 块级容器 -->
<div>这是一个div标签</div>
<p>这是一个段落标签</p>
<!-- 换行 / 分隔线 -->
<br>
<hr>
<!-- 图片 / 链接 -->
<img src="images/pic.jpg" alt="图片">
<a href="http://www.baidu.com">百度网</a>
<!-- 无序列表（顺序无要求） -->
<ul>
    <li>列表标题一</li>
    <li>列表标题二</li>
</ul>
<!-- 有序列表（顺序有要求） -->
<ol>
    <li><a href="#">列表标题一</a></li>
</ol>
```

**表格**
```html
<table>
    <tr>
        <th>姓名</th>   <!-- 表头 -->
        <th>年龄</th>
    </tr>
    <tr>
        <td>张三</td>   <!-- 列 -->
        <td>18</td>
    </tr>
</table>
```
```css
/* 表格边线合并 */
table{ border-collapse: collapse; }
```

**表单（提交数据到 web 服务器）**
```html
<form action="https://www.baidu.com" method="GET">
    <p><label>姓名：</label><input type="text" name="username" value="11" /></p>
    <p><label>密码：</label><input type="password" name="password" /></p>
    <p>
        <label>性别：</label>
        <input type="radio" name="gender" value="0" /> 男
        <input type="radio" name="gender" value="1" /> 女
    </p>
    <p>
        <label>爱好：</label>
        <input type="checkbox" name="like" value="sing" /> 唱歌
        <input type="checkbox" name="like" value="run" /> 跑步
    </p>
    <p><label>照片：</label><input type="file" name="person_pic"></p>
    <p><label>个人描述：</label><textarea name="about"></textarea></p>
    <p>
        <label>籍贯：</label>
        <select name="site">
            <option value="0">北京</option>
            <option value="1">上海</option>
        </select>
    </p>
    <p>
        <input type="submit" value="提交">
        <input type="reset" value="重置">
    </p>
</form>
```
- 关键属性：`action` 提交地址、`method` GET/POST；元素 `name` 参数名、`value` 参数值。
- input 的 type：text / password / radio / checkbox / file / submit / reset / button。

### 1.4 资源路径
- 相对路径（推荐）：`<img src="./images/logo.png">` 或 `src="images/logo.png"`（从当前文档目录算起）
- 绝对路径：`src="/Users/apple/Desktop/.../logo.png"`、`src="C:\demo\images\001.jpg"`（从根目录算起，换电脑易失效）

### 1.5 CSS 基础
- 语法：`选择器{ 属性名:属性值; ... }`
- **三种引入方式**：
```html
<!-- 行内式（几乎不用） -->
<div style="width:100px; height:100px; background:red ">hello</div>

<!-- 内嵌式（学习阶段） -->
<head>
   <style type="text/css">
      h3{ color:red; }
   </style>
</head>

<!-- 外链式（公司开发用，样式与结构分离） -->
<link rel="stylesheet" type="text/css" href="css/main.css">
```

### 1.6 CSS 选择器（六种）
```css
/* 标签选择器：做通用设置 */
p{ color: red; }
/* 类选择器：应用最多，可复用，一个标签可多个类（空格分隔） */
.blue{ color:blue; }
.big{ font-size:20px; }
/* 层级/后代选择器 */
div p{ color: red; }
.con span{ color:red; }
/* id 选择器：唯一，不推荐复用 */
#box{ color:red; }
/* 组选择器：逗号分隔公共样式 */
.box1,.box2,.box3{ width:100px; height:100px; }
/* 伪类选择器：交互改变效果 */
.box1:hover{ width:300px; }
```
```html
<div class="blue">这是一个div</div>
<h3 class="blue big box">这是一个标题</h3>
<p id="box">唯一 id</p>
```

### 1.7 盒子模型（核心难点）
```css
.box2{
    width: 100px;      /* 内容宽度（不是盒子总宽！） */
    height: 100px;     /* 内容高度 */
    background: red;
    border: 10px solid black;      /* 边框 */
    padding: 10px;                 /* 内边距：内容到边框距离 */
    margin: 10px;                  /* 外边距：盒子到外界距离 */
    float: left;                   /* 浮动，让块元素排一行 */
}
```
- **真实尺寸**：盒子宽度 = width + padding左右 + border左右；高度同理。**不受 margin 影响**。
- padding/margin 简写（顺时针上右下左）：1 值四边、2 值上下+左右、3 值上+左右+下、4 值上右下左。

### 1.8 常用样式属性
```css
.box1{
    width: 200px;
    height: 200px;
    background: yellow;                 /* 背景色 */
    /* background: url(images/logo.png);  背景图片 */
    border: 1px solid black;            /* 四边边框 */
    /* 也可分边：border-top/left/right/bottom */
    padding-top: 10px; padding-left: 10px;   /* 单边内边距 */
    margin-top: 10px; margin-left: 10px;     /* 单边外边距 */
    float: left;
}

p{
    font-size: 20px;                 /* 字号 */
    font-family: "Microsoft YaHei";  /* 字体（中文用微软雅黑） */
    font-weight: bold;               /* 加粗 */
    color: red;
    line-height: 100px;              /* 行高 */
    text-align: center;              /* 水平居中 */
    text-indent: 40px;               /* 首行缩进 */
    text-decoration: underline;      /* 下划线 */
}

/* 显示特性 display */
.con{ display: none; }       /* 隐藏且不占位置 */
.box{ display: inline; }     /* 行内显示（不能设宽高） */
.link01{ display: block; }   /* 块显示 */

/* 元素溢出 overflow */
.box1{
    width: 100px; height: 200px;
    overflow: auto;          /* visible/hidden/auto；常用 hidden 解决溢出 */
}
```

---

## 二、HTTP 协议（浏览器与服务器通信）

### 2.1 HTTP 概述
- **HTTP**（HyperText Transfer Protocol）超文本传输协议，1991 年设计；基于 **TCP**，先建连接再传数据；规定浏览器与 Web 服务器通信数据的格式。

### 2.2 URL（统一资源定位符）
```
https://news.163.com/18/1122/10/E178J2O4000189FH.html
协议部分: https://       域名部分: news.163.com    资源路径: /18/...
https://news.163.com/hello.html?page=1&count=10   查询参数: ?page=1&count=10
```
- 域名 = IP 地址的别名；第一个参数用 `?`，后续用 `&` 连接。

### 2.3 HTTP 请求报文（GET / POST）
**GET 请求（无请求体）**
```
GET / HTTP/1.1\r\n                      # 请求行：方式 + 路径 + 版本
Host: www.itcast.cn\r\n                 # 请求头：主机+端口
Connection: keep-alive\r\n              # 长连接
User-Agent: Mozilla/5.0 (...)\r\n       # 客户端名称
Cookie: pgv_pvi=1246921728;\r\n
\r\n                                     # 空行（不能省略）
```
**POST 请求（有请求体）**
```
POST /xmweb?host=mail.itcast.cn HTTP/1.1\r\n
Host: mail.itcast.cn\r\n
Content-Type: application/x-www-form-urlencoded\r\n   # 告诉服务端数据类型
User-Agent: Mozilla/5.0 (...)\r\n
\r\n
username=hello&pass=hello\r\n           # 请求体：请求参数
```
- 结构：请求行、请求头、空行、请求体（可选）；各项用 `\r\n`。GET 获取数据，POST 提交数据。

### 2.4 HTTP 响应报文
```
HTTP/1.1 200 OK\r\n                     # 响应行：版本 + 状态码 + 描述
Server: Tengine\r\n                     # 响应头
Content-Type: text/html; charset=UTF-8\r\n
Transfer-Encoding: chunked\r\n          # 与 Content-Length 二选一
Connection: keep-alive\r\n
Date: Fri, 23 Nov 2018 02:01:05 GMT\r\n
\r\n                                    # 空行
<!DOCTYPE html>... </html>              # 响应体
```
- **状态码**：200 成功、307 重定向、400 错误请求、404 资源不存在、500 服务器错误。

### 2.5 查看通信过程
- Chrome 开发者工具（右击「检查」/ F12）→ Network 面板；Headers 分 General / Response Headers / Request Headers；Response 看响应体。
- **搭建 Python 自带静态 Web 服务器**：
```bash
python3 -m http.server 9000   # 端口不指定默认 8000
```

### 2.6 静态 Web 服务器开发（贯穿实战）

**① 返回固定页面数据**
```python
import socket
if __name__ == '__main__':
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
    tcp_server_socket.bind(("", 9000))
    tcp_server_socket.listen(128)
    while True:
        new_socket, ip_port = tcp_server_socket.accept()
        recv_client_data = new_socket.recv(4096)
        recv_client_content = recv_client_data.decode("utf-8")
        print(recv_client_content)
        # 读取固定页面
        with open("static/index.html", "rb") as file:
            file_data = file.read()
        # 组装响应报文
        response_line = "HTTP/1.1 200 OK\r\n"
        response_header = "Server: PWS1.0\r\n"
        response_body = file_data
        response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
        new_socket.send(response_data)
        new_socket.close()
```

**② 返回指定页面（解析请求路径 + 404）**
```python
import socket
def main():
    tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
    tcp_server_socket.bind(("", 9000))
    tcp_server_socket.listen(128)
    while True:
        new_socket, ip_port = tcp_server_socket.accept()
        recv_client_data = new_socket.recv(4096)
        if len(recv_client_data) == 0:
            print("关闭浏览器了")
            new_socket.close()
            return
        recv_client_content = recv_client_data.decode("utf-8")
        # 分割请求行，取资源路径
        request_list = recv_client_content.split(" ", maxsplit=2)
        request_path = request_list[1]
        if request_path == "/":
            request_path = "/index.html"   # 根目录指定首页
        try:
            with open("static" + request_path, "rb") as file:
                file_data = file.read()
        except Exception as e:
            # 文件不存在 → 404
            response_line = "HTTP/1.1 404 Not Found\r\n"
            response_header = "Server: PWS1.0\r\n"
            with open("static/error.html", "rb") as file:
                file_data = file.read()
            response_body = file_data
            response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
            new_socket.send(response_data)
        else:
            response_line = "HTTP/1.1 200 OK\r\n"
            response_header = "Server: PWS1.0\r\n"
            response_body = file_data
            response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
            new_socket.send(response_data)
        finally:
            new_socket.close()
if __name__ == '__main__':
    main()
```

**③ 面向对象 + 多任务 + 命令行端口（完整版）**
```python
import socket
import threading
import sys

class HttpWebServer(object):
    def __init__(self, port):
        tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
        tcp_server_socket.bind(("", port))
        tcp_server_socket.listen(128)
        self.tcp_server_socket = tcp_server_socket

    @staticmethod
    def handle_client_request(new_socket):
        recv_client_data = new_socket.recv(4096)
        if len(recv_client_data) == 0:
            new_socket.close()
            return
        recv_client_content = recv_client_data.decode("utf-8")
        request_list = recv_client_content.split(" ", maxsplit=2)
        request_path = request_list[1]
        if request_path == "/":
            request_path = "/index.html"
        try:
            with open("static" + request_path, "rb") as file:
                file_data = file.read()
        except Exception as e:
            response_line = "HTTP/1.1 404 Not Found\r\n"
            response_header = "Server: PWS1.0\r\n"
            with open("static/error.html", "rb") as file:
                file_data = file.read()
            response_body = file_data
            response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
            new_socket.send(response_data)
        else:
            response_line = "HTTP/1.1 200 OK\r\n"
            response_header = "Server: PWS1.0\r\n"
            response_body = file_data
            response_data = (response_line + response_header + "\r\n").encode("utf-8") + response_body
            new_socket.send(response_data)
        finally:
            new_socket.close()

    def start(self):
        while True:
            new_socket, ip_port = self.tcp_server_socket.accept()
            # 每个连接建一个子线程处理，防主线程阻塞
            sub_thread = threading.Thread(target=self.handle_client_request, args=(new_socket,))
            sub_thread.setDaemon(True)   # 守护主线程
            sub_thread.start()

def main():
    if len(sys.argv) != 2:
        print("执行命令如下: python3 xxx.py 9000")
        return
    if not sys.argv[1].isdigit():
        print("执行命令如下: python3 xxx.py 9000")
        return
    port = int(sys.argv[1])             # 命令行动态绑定端口
    web_server = HttpWebServer(port)
    web_server.start()

if __name__ == '__main__':
    main()
```

---

## 三、网络编程（socket / TCP）

### 3.1 socket 概述
- socket 是应用程序与 TCP/IP 协议通信的中间抽象层，本质是"文件"；两端都要建 socket 对象。

### 3.2 TCP 客户端
```python
import socket
# 1. 创建socket
tcp_client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 2. 建立连接
tcp_client_socket.connect(("192.168.1.1", 8080))
# 3. 发送数据
tcp_client_socket.send("你好".encode("utf-8"))
# 4. 接收数据（4096 = 最大接收字节数）
recv_data = tcp_client_socket.recv(4096)
print(recv_data.decode("utf-8"))
# 5. 关闭连接
tcp_client_socket.close()
```

### 3.3 TCP 服务端
```python
import socket
# 1. 创建socket
tcp_server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
# 2. 端口号复用（程序退出端口立即释放）
tcp_server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
# 3. 绑定端口
tcp_server_socket.bind(("", 8080))
# 4. 监听
tcp_server_socket.listen(128)
# 5. 等待连接（返回新套接字 + 客户端地址）
new_socket, ip_port = tcp_server_socket.accept()
# 6. 收发数据
recv_data = new_socket.recv(4096)
new_socket.send("收到".encode("utf-8"))
# 7. 关闭连接
new_socket.close()
tcp_server_socket.close()
```

---

## 四、JavaScript（网页交互语言）

### 4.1 概述
- JS 运行在浏览器端、由浏览器解释执行；负责网页与用户交互。三大件：HTML 结构、CSS 样式、JS 行为。
- 使用方式：行内式（事件）、内嵌式（`<script>`）、外链式（`<script src="js/index.js">`）。

### 4.2 变量与数据类型
```javascript
var iNum = 123;               // 弱类型：类型由值决定
var sTr = 'asd';
var iNum = 45, sCount='68';   // 逗号同时定义多个

// 六种数据类型
var iOne = 10.1;              // number
var sStr = '1234';            // string
var bIsTrue = false;          // boolean
var unData;                   // undefined（声明未初始化）
var nullData = null;          // null（空对象）
var oObj = {name:"老王", age:88};   // object

var type = typeof(oObj);      // 获取类型
alert(oObj.name);             // 老王
```

### 4.3 变量作用域
```javascript
var a = 12;                    // 全局变量：函数外定义，各函数可用
function myalert(){
    var b = 23;                // 局部变量：函数内定义，仅函数内可用
    alert(b);
    a++;                       // 可修改全局变量
}
```

### 4.4 函数
```javascript
// 定义 + 调用
function fnAlert(){ alert('hello!'); }
fnAlert();

// 有参数有返回值
function fnAdd(iNum01, iNum02){
    var iRs = iNum01 + iNum02;
    return iRs;                // return 返回 + 函数结束
    alert('here!');            // 不执行
}
var iCount = fnAdd(3, 4);      // 7
```

### 4.5 条件语句
```javascript
// == 会隐式转换类型；=== 值和类型都相等
var sNum01 = '12';
if(sNum01 == 12){ alert('相等！'); }      // == 隐式转换，为 true
if(sNum01 === 12){ alert('相等！'); }     // === 类型不同，为 false

// 多条件判断
var sFruit = "苹果";
if (sFruit == "苹果") { alert("您选择的是苹果"); }
else if (sFruit == "鸭梨") { alert("您选择的是鸭梨"); }
else { alert("不存在!"); }

// 逻辑 && || !
var x = 6, y = 3;
if(x < 10 && y > 1){ alert('都大于'); }
if(x > 5 || y > 7){ alert('至少一个大于'); }
if(!(x == y)){ alert('不等于'); }
```

### 4.6 循环语句
```javascript
var array = [1, 4, 5];
// for
for(var index = 0; index < array.length; index++){
    alert(array[index]);
}
// while
var index = 0;
while (index < array.length) { alert(array[index]); index++; }
// do-while：条件不成立也会执行一次
var index = 0;
do { alert(array[index]); index++; } while (index < array.length);
```

### 4.7 数组
```javascript
var aList = new Array(1,2,3);          // 实例化方式
var aList2 = [1,2,3,'asd'];            // 字面量方式（推荐）
var aList3 = [[1,2,3],['a','b','c']];  // 多维数组

alert(aList2.length);   // 4 长度
alert(aList2[0]);       // 1 取值
aList2.push(5);         // 末尾添加
aList2.pop();           // 末尾删除

// splice(start, num, ele...) 按下标增删
var colors = ["red", "green", "blue"];
colors.splice(0, 1);                  // 删除第一项 → green,blue
colors.splice(1, 0, "yellow", "orange");  // 索引1插入两项
colors.splice(1, 1, "red", "purple");     // 删1项插2项
```

### 4.8 获取与操作标签元素
```javascript
// 需等页面加载完成（放页面最底或 window.onload）
window.onload = function(){
    var oInput = document.getElementById('input1');  // 获取标签
    var oA = document.getElementById('link1');
    // 读取属性
    var sValue = oInput.value;
    // 设置属性：class 写成 className；带横杠改驼峰
    oA.className = 'sty02';
    oA.style.color = 'red';
    oA.style.fontSize = sValue;
    // innerHTML 读取/设置包裹内容
    var oDiv = document.getElementById('div1');
    oDiv.innerHTML = '<a href="http://www.itcast.cn">传智播客</a>';
};
```

### 4.9 字符串拼接
```javascript
var iNum1 = 10, fNum2 = 11.1, sStr = 'abc';
alert(iNum1 + fNum2);   // 21.1（数字相加）
alert(fNum2 + sStr);    // 11.1abc（数字隐式转字符串拼接）
```

### 4.10 定时器
```javascript
function hello(){ alert('hello'); }
// 只执行一次
var t1 = setTimeout(hello, 500);
clearTimeout(t1);                       // 清除
// 反复执行
var t2 = setInterval(hello, 1000);
clearInterval(t2);                      // 清除
```

### 4.11 JS 对象
```javascript
// 方式一：Object 实例化
var person = new Object();
person.name = 'tom';
person.age = '25';
person.sayName = function(){ alert(this.name); };
// 方式二：对象字面量（推荐）
var person2 = {
    name:'Rose',
    age: 18,
    sayName:function(){ alert('My name is ' + this.name); }
};
alert(person2.age);
person2.sayName();
```

---

## 五、jQuery（JS 函数库）

### 5.1 概述与引入
- jQuery 对 JS 的封装，免费开源，简化编程，兼容主流浏览器。
```html
<script src="js/jquery-1.12.4.min.js"></script>
```

### 5.2 入口函数
```javascript
// 两种写法，比 window.onload 更快
$(document).ready(function(){ ... });   // 完整
$(function(){ ... });                   // 简化（推荐）
```

### 5.3 选择器
```javascript
$('#myId')                 // id选择器
$('.myClass')              // 类选择器
$('li')                    // 标签选择器
$('#ul1 li span')          // 层级选择器
$('input[name=first]')     // 属性选择器
var ok = $("div").length > 0;   // length 判断是否选择成功
```

### 5.4 操作元素
```javascript
$(function(){
    var $div = $("#div1");
    $div.html();                    // 获取 html 内容
    $div.html("<span>你好</span>"); // 设置（清除原内容）
    $div.append("<span>追加</span>"); // 追加

    var $a = $("#link01");
    $a.prop("id");                              // 获取属性
    $a.prop({"href":"http://www.baidu.com","class":"a01"});  // 设置属性

    var $input = $('#input01');
    $input.val();           // 获取 value
    $input.val("222222");   // 设置 value
    $div.css({'background':'gold'});   // 设置样式
});
```

### 5.5 选择集过滤与转移
```javascript
$(function(){
    var $div = $('#div01');
    $("div").has("#mytext");     // 过滤：包含指定选择器的标签
    $("div").eq(1);              // 过滤：指定索引标签

    $div.prev();        $div.prevAll();      // 上一个 / 上面所有同级
    $div.next();        $div.nextAll();      // 下一个 / 下面所有同级
    $div.parent();                            // 父元素
    $div.children();                          // 所有子元素
    $div.siblings();                          // 其它同级元素
    $div.find('.sp02');                       // 指定选择器元素
});
```

### 5.6 事件与事件代理
```javascript
$(function(){
    $('li').click(function(){
        $(this).css({'background':'gold'});   // $(this) 是 jquery 对象
        alert($(this).index());               // index() 取索引
    });
    $('#text1').focus(function(){ $(this).css({'background':'red'}); });
    $('#text1').blur(function(){ $(this).css({'background':'white'}); });
    $('#div1').mouseover(function(){ $(this).css({'background':'gold'}); });
    $('#div1').mouseout(function(){ $(this).css({'background':'white'}); });
});

// 事件代理：父元素代理子元素，减少绑定、新子元素也有操作
$(function(){
    $list = $('#list');
    $list.delegate('li', 'click', function(){
        $(this).css({background:'red'});   // $(this) 当前点击的子元素
    });
});
```

### 5.7 ajax（前后台通信）
```javascript
// 完整方式
$.ajax({
    url:'http://t.weather.sojson.com/api/weather/city/101010100',
    type:'GET',
    dataType:'JSON',
    success:function (response) { console.log(response); },
    error:function () { alert("请求失败,请稍后再试!"); },
    async:true
});

// 简写方式
$.get("http://t.weather.sojson.com/api/weather/city/101010100",
    function(dat,status){ alert(dat); }).error(function(){ alert("网络异常"); });

$.post("test.php", {"func": "getNameAndTime"},
    function(data){ alert(data.name); }, "json").error(function(){ alert("网络异常"); });
```

### 5.8 json 数据格式
```javascript
// 两种格式
var obj = {"name":"tom","age":18};           // 对象格式（key和字符串必须双引号）
var arr = ["tom",18,"programmer"];           // 数组格式

// json 字符串转 JS 对象
var sJson = '{"name":"tom","age":18}';
var oPerson = JSON.parse(sJson);
alert(oPerson.name);   // tom
```

---

## 六、Linux 基础命令

### 6.1 终端命令格式
```
command [-options] [parameter]      # 命令名 选项(可选) 参数(可选)
ls -l /home          # 短选项 -l；长选项 --help
```
- 选项调整功能（`-r`）；参数是操作对象（文件名/目录名）。cd 切换目录必须存在。

### 6.2 目录与文件操作命令
```bash
ls              # 查看当前目录信息
tree            # 树状查看多层目录
pwd             # 查看当前目录路径
clear           # 清除终端

cd ~            # 主目录
cd ..           # 上一级
cd .            # 当前
cd -            # 上一次目录
cd /home/python/Desktop   # 绝对路径

touch 1.txt     # 创建文件
mkdir mydir     # 创建目录
rm 1.txt        # 删除文件
rm -r mydir     # 递归删除目录
rmdir emptydir  # 删除空目录
cp a.txt b.txt  # 拷贝文件
cp -r dir1 dir2 # 递归拷贝目录
mv a.txt b.txt  # 移动 / 重命名
```

### 6.3 命令选项汇总
```bash
ls -l -h -a              # 列表/大小单位/隐藏文件
cp -i                    # 交互提示（防覆盖）
cp -r                    # 递归拷贝目录
cp -a                    # 拷贝且保留权限
mkdir -p a/b/c           # 创建嵌套目录
rm -rf mydir             # 强制递归删除
find /home -name "*.txt" # 查找文件（通配符需引号）
find /home -name "a?"    # ? 匹配任意一个字符
grep -i -n -v 文本 文件   # 忽略大小写/行号/不包含的行
```

### 6.4 路径与查看帮助
```bash
# 绝对路径从 / 算起；相对路径从当前算起，遵循最短路径原则
cd /home/python/Desktop
cd ../Desktop
# 查看帮助
命令 --help
man 命令        # 空格下屏 / 回车下行 / b 上屏 / q 退出
# Tab 自动补全，两次 Tab 查看候选
```

---

## 七、Linux 高级命令

### 7.1 查看文件内容 / 重定向 / 管道
```bash
cat 1.txt              # 查看小文件
more 1.txt             # 分屏查看大文件
gedit 1.txt            # 查看编辑文件

tree > tree.txt        # 重定向：覆盖写入（w 模式）
echo "追加" >> 1.txt    # 重定向：追加写入（a 模式）
ls / | grep 'lib'      # 管道：一个命令输出作另一命令输入
```

### 7.2 文件权限（chmod）
```bash
# 字母法：chmod [角色][操作符][权限] 文件
chmod u+x 1.sh          # 给所有者加执行权限
chmod g-w 1.sh          # 给用户组撤销写权限
chmod a=rwx 1.sh        # 所有用户设读写执行

# 数字法：r=4 w=2 x=1 -=0
chmod 755 1.sh          # 所有者7(4+2+1)，组/其他5(4+1)
chmod 644 1.sh          # 所有者6(4+2)，组/其他4
```

### 7.3 压缩解压缩
```bash
# .gz（推荐，占空间少）
tar -zcvf a.tar.gz dir1     # 压缩
tar -zxvf a.tar.gz -C /目标  # 解压到指定目录
# .bz2
tar -jcvf a.tar.bz2 dir1
tar -jxvf a.tar.bz2 -C /目标
# .zip（通用但占空间多）
zip -r a.zip dir1
unzip a.zip -d /目标
```

### 7.4 链接命令
```bash
# 软链接（类似快捷方式；源文件需绝对路径；删源文件则失效）
ln -s /home/python/1.txt soft_link
# 硬链接（别名；删源文件仍可访问；不能对目录）
ln /home/python/1.txt hard_link
```

### 7.5 vim 编辑器
```
vim 1.txt
# 命令模式（默认）：i 进入编辑模式，Esc 回命令模式，: 进末行模式
yy           # 复制光标所在行
p            # 粘贴
dd           # 删除/剪切当前行
u            # 撤销
ctrl+r       # 反撤销
/搜索内容     # 搜索
:%s/旧/新/g   # 全局替换
G / gg       # 最后一行 / 第一行
:w           # 保存
:wq / :x     # 保存退出
:q!          # 强制退出
```

### 7.6 用户 / 权限 / 远程
```bash
sudo useradd -m laowang        # 创建用户（-m 建主目录，默认建同名组）
sudo passwd laowang            # 设置密码
su - laowang                   # 切换用户
id laowang                     # 查看用户（或看 /etc/passwd，组看 /etc/group）
sudo usermod -G sudo laowang   # 加 sudo 附加组
sudo userdel -r laowang        # 删除用户（默认删同名组）
sudo groupadd dev              # 创建用户组
sudo groupdel dev              # 删除用户组

sudo -s                        # 切换到 root（谨慎）
whoami                         # 查看当前用户
exit                           # 退出登录
who                            # 查看所有登录用户
which python3                  # 查看命令位置
shutdown -h now                # 关机
reboot                         # 重启

sudo apt-get install openssh-server   # 服务端
ssh 用户名@ip地址                    # 远程登录
scp 本地文件 用户@ip:远程路径         # 远程拷贝文件
scp -r 本地目录 用户@ip:远程路径      # 远程拷贝目录
scp 用户@ip:远程文件 本地路径         # 反向拷贝
```

### 7.7 软件安装与卸载
```bash
# 离线 deb 包
sudo dpkg -i 安装包.deb     # 安装
sudo dpkg -r 安装包名       # 卸载
# 在线 apt（改国内镜像源 + update 提速）
sudo apt-get install 安装包
sudo apt-get remove 安装包
sudo apt-get update         # 改完镜像源后执行
```

---

## 八、MySQL 基础

### 8.1 数据库 / SQL 概述
- 数据库 = 存储数据的仓库（本质文件系统）；DBMS = 管理数据库的软件（MySQL 是关系型数据库，二维表存储）。
- SQL：DDL（CREATE）、DML（INSERT/DELETE/UPDATE）、DQL（SELECT）、DCL（GRANT/REVOKE）。

### 8.2 MySQL 使用与库表操作
```sql
-- 连接
mysql -uroot -p密码
-- 数据库
CREATE DATABASE stock_db charset=utf8;
SHOW DATABASES;
USE stock_db;
SELECT database();
DROP DATABASE stock_db;
-- 表
CREATE TABLE student(
    id int primary key auto_increment,
    name varchar(20) not null,
    age int
);
SHOW TABLES;
DESC student;             -- 查看表结构
DROP TABLE student;
source 文件.sql;          -- 导入数据
```

### 8.3 数据类型与约束
```sql
-- 常用类型：int / float(m,d) / varchar(n) / date
-- 约束：primary key 主键、not null 非空、unique 唯一、default 默认、auto_increment 自增
CREATE TABLE info(
    id int primary key auto_increment,
    code varchar(20) not null unique,
    short varchar(20),
    chg float,
    price float default 0
);
```

### 8.4 增删改（DML）
```sql
INSERT INTO student(name, age) VALUES ('张三', 18);
UPDATE student SET age = 20 WHERE name = '张三';
DELETE FROM student WHERE id = 3;
```

### 8.5 基本查询（DQL）
```sql
SELECT * FROM student;                        -- 所有列
SELECT DISTINCT age FROM student;              -- 去重
SELECT name AS 姓名, age AS 年龄 FROM student;  -- 别名
SELECT * FROM student WHERE age > 18 AND gender = '男';
SELECT * FROM student WHERE age BETWEEN 18 AND 60;
SELECT * FROM student WHERE name LIKE '张%';   -- 模糊查询
```

---

## 九、MySQL 查询（进阶）

### 9.1 排序
```sql
SELECT * FROM student ORDER BY age ASC;    -- 升序
SELECT * FROM student ORDER BY age DESC;   -- 降序
SELECT * FROM student ORDER BY age DESC, id ASC;   -- 多字段
```

### 9.2 聚合函数
```sql
SELECT COUNT(*) FROM student;     -- 总行数
SELECT MAX(age) FROM student;     -- 最大
SELECT MIN(age) FROM student;     -- 最小
SELECT SUM(age) FROM student;     -- 求和
SELECT AVG(age) FROM student;     -- 平均
```

### 9.3 分组查询
```sql
-- 按性别分组统计人数
SELECT gender, COUNT(*) FROM student GROUP BY gender;
-- having 对分组结果过滤（where 不能与聚合函数连用）
SELECT gender, AVG(age) FROM student GROUP BY gender HAVING AVG(age) > 18;
```

### 9.4 连接查询（多表）
```sql
-- 内连接：返回两表匹配的行（交集）
SELECT * FROM info INNER JOIN focus ON info.id = focus.info_id;
SELECT i.code, i.short, f.note_info
FROM info AS i INNER JOIN focus AS f ON i.id = f.info_id;
```

### 9.5 三范式
- 1NF：每列不可再分（原子性）；2NF：非主键列完全依赖主键；3NF：非主键列间无传递依赖。

---

## 十、MySQL 高级

### 10.1 事务
```sql
START TRANSACTION;        -- 开启事务（或 BEGIN）
INSERT INTO student(name, age) VALUES ('李四', 22);
UPDATE student SET age = 23 WHERE name = '李四';
COMMIT;                   -- 提交（永久生效）
-- 出错时回滚
ROLLBACK;
```
- ACID：A 原子性（不可分割）、C 一致性、I 隔离性、D 持久性。

### 10.2 索引
```sql
CREATE INDEX idx_code ON info(code);   -- 创建索引，提高查询速度
SHOW INDEX FROM info;                  -- 查看索引
DROP INDEX idx_code ON info;           -- 删除索引
```
- 索引=书的目录，加快查询；占用磁盘；适合频繁查询列，不适合频繁增删改的表。

### 10.3 PyMySQL（Python 操作 MySQL）
```python
import pymysql

def query_data():
    # 1. 建立连接
    conn = pymysql.connect(host="localhost", port=3306, user="root",
                           password="mysql", database="stock_db", charset="utf8")
    # 2. 获取游标
    cursor = conn.cursor()
    # 3. 执行 SQL
    sql = "select * from info;"
    cursor.execute(sql)
    result = cursor.fetchall()   # 获取结果集
    print(result)
    # 4. 关闭游标与连接
    cursor.close()
    conn.close()

def insert_data():
    conn = pymysql.connect(host="localhost", port=3306, user="root",
                           password="mysql", database="stock_db", charset="utf8")
    cursor = conn.cursor()
    cursor.execute("insert into student(name, age) values('王五', 25)")
    conn.commit()      # 增删改必须 commit 提交（事务）
    cursor.close()
    conn.close()

# 数据转 json
import json
data_list = [{"code": "000007", "short": "股票A"}, {"code": "000008", "short": "股票B"}]
json_str = json.dumps(data_list, ensure_ascii=False)   # 中文正常显示
print(json_str)
```

---

## 十一、多任务（进程 / 线程）

### 11.1 多任务概述
- 多任务：系统同时运行多个任务；多核 CPU 才真正并行，单核是快速切换（并发）。
- 进程/线程/协程对比：进程=资源分配最小单位（占用多、稳定）；线程=CPU 调度最小单位（共享进程资源、快）；协程更轻量。

### 11.2 多进程（multiprocessing）
```python
import multiprocessing
import time

def dance():
    for i in range(3):
        time.sleep(1)
        print("跳舞")

def sing():
    for i in range(3):
        time.sleep(1)
        print("唱歌")

if __name__ == '__main__':
    p1 = multiprocessing.Process(target=dance)
    p2 = multiprocessing.Process(target=sing)
    p1.start()
    p2.start()
```
- 进程资源独立；进程间通信用队列：
```python
import multiprocessing
def producer(q):
    q.put("数据")
def consumer(q):
    print(q.get())
if __name__ == '__main__':
    q = multiprocessing.Queue()
    multiprocessing.Process(target=producer, args=(q,)).start()
    multiprocessing.Process(target=consumer, args=(q,)).start()
```

### 11.3 多线程（threading）
```python
import threading
import time

def task1():
    for i in range(3):
        time.sleep(1)
        print("任务1")

if __name__ == '__main__':
    t1 = threading.Thread(target=task1)
    t1.start()
```
- 线程共享全局变量（进程不共享），可共享同一份数据。

### 11.4 互斥锁（线程同步）
```python
import threading

lock = threading.Lock()
g_num = 0

def add():
    global g_num
    for _ in range(1000000):
        lock.acquire()      # 上锁：保证同时只有一个线程访问
        g_num += 1
        lock.release()      # 解锁

if __name__ == '__main__':
    t1 = threading.Thread(target=add)
    t2 = threading.Thread(target=add)
    t1.start(); t2.start()
    t1.join(); t2.join()
    print(g_num)            # 结果为 2000000（不加锁可能错乱）

# 也可用 with 上下文管理
def add():
    global g_num
    for _ in range(1000000):
        with lock:
            g_num += 1
```

---

## 十二、正则表达式

### 12.1 概述
- 正则 = 记录文本规则的代码，检索/替换符合模式的文本；爬虫、表单验证、日志分析常用。

### 12.2 匹配单个字符
```python
import re
# . 匹配任意1个字符（除换行）
re.match(r"a.c", "abc")      # 匹配成功
# [] 匹配列举字符
re.match(r"[aeiou]", "i")
# \d 数字、\D 非数字、\s 空白、\S 非空白、\w 单词字符、\W 非单词字符
re.match(r"\d\d", "35")      # 匹配两个数字
```

### 12.3 匹配多个字符
```python
# * 0次或无限次   + 1次或无限次   ? 0次或1次
re.match(r"ab*c", "ac")        # 匹配成功（b出现0次）
re.match(r"ab+c", "abc")       # 匹配成功（b出现1次）
re.match(r"ab?c", "ac")        # 匹配成功
# {m} {m,n}
re.match(r"\d{8}", "12345678")      # 恰好8位数字
re.match(r"\d{2,4}", "123")         # 2到4位数字
```

### 12.4 开头和结尾
```python
# ^ 开头 $ 结尾
re.match(r"^hello", "hello world")     # 匹配成功
re.match(r"^\d{8}$", "12345678")       # 8位纯数字
```

### 12.5 re 模块常用函数
```python
import re
# match：从起始位置匹配，成功返回对象，失败 None
result = re.match(r"^hello", "hello world")
if result:
    print(result.group())     # hello

# search：任意位置匹配
re.search(r"\d+", "abc123def")    # 匹配到 123

# findall：找出所有匹配，返回列表
re.findall(r"\d+", "a1 b22 c333")  # ['1', '22', '333']
```

### 12.6 生成器（补充）
```python
# 生成器表达式
gen = (x * 2 for x in range(5))
print(next(gen))     # 0
print(next(gen))     # 2

# yield 生成器
def mygen():
    yield 1
    yield 2
    yield 3
for v in mygen():
    print(v)         # 1 2 3
```

---

## 十三、闭包与装饰器

### 13.1 闭包（Closure）
```python
def outer():
    x = 10
    def inner():
        return x        # 内部函数使用外部变量
    return inner        # 返回内部函数

func = outer()
print(func())           # 10（外部变量 x 仍存活）

# 修改外部变量需 nonlocal
def counter():
    count = 0
    def add():
        nonlocal count
        count += 1
        return count
    return add

c = counter()
print(c())   # 1
print(c())   # 2
```
- 闭包构成条件：①函数嵌套 ②内部函数引用外部变量 ③外部函数返回内部函数。

### 13.2 装饰器（Decorator）
```python
def wrapper(fn):
    def inner():
        print("日志：调用前")   # 增强功能
        fn()                    # 执行原函数
        print("日志：调用后")
    return inner

@wrapper          # 等价于 f = wrapper(f)
def f():
    print("原函数")

f()   # 调用时自动带上增强功能
```

### 13.3 通用装饰器（不定长参数 + 返回值）
```python
def wrapper(fn):
    def inner(*args, **kwargs):
        print("额外功能")
        return fn(*args, **kwargs)   # 透传参数，返回原结果
    return inner

@wrapper
def add(a, b):
    return a + b

@wrapper
def greet(name="同学"):
    print(f"你好，{name}")

print(add(1, 2))   # 3
greet("小王")
```
- 可装饰任意参数、任意返回值的函数；在 Web 框架中广泛用于路由、日志、权限。

### 13.4 带参数的装饰器
```python
def route(path):
    def decorator(func):
        def inner(*args, **kwargs):
            return func(*args, **kwargs)
        return inner
    return decorator

@route("/index.html")    # 装饰器接收参数
def index():
    return "首页数据"
```

---

## 十四、miniweb（Web 框架实战）

### 14.1 Web 框架与服务器关系
- 静态资源：不需要常变化的资源（png/jpg/css/js），服务器提前准备。
- 动态资源：会经常变化，`.html` 请求视为动态资源请求，交 Web 框架处理。
- 流程：浏览器 → web 服务器 →（动态请求）→ web 框架 → 结果 → web 服务器 → 响应 → 浏览器。
- **WSGI 协议**：web 服务器与 web 框架协同工作的规则。

### 14.2 web 服务器 + 框架（动态资源判断）
```python
# web.py 核心：按后缀判断动静
if request_path.endswith(".html"):
    """动态资源请求，交给框架处理"""
    env = {"request_path": request_path}
    status, headers, response_body = framework.handle_request(env)
    response_line = "HTTP/1.1 %s\r\n" % status
    response_header = ""
    for header in headers:
        response_header += "%s: %s\r\n" % header
    response_data = (response_line + response_header + "\r\n" + response_body).encode("utf-8")
    new_socket.send(response_data)
    new_socket.close()
else:
    """静态资源请求"""
    # 打开 static + request_path，返回文件
```

### 14.3 路由列表
```python
# 路由 = 请求 URL 到处理函数的映射
route_list = [
    ("/index.html", index),
    ("/center.html", center)
]

def handle_request(env):
    request_path = env["request_path"]
    for path, func in route_list:        # 遍历路由列表
        if request_path == path:
            result = func()
            return result
    else:
        result = not_found()             # 找不到返回 404
        return result
```

### 14.4 装饰器方式添加路由（自动注册）
```python
route_list = []

def route(path):
    def decorator(func):
        route_list.append((path, func))   # 装饰时自动加入路由
        def inner():
            return func()
        return inner
    return decorator

@route("/index.html")
def index():
    status = "200 OK"
    response_header = [("Server", "PWS2.0")]
    with open("template/index.html", "r") as file:
        file_data = file.read()
    data = time.ctime()
    result = file_data.replace("{%content%}", data)   # 模板替换
    return status, response_header, result

@route("/center.html")
def center():
    ...
```

### 14.5 模板替换 + 数据库查询
```python
@route("/index.html")
def index():
    status = "200 OK"
    response_header = [("Server", "PWS2.0")]
    with open("template/index.html", "r") as file:
        file_data = file.read()

    # 从数据库查询
    conn = pymysql.connect(host="localhost", port=3306, user="root",
                           password="mysql", database="stock_db", charset="utf8")
    cursor = conn.cursor()
    cursor.execute("select * from info;")
    result = cursor.fetchall()
    cursor.close(); conn.close()

    # 把查询数据拼成表格行，替换模板变量
    data = ""
    for row in result:
        data += '''<tr><td>%s</td><td>%s</td>
                   <td><input type="button" value="添加"></td></tr>''' % row
    result_html = file_data.replace("{%content%}", data)
    return status, response_header, result_html
```

### 14.6 数据接口（ajax 配合）
```python
import json
@route("/center_data.html")
def center_data():
    status = "200 OK"
    response_header = [("Server", "PWS2.0"), ("Content-Type", "text/html;charset=utf-8")]
    conn = pymysql.connect(host="localhost", port=3306, user="root",
                           password="mysql", database="stock_db", charset="utf8")
    cursor = conn.cursor()
    cursor.execute('''select i.code, i.short, i.chg, i.turnover, i.price, i.highs, f.note_info
                      from info as i inner join focus as f on i.id = f.info_id;''')
    result = cursor.fetchall()
    cursor.close(); conn.close()

    center_data_list = []
    for row in result:
        center_dict = {
            "code": row[0], "short": row[1], "chg": row[2],
            "turnover": row[3], "price": str(row[4]), "highs": str(row[5]),
            "note_info": row[6]
        }
        center_data_list.append(center_dict)
    json_str = json.dumps(center_data_list, ensure_ascii=False)
    return status, response_header, json_str
```
```javascript
// 前端 ajax 请求并渲染
$.get("center_data.html", function (data) {
    var $table = $(".table");
    for (var i = 0; i < data.length; i++) {
        var obj = data[i];
        var row_html = '<tr><td>' + obj.code + '</td><td>' + obj.short +
                       '</td><td>' + obj.chg + '</td></tr>';
        $table.append(row_html);
    }
}, "json");
```

### 14.7 logging 日志
```python
import logging
# 配置一次，整个程序生效（在入口模块）
logging.basicConfig(level=logging.DEBUG,
                    format='%(asctime)s - %(filename)s[line:%(lineno)d] - %(levelname)s: %(message)s',
                    filename="log.txt",
                    filemode="w")

# 5 个等级（低→高）：DEBUG < INFO < WARNING < ERROR < CRITICAL；默认 WARNING
logging.debug('调试信息')
logging.info('正常运行')
logging.warning('未按预期（如密码错误）')
logging.error('程序出错')
logging.critical('严重错误')

# 应用：动态/静态请求打点
if request_path.endswith(".html"):
    logging.info("动态资源请求:" + request_path)
else:
    logging.info("静态资源请求:" + request_path)

# 参数校验失败
if len(sys.argv) != 2:
    logging.warning("用户在命令行启动程序参数个数不正确!")
# 路由未匹配
logging.error("没有设置相应的路由:" + request_path)
```

---

## 十五、学习路线建议

1. **课程主线**：HTML/CSS（前端结构样式）→ JavaScript（交互）→ jQuery（简化交互+ajax）→ HTTP（通信协议）→ socket/TCP（网络基础）→ MySQL（数据存储）→ 多任务（并发）→ 正则（文本处理）→ 闭包/装饰器（Python 进阶）→ miniweb（前后端整合实战）。
2. **重点难点**：
   - CSS 盒子模型真实尺寸计算；
   - HTTP 请求/响应报文结构（GET vs POST）；
   - JS 的 `==` 与 `===` 区别、作用域；
   - Linux 文件权限字母法/数字法；
   - MySQL 内连接、分组 having、三范式；
   - 多线程共享变量的互斥锁；
   - 装饰器 `*args/**kwargs` 通用写法；
   - 路由 + 模板替换 + ajax 数据接口的开发链路。
3. **每章配综合案例**：
   - CSS：静态页面布局；
   - JS：表单交互、定时器；
   - HTTP：静态 Web 服务器（面向对象 + 多任务 + 命令行端口）；
   - MySQL：股票数据查询；
   - miniweb：股票信息展示 + 个人中心 + 添加/删除 + logging 日志。
4. **动手贯通项目**：改造 miniweb 项目——新增一个 `@route("/xxx.html")` 处理函数、一个数据接口、一段日志输出，即可把网络、数据库、前端、闭包装饰器全部串起来。
5. **衔接下一阶段**：掌握 P1 后可进阶到 Flask/Django 框架、ORM（SQLAlchemy）、前端框架（Vue/React）、爬虫、数据分析等。


