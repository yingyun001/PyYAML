# 对 Python 3 的支持

从版本 3.08 开始，PyYAML 和 LibYAML 邦定就提供了对 Python 3 的支持。Python 2 和 Python 3 在 PyYAML 方面还是有一定的区别的。

* Python 2
  * `str` 被转换成了 `!!str`，`!!python/str` 或 `!binary`，最终是哪一个取决于该字符串对象的编码方式是 ASCII，UTF-8 还是二进制字符串。
  * `unicode` 对象被转换成了 `!!python/unicode` 或 `!!str` 节点，最终是哪一个取决于该对象是否是 ASCII 字符串。
  * yaml.dump(data) 将文档输出为以 UTF-8 为编码格式的 `str` 对象。
  * yaml.dump(data, encoding=('utf-8'|'utf-16-be'|'utf-16-le')) 生成了有具体编码格式的 `str` 对象。
  * yaml.dump(data, encoding=None) 生成了一个 `unicode` 对象。

* Python 3
  * `str` 对象被转换成了 `!!str` 节点。
  * `bytes` 对象对转换成了 `!!binary` 节点。
  * 出于兼容性的考虑，PyYAML 仍然支持 `!!python/str` 和 `!python/unicode` 标签，相应的节点被转换成了 `str` 对象。
  * yaml.dump(data) 将文档输出为一个 `str` 对象。
  * yaml.dump(data, encoding=('utf-8'|'utf-16-be'|'utf-16-le')) 生成了有具体编码格式的 `bytes` 对象。

--- 

Main：这里面的 node 指的是啥？没有看懂！

---
