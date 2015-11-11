# 安装

1. 下载源码包 **PyYAML-3.08.tar.gz**，并解压。

   ~~~ bash   
   $ wget http://pyyaml.org/download/pyyaml/PyYAML-3.08.tar.gz
   $ tar -zxvf PyYAML-3.08.tar.gz   
   ~~~

2. 进入到 **PyYAML-3.08** 目录中
   
   ~~~ bash
   cd PyYAML-3.08
   ~~~

3. 然后运行

   ~~~ bash
   $ sudo python setup.py install
   [helen@yingyun PyYAML-3.08]$ sudo python setup.py install
   [sudo] password for helen: 
   running install
   running build
   running build_py
   running build_ext
   running install_lib
   running install_egg_info
   Writing /usr/lib64/python2.7/site-packages/PyYAML-3.08-py2.7.egg-info
   ~~~
   
   > **提醒：**
   > 如果您遇到如下错误，是因为您没有安装 `python-devel`。
     ~~~ bash
     ext/_yaml.c:4:20: 致命错误：Python.h：没有那个文件或目录
     ~~~

     
   > **注意**
   > 如果您想邦定 LibYAML（比纯 Python 版本要块的多）的话，您需要下载并安装 `LibYAML`。然后通过执行来邦定。
     ~~~ bash
     $ wget http://pyyaml.org/download/libyaml/yaml-0.1.5.tar.gz
     $ tar -zxvf yaml-0.1.5.tar.gz
     $ make yaml-0.1.5
     $ ./configure
     $ make
     # make install
     $ cd ../PyYAML-3.08
     $ python setup.py --with-libyaml install
     ~~~
 
   为了使用基于 parser（解析器） 和 emitter（发射器？）的 [LibYAML](http://pyyaml.org/wiki/LibYAML)，需要用到 **CParser** 与 **CEmitter**。例如，

   ~~~ python
   from yaml import load, dump
   try:
       from yaml import CLoader as Loader, CDumper as Dumper
   except ImportError:
       from yaml import Loader, Dumper
   
   # ...
   
   data = load(stream, Loader=Loader)
    
   # ...
   
   output = dump(data, Dumper=Dumper)
   ~~~
   
   ---
   
   我自己也做了试验：
   版本：

   ~~~ bash
   [helen@yingyun python]$ rpm -qa | grep yaml
   libyaml-0.1.4-11.el7_0.x86_64
   [helen@yingyun python]$ rpm -qa | grep -i pyyaml
   PyYAML-3.10-11.el7.x86_64
   ~~~

   ~~~ yaml
   # 创建 test.yaml 文件
   name: 张亚琪
   anotherName: 张瀛匀
   age: 24
   hobby:
      - swimming
      - movie    
   pet:
      - name: blue cat
      - age: 2
   ~~~
   
   ~~~ python
   # 创建 test.py 文件
   from yaml import load, load_all, dump
   f = open('test.yaml')
   data = load(f)
   print data
   print '------------------'
   f = open('test.yaml')
   for Data in load(f):
      print Data
   print '------------------'
   stream = file('test.yaml', 'r')
   for data in load(stream):
      print data
   print '------------------'
   stream = file('test.yaml', 'r')
   for data in load_all(stream):
      print data
   ~~~
   ~~~ bash
   # 执行结果
   [root@yingyun python]# python test001.py 
   {'pet': [{'name': 'blue cat'}, {'age': 2}], 'hobby': ['swimming', 'movie'], 'age': 24, 'anotherName': u'\u5f20\u701b\u5300', 'name': u'\u5f20\u4e9a\u742a'}
   ------------------
   pet
   hobby
   age
   anotherName
   name
   ------------------
   pet
   hobby
   age
   anotherName
   name
   ------------------
   {'pet': [{'name': 'blue cat'}, {'age': 2}], 'hobby': ['swimming', 'movie'], 'age': 24, 'anotherName': u'\u5f20\u701b\u5300', 'name': u'\u5f20\u4e9a\u742a'}
   ~~~
  
   > **Error**
   > 这里还有个问题没有解决：
     ~~~ bash
     # 如果 test.py 的导入部分写成了下面这样，就会报出错误：
     from yaml import load, dump
     try:
         from yaml import CLoader as Loader, CDumper as Dumper
     except ImportError:
         from yaml import Loader, Dumper
     # for 循环写成：
     for data in load(stream, Loader=Loader):
     ~~~
     错误如下：
     ~~~ bash
     Traceback (most recent call last):
       File "test001.py", line 12, in <module>
         for data in load(stream, Loader=Loader):
       File "/usr/lib64/python2.7/site-packages/yaml/__init__.py", line 73, in load
         loader.dispose()
     AttributeError: 'CLoader' object has no attribute 'dispose'
     ~~~

LibYAML is a YAML 1.1 parser and emitter written in C.

> **注意**
  纯 Python 程序和基于 parser 和 emitter 的 [LibYAML](http://pyyaml.org/wiki/LibYAML) 会有些许不同之处，但无关紧要。
