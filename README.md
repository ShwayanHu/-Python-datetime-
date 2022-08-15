# 常用操作：Python 的 `datetime`库

* [常用操作：Python 的 datetime库](#常用操作python-的-datetime库)
   * [为什么要自己整理一遍datetime库？](#为什么要自己整理一遍datetime库)
   * [创建date类对象](#创建date类对象)
      * [日期比较的方法](#日期比较的方法)
      * [获取两个日期之间的差距](#获取两个日期之间的差距)
      * [日期的“漂亮打印”](#日期的漂亮打印)
   * [创建time类](#创建time类)
      * [时间比较的方法](#时间比较的方法)
      * [时间的“漂亮打印”](#时间的漂亮打印)
   * [根据字符串创建datetime对象](#根据字符串创建datetime对象)
   * [参考列表](#参考列表)

***为了避免plagiarism，所有引用均已在文末列出。***

___本笔记重点参考了[Python datetime模块详解、示例](https://steven.blog.csdn.net/article/details/64906245?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-64906245-blog-113701990.pc_relevant_multi_platform_featuressortv2removedup&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-64906245-blog-113701990.pc_relevant_multi_platform_featuressortv2removedup&utm_relevant_index=2)，特此声明。___





## 为什么要自己整理一遍`datetime`库？

日期类型数据对我来说非常难以处理，每次遇见它们都会非常头疼，但是往往很多时候又不得不处理它们（例如处理金融数据的时候）。在各种书籍中，作者一般默认大家掌握了datetime的使用方式从而不予介绍，或者只是浅浅地涉猎一点点皮毛，这对于我全面使用datetime库处理日期类型数据的学习是不利的。因此，在这里整理一些可能比较常用的datetime操作，加深记忆。



如果日后有非常重要的但未在此呈现的操作，请pull仓库或联系我或者等我慢慢补充;-)



## `datetime`有哪些类？

`datetime`中一共有6个类：

- `date`
- `time`
- `datetime`
- `Datetime_CAPI`
- `timedelta`
- `tzinfo`



其中，比较常用的是`date`、`time`、`datetime`、`timedelta`四个类。下面将对其进行逐个的整理。



## `date`类

### 创建`date`类对象

**`date`的中文叫日期，因此只能精确到“日”**

传入3个整数，分别代表年、月和日即可创建`date`类对象。

```python
import datetime
a = datetime.date(2022,8,15)
```

### `date`类的属性

`date`类对象有3个属性，分别是`year`，`month`和`day`，其可以获取对象的年、月和日字符串。

```python
print("year:{}\nmonth:{}\nday:{}".format(a.year,a.month,a.day))
# year:2022
# month:8
# day:15
```

### `date`类的方法

#### 日期比较的方法

1. `__eq__()`：相同
2. `__ge__()`：大于等于
3. `__gt__()`：大于
4. `__le__()`：小于等于
5. `__lt__()`：小于
6. `__ne__()`：不等

**“大于”指的是“晚于”，即日期更晚；“小于”指的是“早于”，即日期更早**。例如：

```python
import datetime
a = datetime.date(2022,8,15)
b = datetime.date(2022,7,1)
print(a.__gt__(b))
# True
```

#### 获取两个日期之间的差距

`__sub__()`和`__rsub__()`可以计算两个日期之间相差几天。区别是，运算的方向不同。

```python
import datetime
a = datetime.date(2022,8,15)
b = datetime.date(2022,7,1)

print("a-b:{}".format(a.__sub__(b)))
print("b-a:{}".format(a.__rsub__(b)))

# a-b:45 days, 0:00:00
# b-a:-45 days, 0:00:00
```

在金融中非常注意天数的计算方式。问题是，2022年8月1日和2022年8月5日之间到底相差3天、4天还是5天？

```python
import datetime
a = datetime.date(2022,8,5)
b = datetime.date(2022,8,1)

print("a-b:{}".format(a.__sub__(b)))
print("b-a:{}".format(a.__rsub__(b)))

# a-b:4 days, 0:00:00
# b-a:-4 days, 0:00:00
```

**`datetime`中的日期之差符合金融中“算头不算尾（或者随便你如何理解）”的一般规则。**

这里的函数返回值中为什么带了日期`0:00:00`？这是因为`__sub__()`和`__rsub__()`的返回值是`datetime.timedelta`类的对象。这会在稍后进行介绍。



不过，如果你需要获得相差的日期，也可以通过`.days`访问这一属性。

```python
import datetime
a = datetime.date(2022,8,5)
b = datetime.date(2022,8,1)

print("a-b:{}".format(a.__sub__(b).days))
print("b-a:{}".format(a.__rsub__(b).days))

# a-b:4
# b-a:-4
```



### 日期的“漂亮打印”

`__format__()`可以将`date`对象转化成为漂亮的字符串。这个函数接收一个参数，并且返回一个漂亮的字符串。

```python
import datetime
a = datetime.date(2022,8,5)

print(a.__format__('%Y-%m-%d'))
print(a.__format__('%Y/%m/%d'))
print(a.__format__('%y-%m-%d'))
print(a.__format__('%w'))

# 2022-08-05
# 2022/08/05
# 22-08-05
# 5
```



python中的所有的格式化符号见下图：

![python中的所有的格式化符号](https://pic.imgdb.cn/item/62fa193c16f2c2beb1beda35.jpg)



与此方法`__format__()`等价的另一个方法是`strftime()`方法。二者只是名字不同而已。



## `time`类

#### 创建`time`类

`time`类的创建最多可以接收5个参数：`hour`, `minute`, `second`, `microsecond`, 和`tzinfo`。

```python
import datetime

a = datetime.time(18,12,24)
print(a)
print(a.hour)
print(a.minute)
print(a.second)

# 18:12:24
# 18
# 12
# 24
```

#### `time`类的方法

##### 时间比较的方法

这与`date`类完全一样！好耶！

##### 时间的“漂亮打印”

这仍然与`date`类完全一样！

## `datetime`类

`datetime`类可以看成是`date`类和`time`类的集合体。

### `datetime`的拆分与组成

你可以依次向datetime的构造函数传入年月日时分秒等参数，从而创建`datetime`对象。由于`datetime`对象由两部分构成，你可以使用`date()`和`time()`分别得到二者的日期部分和时间部分。**需要注意的是，`date()`和`time()`得到的是类的实例，而非相应的字符串。**

```python
import datetime
a = datetime.datetime(2022,8,15,19,38,57.842182)
a = datetime.datetime.now()
print(a)
print("date:{}".format(a.date()))
print("time:{}".format(a.time()))

# 2022-08-15 19:38:57.842182
# date:<built-in method date of datetime.datetime object at 0x10041dc80>
# time:<built-in method time of datetime.datetime object at 0x10041dc80>
```



与“拆分”相对应地，你也可以使用`combine()`函数将`date`和`time`的对象合并成为一个`datetime`类的对象。

```python
import datetime

a = datetime.datetime.now()
print(a)
print("date:{}".format(a.date()))
print("time:{}".format(a.time()))
b = datetime.datetime.combine(a.date(),a.time())
print(b)
# 2022-08-15 19:45:09.226494
# date:2022-08-15
# time:19:45:09.226494
# 2022-08-15 19:45:09.226494
```



### 根据字符串创建`datetime`对象

***这可能是最常用的、最重要的函数之一。虽然它出现在最后，但是仍然希望你记住他。***

`strptime()`可以根据字符串创建`datetime`对象。它接收2个字符串参数，将第一个参数根据第二个参数的格式进行解读，并且创建对象。

```python
import datetime

a = datetime.datetime.strptime('2022-8-15 15:25','%Y-%m-%d %H:%M')
print(a)

# 2022-08-15 15:25:00
```



## 参考列表

1. [Python datetime模块详解、示例](https://steven.blog.csdn.net/article/details/64906245?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-64906245-blog-113701990.pc_relevant_multi_platform_featuressortv2removedup&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-64906245-blog-113701990.pc_relevant_multi_platform_featuressortv2removedup&utm_relevant_index=2)
2. [Python--时间日期格式化符号](https://blog.csdn.net/weixin_50648794/article/details/121018749)

