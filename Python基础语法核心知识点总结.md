# Python基础语法核心知识点总结

> 学习版本：Python 3.7（课程基准）；建议新同学上手 Python 3.10+。

---

## 一、Python 简介与环境搭建

### 1.1 Python 是什么
- **定义**：Python 是一种解释型、面向对象、动态数据类型的高级程序设计语言。
- **优点**：简单易学、免费开源、应用领域广泛（Web、数据分析、人工智能、自动化运维等）。

### 1.2 知名 Python 框架
| 框架 | 出品方 | 领域 |
| --- | --- | --- |
| TensorFlow | Google | 机器学习 |
| Scikit-learn | 开源社区 | 机器学习 |
| Paddle | 百度 | 深度学习 |
| PyTorch | Meta | 深度学习（AI 应用开发主流） |

### 1.3 版本演进
- **Python 2.X**：已停止维护（官方 2020 年退役），不建议新项目使用。
- **Python 3.X**：主流版本，课程以 3.7 为基准，当前推荐 3.10 ~ 3.13。

### 1.4 第一个程序
```python
# 第一个 Python 程序
print("hello world")        # 字符串输出
print(1 + 2)                # 输出计算结果 3
print("你好", "Python", sep="-", end="!\n")   # sep 指定分隔符，end 指定结尾
```

**PyCharm 核心操作回顾**
- 新建项目：`Create New Project` → 选根目录与解释器 → `Create`。
- 新建文件：项目区右键 → `New` → `Python File`（上传服务器时**文件名勿用中文**）。
- 运行：文件内右键 → `Run`。
- 设置：`File → Settings`（主题 `Appearance & Behavior`，字体 `Editor → Font`，解释器 `Project → Project Interpreter`）。

### 1.5 注释
```python
# 单行注释：解释器不执行
print('hello')              # 行尾注释（代码后空两格再加注释）

""" 多行注释
这里写多行说明文字
"""
'''
也可以用单引号三引号
写多行注释
'''
```

---

## 二、变量与数据类型

### 2.1 变量的定义与使用
```python
# 单个变量赋值
name = 'TOM'
age = 18
print(name)     # TOM
print(age)      # 18

# 多个变量同时赋值
num1, num2, num3 = 10, 20, 30
print(num1, num2, num3)     # 10 20 30

# 多个变量赋同一个值
a = b = 100
print(a, b)                 # 100 100
```

### 2.2 标识符命名
```python
# 合法命名示例
my_name = 'Tom'
_school = '清华大学'
number1 = 10

# 不合法（运行会报错）
# 1number = 10    # 不能以数字开头
# class = 10      # 不能使用关键字
# my-name = 10    # 不能包含除下划线外的符号
```

### 2.3 八大基本数据类型
```python
# 用 type() 检测数据类型
a = 1                       # int
b = 1.1                     # float
c = True                    # bool
d = '12345'                 # str
e = [10, 20, 30]            # list
f = (10, 20, 30)            # tuple
h = {10, 20, 30}            # set
g = {'name': 'TOM', 'age': 20}   # dict

print(type(a), type(b), type(c), type(d))
print(type(e), type(f), type(h), type(g))
# <class 'int'> <class 'float'> <class 'bool'> <class 'str'>
# <class 'list'> <class 'tuple'> <class 'set'> <class 'dict'>

# 布尔值也能参与运算：True 视为 1，False 视为 0
print(True + True)          # 2
print(False * 10)           # 0
```

---

## 三、输入与输出

### 3.1 print() 输出
```python
print('hello world')        # 输出字符串
age = 18
print(age)                  # 输出变量值 18

# 一次输出多个值
print(1, 2, 3)              # 1 2 3（空格分隔）
print('A', 'B', sep=', ')   # A, B（自定义分隔符）
```

### 3.2 格式化输出

**① 格式化符号 `%`**
```python
name = 'TOM'
age = 18
weight = 75.5
student_id = 1

print('我的名字是%s' % name)            # 我的名字是TOM
print('我的学号是%04d' % student_id)     # 我的学号是0001
print('我的体重是%.2f公斤' % weight)     # 我的体重是75.50公斤
print('我的名字是%s，今年%d岁了' % (name, age))   # 多参数
print('我的名字是%s，明年%d岁了' % (name, age + 1))

# 补充：格式说明与对齐
print('%-8s|' % 'left')      # 左对齐
print('%8s|' % 'right')      # 右对齐
```

**② f-字符串（推荐，Python 3.6+）**
```python
name, age = 'TOM', 18
print(f'我的名字是{name}, 明年{age + 1}岁了')   # 表达式直接算
print(f'1 年后我 {age + 1} 岁，10 年后 {age + 10} 岁')

# 三引号 + f-string 组合多行文本
msg = f"""姓名：{name}
年龄：{age}"""
print(msg)
```

### 3.3 转义字符与结束符
```python
print('第一行\n第二行')       # \n 换行
print('a\tb')               # \t 制表符（对齐）
print('他说：\'你好\'')       # \' 转义单引号

# print 默认结束符是 \n，可修改
print('hello', end='')      # 不换行
print(' world')             # 结果: hello world
print('A', end='---')
print('B')                  # A---B
```

