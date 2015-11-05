> **注意**：对从不受信任的资源中所获取到的数据调用 `yaml.load` 是不安全的。`yaml.load` 和 `pickle.load` 同样强大，可以调用任何 Python 方法。Check the `yaml.safe_load` function though.

---

Main:
关于 `pickle.load`，我看了这个人的 [blog](http://oldj.net/article/python-pickle/)，简单易懂，以后要应用一下。至于后半句 'may call any Python function'，我还不太理解。稍后研究。

---

**`yaml.load` 方法将一个 YAML 文档转换成了一个 Python 对象。**

~~~ python
>>> import yaml
>>> 
>>> name = yaml.load("""
...           - Lisa
...           - Jenny
...           - Bush
...           - Monica
...           - Joshua
...        """)
>>> 
>>> print name
['Lisa', 'Jenny', 'Bush', 'Monica', 'Joshua']
~~~

`yaml.load` 可以接受一个字节字符串，Unicode 字符串，打开的二进制文件对象或者一个打开的文本文件对象。字节字符串或文件需要是由 **utf-8**，**utf-16** 或者 **utf-16-le** 进行编码的。`yaml.load` 通过在字符串和文件的起始部分检查 **BOM**（字节顺序标记）序列来检测使用到的编码方式。如果没有 **BOM** 的话，就假设它使用了 **utf-8** 这种编码方式。

`yaml.load` 会返回一个 Python 对象。

~~~ python
# 举例1：字符串
# coding: utf-8
import yaml

data = yaml.load(u"""
   hello: Привет!
""")

print data
~~~

~~~ bash
# 执行结果
{'hello': u'\u041f\u0440\u0438\u0432\u0435\u0442!'}
~~~

~~~ python
# 举例2：文件对象
# coding: utf-8
import yaml

stream = file('test.yaml', 'r')
data = yaml.load(stream)
print data # data 就是与 test.yaml  的 Python 对象。 
~~~
~~~ bash
# 返回结果
{'pet': [{'name': 'blue cat'}, {'age': 2}], 'hobby': ['swimming', 'movie'], 'age': 24, 'anotherName': u'\u5f20\u701b\u5300', 'name': u'\u5f20\u4e9a\u742a'}
~~~

如果一个字符串或者文件中包含了多个 yaml 文档，您就需要使用 `yaml.load_all` 这个函数了。

~~~ python
import yaml

documents = """
---
   name: Helen
   description: >
      She is a American author, political activist, and lecturer.
      In Greek myths, she was considered the most beautiful woman in the world.
---
   name: Elizabeth
   description: >
      She was Queen of England and Ireland from 17 November 1558 until her death.
      I think I will have a cat named Elizabeth in future.:)
---
   name: Victoria
   description: >
      She was Queen of the United Kingdom of Great Britain and Ireland from 20 June 1837 until her death.
      If I have another cat, I will name after Victoria.
      It is one of the first urban settlements in Hong Kong after it became a British colony in 1842.
"""

for data in yaml.load_all(documents):
   print data
~~~
~~~ bash
# 运行结果
{'name': 'Helen', 'description': 'She is a American author, political activist, and lecturer. In Greek myths, she was considered the most beautiful woman in the world.\n'}
{'name': 'Elizabeth', 'description': 'She was Queen of England and Ireland from 17 November 1558 until her death. I think I will have a cat named Elizabeth in future.:)\n'}
{'name': 'Victoria', 'description': 'She was Queen of the United Kingdom of Great Britain and Ireland from 20 June 1837 until her death. If I have another cat, I will name after Victoria. It is one of the first urban settlements in Hong Kong after it became a British colony in 1842.\n'}
~~~

PyYAML 还允许您来构建任何类型的 Python 对象。
~~~ python
import yaml
document = ("""
          none: [~, null] # 空值
          bool: [true, false, on, off] # 布尔值
          int: 1 # 字符值
          float: 3.1415926 # 浮点值
          list: ['guitar', 'piano', 'drum'] # 列表
          tumple: (apple, pear, orange) # 元组
          dist: {'name': helen, 'age': 24} # 字典
""")

for data in yaml.load(document):
   print data

print yaml.load(document)

print yaml.dump(yaml.load(document), default_flow_style=False)
~~~
~~~ bash
# 运行结果
none
dist
int
bool
float
list
tumple
{'none': [None, None], 'dist': {'age': 24, 'name': 'helen'}, 'int': 1, 'bool': [True, False, True, False], 'float': 3.1415926, 'list': ['guitar', 'piano', 'drum'], 'tumple': '(apple, pear, orange)'}
bool:
- true
- false
- true
- false
dist:
  age: 24
  name: helen
float: 3.1415926
int: 1
list:
- guitar
- piano
- drum
none:
- null
- null
tumple: (apple, pear, orange)

~~~

