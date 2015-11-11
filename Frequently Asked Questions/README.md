# 常见问题

**Dictionaries without nested collections are not dumped correctly**

为什么
~~~ python
import yaml
document = """
   a: 01
   b:
      ba: 21
      bb: 22
"""
print yaml.dump(yaml.load(document))
~~~
运行结果竟然是：
~~~ bash
a: 1
b: {ba: 21, bb: 22}
~~~
这并不是嵌套映射的格式，但输出的结果确实是对的。

默认情况下，PyYAML 是根据文件中是否含有嵌套集合来选择集合格式的。如果集合中有嵌套集合，PyYAML 就会以 block（区块式）的方式来显示。否则就会采用 flow（流体？）式的格式显示。

如果您想让集合始终以区块式的方式呈现出来，那么就需要将 `dump()` 这个函数的 `default_flow_style` 参数设置成 `False` 了。看下面的例子：

~~~ python
# coding:utf-8
import yaml

document = """
a: 1
b:
   d: 2
   e: 3
"""

print yaml.dump(yaml.load(document))
print '将 flow_style 设置成 False 后的结果：'
print yaml.dump(yaml.load(document), default_flow_style=False)
~~~
运行结果：
~~~ bash
[root@yingyun python]# python noFlowStyle.py 
a: 1
b: {d: 2, e: 3}

将 flow_style 设置成 False 后的结果：
a: 1
b:
  d: 2
  e: 3

~~~