### 3.4 输入 input()
```python
# input 接收的所有数据默认都是字符串
password = input('请输入您的密码：')
print(f'您输入的密码是{password}')
print(type(password))       # <class 'str'>

# 输入数值必须转类型
age = int(input('请输入您的年龄：'))
print(f'明年您 {age + 1} 岁')   # 不转换会报错：字符串不能和整数相加
```

---

## 四、数据类型转换

### 4.1 必要性
```python
num = input('请输入数字：')
print(type(num))            # str
num = int(num)              # 转整型
print(num + 1)              # 成功运算
```

### 4.2 常用转换函数
```python
# int / float / str
print(int('10'))            # 10
print(float('3.14'))        # 3.14
print(str(100), type(str(100)))   # 100 <class 'str'>

# list / tuple / set 互转
print(list('abc'))          # ['a','b','c']
print(list((1, 2, 3)))      # [1, 2, 3]
print(tuple([1, 2, 3]))     # (1, 2, 3)
print(set([1, 1, 2, 2]))    # {1, 2}（去重）

# eval：把字符串还原为原本类型
print(type(eval('10')))             # int
print(type(eval('[1, 2, 3]')))      # list
print(type(eval('(100, 200, 300)')))  # tuple
print(eval('1 + 2'))                # 3（当表达式求值）

# chr / ord：字符与编码互转
print(ord('A'))             # 65
print(chr(65))              # A

# bin / oct / hex
print(bin(10))              # 0b1010
print(oct(10))              # 0o12
print(hex(10))              # 0xa
```

> **安全提示**：`eval()` 会执行字符串中的代码，切勿对不可信的用户输入使用（有代码注入风险）。

---

## 五、运算符

### 5.1 算数运算符
```python
print(1 + 2)        # 3
print(10 / 2)       # 5.0（除永远是浮点）
print(9 // 4)       # 2（整除）
print(9 % 4)        # 1（取余）
print(2 ** 4)       # 16（幂）
print(5 % 2)        # 1（判断奇偶常用）
print(-9 // 4)      # -3（向负无穷取整）

# 优先级：() > ** > * / // % > + -
print((1 + 2) * 3)  # 9
print(2 ** 3 ** 2)  # 512（** 右结合：3**2=9，2**9=512）
```

### 5.2 赋值与复合赋值
```python
# 多个变量
a, b, c = 10, 0.5, 'hello'
print(a, b, c)

# 复合赋值：先算右侧，再算算术，最后赋值
c = 10
c += 1 + 2          # c = 10 + 3 = 13
print(c)            # 13

a = 100
a += 1              # 101
b = 2
b *= 3              # 6
```

### 5.3 比较运算符
```python
a, b = 7, 5
print(a == b)       # False
print(a != b)       # True
print(a > b)        # True
print(a <= b)       # False

# 字符串比较按字典序
print('abc' < 'abd')    # True
print('A' < 'a')        # True（大写字母 ASCII 更小）
```

### 5.4 逻辑运算符
```python
a, b, c = 1, 2, 3
print((a < b) and (b < c))   # True
print((a > b) and (b < c))   # False
print((a > b) or (b < c))    # True
print(not (a > b))           # True

# 数字逻辑运算（短路 + 返回具体值）
x, y, z = 0, 1, 2
print(x and y)      # 0（and：有 0 则返回 0）
print(y and z)      # 2（and：全非 0 返回最后一个）
print(x or y)       # 1（or：返回第一个非 0）
print(x or 0)       # 0（全 0 返回 0）
```

---

## 六、流程控制之条件语句（if）

### 6.1 基础 if
```python
age = 20
if age >= 18:
    print('已经成年，可以上网')
print('系统关闭')          # 无论是否满足条件都执行（无缩进）
```

### 6.2 if...else...
```python
age = int(input('请输入您的年龄：'))
if age >= 18:
    print(f'您的年龄是{age}, 已经成年，可以上网')
else:
    print(f'您的年龄是{age}, 未成年，请回家写作业')
```

### 6.3 多重判断 if...elif...else
```python
age = int(input('请输入您的年龄：'))
if age < 18:
    print('童工一枚')
elif 18 <= age <= 60:        # 可化简写法（等价于 age>=18 and age<=60）
    print('合法工龄')
else:
    print('可以退休')
```

### 6.4 if 嵌套（坐公交）
```python
money = 1    # 1 有钱，0 没钱
seat = 0     # 1 有空座，0 无空座
if money == 1:
    print('土豪，顺利上车')
    if seat == 1:
        print('有空座，可以坐下')
    else:
        print('没有空座，站等')
else:
    print('没钱，不能上车')
```

### 6.5 三目运算符
```python
a, b = 1, 2
c = a if a > b else b
print(c)                    # 2

# 嵌套三目（尽量少用，可读性差）
score = 85
level = '优' if score >= 90 else ('良' if score >= 80 else '中')
print(level)                # 良
```

