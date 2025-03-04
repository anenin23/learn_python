10. Brief Tour



## import 语句的两种风格

在Python中，有两种主要的导入模块方式：

1. `import os` - 完整导入模式
2. `from os import *` - 通配符导入模式

### 为什么推荐 `import os` 而非 `from os import *`？

这段教程建议使用 `import os` 风格而不是 `from os import *`，主要是为了避免**命名冲突**问题，特别是防止 `os.open()` 函数覆盖（shadowing）内置的 `open()` 函数。

### 命名冲突问题的示意图

以下是一个示意图来帮助你理解导入方式的不同：

```
【import os】方式:                    【from os import *】方式:
                                    
命名空间是分离的                      所有函数直接进入当前命名空间
                                    
当前命名空间      os命名空间           当前命名空间
+-----------+  +------------+      +------------------+
|           |  |            |      |                  |
| open() ←--|--| os.open()  |      | open()  ← os模块的open()覆盖了内置函数!|
|           |  |            |      |                  |
+-----------+  +------------+      +------------------+
```

### 实际例子

让我们通过具体例子来看这两种导入方式的区别：

```python
# 推荐方式：import os
import os

# 使用内置的open()函数打开文件
file = open('test.txt', 'w')  # 这是Python内置的open()函数
file.write('Hello')
file.close()

# 如果需要使用os模块的open函数，明确使用命名空间
fd = os.open('test2.txt', os.O_WRONLY | os.O_CREAT)  # 这是os模块的open()函数
os.write(fd, b'Hello')
os.close(fd)
```

相比之下，如果使用 `from os import *`：

```python
# 不推荐方式：from os import *
from os import *

# 此时当前命名空间中的open()是os模块的open()，而不是内置的open()
# 这会导致混淆，因为两个函数用法完全不同
```

### 为什么会有问题？

`os.open()` 和内置的 `open()` 函数虽然名字相似，但功能和用法截然不同：

1. **内置 `open()`**：
   - 返回一个文件对象
   - 使用方便的高级接口（如 `read()`, `write()` 方法）
   - 例如: `file = open('file.txt', 'r')`
2. **`os.open()`**：
   - 返回一个文件描述符（整数）
   - 是底层系统调用的封装，需要与 `os.read()`, `os.write()`, `os.close()` 一起使用
   - 例如: `fd = os.open('file.txt', os.O_RDONLY)`

### 最佳实践

1. 优先使用明确的导入: `import os`
2. 如果需要从模块导入特定函数，指明具体函数: `from os import path, listdir`
3. 避免使用通配符导入: `from os import *`

这样的编码习惯会让你的代码更加清晰，避免不必要的命名冲突，特别是在大型项目中。

希望这个解释对你有所帮助！如果你有任何其他Python学习上的问题，请随时提问。