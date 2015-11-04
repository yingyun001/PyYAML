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

~~~ python

~~~
