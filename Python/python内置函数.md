# python内置函数

Python 解释器内置了很多函数和类型，您可以在任何时候使用它们。以下按字母表顺序列出它们。

|||内置函数|||
|-|-|-|-|-|
abs()|delattr()|hash()|memoryview()|set()
all()|dict()|help()|min()|setattr()
any()|dir()|hex()|next()|slice()
ascii()|divmod()|id()|object()|sorted()
bin()|enumerate()|input()|oct()|staticmethod()
bool()|eval()|int()|open()|str()
breakpoint()|exec()|isinstance()|ord()|sum()
bytearray()|filter()|issubclass()|pow()|super()
bytes()|float()|iter()|print()|tuple()
callable()|format()|len()|property()|type()
chr()|frozenset()|list()|range()|vars()
classmethod()|getattr()|locals()|repr()|zip()
compile()|globals()|map()|reversed()|\_\_import\_\_()
complex()|hasattr()|max()|round()

+ abs(x)

返回一个数的绝对值。 参数可以是一个整数或浮点数。 如果参数是一个复数，则返回它的模。 如果 x 定义了 \_\_abs\_\_()，则 abs(x) 将返回 x.\_\_abs\_\_()。

+ all(iterable)

如果 iterable 的所有元素均为真值（或可迭代对象为空）则返回 True 。

+ any(iterable)

如果 iterable 的任一元素为真值则返回 True。 如果可迭代对象为空，返回 False 。

+ ascii(object)

就像函数 repr()，返回一个对象可打印的字符串，但是 repr() 返回的字符串中非 ASCII 编码的字符，会使用 \x、\u 和 \U 来转义。生成的字符串和 Python 2 的 repr() 返回的结果相似。

```python
print(ascii('￥'), repr('￥'))

# 输出：'\uffe5' '￥'
```

+ bin(x)

将一个整数转变为一个前缀为“0b”的二进制字符串。结果是一个合法的 Python 表达式。如果 x 不是 Python 的 int 对象，那它需要定义 \_\_index\_\_() 方法返回一个整数。

```python
print(bin(111), bin(-111))

# 输出：0b1101111 -0b1101111
```

+ class bool(x)

返回一个布尔值，True 或者 False。 x 使用标准的 真值测试过程 来转换。

+ breakpoint(*args, **kws)

此函数将你置于调用站点的调试器中。具体来说，它调用 sys.breakpointhook() ，直接传递 args 和 kws 。默认情况下， sys.breakpointhook() 调用 pdb.set_trace() 且没有参数。在这种情况下，它纯粹是一个便利函数，因此您不必显式导入 pdb 且键入尽可能少的代码即可进入调试器。但是， sys.breakpointhook() 可以设置为其他一些函数并被 breakpoint() 自动调用，以允许进入你想用的调试器。

3.7 新版功能.

+ class bytearray(source=None, encoding=None, errors='strict')

返回一个新的 bytes 数组。 bytearray 类是一个可变序列，包含范围为 0 <= x < 256 的整数。

+ class bytes(value=b'', encoding=None, errors='strict')

返回一个新的“bytes”对象， 是一个不可变序列，包含范围为 0 <= x < 256 的整数。

+ callable(object)

如果参数 object 是可调用的就返回 True，否则返回 False。

+ chr(i)

返回 Unicode 码位为整数 i 的字符的字符串格式。例如，chr(97) 返回字符串 'a'

+ @classmethod

把一个方法封装成类方法。

+ compile(source, filename, mode, flags=0, dont_inherit=False, optimize=-1)

将 source 编译成代码或 AST 对象。代码对象可以被 exec() 或 eval() 执行。source 可以是常规的字符串、字节字符串，或者 AST 对象。

+ class complex()

返回值为 real + imag*1j 的复数，或将字符串或数字转换为复数。

+ delattr(object, name)

实参是一个对象和一个字符串。该字符串必须是对象的某个属性。如果对象允许，该函数将删除指定的属性。