### 6.6 综合案例：猜拳游戏
```python
import random
computer = random.randint(0, 2)        # 0石头 1剪刀 2布
player = int(input('请出拳：0-石头，1-剪刀，2-布：'))

if (player == 0 and computer == 1) or \
   (player == 1 and computer == 2) or \
   (player == 2 and computer == 0):
    print('玩家获胜')
elif player == computer:
    print('平局')
else:
    print('电脑获胜')
print(f'电脑出的是：{computer}')
```

---

## 七、流程控制之循环

### 7.1 while 循环
```python
i = 0
while i < 5:
    print('媳妇儿，我错了')
    i += 1                 # 必须更新计数器，否则死循环
print('任务结束')
```

### 7.2 while 累加应用
```python
# 1-100 累加和
i, result = 1, 0
while i <= 100:
    result += i
    i += 1
print(result)               # 5050

# 1-100 偶数累加（方法一：取余判断）
i, result = 1, 0
while i <= 100:
    if i % 2 == 0:
        result += i
    i += 1
print(result)               # 2550

# 1-100 偶数累加（方法二：步长为 2）
i, result = 0, 0
while i <= 100:
    result += i
    i += 2
print(result)               # 2550
```

### 7.3 break 与 continue
```python
# break：终止整个循环
i = 1
while i <= 5:
    if i == 4:
        print('吃饱了不吃了')
        break
    print(f'吃了第{i}个苹果')
    i += 1

# continue：跳过本次，继续下一次
i = 1
while i <= 5:
    if i == 3:
        print('大虫子，第3个不吃了')
        i += 1              # continue 前必须更新计数器，否则死循环
        continue
    print(f'吃了第{i}个苹果')
    i += 1
```

### 7.4 while 循环嵌套（九九乘法表）
```python
j = 1
while j <= 9:
    i = 1
    while i <= j:
        print(f'{i}*{j}={j*i}', end='\t')
        i += 1
    print()                 # 换行
    j += 1
```

### 7.5 for 循环
```python
# 遍历字符串
for i in 'itwebpython':
    print(i)

# range() 生成数字序列
for i in range(1, 10, 2):   # 不包含 end
    print(i)                # 1 3 5 7 9
for i in range(10):
    print(i)                # 0-9

# for + enumerate 同时取下标
list1 = ['a', 'b', 'c', 'd']
for index, char in enumerate(list1, start=1):
    print(f'下标是{index}, 对应的字符是{char}')
```

### 7.6 循环 else
```python
# 正常结束执行 else
for i in 'ithepython':
    print(i)
else:
    print('循环正常结束')      # 会执行

# break 终止不执行 else
for i in 'ithepython':
    if i == 'e':
        break
    print(i)
else:
    print('不会执行这段')      # break 后不执行

# continue 正常结束仍执行 else
for i in 'it':
    if i == 'e':
        continue
    print(i)
else:
    print('continue 后仍会执行 else')
```

### 7.7 for 打印星号
```python
# 正方形 5x5
for j in range(5):
    for i in range(5):
        print('*', end='')
    print()

# 三角形
for j in range(5):
    for i in range(j + 1):
        print('*', end='')
    print()
```

---

## 八、数据序列（字符串 / 列表 / 元组 / 字典 / 集合）

### 8.1 下标与切片
```python
name = 'abcdefg'
print(name[0])          # a
print(name[-1])         # g（负索引从右往左）

# 切片 序列[开始:结束:步长]，不包含结束位
print(name[2:5])        # cde
print(name[:5])         # abcde
print(name[1:])         # bcdefg
print(name[::2])        # aceg
print(name[:-1])        # abcdef
print(name[-4:-1])      # def
print(name[::-1])       # gfedcba（倒序）
```

### 8.2 字符串（str）
```python
mystr = "hello world and itcast and iteduca and Python"

# 查找
print(mystr.find('and'))        # 12（找不到返回 -1）
print(mystr.index('and'))       # 12（找不到报错）
print(mystr.count('and'))       # 3
print(mystr.rfind('and'))       # 从右找

# 修改（字符串不可变，返回新串）
print(mystr.replace('and', 'he'))          # 全部替换
print(mystr.replace('and', 'he', 1))       # 只替换 1 次
print(mystr.split('and'))                  # ['hello world ', ' itcast ', ...]
print(mystr.upper())                       # 全大写
print(mystr.capitalize())                  # 首字母大写其余小写
print(mystr.title())                       # 每个单词首字母大写

# join：用字符拼接序列
list1 = ['it', 'zhiye', 'jiao', 'yu']
print('_'.join(list1))          # it_zhiye_jiao_yu

# 去除空白
s = '  hello  '
print(s.strip())                # 'hello'
print(s.lstrip(), s.rstrip())

# 判断
print(mystr.startswith('hello'))   # True
print(mystr.endswith('Python'))    # True
print('12345'.isdigit())           # True
print('abc'.isalpha())             # True
print('abc123'.isalnum())          # True
print('   '.isspace())             # True

# 对齐
print('hello'.ljust(10, '*'))   # hello*****
print('hello'.rjust(10, '*'))   # *****hello
print('hello'.center(10, '*'))  # **hello***
```

