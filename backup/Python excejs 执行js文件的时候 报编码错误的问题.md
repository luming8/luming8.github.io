# Python excejs 执行js文件的时候 报编码错误的问题
##执行js的时候报图中的编码错误，直接执行js文件时能正常编译，在网上未找到关于这个问题的文章 头疼了好久 最终在各位大佬的帮助下解决了问题，便记录了下来：

![alt text](https://img2023.cnblogs.com/blog/2367790/202305/2367790-20230530150838227-688705430.png)


## 解决办法：

一、修改报错文件 subprocess.py 中的 encoding 编码： encoding=None ---> encoding='utf-8'

![alt text](https://img2023.cnblogs.com/blog/2367790/202305/2367790-20230530150802921-1981061229.png)

二 、在引包的时候直接修改encoding得值，使用方便 不用修改 源代码

```python
  import subprocess
  from functools import partial
  #处理execjs编码报错问题, 需在 import execjs之前
  subprocess.Popen = partial(subprocess.Popen, encoding="utf-8")
  import execjs
```