delattr(x, 'foobar') 等价于 del x.foobar

+ class dict()

创建一个新的字典。dict 对象是一个字典类

+ dir()

如果没有实参，则返回当前本地作用域中的名称列表。如果有实参，它会尝试返回该对象的有效属性列表。

如果对象有一个名为 \_\_dir\_\_() 的方法，那么该方法将被调用，并且必须返回一个属性列表。这允许实现自定义 \_\_getattr\_\_() 或 \_\_getattribute\_\_() 函数的对象能够自定义 dir() 来报告它们的属性。

+ divmod(a, b)

它将两个（非复数）数字作为实参，并在执行整数除法时返回一对商和余数。

+ enumerate(iterable, start=0)

返回一个枚举对象。iterable 必须是一个序列，或 iterator，或其他支持迭代的对象。 enumerate() 返回的迭代器的 \_\_next\_\_() 方法返回一个元组，里面包含一个计数值（从 start 开始，默认为 0）和通过迭代 iterable 获得的值。

+ eval()

实参是一个字符串，以及可选的 globals 和 locals。globals 实参必须是一个字典。locals 可以是任何映射对象。

expression 参数会作为一个 Python 表达式（从技术上说是一个条件列表）被解析并求值，并使用 globals 和 locals 字典作为全局和局部命名空间。

+ exec()

这个函数支持动态执行 Python 代码。object 必须是字符串或者代码对象。

+ filter(function, iterable)

用 iterable 中函数 function 返回真的那些元素，构建一个新的迭代器。

+ class float(x)

返回从数字或字符串 x 生成的浮点数。

+ format(value, format_spec)

返回 value.\_\_format\_\_(format_spec)

+ class frozenset(iterable)

返回一个新的 frozenset 对象，它包含可选参数 iterable 中的元素。

+ getattr(object, name, default=None)

返回对象命名属性的值。name 必须是字符串。

+ globals()

返回表示当前全局符号表的字典。

+ hasattr(object, name)

该实参是一个对象和一个字符串。如果字符串是对象的属性之一的名称，则返回 True，否则返回 False。

+ hash(object)

返回该对象的哈希值（如果它有的话）。

+ help()

启动内置的帮助系统

+ hex(x)

将整数转换为以“0x”为前缀的小写十六进制字符串。如果 x 不是 Python int 对象，则必须定义返回整数的 \_\_index\_\_() 方法。

+ id(object)

返回对象的“标识值”

+ input([prompt])

如果存在 prompt 实参，则将其写入标准输出，末尾不带换行符。接下来，该函数从输入中读取一行，将其转换为字符串（除了末尾的换行符）并返回。当读取到 EOF 时，则触发 EOFError。

+ class int(x)

返回一个基于数字或字符串 x 构造的整数对象，或者在未给出参数时返回 0。如果 x 定义了 \_\_int\_\_()，int(x) 将返回 x.\_\_int\_\_()。 如果 x 定义了 \_\_index\_\_()，它将返回 x.\_\_index\_\_()。 如果 x 定义了 \_\_trunc\_\_()，它将返回 x.\_\_trunc\_\_()。 对于浮点数，它将向零舍入。

+ isinstance(object, classinfo)

如果参数 object 是参数 classinfo 的实例或者是其 (直接、间接或 虚拟) 子类则返回 True。 如果 object 不是给定类型的对象，函数将总是返回 False。

+ issubclass(class, classinfo)

如果 class 是 classinfo 的 (直接、间接或 虚拟) 子类则返回 True。

+ iter(object)

返回一个 iterator 对象。

+ len(s)

返回对象的长度（元素个数）

+ class list()

返回一个列表

+ locals()

更新并返回表示当前本地符号表的字典。

+ map(function, iterable, ...)

返回一个将 function 应用于 iterable 中每一项并输出其结果的迭代器。

+ max()

返回可迭代对象中最大的元素，或者返回两个及以上实参中最大的。