### 8.3 列表（list）—— 可变类型
```python
name_list = ['Tom', 'Lily', 'Rose']

# 查
print(name_list[0])                 # Tom
print(name_list.index('Lily'))      # 1
print(name_list.count('Tom'))       # 1
print(len(name_list))               # 3
print('Tom' in name_list)           # True

# 增
name_list.append('xiaoming')            # 末尾追加一个
name_list.extend(['A', 'B'])            # 展开序列追加
name_list.insert(1, 'inserted')         # 指定位置插入
print(name_list)

# 删
name_list.remove('Lily')            # 删除第一个匹配
del name_list[0]                    # 按下标删
name_list.pop()                     # 删除末尾并返回
name_list.clear()                   # 清空

# 改
num_list = [1, 5, 2, 3, 6, 8]
num_list[0] = 100                   # 按下标修改
num_list.sort()                     # 升序
num_list.sort(reverse=True)         # 降序
num_list.reverse()                  # 逆置
copy_list = num_list.copy()         # 复制

# 遍历
for i in name_list:
    print(i)

# 嵌套取值
matrix = [['小明', '小红'], ['Tom', 'Lily'], ['张三', '李四']]
print(matrix[2][1])     # 李四
```

**随机分配办公室（综合）**
```python
import random
teachers = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H']
offices = [[], [], []]

for name in teachers:
    index = random.randint(0, 2)
    offices[index].append(name)

for i, room in enumerate(offices, start=1):
    print(f'办公室{i}：{room}')
```

### 8.4 元组（tuple）—— 不可变
```python
# 定义
t1 = (10, 20, 30)
t2 = (10,)                  # 单元素必须加逗号，否则是 int
print(type(t2))             # tuple
print(type((20)))           # int

# 只支持查找
tuple1 = ('aa', 'bb', 'cc', 'bb')
print(tuple1[0])            # aa
print(tuple1.index('cc'))   # 2
print(tuple1.count('bb'))   # 2
print(len(tuple1))          # 4

# 易错：元组内含列表，列表可改
t2 = (10, 20, ['aa', 'bb'], 50)
t2[2][0] = 'AAAAA'
print(t2)                   # (10, 20, ['AAAAA', 'bb'], 50)
```

### 8.5 字典（dict）—— 可变
```python
dict1 = {'name': 'Tom', 'age': 20, 'gender': '男'}

# 增 / 改（key 存在改，不存在增）
dict1['name'] = 'Rose'
dict1['id'] = 110
print(dict1)

# 删
del dict1['gender']
# dict1.clear()

# 查
print(dict1['name'])            # key 不存在会报错
print(dict1.get('name'))        # 安全，返回 None
print(dict1.get('xxx', '默认值'))  # key 不存在返回默认值
print(dict1.keys())             # dict_keys([...])
print(dict1.values())           # dict_values([...])
print(dict1.items())            # dict_items([(key, value), ...])

# 遍历
for key in dict1.keys():
    print(key)
for value in dict1.values():
    print(value)
for key, value in dict1.items():
    print(f'{key} = {value}')
```

### 8.6 集合（set）—— 可变
```python
# 创建（空集合只能用 set()，{} 是空字典）
s1 = {10, 20, 30, 10, 20}     # 自动去重
print(s1)                     # {10, 20, 30}
s2 = set()                    # 空集合
s3 = set('abc')               # {'a','b','c'}

# 增
s1.add(100)                   # 已存在则不做任何事
s1.update([200, 300])         # 追加序列
# 删
s1.remove(10)                 # 不存在报错
s1.discard(999)               # 不存在不报错
s1.pop()                      # 随机删除一个并返回

# 查
print(10 in s1)

# 补充：集合运算
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}
print(a | b)    # 并集 {1,2,3,4,5,6}
print(a & b)    # 交集 {3,4}
print(a - b)    # 差集 {1,2}
```

### 8.7 公共操作
```python
# + 合并（字符串/列表/元组）
print('aa' + 'bb')              # aabb
print([1, 2] + [3, 4])          # [1,2,3,4]
print((1, 2) + (3, 4))          # (1,2,3,4)

# * 复制
print('-' * 10)                 # ----------
print([0] * 3)                  # [0,0,0]

# len / max / min
print(len('abcdefg'))           # 7
print(max([10, 20, 30]))        # 30
print(min('abc'))               # a

# range / enumerate 见 7.5
```

### 8.8 推导式
```python
# 列表推导式
list1 = [i for i in range(10)]                      # [0..9]
list2 = [i for i in range(10) if i % 2 == 0]        # [0,2,4,6,8]
list3 = [i**2 for i in range(5)]                    # 平方
list4 = [(i, j) for i in range(1, 3) for j in range(3)]  # 双重循环

# 字典推导式
dict1 = {i: i**2 for i in range(1, 5)}              # {1:1, 2:4, 3:9, 4:16}
keys = ['name', 'age', 'gender']
vals = ['Tom', 20, 'man']
dict2 = {keys[i]: vals[i] for i in range(len(keys))}  # 两列表合并
counts = {'MBP': 268, 'HP': 125, 'DELL': 201}
dict3 = {k: v for k, v in counts.items() if v >= 200}  # 过滤

# 集合推导式（自动去重）
set1 = {i**2 for i in [1, 1, 2]}                    # {1, 4}
```

