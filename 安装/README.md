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
   
