# Dumping YAML

**`yaml.dump`** 方法接收到一个 Python 类并将其转储为 YAML 文件。

~~~ python
import yaml

data = yaml.dump({
          'name': 'helen',
          'age': 24,
          'score': [99, 100]
       })

print data
~~~

~~~ bash
# 运行结果
age: 24
name: helen
score: [99, 100]

~~~

**`yaml.dump`** 函数有第二个可选参数，前提是这个参数必须是一个可写的文本文件或二进制文件。本例中，**`yaml.dump`** 将会把生成的 YAML 文件写至该文件中。

~~~ python
import yaml

stream = file('Stu.yaml', 'w')
data = {
          'name': 'helen',
          'age': 24,
          'score': [99, 100]
       }
result = yaml.dump(data, stream, )
print result
~~~

~~~ bash
[helen@yingyun python]$ cat Stu.yaml 
age: 24
name: helen
score: [99, 100]
~~~

如果您需要将多个 YAML 文档写入一个文件中的话，请您使用 **`yaml.dump_all`** 函数，这个函数可以接收列表或者 Python 对象的生成器。

Python 对象被序列化为一个 YAML 文件。第二个可选参数是一个可打开的文件。

~~~ python
import yaml

data = yaml.dump([1,2,3], explicit_start = True)
print data

data = yaml.dump_all([1,2,3], explicit_start = True)
print data
~~~
**这里加上 explicit_start 这个参数，是为了证实 `yaml.dump_all` 会输出多个 YAML 文档。**
~~~ bash
# 运行结果
--- [1, 2, 3]

--- 1
--- 2
--- 3
...
~~~

您还可以转储 Python 类的实例。

~~~ python
import yaml

class Student:
   def __init__(self, name, score):
      self.name = name
      self.score = score
   def __str__(self):
      return "%s(name: %r, score: %r)" % (self.__class__.__name__, self.name, self.score)

print yaml.dump(Student('helen', 100))
~~~

~~~ bash
# 运行结果
!!python/object:__main__.Student {name: helen, score: 100}
~~~
**`yaml.dump`** 支持许多关键字参数，它们是用于为 emitter 指定格式的详细信息的。例如，您可以优先设置缩进和宽度，使用规范化的 YAML 格式或者自定义标量和集合。

~~~ python
>>> import yaml
>>> print yaml.dump(range(2,22))
[2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21]
~~~

~~~ python
>>> print yaml.dump(range(50), width = 15, indent = 3)
[0, 1, 2, 3, 4, 5,
   6, 7, 8, 9, 10,
   11, 12, 13, 14,
   15, 16, 17, 18,
   19, 20, 21, 22,
   23, 24, 25, 26,
   27, 28, 29, 30,
   31, 32, 33, 34,
   35, 36, 37, 38,
   39, 40, 41, 42,
   43, 44, 45, 46,
   47, 48, 49]
~~~

~~~ python
>>> print yaml.dump(range(5), canonical=True) # 如果在这里加上 default_flow_style = False/True 并没有什么用。
---
!!seq [
  !!int "0",
  !!int "1",
  !!int "2",
  !!int "3",
  !!int "4",
]
~~~

~~~ python
>>> print yaml.dump(range(5), default_flow_style = False)
- 0
- 1
- 2
- 3
- 4
~~~

~~~ python

---

Main:这是怎么回事？
~~~ python
>>> print yaml.dump(range(5), default_flow_style = False, default_style = "*")
- !!int "0"
- !!int "1"
- !!int "2"
- !!int "3"
- !!int "4"
~~~

~~~ python
>>> print yaml.dump(range(5), default_flow_style = False, default_style = "")
- 0
- 1
- 2
- 3
- 4
~~~

---

**原来是空格的问题！**
~~~ python
>>> print yaml.dump(range(5), default_flow_style = True, default_style = " ")
[!!int "0", !!int "1", !!int "2", !!int "3", !!int "4"]

>>> print yaml.dump(range(5), default_flow_style = False, default_style = " ")
- !!int "0"
- !!int "1"
- !!int "2"
- !!int "3"
- !!int "4"
~~~