### 8.9 可变与不可变类型（关键概念）
```python
# 不可变：修改 = 生成新对象，id 变化
str1 = 'hello'
print(id(str1))
str1 += ' world'            # 生成新对象
print(id(str1))             # id 变了

# 可变：原地修改，id 不变
list1 = [1, 2, 3]
print(id(list1))
list1.append(4)
list1[0] = 99
print(id(list1))            # id 不变

# 函数传参差异
def modify_data(num, lst):
    num += 10               # 不可变：不影响外部
    lst.append(4)           # 可变：影响外部

a, b = 5, [1, 2, 3]
modify_data(a, b)
print(a)                    # 5
print(b)                    # [1, 2, 3, 4]
```

---

## 九、函数

### 9.1 定义、调用、返回值
```python
def add_num(a, b):
    return a + b            # return 返回结果

result = add_num(10, 20)
print(result)               # 30

# 多返回值（默认打包为元组）
def return_multi():
    return 1, 2, 3

r = return_multi()
print(r)                    # (1, 2, 3)
x, y, z = return_multi()    # 拆包
print(x, y, z)              # 1 2 3

# return 后的代码不执行
def test():
    return 1
    print('不会执行')        # 永不执行
```

### 9.2 参数
```python
# 位置参数：顺序个数必须一致
def user_info(name, age, gender):
    print(f'{name}, {age}, {gender}')
user_info('TOM', 20, '男')

# 关键字参数：顺序可乱，必须在位置参数后
user_info('Rose', age=20, gender='女')
user_info('Rose', gender='女', age=20)

# 缺省参数（默认参数）：位置参数必须在缺省参数前
def f(name, age, gender='男'):
    print(f'{name}, {age}, {gender}')
f('TOM', 20)                 # 用默认值
f('Rose', 18, '女')          # 覆盖默认值

# 不定长位置参数 *args → 元组
def f(*args):
    print(args)
f(1, 2, 3)                  # (1, 2, 3)

# 不定长关键字参数 **kwargs → 字典
def f(**kwargs):
    print(kwargs)
f(name='TOM', age=18)       # {'name':'TOM', 'age':18}

# 混用：位置 + 可变位置 + 关键字 + 可变关键字
def full(a, b, *args, c=0, **kwargs):
    print(a, b, args, c, kwargs)
full(1, 2, 3, 4, c=10, x=100, y=200)
# 1 2 (3, 4) 10 {'x':100, 'y':200}
```

### 9.3 变量作用域与 global
```python
a = 100                     # 全局变量

def testA():
    print(a)                # 可读全局变量

def testB():
    global a                # 声明后才能在函数内修改全局
    a = 200

testB()
print(a)                    # 200

# 不加 global，函数内赋值是定义局部变量
b = 100
def testC():
    b = 50                  # 局部变量，不影响外部
testC()
print(b)                    # 100
```

### 9.4 函数嵌套调用与引用
```python
def testB():
    print('B start')
    print('B end')

def testA():
    print('A start')
    testB()                 # A 内调用 B，B 执行完才回到 A
    print('A end')

testA()

# 引用：id 判断是否同一对象
def f(a):
    print(id(a))
    a += a
    print(id(a))

b = 100
f(b)                        # int 不可变，前后 id 不同
c = [11, 22]
f(c)                        # list 可变，前后 id 相同
```

### 9.5 说明文档
```python
def sum_num(a, b):
    """ 求两数之和 """
    return a + b

help(sum_num)               # 查看说明文档
print(sum_num.__doc__)      # 或直接取 __doc__
```

### 9.6 lambda 表达式
```python
# 基本
fn = lambda a, b: a + b
print(fn(1, 2))             # 3

# 无参 / 默认参数 / 可变参数
f1 = lambda: 100
f2 = lambda a, b, c=100: a + b + c
f3 = lambda *args: args
f4 = lambda **kwargs: kwargs

# 带判断
f5 = lambda a, b: a if a > b else b
print(f5(1000, 500))        # 1000

# 列表按字典 key 排序
students = [
    {'name': 'TOM', 'age': 20},
    {'name': 'ROSE', 'age': 19},
    {'name': 'Jack', 'age': 22}
]
students.sort(key=lambda x: x['age'])           # 按 age 升序
students.sort(key=lambda x: x['name'], reverse=True)  # 按 name 降序
```

