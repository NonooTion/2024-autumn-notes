# Python基础语法

## 字面量

代码中，被写下来的固定的值

Python中常用的有六种类型的值

Number,String,List,Tuple,Set,Dictionary



## 注释

单行注释使用井号

多行注释使用三个引号引起来



## 变量

变量通过 变量名=字面量 的形式来定义



## 数据类型

通过type()语句获取数据类型

```2
money=123
print(type(money))
```



## 数据类型转换

常见转换语句

int(x) 将x转换为一个整数

float(x) 将x转换为一个浮点数

str(x) 将对象x转换为字符串

```py
# 将数字类型转换为字符串类型
number=1.32
print(type(number))
toString=str(number)
print(type(toString))
```



## 标识符

变量的名字，方法的名字，类的名字，统称为标识符，用于做内容的标识

- 内容限定(英文，中文，数字，下划线)
- 大小写敏感
- 不可使用关键字(False True None and...)



## 运算符

加减乘除取余与C，JAVA相同

// 取整除 返回商的整数部分

** 指数

赋值运算符



## 字符串

### 字符串的三种定义方式

单引号定义法，双引号定义法，三引号定义法

转义字符\



### 字符串拼接

使用+来进行字符串拼接

其他类型的数据使用+与字符串拼接前，需要先将类型转换为字符串类型



### 字符串格式化

使用%s作为字符串类型占位符

```python
class_name='网安班'
student_name='陈木华大王爱玩原神'
message="班级:%s,姓名:%s" %(class_name,student_name)
print(message)
```

%d 整数类型占位符

%f 浮点数类型占位符



将内容转换为对应类型，放入占位位置



### 格式化的精度控制

使用辅助符号"m.n"来控制数据的宽度和精度

m,控制宽度，要求是数字

.n控制小数点精度，要求是数字，四舍五入



### 快速格式化

f"内容{变量}"  的格式来快速格式化

不限数据类型 不做精度控制

```py
num1=1.235
message=f"num1: {num1}"
print(message)
```



## 数据输入

使用input来获取输入的信息

返回值为字符串类型，使用前如有必要，需要转换

```py
num1=float(input("请输入一个浮点数"))
num2=int(input("请输入一个整数"))

print(num1+num2)
```



# Python判断语句

## 布尔类型和比较运算符

Python布尔类型具有两个值用于判断，分别是True(1)和False(0)

比较运算符与其他编程语言类似



## If语句的基本格式

```py
age=30
if age==30:
    print("my age is %s" % age) 
```



## if-else语句的格式

```py
age=int(input("Enter your age:"))
if age<18:
    print("未成年")
else :
    print("成年")
```



if-elif-else语句的格式

```py
age=int(input("Enter your age:"))
if age<8:
    print("儿童")
elif age<18:
    print("未成年")
else:
    print("成年")
```



判断语句可以进行嵌套



# Python循环语句

## while循环

```py
# 求从1到100的和
i=1
sum=0
while i<=100:
    sum+=i
    i+=1
print(sum)
```

while循环可以嵌套，可以通过break跳出循环，continue跳过循环



## for循环

### 基础语法

同while循环不同，for循环无法定义循环条件，只能从被处理的数据集中，依次取出内容进行处理

```py
#for循环遍历字符串
name="陈木华"
for i in name:
    print(i)
```



### range语句

for循环语句，本质上是遍历：序列类型

通过range语句，可以获得一个简单的数字序列



- range(num)

获得一个从0开始，到num结束的数字序列，不包含num本身

- range(num1,num2)

获得一个从num1开始，到num2结束的数字序列，不包含num2本身

- range(num1,num2,step) 

获得一个从num1开始,到num2结束的数字序列，不包括num2,数字之间的步长为step

```py
#打印九九乘法表
for x in range(1,10):
    for y in  range (1,x+1):
        print("%s * %s = %s     " % (y,x,x*y),end='')
    print()
```



# Python函数

函数通过def关键字进行定义

```py
def 函数名(传入参数):
	函数体
	return 返回值
```

Python中有一个特殊的字面量 None 无返回值的函数，实际返回了None



通过global关键字在函数内定义全局变量



# Python数据容器

## 列表list

