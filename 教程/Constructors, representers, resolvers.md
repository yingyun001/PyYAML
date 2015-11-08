# Constructors, representers, resolvers

您可以自定义一个具有特殊用途的标签。最简便的方法就是定义一个 **`yaml.YAMLObject** 的子类：

~~~ python
mport yaml

class Stu(yaml.YAMLObject):
   yaml_tag = u'!Stu'
   def __init__(self, name, score):
      self.name = name
      self.score = score
   def __repr__(self):
      return '%s(name(repr) = %r, name(str) = %s, score = %r)' % (self.__class__.__name__, self.name, self.name, self.score)

data = yaml.load("""
          !Stu
          name: helen
          score: 100
       """)

print data
~~~

~~~ bash
# 运行结果
[zhangyaqi@localhost PyYAML]$ python tag01.py 
Stu(name(repr) = 'helen', name(str) = helen, score = 100)
~~~

**`yaml.YAMLObject`** 使用了 metaclass 来定义构造方法，这个构造方法将 YMAL 文件转换成一个类的实例。除了定义构造方法，还定义了表示器，该表示器可以将一个类的实例转换为一个 YAML 文件。