### 9.7 递归
```python
# 3 以内累加
def sum_numbers(num):
    if num == 1:            # 基线条件（出口）
        return 1
    return num + sum_numbers(num - 1)   # 递归条件
print(sum_numbers(3))       # 6

# 阶乘
def factorial(n):
    if n == 1:
        return 1
    return n * factorial(n - 1)
print(factorial(5))         # 120

# 斐波那契（含记忆化优化）
def fib(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 2:
        return 1
    memo[n] = fib(n - 1, memo) + fib(n - 2, memo)
    return memo[n]
print(fib(6))               # 8

# 嵌套列表扁平化
def flatten(nested):
    result = []
    for item in nested:
        if isinstance(item, list):
            result.extend(flatten(item))
        else:
            result.append(item)
    return result
print(flatten([1, [2, [3, 4]], 5]))   # [1, 2, 3, 4, 5]
```

### 9.8 高阶函数
```python
# 把函数作为参数
def sum_num(a, b, f):
    return f(a) + f(b)
print(sum_num(-1, 2, abs))  # 3

# map：每个元素作用 func
list1 = [1, 2, 3, 4, 5]
print(list(map(lambda x: x ** 2, list1)))   # [1, 4, 9, 16, 25]

# reduce：两两累积（需要 import）
from functools import reduce
print(reduce(lambda a, b: a + b, list1))    # 15

# filter：过滤满足条件的元素
list2 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(list(filter(lambda x: x % 2 == 0, list2)))  # [2,4,6,8,10]
```

### 9.9 综合应用：学员管理系统（骨架）
```python
info = []       # 全局列表，存学员字典

def add_info():
    global info
    new_name = input('请输入姓名：')
    for i in info:
        if new_name == i['name']:
            print('该用户已存在！')
            return
    info_dict = {'name': new_name}
    info_dict['tel'] = input('请输入手机号：')
    info.append(info_dict)
    print(info)

def show_all():
    print('姓名\t手机号')
    for i in info:
        print(f'{i["name"]}\t{i["tel"]}')

# 主循环
while True:
    cmd = input('1添加 2显示 3退出：')
    if cmd == '1':
        add_info()
    elif cmd == '2':
        show_all()
    elif cmd == '3':
        break
```

---

## 十、文件操作

### 10.1 打开 / 写 / 读 / 关闭
```python
# 写
f = open('test.txt', 'w')       # w 模式：不存在创建，存在清空
f.write('hello world\n')
f.write('第二行\n')
f.close()

# 追加
f = open('test.txt', 'a')
f.write('追加的内容\n')
f.close()

# 读（r 模式文件不存在会报错）
f = open('test.txt', 'r')
print(f.read())                 # 读全部
f.close()

f = open('test.txt')
print(f.readline())             # 一次读一行
f.close()

f = open('test.txt')
print(f.readlines())            # 按行返回列表
f.close()

# seek 移动指针
f = open('test.txt', 'r')
f.read(5)                       # 先读 5 字节
f.seek(0)                       # 回到开头
print(f.read(2))                # 从头读 2 字节
f.close()
```

**打开模式速查**
| 模式 | 说明 |
| --- | --- |
| `r` | 只读（默认），不存在报错 |
| `w` | 写，不存在创建，存在清空 |
| `a` | 追加，指针在结尾 |
| `rb / wb` | 二进制读写 |
| `r+ / w+ / a+` | 读写结合 |

### 10.2 with 语句（推荐写法）
```python
# with 自动关闭文件，无需手动 close
with open('test.txt', 'w') as f:
    f.write('用 with 写文件\n')

with open('test.txt', 'r') as f:
    print(f.read())
```

### 10.3 文件备份
```python
old_name = input('请输入要备份的文件名：')
index = old_name.rfind('.')
if index > 0:
    postfix = old_name[index:]
    new_name = old_name[:index] + '[备份]' + postfix

    with open(old_name, 'rb') as old_f, open(new_name, 'wb') as new_f:
        while True:
            con = old_f.read(1024)      # 分块读取
            if len(con) == 0:
                break
            new_f.write(con)
    print(f'备份完成：{new_name}')
else:
    print('无效文件名')
```

### 10.4 os 模块操作
```python
import os
print(os.getcwd())                      # 当前目录
print(os.listdir('.'))                  # 目录文件列表
os.rename('test.txt', 'test2.txt')      # 重命名
os.remove('test2.txt')                  # 删除文件
os.mkdir('new_dir')                     # 建文件夹
os.rmdir('new_dir')                     # 删文件夹
os.chdir('C:/Users')                    # 切换目录
```

---

## 十一、面向对象

### 11.1 类与对象、self
```python
class Washer():
    def wash(self):
        print('我会洗衣服')
        print(self)         # self 是调用该方法的对象

haier1 = Washer()
haier1.wash()
```

### 11.2 对象属性（类外 / 类内）
```python
class Washer():
    def print_info(self):
        print(f'宽度是{self.width}, 高度是{self.height}')

haier1 = Washer()
haier1.width = 500          # 类外添加
haier1.height = 800
haier1.print_info()         # 类内获取
```

### 11.3 魔法方法
```python
class Washer():
    def __init__(self, width, height):   # 初始化，创建时自动调用
        self.width = width
        self.height = height

    def __str__(self):                   # print(对象) 时返回
        return f'洗衣机：{self.width}x{self.height}'

    def __del__(self):                   # 删除对象时调用
        print('对象已删除')

haier = Washer(500, 800)
print(haier)                # 洗衣机：500x800
del haier
```

