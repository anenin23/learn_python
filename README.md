# learn_python

2025-02-28

The execution of a function introduces a new symbol table used for the local variables of the function. More precisely, all variable assignments in a function store the value in the local symbol table; whereas variable references first look in the local symbol table, then in the local symbol tables of enclosing functions, then in the global symbol table, and finally in the table of built-in names. Thus, global variables and variables of enclosing functions cannot be directly assigned a value within a function (unless, for global variables, named in a global statement, or, for variables of enclosing functions, named in a nonlocal statement), although they may be referenced.

这段内容讲的是Python中函数的变量作用域规则：

1. **函数的符号表（Symbol Table）**：当一个函数被执行时，Python会创建一个新的符号表，用于存储该函数的局部变量。
2. **变量查找顺序**：当在函数中引用一个变量时，Python会按照以下顺序查找:
   - 先在函数的局部符号表中查找
   - 然后在外层函数的局部符号表中查找（如果有嵌套函数）
   - 接着在全局符号表中查找
   - 最后在内置名称表中查找
3. **变量赋值规则**：在函数内部进行变量赋值时，默认情况下值会存储在局部符号表中。这意味着在函数内部创建的变量默认是局部变量。
4. **访问非局部变量**：函数内部不能直接对全局变量或外层函数的变量进行赋值（除非使用特殊声明）。但可以引用（读取）这些变量。
5. **特殊声明**：
   - 使用`global`关键字可以在函数内部修改全局变量
   - 使用`nonlocal`关键字可以在嵌套函数中修改外层函数的变量

你对这部分内容有什么具体问题吗？或者需要一些例子来更好地理解这些概念？

---

```
def ask_ok(prompt, retries=4, reminder='Please try again!'):
    while True:
        reply = input(prompt)
        if reply in {'y', 'ye', 'yes'}:
            return True
        if reply in {'n', 'no', 'nop', 'nope'}:
            return False
        retries = retries - 1
        if retries < 0:
            raise ValueError('invalid user response')
        print(reminder)
```

这段代码中的 `retries` 参数是用来控制用户有多少次尝试输入有效回答的机会。我来解释一下：

在函数定义 `def ask_ok(prompt, retries=4, reminder='Please try again!'):` 中，`retries=4` 是一个默认参数值，表示如果调用函数时没有指定这个参数，它会自动使用值 4。

所以在不同的调用方式中：

1. `ask_ok('Do you really want to quit?')` - 这里只提供了必须的 `prompt` 参数，所以 `retries` 会使用默认值 4，意味着用户有 4 次机会输入有效回答。
2. `ask_ok('OK to overwrite the file?', 2)` - 这里指定了 `retries` 参数为 2，覆盖了默认值 4，所以用户只有 2 次机会输入有效回答。
3. `ask_ok('OK to overwrite the file?', 2, 'Come on, only yes or no!')` - 这里不仅指定了 `retries` 为 2，还自定义了提醒消息为 "Come on, only yes or no!"，这会替代默认的 "Please try again!" 提醒。

函数的工作逻辑是：

- 每次用户输入无效回答（既不是表示"是"也不是表示"否"的回答）时，`retries` 会减 1
- 如果 `retries` 变为小于 0，函数会抛出 `ValueError` 异常
- 否则，会显示 `reminder` 消息并继续请求用户输入

所以参数 2 就是告诉函数用户只有 2 次尝试的机会，而不是默认的 4 次。



---

