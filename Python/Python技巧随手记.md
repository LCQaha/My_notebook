# Python 技巧随手记

## 开发环境使用技巧

1. 为函数添加注释
   C 语言的注释一般添加在函数定义的上方，但在 Python 中，注释一般添加在函数定义的下方。
   遵照此规则添加的注释才能成功的被 vscode 等集成开发环境识别到，然后在鼠标悬停与对应函数时可以看到注释。

## 一些知识

### 关于字符串

1. `ord()`和`chr()`可以将字符串转换为对应的 Unicode 码（十进制），反之亦然。
2. 通过方法`str.encode()`可以将字符串编码为字节序列，通过`str.decode()`可以将字节序列解码为字符串。括号中应以字符串的形式输入编码格式。

3. 可以在 Python 文件前增加一些说明性注释，这样可以在特定环境（如 linux）下确保文件打开时的编码方式正确。

   ```python
   #! /usr/bin/env python3
   # -*- coding: utf-8 -*-

   """
   1.用于告诉Linux等系统，这是一个Python文件
   2.用于告诉Python解释器，使用UTF-8编码
   """
   ```

### 关于基本数据结构

1. 一个元组中如果只有一个元素，需要加一个逗号，如：`(1,)`，否则会被认为是一个整数。

### 关于函数

1. 在设置一个函数的参数默认值时，务必选择不可变对象（如`1`,`2`,`3`），不得使用`[]`等可变对象。
   ```python
      def func(a=[]):
         a.append(1)
         return a
   ```
   若使用默认值连续调用两次该函数，将返回`[1,1]`，这是因为上述函数直接对默认值`[]`进行了修改。
2. 可以通过在一个列表（或其他可迭代对象）前加`*`将整个对象里的元素作为可变参数批量传入函数。

   ```python
   def func(*args):
      return sum([i**2 for i in args])


   numbers = [1,2,3]
   print(func(*numbers))
   print(func(numbers[0],numbers[1],numbers[2]))
   ```

   同样的，也可以在一个字典前加两个`*`，传入大量可变关键字参数。

3. 可以通过下列方式限定可变关键字参数名称。

   ```python
   def func(a,b,*,c):
      pass

   ```

   这个例子中，`c`只能通过关键字的形式传入，如果`c`没有默认值，则必须传入一个值，否则函数无法工作。

4. 在闭包中，如果内层函数用了外层的变量，则内层函数必须用`nonlocal`关键字声明，否则会报错。
5. 装饰器

   ```python
   """
   一个完整的装饰器定义如下所示：
   这里展示一个输出函数名称的装饰器
   """

   import functools

   def log(text: str) -> Callable:                    # 定义一个装饰器，可输入参数（限定为str类型），限定返回值为一个函数
      def decorator(func: Callable) -> Callable:      # 定义一个内部函数，可输入参数为函数，限定返回值为一个函数
         @functools.wraps(func)                       # 将内部函数的元数据复制到外部函数上
         def wrapper(*args, **kwargs):                # 执行装饰器具体操作的函数
            print(text)
            print(func.__name__)
            return func(*args, **kwargs)
         return wrapper
      return decorator

   ```

6. `functools.partial`实现偏函数
   我们平常使用函数，都是在函数中设定默认值，但总有一些特殊环境，需要将默认值修改成固定的值（不改变函数定义），这时只需要将函数名和需要设定的参数传入即可。
   若没有设定默认参数，而是仅传入值，则会被认成位置参数，在函数使用时，传入所有数据前，先将这个值传入。

   ```python
   fun1=functools.partial(max,10)
   print(fun1(1,2)) # 输出：10

   fun2=functools.partial(int,base=2)
   print(fun2('101')) # 输出：5
   ```

7. 获取对象信息
   对于一个对象，可以使用`type()`函数来获取对象的类型，使用`insistance()`判断对象是否属于某种类型，使用`dir()`函数来获取对象的所有属性和方法。

   使用`hasattr()`函数来判断对象是否含有某个属性，使用`getattr()`函数来获取对象的某个属性，使用`setattr()`函数来设置对象的某个属性。