### 11.4 继承
```python
class Master(object):
    def __init__(self):
        self.kongfu = '[古法配方]'
    def make_cake(self):
        print(f'用{self.kongfu}做煎饼')

# 单继承
class Prentice(Master):
    pass
daqiu = Prentice()
print(daqiu.kongfu)         # 继承父类属性
daqiu.make_cake()           # 继承父类方法

# 多继承：默认用第一个父类的同名成员
class School(object):
    def __init__(self):
        self.kongfu = '[学校配方]'
    def make_cake(self):
        print(f'用{self.kongfu}做煎饼')

class Stu(School, Master):
    pass
s = Stu()
print(s.kongfu)             # 学校配方（第一个父类）
```

### 11.5 重写与调用父类方法
```python
class Master(object):
    def __init__(self):
        self.kongfu = '[古法配方]'
    def make_cake(self):
        print('古法')

class School(object):
    def __init__(self):
        self.kongfu = '[学校配方]'
    def make_cake(self):
        print('学校')

class Prentice(School, Master):
    def __init__(self):
        self.kongfu = '[独创配方]'      # 重写属性

    def make_cake(self):                # 重写方法
        print(f'用{self.kongfu}')

    # 调用父类方法（需先调父类初始化）
    def make_master_cake(self):
        Master.__init__(self)
        Master.make_cake(self)
    def make_school_cake(self):
        School.__init__(self)
        School.make_cake(self)

daqiu = Prentice()
daqiu.make_cake()            # 用独创配方
daqiu.make_master_cake()     # 古法
daqiu.make_school_cake()     # 学校
```

### 11.6 super() 与多层继承
```python
class Master(object):
    def __init__(self):
        self.kongfu = '[古法配方]'
    def make_cake(self):
        print('古法')

class School(Master):
    def __init__(self):
        self.kongfu = '[学校配方]'
    def make_cake(self):
        print('学校')
        super().__init__()      # super 自动找父类
        super().make_cake()

class Prentice(School):
    def __init__(self):
        self.kongfu = '[独创配方]'
    def make_old_cake(self):
        super().__init__()      # 一次性调父类
        super().make_cake()

daqiu = Prentice()
daqiu.make_old_cake()
```

### 11.7 私有权限
```python
class Prentice:
    def __init__(self):
        self.kongfu = '[配方]'
        self.__money = 2000000      # 私有属性

    def __info(self):               # 私有方法
        print('私有方法')

    def get_money(self):            # 对外获取
        return self.__money
    def set_money(self):
        self.__money = 500          # 对外修改

daqiu = Prentice()
# print(daqiu.__money)             # 类外不能访问，报错
print(daqiu.get_money())            # 2000000

class Tusun(Prentice):              # 子类不继承私有成员
    pass
t = Tusun()
print(t.get_money())                # 可通过公有方法间接访问
# t.__info()                        # 私有方法不能调用
```

### 11.8 多态
```python
class Dog(object):
    def work(self):
        print('指哪打哪')

class ArmyDog(Dog):
    def work(self):
        print('追击敌人')

class DrugDog(Dog):
    def work(self):
        print('追查毒品')

class Person:
    def work_with_dog(self, dog):
        dog.work()          # 传入不同对象，结果不同

daqiu = Person()
daqiu.work_with_dog(ArmyDog())   # 追击敌人
daqiu.work_with_dog(DrugDog())   # 追查毒品
```

### 11.9 类属性 / 实例属性 / 类方法 / 静态方法
```python
class Dog(object):
    tooth = 10                      # 类属性（所有实例共有）

    def __init__(self, age):
        self.age = age              # 实例属性

    @classmethod
    def get_tooth(cls):             # 类方法，参数 cls
        return cls.tooth

    @staticmethod
    def info():                     # 静态方法，无 self/cls
        print('这是一个狗类')

wangcai = Dog(5)
xiaohei = Dog(3)

# 类属性：类和对象都能访问，只能通过类修改
print(Dog.tooth, wangcai.tooth, xiaohei.tooth)   # 10 10 10
Dog.tooth = 12
print(wangcai.tooth)                # 12
wangcai.tooth = 20                  # 这实际是新建实例属性
print(Dog.tooth)                    # 12（类属性未变）

# 实例属性：不能通过类访问
print(wangcai.age)                  # 5
# print(Dog.age)                    # 报错

# 类方法 / 静态方法调用
print(Dog.get_tooth())              # 12
Dog.info()
wangcai.info()
```

### 11.10 综合案例：烤地瓜
```python
class SweetPotato():
    def __init__(self):
        self.cook_time = 0
        self.cook_static = '生的'
        self.condiments = []

    def cook(self, time):
        self.cook_time += time
        if 0 <= self.cook_time < 3:
            self.cook_static = '生的'
        elif 3 <= self.cook_time < 5:
            self.cook_static = '半生不熟'
        elif 5 <= self.cook_time < 8:
            self.cook_static = '熟了'
        else:
            self.cook_static = '烤糊了'

    def add_condiments(self, condiment):
        self.condiments.append(condiment)

    def __str__(self):
        return f'烤了{self.cook_time}分钟, 状态是{self.cook_static}, 调料有{self.condiments}'

d = SweetPotato()
d.cook(2); d.add_condiments('酱油')
d.cook(2); d.add_condiments('辣椒面')
d.cook(3)
print(d)        # 烤了7分钟, 状态是熟了, 调料有['酱油', '辣椒面']
```