```py
#声明空列表
elements=[]
elements_2=list()
#列表的定义方式
elements = ["元素1", "元素2", "元素3"]
print(elements)
print(type(elements))

#列表存储的数据类型不做限制
elements =[123,True,'陈木华']
print(elements)

#使用下标索引可以取出列表中的元素
print(elements[0])
# 可以使用负数下标从后向前取元素
print(elements[-1])

"""
列表常用操作:
1.index(元素) 查询指定元素在列表中的下标值，如果元素不存在，报错ValueError

2.列表[下标]=值 进行列表的赋值

3.insert(下标，元素) 在列表对应下标位置后插入元素

4.append(元素) 在列表末尾追加元素

5.extend(其他数据容器) 将其他数据容器的内容取出，依次追加到列表尾部

6.del 列表[下标]进行删除

7.pop(下标)进行删除

8.remove(元素) 删除某元素在列表中的第一个匹配项

9.clear() 清空整个列表

10.count(元素) 统计某个元素在列表内的数量

11.len() 统计列表内有多少元素
"""
```



## 元组tuple

可以封装多个、不同类型的元素在内，一旦定义完成，就不可修改

元组使用小括号进行定义，使用逗号隔开各个数据



## 字符串str

从容器的角度来看，字符串是字符的容器，一个字符串可以存放任意数量的字符

```py
my_str="陈木华,禁逗!"

#字符串操作
# 1.通过下标索引取值
print(my_str[0])
print(my_str[-1])
# 字符串是一个不可修改的容器
# 2.index方法获取元素索引
print(my_str.index('华'))

# 3. replace(字符串1，字符串2) 将字符串中的字符串1替换为字符串2
print(my_str.replace("禁逗","嘿嘿，逗你的"))

# 4.split(分隔字符串) 按照指定的分隔字符串，将字符串划分成多个字符串，存入列表对象中
print(my_str.split(","))

# 5. strip(字符串) 去前后指定字符串 strip()去前后空格
print(my_str.strip("陈怒"))
#按照字符删除

# 6.count(字符串) 统计字符串出现的次数
print(my_str.count("陈"))

# 7.len()统计字符串的长度

print(len(my_str))
```

## 序列

内容连续、有序、可使用下标索引的一类数据容器

列表、元组、字符串均可以视为序列

切片操作：从一个序列中，取出一个子序列



语法为 序列[起始下标:结束下标:步长]



## 集合set

不支持元素的重复，自带去重功能

使用大括号{}定义集合

```py
my_set=set()

#集合操作
#1.add(元素) 添加元素
my_set.add(1)
my_set.add(2)
my_set.add(3)
my_set.add(3)
my_set.add(4)
my_set.add(5)


print(f"添加后my_set容器内的元素: {my_set}")

#2.remove(元素) 移除元素
my_set.remove(2)

print(f"移除后my_set容器内的元素: {my_set}")

#3.pop() 随机取出元素
my_set.pop()
print(my_set)

#4.clear()清空集合

#5.difference(集合2) 取差集

#6.difference_update(集合2) 在集合1，删除与集合2相同的元素

#6.union(集合2) 合并集合1和集合
```



## 字典

通过{}定义字典，存储的元素是一个个的键值对

```py
{key:value,key:value,...,key:value}
my_dict=dict()

#增加，修改元素
my_dict[key]=value

#删除元素
my_dict.pop(key)

#获取全部keys
keys=my_dict.keys()

```



## 数据容器的通用操作

所有数据容器都支持for循环遍历

列表、元组、字符支持while循环遍历，集合、字典不支持



通用统计功能:

1. len()统计容器的元素个数
2. max()统计容器的最大元素
3. min()统计容器的最小元素



通用转换功能

1. list(容器) 容器转列表
2. tuple(容器) 容器转元组
3. set(容器) 容器转集合



通用排序功能

sorted(容器，[reverse=True]) 将给定容器进行排序

排序的结果会变为列表对象



## Python函数进阶

### 多个返回值

python的函数允许有多个返回值

```py
def func():
    return 返回值1，返回值2,...

变量1,变量2,...=func()
```

### 函数参数类型

python函数具有四种常见的参数使用方式

1. 位置参数

上面写的函数都是位置参数，根据传入参数的位置传递参数

2. 关键字参数

通过键=值的方式传递参数

```py
def user_info(name,age,gender):
    print(f"姓名:{name} 年龄:{age} 性别:{gender}")
    
#关键字传参
user_info(name='小米',age=20,gender='男')
#关键字传参可以不按照顺序
user_info(age=20,name='小米',gender='男')
#可以和位置参数混用，位置参数必须在前
user_info("小米",age=20,gender='男')
```