### 拉起错误

1. 在函数中的恰当位置使用 `raise` 语句，抛出异常。

   ```python
   def my_abs(x):
      if not isinstance(x, (int, float)):
         raise TypeError('bad operand type')
      if x >= 0:
         return x
      else:
         return -x

   ```

   - `isinstance()` 函数用于判断一个对象的类型，这里用于判断是否为数字。
   - `TypeError` 是一个内置的异常类型，用于表示类型错误。

### 关于`sys`

1. `sys.argv`
   用于存储程序在运行时产生的数据。

   ```python

   '''
   假设这个文件叫 test.py
   '''
   import sys

   print(sys.argv)  # 输出['test.py']

   __author__ = 'LCQ'

   print(sys.argv) # 输出['test.py', 'LCQ']
   ```

### 关于类(class)

1. 在类中定义一个名为`__slot__`的元组，可以限制添加的参数类型。如果出现了不属于该元组的额外参数，则会报错。但这个特性对继承的子类无效。
2. 在类中，有时我们并不想让实例可以直接修改某些参数，比如录入学生成绩时，满分 100，你录进去一个 107，显然是不对的，这时就要通过一些限制手段来纠正错误。

   ```python
   class Student():

      def __init__(self,score):
         Student._score=score

      def change_score(self,score):
         if not isinstance(score,int):
            raise ValueError('score must be an integer!')
         elif score>100 or score<0:
            raise ValueError('score must between 0 ~ 100!')
         else:
            self._score=score

      def get_score(self):
         return self._score
   ```

   这样就有了一个简单的防止错误信息流入的机制，当然，这种调用函数的方式并不是最好的方法，还可以做一些修改：

   ```python
   class Student():

      def __init__(self,score):
         Student._score=score

      @property
      def score(self,score):
         if not isinstance(score,int):
            raise ValueError('score must be an integer!')
         elif score>100 or score<0:
            raise ValueError('score must between 0 ~ 100!')
         else:
            self._score=score

      @score.setter
      def score(self):
         return self._score
   ```

   这样就可以直接读取和修改目标参数，但又增加了限制。

3. `MixIn`
   有些类是拿来用的，有些类则是专门用来为其他类增加功能的，建议将后者加上`MixIn`以便区分。
   比如狗是地上跑的哺乳动物，蝙蝠是飞的哺乳动物。
   我们就可以定义一个`RunningMixIn()`类，用`Dog()`和`Bat()`去继承他。

4. 定制类
   在类定义中，有很多特殊的实例方法。比如：`__len__`可以设置对象使用`len()`方法时的返回值，`__str__`可以设置对象在`print()`时的值。
   下面列举几个常用的特殊方法：

   - `__str__`：返回值是类的说明字符串，可通过`print`打印出来
   - `__iter__`：返回一个可迭代对象（一般是 self 也可以是别的类，但目标类中必须包含`__next__`），python 通过不断调用`__next__`来回去迭代值。
   - `__next__`：返回下一个迭代值，当遇到`raise StopIteration()`时停止迭代。
   - `__getitem__`：包含参数`n`，用于返回实例的第`n`个位置的数据。
   - `__getattr__`：包含一个参数`attr`，用于在 Python 调用不存在的参数时返回一些内容。

     ```python
     '例子：一个链式调用API方法'
     """
     >>>Chain().status.user.timeline.list
     'Chain/status/user/timeline/list'
     """

     class Chain(object):
        def __init__(self, path=''):
           self._path = path


        def **getattr**(self, path):
           return Chain('%s/%s' % (self._path, path))

        def __str__(self):
           return self._path

        __repr__ = __str__

     ```

   - `__call__`：在创建的实例后加上一对括号，就会执行这个方法。

5. 枚举类

   ```python
   from enum import Enum

   Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
   ```