+ class memoryview(obj)

返回由给定实参创建的“内存视图”对象

+ min()

返回可迭代对象中最小的元素，或者返回两个及以上实参中最小的。

+ next(iterator)

通过调用 iterator 的 \_\_next\_\_() 方法获取下一个元素。如果迭代器耗尽，则返回给定的 default，如果没有默认值则触发 StopIteration

+ class object()

返回一个没有特征的新对象。

+ oct(x)

将一个整数转变为一个前缀为“0o”的八进制字符串。

+ open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)

打开 file 并返回对应的 file object。如果该文件不能打开，则触发 OSError。

|字符|含义|
|-|-|
'r'|读取（默认）
'w'|写入，并先截断文件
'x'|排它性创建，如果文件已存在则失败
'a'|写入，如果文件存在则在末尾追加
'b'|二进制模式
't'|文本模式（默认）
'+'|打开用于更新（读取与写入）

+ ord(c)

对表示单个 Unicode 字符的字符串，返回代表它 Unicode 码点的整数。

+ pow(base, exp[, mod])

返回 base 的 exp 次幂；如果 mod 存在，则返回 base 的 exp 次幂对 mod 取余

+ print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)

将 objects 打印到 file 指定的文本流，以 sep 分隔并在末尾加上 end。 sep, end, file 和 flush 如果存在，它们必须以关键字参数的形式给出。

+ class property(fget=None, fset=None, fdel=None, doc=None)

返回 property 属性。

+ class range(start, stop[, step])

返回一个range对象

+ repr(object)

返回包含一个对象的可打印表示形式的字符串。

+ reversed(seq)

返回一个反向的 iterator。 seq 必须是一个具有 \_\_reversed\_\_() 方法的对象或者是支持该序列协议（具有从 0 开始的整数类型参数的 \_\_len\_\_() 方法和 \_\_getitem\_\_() 方法）。

+ round(number[, ndigits])

返回 number 舍入到小数点后 ndigits 位精度的值。 如果 ndigits 被省略或为 None，则返回最接近输入值的整数。

+ class set([iterable])

返回一个新的 set 对象

+ setattr(object, name, value)

此函数与 getattr() 两相对应。 其参数为一个对象、一个字符串和一个任意值。 字符串指定一个现有属性或者新增属性。 函数会将值赋给该属性，只要对象允许这种操作。 例如，setattr(x, 'foobar', 123) 等价于 x.foobar = 123。

+ class slice(start, stop[, step])

返回一个表示由 range(start, stop, step) 所指定索引集的 slice 对象。 其中 start 和 step 参数默认为 None。

+ sorted(iterable, *, key=None, reverse=False)

根据 iterable 中的项返回一个新的已排序列表。

+ @staticmethod

将方法转换为静态方法。

+ class str(object=b'', encoding='utf-8', errors='strict')

返回一个 str 版本的 object

+ sum(iterable, /, start=0)

从 start 开始自左向右对 iterable 的项求和并返回总计值。 iterable 的项通常为数字，而 start 值则不允许为字符串。

+ super([type[, object-or-type]])
返回一个代理对象，它会将方法调用委托给 type 的父类或兄弟类。 这对于访问已在类中被重载的继承方法很有用。

+ class tuple([iterable])

返回一个元组

+ class type(object)

传入一个参数时，返回 object 的类型。 返回值是一个 type 对象，通常与 object.\_\_class\_\_ 所返回的对象相同。

推荐使用 isinstance() 内置函数来检测对象的类型，因为它会考虑子类的情况。

+ vars([object])

返回模块、类、实例或任何其它具有 \_\_dict\_\_ 属性的对象的 \_\_dict\_\_ 属性。

+ zip(*iterables)

创建一个聚合了来自每个可迭代对象中的元素的迭代器。

+ \_\_import\_\_(name, globals=None, locals=None, fromlist=(), level=0)

此函数会由 import 语句发起调用。