3. 缺省参数

定义函数时，可以给函数的参数赋一个默认值，函数调用时，未传入参数，则使用默认值

```py
def user_info(name,age,gender='男')
```

4. 不定长参数

位置传递

```py
def user_info(*args):
    print(args)
传进的所有参数都会被args变量收集，并按照传进参数的位置合并为一个元组
```

关键字传递

```py
def user_info(**kwargs)
传进的参数应该是key=value形式的，组成一个字典
```

# Python文件操作

### Python文件读取操作

```py
f=open("open.txt", "r", encoding="utf-8")
print("读取10个字节的数据:\n%s" %f.read(10))
f.close()

f=open("open.txt", "r", encoding="utf-8")
print(f"读取全部数据:\n{f.read()}")
f.close()

f=open("open.txt", "r", encoding="utf-8")
print(f"读取一行数据:\n{f.readline()}")
f.close()

f=open("open.txt", "r", encoding="utf-8")
#返回值是一个列表
print(f"读取所有行数据:\n{f.readlines()}")
f.close()

f=open("open.txt", "r", encoding="utf-8")
#使用循环读取文件内容
print("使用循环读取文件内容:")
for line in f:
    print(line, end="")
f.close()
```



## Python文件的写入操作

```py
1.打开文件
f=open("python.txt","w",encoding="utf-8")
2.文件写入 实际上并未真正写入文件，而是会积攒在程序的内存中
f.write("写入文件")
3.内容刷新 内容真正写入文件
f.flush()
```



## Python文件的追加操作

```py
1.打开文件
f=open("python.txt","a",encoding="utf-8")
2.文件写入 实际上并未真正写入文件，而是会积攒在程序的内存中
f.write("写入文件")
3.内容刷新 内容真正写入文件
f.flush()
```

追加操作是在原有数据后追加，而写入操作是直接替换



# Python异常、模块与包

## 捕获常规异常

将可能出现问题的代码写在try代码块中，使用except代码块捕捉异常

```py
try:
    f=open("asdbnjkb.txt","r",encoding="utf-8")
except:
    print("文件不存在")
```

## 捕获指定的异常

```py
try:
    1/0
except ZeroDivisionError as e:
    print(e)
```

```py
#捕获多个异常
try:
    1/0
except (NameError, ZeroDivisionError) as e:
    print(e)
```

## 捕获全部异常

```py
try:
    1/0
    open("123.txt","r",encoding="utf-8")
except Exception as e:
    print(e)
```

## else代码块

没有出现异常时执行的代码块

## finally代码块

无论如何都必须执行的代码块



# Python模块

## 模块导入语法

```py
[from 模块名] import [模块|类|变量|函数|*] [as 别名]
```



# Python包

Python包就是一个包含很多Python模块的文件夹，包的本质从逻辑上看仍然时模块

包导入语法

```py
import 包名.模块名
```



# Python面向对象

```py
class user:
    user_name=None
    password=None
//构造方法
    def __init__(self,user_name,password):
        self.user_name=user_name
        self.password=password
//成员方法，必须要有self参数，相当于Java或CPP中的this
    def output(self):
        print(f"user_name: {self.user_name}")
        print(f"password: {self.password}")

if __name__ == '__main__':
    newUser=user('user_name','password')
    newUser.output()
```

## 魔术方法

Python类内置了许多方法，各自有各自的功能，上面使用的init方法就是内置方法之一

| 魔术方法   | 作用                       |
| ---------- | -------------------------- |
| __init\_\_ | 构造方法                   |
| __str\_\_  | 字符串方法                 |
| __lt\_\_   | 小于、大于符号比较         |
| __le\_\_   | 小于等于、大于等于符号比较 |
| __eq\_\_   | ==符号比较                 |

## 封装

在变量名或方法名以两个下划线开头，即可设置私有成员变量和私有成员函数

##  继承

```py
class user:
    user_name=None
    password=None

    def __init__(self,user_name,password):
        self.user_name=user_name
        self.password=password

    def output(self):
        print(f"user_name: {self.user_name}")
        print(f"password: {self.password}")
#单继承语法
class student(user):
    student_id=None
    def __init__(self,user_name,password,student_id):
        self.student_id=student_id
        super().__init__(user_name,password)
    
    def output(self):
        print(f"student_id: {self.student_id}")
        print(f"user_name: {self.user_name}")
        print(f"password: {self.password}")
```

