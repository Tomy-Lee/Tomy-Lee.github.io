---
layout:     post
title: Python模板之sys
subtitle:  sys模板常用函数功能整理
date:       2018-1-25
author:     "tomylee"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Python
---
今天在学习python时看到了sys模板，在这里整理和总结一下。sys模块提供了一系列有关Python运行环境的变量和函数。sys模板功能很多，我只列举一些常用的功能。

### sys模块的常见函数列表
#### （1）sys.argv
实现从程序外部向程序传递参数，可以用sys.argv获取当前正在执行的命令行参数的参数列表(list)，len(sys.argv)获取总的参数个数。
|变量|	解释|
|---|---|
|sys.argv[0]|	当前程序名|
|sys.argv[1]|	第一个参数|
|sys.argv[2]|	第二个参数|
|...|...|
示例代码：
```python
import sys

# 获取程序名字
print ('The name of this program is: ',sys.argv[0])
# 获取参数列表
print ('The command line arguments are:')
for i in sys.argv:
    print (i)
# 统计参数个数
print ('There are ',len(sys.argv)-1,'arguments')
```
![1](img/QQ截图20180125123524.png)

#### （2）sys.exit([arg])
执行到主程序末尾，解释器自动退出，但是如果需要中途退出程序，可以调用sys.exit函数，带有一个可选的整数参数返回给调用它的程序，表示你可以在主程序中捕获对sys.exit的调用。（0是正常退出，其他为异常）当参数非0时，会引发一个SystemExit异常，从而可以在主程序中捕获该异常。
```python
import sys

print ('------running------')

try:
    sys.exit(1)
except SystemExit:
    print ('SystemExit exit 1')

print ('exited')
```
![2](img/捕获.PNG)

#### （3）sys.platform
获取当前执行环境的平台，win32表示Windows 32bit操作系统，linux2表示linux平台；
```python
# linux 
>>> import sys
>>> sys.platform
'linux2'

# windows
>>> import sys
>>> sys.platform
'win32'
```
#### （4）sys.path
获取指定模块搜索路径的字符串集合，可以将写好的模块放在得到的某个路径下，就可以在程序中import时正确找到。path是一个目录列表，供Python从中查找第三方扩展模块。在python启动时，sys.path根据内建规则、PYTHONPATH变量进行初始化。
```python
>>> import sys
>>> sys.path
['', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\Lib\\idlelib', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\python34.zip', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\DLLs', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib\\site-packages', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib\\site-packages\\FontTools', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib\\site-packages\\win32', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib\\site-packages\\win32\\lib', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib\\site-packages\\Pythonwin']
```
我们还可以修改sys.path的内容，让python能找到自己定义的模块：
```python
# 在path的开始位置 插入test
>>> sys.path.insert(0,'test')
>>> sys.path
['test', '', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\Lib\\idlelib', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\python34.zip', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\DLLs', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib\\site-packages', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib\\site-packages\\FontTools', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib\\site-packages\\win32', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib\\site-packages\\win32\\lib', 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib\\site-packages\\Pythonwin']

# 可以成功import test
>>> import test

# 找不到 other 模块
>>> import other
Traceback (most recent call last):
  File "<pyshell#4>", line 1, in <module>
    import other
ImportError: No module named 'other'
# 添加path
>>> sys.path.insert(0,'other')
>>> import other
```
还可以用sys.path.append(“mine module path”)添加自定义的module。
#### （5）sys.builtin_module_names
sys.builtin_module_names返回一个列表，包含内建模块的名字。
```python
>>> import sys
>>> print (sys.builtin_module_names)
('_ast', '_bisect', '_codecs', '_codecs_cn', '_codecs_hk', '_codecs_iso2022', '_codecs_jp', '_codecs_kr', '_codecs_tw', '_collections', '_csv', '_datetime', '_functools', '_heapq', '_imp', '_io', '_json', '_locale', '_lsprof', '_md5', '_multibytecodec', '_opcode', '_operator', '_pickle', '_random', '_sha1', '_sha256', '_sha512', '_sre', '_stat', '_string', '_struct', '_symtable', '_thread', '_tracemalloc', '_warnings', '_weakref', '_winapi', 'array', 'atexit', 'audioop', 'binascii', 'builtins', 'cmath', 'errno', 'faulthandler', 'gc', 'itertools', 'marshal', 'math', 'mmap', 'msvcrt', 'nt', 'parser', 'signal', 'sys', 'time', 'winreg', 'xxsubtype', 'zipimport', 'zlib')
```
#### （6）sys.modules
sys.modules是一个全局字典，该字典是python启动后就加载在内存中。每当程序员导入新的模块，sys.modules将自动记录该模块。当第二次再导入该模块时，python会直接到字典中查找，从而加快了程序运行的速度。它拥有字典所拥有的一切方法。
```python
>>> print (sys.modules.keys())
dict_keys(['linecache', 'reprlib', '_imp', '_multibytecodec', 'ctypes._endian', 'codecs', 'string', 'marshal', 'os', 'importlib.machinery', 'idlelib.CallTips', 'idlelib.CallTipWindow', 'platform', 'operator', 'functools', '_locale', 'idlelib.SearchDialog', '_bz2', 'idlelib.RemoteObjectBrowser', 'select', '_hashlib', 'encodings.idna',......

>>> print (sys.modules.values())
dict_values([<module 'linecache' from 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib\\linecache.py'>, <module 'reprlib' from 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib\\reprlib.py'>, <module '_imp' (built-in)>, <module '_multibytecodec' (built-in)>, <module 'ctypes._endian' from 'D:\\WinPython-64bit-3.4.3.5\\python-3.4.3.amd64\\lib\\ctypes\\_endian.py'>, ......
```

#### （7）sys.stdin, sys.stdout, sys.stderr
stdin , stdout 以及stderr 变量包含与标准I/O 流对应的流对象。如果需要更好地控制输出,而print 不能满足要求, 它们就是你所需要的。也可以替换它们, 这时就可以重定向输出和输入到其它设备( device ), 或者以非标准的方式处理它们。

#### （8）sys.setdefaultencoding()
设置系统默认编码，执行dir（sys）时不会看到这个方法，在解释器中执行不通过，可以先执行reload(sys)，在执行 setdefaultencoding('utf8')，此时将系统默认编码设置为utf8。



