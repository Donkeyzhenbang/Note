# Python

**Py运算**

```python
#! /usr/bin/env python3
# -*- coding: utf-8 -*-

17 // 5 #整除
2 ** 3 #乘方2
-1 // 3 #结果是-1 py整除向下取整
_ #py交互中 _表示上一个值
```

fibo.py文件如下

```py
# Fibonacci numbers module

def fib(n):    # write Fibonacci series up to n
    a, b = 0, 1
    while a < n:
        print(a, end=' ')
        a, b = b, a+b
    print()

def fib2(n):   # return Fibonacci series up to n
    result = []
    a, b = 0, 1
    while a < n:
        result.append(a)
        a, b = b, a+b
    return result
```

**导入包**

```py
import fibo
from fibo import fib, fib2
import fibo as fib
from fibo import fib as fibonacci
```

**以脚本方式执行模块**
可以用以下方式运行 Python 模块：

```python
python fibo.py <arguments>
```

这项操作将执行模块里的代码，和导入模块一样，但会把 `__name__` 赋值为 `"__main__"`。 也就是把下列代码添加到模块末尾：

```py
if __name__ == "__main__":
    import sys
    fib(int(sys.argv[1]))
```

这个文件既能被用作脚本，又能被用作一个可供导入的模块，因为解析命令行参数的那两行代码只有在模块作为“main”文件执行时才会运行：

```py
$ python fibo.py 50
0 1 1 2 3 5 8 13 21 34
```

当这个模块被导入到其它模块时，那两行代码不运行：

```
>>> import fibo
```

**模块搜索路径**

当导入一个名为 `spam` 的模块时，解释器首先会搜索具有该名称的内置模块。 这些模块的名称在 [`sys.builtin_module_names`](https://docs.python.org/zh-cn/3/library/sys.html#sys.builtin_module_names) 中列出。 如果未找到，它将在变量 [`sys.path`](https://docs.python.org/zh-cn/3/library/sys.html#sys.path) 所给出的目录列表中搜索名为 `spam.py` 的文件。 [`sys.path`](https://docs.python.org/zh-cn/3/library/sys.html#sys.path) 是从这些位置初始化的:

- 被命令行直接运行的脚本所在的目录（或未指定文件时的当前目录）。
- [`PYTHONPATH`](https://docs.python.org/zh-cn/3/using/cmdline.html#envvar-PYTHONPATH) （目录列表，与 shell 变量 `PATH` 的语法一样）。
- 依赖于安装的默认值（按照惯例包括一个 `site-packages` 目录，由 [`site`](https://docs.python.org/zh-cn/3/library/site.html#module-site) 模块处理）。

# Django