---

## 十二、模块、包与异常

### 12.1 导入模块
```python
import math
print(math.sqrt(9))             # 3.0

from math import sqrt
print(sqrt(9))

from math import *
print(sqrt(16))                 # 4.0

import math as m                # 模块别名
print(m.pi)

from math import sqrt as sq     # 功能别名
print(sq(25))
```

### 12.2 制作模块与 __all__
```python
# my_module.py 内容
__all__ = ['testA']             # from ... import * 只导入这些

def testA():
    print('testA')

def testB():
    print('testB')

if __name__ == '__main__':      # 仅当前文件运行时执行
    testA()

# 其他文件调用
# import my_module
# my_module.testA()
```

### 12.3 包
```python
# 目录结构
# mypackage/
#   __init__.py
#   my_module1.py
#   my_module2.py

# 导入方式一
# import mypackage.my_module1
# mypackage.my_module1.info_print1()

# 导入方式二（需在 __init__.py 配 __all__）
# from mypackage import *
# my_module1.info_print1()
```

### 12.4 异常捕获
```python
# 基础捕获
try:
    f = open('test.txt', 'r')
except:
    f = open('test.txt', 'w')   # 失败则创建

# 捕获指定异常 + 多个 + 描述信息
try:
    print(1 / 0)
except (NameError, ZeroDivisionError) as result:
    print(f'出错：{result}')

# Exception 捕获所有
try:
    print(num)
except Exception as result:
    print(result)

# else / finally
try:
    print(1)
except Exception as result:
    print(result)
else:
    print('没有异常时执行')
finally:
    print('无论是否异常都执行')
```

### 12.5 异常的传递
```python
import time
try:
    f = open('test.txt')
    try:
        while True:
            content = f.readline()
            if len(content) == 0:
                break
            time.sleep(2)
            print(content)
    except:
        print('读取中意外终止')
    finally:
        f.close()
        print('已关闭文件')
except:
    print('没有这个文件')
```

### 12.6 自定义异常
```python
class ShortInputError(Exception):
    def __init__(self, length, min_len):
        self.length = length
        self.min_len = min_len
    def __str__(self):
        return f'你输入的长度是{self.length}, 不能少于{self.min_len}个字符'

def main():
    try:
        con = input('请输入密码：')
        if len(con) < 3:
            raise ShortInputError(len(con), 3)    # 抛出自定义异常
    except Exception as result:
        print(result)
    else:
        print('密码输入完成')

main()
```

---

## 十三、综合练习（建议动手）

1. **质数判断**：输入整数，判断是否为质数（for + 条件）。
2. **字符串统计**：统计字符串中每个字符出现次数（字典）。
3. **列表去重保持顺序**：用循环 + `not in` 实现。
4. **成绩分级**：if/elif 实现 A/B/C/D 等级。
5. **二分查找**：递归实现。
6. **批量改文件名**：`os.listdir` + `os.rename`。
7. **ATM 取款机**：函数封装 + 循环菜单 + 余额管理。

```python
# 示例：统计字符出现次数
def char_count(s):
    count = {}
    for ch in s:
        count[ch] = count.get(ch, 0) + 1   # 用 get 安全累加
    return count
print(char_count('hello'))      # {'h':1, 'e':1, 'l':2, 'o':1}

# 示例：列表去重保持顺序
def dedup(lst):
    result = []
    for x in lst:
        if x not in result:
            result.append(x)
    return result
print(dedup([1, 2, 1, 3, 2, 4]))    # [1, 2, 3, 4]
```

---

## 十四、学习路线建议

1. **先建立「动手」习惯**：学语法时随手在 PyCharm 里敲代码验证，别只看不写。
2. **以「数据 → 结构 → 流程」为主线**：先掌握变量与 8 大数据类型，再学容器（list/tuple/dict/set），再学 if/循环如何操作它们。
3. **重点难点**：
   - 可变/不可变类型与传参差异（面试高频）；
   - 切片与负索引；
   - 函数参数四大类型与 `global` 作用域；
   - 列表/字典推导式；
   - 面向对象三大特性 + 魔法方法。
4. **每章配综合案例**：猜拳（条件+随机）、九九乘法表（嵌套循环）、学员管理系统（函数+全局+列表+字典）、烤地瓜/搬家具（面向对象）、文件备份、批量改名。
5. **衔接下一阶段**：基础掌握后，进阶顺序建议：常用内置库（random/math/os/sys/time）→ 函数式编程（map/reduce/filter）→ 面向对象设计模式 → 再到 AI/数据分析（NumPy、Pandas、PyTorch 等）。
