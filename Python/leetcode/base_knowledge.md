# Python 基础知识点

## 基本方法

1. `len()`
   用于获取列表、字符串、字典、集合等可迭代对象的长度。

## 按数据类型

### 列表

1. `enumerate()`
   函数获取列表的索引和值
   `return (index, value)`

## 其他

1. `zip(iterable1, iterable2, ...)`
   将多个可迭代对象组合成一个可迭代对象。
   示例：

   ```python
   for i, j in zip(range(5), range(5)):
       print(i, j)     #依次输出：0 0 1 1 2 2 3 3 4 4
   ```

2. `Counter()`
   是一个字典的子类，用于统计一个可哈希对象中的元素出现的次数。
   相对于字典，它有比较好的性质：

   - 当键不存在时，`Counter`会返回 0，而字典会抛出`KeyError`。
   - **注意**：`Counter`的使用会导致更高的时间和空间复杂度，慎用。（2134 题）

3. `collections.defaultdict()`
   字典子类，可以通过工厂函数为缺失的键创建一个默认值。

   - `int`->`0`
   - `list`->`[]`
   - `set`->`set()`
   - `int`->`0`
   - `str`->`""`

4. `bin()`
   `bin`方法将一个整数转换为二进制字符串`"0bxxx"`，并返回。
   类似的用法：
   - `oct`
   - `hex`
   - `format(s,p)`，s 为字符串，p 为转换的模式（如：`'b'`、`'x'`等）
   - `int(s,p)`，s 为字符串，p 为转换的模式（如：`2`、`8`、`16`等）

## 按算法

### 滑动窗口

在滑动窗口的一般套路中，对于长度为`k`的窗口，一般采取先计算前`k-1`个值，然后通过循环遍历整个列表，每次移动窗口，并更新窗口内的值。

1. 如果前`k-1`个值，采取在循环外切片的方法，则须注意：要避免`k-1=-1`，

#### 代码示例

1. 1456.定长子串中元音的最大数目：使用`enumerate()`方法进行枚举。

   ```python
   def maxVowels(self, s: str, k: int) -> int:
      # 对于需要重复使用的变量，应提前定义
      ans = count = 0
      std_str = "aeiou"

      for idx, value in enumerate(s):
         # 窗口进元素
         if value in std_str:
            count += 1

         # 确保定长
         if idx < k - 1:
            continue

         # 更新
         ans = max(ans, count)

         # 窗口出元素
         if s[idx-k+1] in std_str:
            count -= 1

      return ans

   ```

   缺点：

   - `if idx < k - 1`大量执行非必要的条件判断也会增大开销。
   - `s[idx-k+1]`是一个寻址的过程，需要对数组进行遍历，这是一个耗时的操作。连续内存访问更高效。
   - 窗口只有`n-k`个，但循环却有`n`次，这是一种浪费。

2. 1423.可获得的最大点数
   错误代码：

   ```python
   class Solution:
      def maxScore(self, cardPoints: List[int], k: int) -> int:
         total = sum(cardPoints)
         not_get = sum(cardPoints[:len(cardPoints)-k-1]) # 错误代码，会计算出-1，从而留下整个数组。
         res = -inf
         for in_, out in zip(cardPoints[len(cardPoints)-k-1:], cardPoints):
            not_get += in_
            res = max(res, total - not_get)
            not_get -= out
         return res
   ```

   缺点：

   - 使用逆向思维，但未考虑取走所有元素的情况。

   正确代码：
   将第一次计算拿出来，更改逻辑，避免错误。

   ```python
   class Solution:
      def maxScore(self, cardPoints: List[int], k: int) -> int:
         total = sum(cardPoints)
         not_get = sum(cardPoints[:len(cardPoints)-k])
         res = total - not_get
         for in_, out in zip(cardPoints[len(cardPoints)-k:], cardPoints):
            not_get += in_ - out
            res = max(res, total - not_get)
         return res
   ```
