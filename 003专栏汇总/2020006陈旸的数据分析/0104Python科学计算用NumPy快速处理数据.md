# 0104Python 科学计算：用 NumPy 快速处理数据

陈旸 2018-12-21




12:03


讲述：陈旸 大小：11.05M

上一节我讲了 Python 的基本语法，今天我来给你讲下 Python 中一个非常重要的第三方库 NumPy。

它不仅是 Python 中使用最多的第三方库，而且还是 SciPy、Pandas 等数据科学的基础库。它所提供的数据结构比 Python 自身的「更高级、更高效」，可以这么说，NumPy 所提供的数据结构是 Python 数据分析的基础。

我上次讲到了 Python 数组结构中的列表 list，它实际上相当于一个数组的结构。而 NumPy 中一个关键数据类型就是关于数组的，那为什么还存在这样一个第三方的数组结构呢？

实际上，标准的 Python 中，用列表 list 保存数组的数值。由于列表中的元素可以是任意的对象，所以列表中 list 保存的是对象的指针。虽然在 Python 编程中隐去了指针的概念，但是数组有指针，Python 的列表 list 其实就是数组。这样如果我要保存一个简单的数组 [0,1,2]，就需要有 3 个指针和 3 个整数的对象，这样对于 Python 来说是非常不经济的，浪费了内存和计算时间。

使用 NumPy 让你的 Python 科学计算更高效

为什么要用 NumPy 数组结构而不是 Python 本身的列表 list？这是因为列表 list 的元素在系统内存中是分散存储的，而 NumPy 数组存储在一个均匀连续的内存块中。这样数组计算遍历所有的元素，不像列表 list 还需要对内存地址进行查找，从而节省了计算资源。

另外在内存访问模式中，缓存会直接把字节块从 RAM 加载到 CPU 寄存器中。因为数据连续的存储在内存中，NumPy 直接利用现代 CPU 的矢量化指令计算，加载寄存器中的多个连续浮点数。另外 NumPy 中的矩阵计算可以采用多线程的方式，充分利用多核 CPU 计算资源，大大提升了计算效率。

当然除了使用 NumPy 外，你还需要一些技巧来提升内存和提高计算资源的利用率。一个重要的规则就是：避免采用隐式拷贝，而是采用就地操作的方式。举个例子，如果我想让一个数值 x 是原来的两倍，可以直接写成 x*=2，而不要写成 y=x*2。

这样速度能快到 2 倍甚至更多。

既然 NumPy 这么厉害，你该从哪儿入手学习呢？在 NumPy 里有两个重要的对象：ndarray（N-dimensional array object）解决了多维数组问题，而 ufunc（universal function object）则是解决对数组进行处理的函数。下面，我就带你一一来看。

ndarray 对象

ndarray 实际上是多维数组的含义。在 NumPy 数组中，维数称为秩（rank），一维数组的秩为 1，二维数组的秩为 2，以此类推。在 NumPy 中，每一个线性的数组称为一个轴（axes），其实秩就是描述轴的数量。

下面，你来看 ndarray 对象是如何创建数组的，又是如何处理结构数组的呢？

创建数组

import numpy as np


a = np.array([1, 2, 3])


b = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])


b[1,1]=10


print a.shape


print b.shape


print a.dtype


print b


运行结果：

(3L,)


(3L, 3L)


int32


[[ 1  2  3]


 [ 4 10  6]


 [ 7  8  9]]


创建数组前，你需要引用 NumPy 库，可以直接通过 array 函数创建数组，如果是多重数组，比如示例里的 b，那么该怎么做呢？你可以先把一个数组作为一个元素，然后嵌套起来，比如示例 b 中的 [1,2,3] 就是一个元素，然后 [4,5,6][7,8,9] 也是作为元素，然后把三个元素再放到 [] 数组里，赋值给变量 b。

当然数组也是有属性的，比如你可以通过函数 shape 属性获得数组的大小，通过 dtype 获得元素的属性。如果你想对数组里的数值进行修改的话，直接赋值即可，注意下标是从 0 开始计的，所以如果你想对 b 数组，九宫格里的中间元素进行修改的话，下标应该是 [1,1]。

结构数组

如果你想统计一个班级里面学生的姓名、年龄，以及语文、英语、数学成绩该怎么办？当然你可以用数组的下标来代表不同的字段，比如下标为 0 的是姓名、下标为 1 的是年龄等，但是这样不显性。

实际上在 C 语言里，可以定义结构数组，也就是通过 struct 定义结构类型，结构中的字段占据连续的内存空间，每个结构体占用的内存大小都相同，那在 NumPy 中是怎样操作的呢？

import numpy as np


persontype = np.dtype({


    'names':['name', 'age', 'chinese', 'math', 'english'],


    'formats':['S32','i', 'i', 'i', 'f']})


peoples = np.array([("ZhangFei",32,75,100, 90),("GuanYu",24,85,96,88.5),


       ("ZhaoYun",28,85,92,96.5),("HuangZhong",29,65,85,100)],


    dtype=persontype)


ages = peoples[:]['age']


chineses = peoples[:]['chinese']


maths = peoples[:]['math']


englishs = peoples[:]['english']


print np.mean(ages)


print np.mean(chineses)


print np.mean(maths)


print np.mean(englishs)


运行结果：

28.25


77.5


93.25


93.75


你看下这个例子，首先在 NumPy 中是用 dtype 定义的结构类型，然后在定义数组的时候，用 array 中指定了结构数组的类型 dtype=persontype，这样你就可以自由地使用自定义的 persontype 了。比如想知道每个人的语文成绩，就可以用 chineses = peoples [:][‘chinese’]，当然 NumPy 中还有一些自带的数学运算，比如计算平均值使用 np.mean。

ufunc 运算

ufunc 是 universal function 的缩写，是不是听起来就感觉功能非常强大？确如其名，它能对数组中每个元素进行函数操作。NumPy 中很多 ufunc 函数计算速度非常快，因为都是采用 C 语言实现的。

连续数组的创建

NumPy 可以很方便地创建连续数组，比如我使用 arange 或 linspace 函数进行创建：

x1 = np.arange(1,11,2)


x2 = np.linspace(1,9,5)


np.arange 和 np.linspace 起到的作用是一样的，都是创建等差数组。这两个数组的结果 x1,x2 都是 [1 3 5 7 9]。结果相同，但是你能看出来创建的方式是不同的。

arange () 类似内置函数 range ()，通过指定初始值、终值、步长来创建等差数列的一维数组，默认是不包括终值的。

linspace 是 linear space 的缩写，代表线性等分向量的含义。linspace () 通过指定初始值、终值、元素个数来创建等差数列的一维数组，默认是包括终值的。

算数运算

通过 NumPy 可以自由地创建等差数组，同时也可以进行加、减、乘、除、求 n 次方和取余数。

x1 = np.arange(1,11,2)


x2 = np.linspace(1,9,5)


print np.add(x1, x2)


print np.subtract(x1, x2)


print np.multiply(x1, x2)


print np.divide(x1, x2)


print np.power(x1, x2)


print np.remainder(x1, x2)


运行结果：

[ 2.  6. 10. 14. 18.]


[0. 0. 0. 0. 0.]


[ 1.  9. 25. 49. 81.]


[1. 1. 1. 1. 1.]


[1.00000000e+00 2.70000000e+01 3.12500000e+03 8.23543000e+05


 3.87420489e+08]


[0. 0. 0. 0. 0.]


我还以 x1, x2 数组为例，求这两个数组之间的加、减、乘、除、求 n 次方和取余数。在 n 次方中，x2 数组中的元素实际上是次方的次数，x1 数组的元素为基数。

在取余函数里，你既可以用 np.remainder (x1, x2)，也可以用 np.mod (x1, x2)，结果是一样的。

统计函数

如果你想要对一堆数据有更清晰的认识，就需要对这些数据进行描述性的统计分析，比如了解这些数据中的最大值、最小值、平均值，是否符合正态分布，方差、标准差多少等等。它们可以让你更清楚地对这组数据有认知。

下面我来介绍下在 NumPy 中如何使用这些统计函数。

计数组 / 矩阵中的最大值函数 amax ()，最小值函数 amin ()

import numpy as np


a = np.array([[1,2,3], [4,5,6], [7,8,9]])


print np.amin(a)


print np.amin(a,0)


print np.amin(a,1)


print np.amax(a)


print np.amax(a,0)


print np.amax(a,1)


运行结果：

1


[1 2 3]


[1 4 7]


9


[7 8 9]


[3 6 9]


amin () 用于计算数组中的元素沿指定轴的最小值。对于一个二维数组 a，amin (a) 指的是数组中全部元素的最小值，amin (a,0) 是延着 axis=0 轴的最小值，axis=0 轴是把元素看成了 [1,4,7], [2,5,8], [3,6,9] 三个元素，所以最小值为 [1,2,3]，amin (a,1) 是延着 axis=1 轴的最小值，axis=1 轴是把元素看成了 [1,2,3], [4,5,6], [7,8,9] 三个元素，所以最小值为 [1,4,7]。同理 amax () 是计算数组中元素沿指定轴的最大值。

统计最大值与最小值之差 ptp ()

a = np.array([[1,2,3], [4,5,6], [7,8,9]])


print np.ptp(a)


print np.ptp(a,0)


print np.ptp(a,1)


运行结果：

8


[6 6 6]


[2 2 2]


对于相同的数组 a，np.ptp (a) 可以统计数组中最大值与最小值的差，即 9-1=8。同样 ptp (a,0) 统计的是沿着 axis=0 轴的最大值与最小值之差，即 7-1=6（当然 8-2=6,9-3=6，第三行减去第一行的 ptp 差均为 6），ptp (a,1) 统计的是沿着 axis=1 轴的最大值与最小值之差，即 3-1=2（当然 6-4=2, 9-7=2，即第三列与第一列的 ptp 差均为 2）。

统计数组的百分位数 percentile ()

a = np.array([[1,2,3], [4,5,6], [7,8,9]])


print np.percentile(a, 50)


print np.percentile(a, 50, axis=0)


print np.percentile(a, 50, axis=1)


运行结果：

5.0


[4. 5. 6.]


[2. 5. 8.]


同样，percentile () 代表着第 p 个百分位数，这里 p 的取值范围是 0-100，如果 p=0，那么就是求最小值，如果 p=50 就是求平均值，如果 p=100 就是求最大值。同样你也可以求得在 axis=0 和 axis=1 两个轴上的 p% 的百分位数。

统计数组中的中位数 median ()、平均数 mean ()

a = np.array([[1,2,3], [4,5,6], [7,8,9]])


#求中位数

print np.median(a)


print np.median(a, axis=0)


print np.median(a, axis=1)


#求平均数

print np.mean(a)


print np.mean(a, axis=0)


print np.mean(a, axis=1)


运行结果：

5.0


[4. 5. 6.]


[2. 5. 8.]


5.0


[4. 5. 6.]


[2. 5. 8.]


你可以用 median () 和 mean () 求数组的中位数、平均值，同样也可以求得在 axis=0 和 1 两个轴上的中位数、平均值。你可以自己练习下看看运行结果。

统计数组中的加权平均值 average ()

a = np.array([1,2,3,4])


wts = np.array([1,2,3,4])


print np.average(a)


print np.average(a,weights=wts)


运行结果：

2.5


3.0


average () 函数可以求加权平均，加权平均的意思就是每个元素可以设置个权重，默认情况下每个元素的权重是相同的，所以 np.average (a)=(1+2+3+4)/4=2.5，你也可以指定权重数组 wts=[1,2,3,4]，这样加权平均 np.average (a,weights=wts)=(1*1+2*2+3*3+4*4)/(1+2+3+4)=3.0。

统计数组中的标准差 std ()、方差 var ()

a = np.array([1,2,3,4])


print np.std(a)


print np.var(a)


运行结果：

1.118033988749895


1.25


方差的计算是指每个数值与平均值之差的平方求和的平均值，即 mean ((x - x.mean ())** 2)。标准差是方差的算术平方根。在数学意义上，代表的是一组数据离平均值的分散程度。所以 np.var (a)=1.25, np.std (a)=1.118033988749895。

NumPy 排序

排序是算法中使用频率最高的一种，也是在数据分析工作中常用的方法，计算机专业的同学会在大学期间的算法课中学习。

那么这些排序算法在 NumPy 中实现起来其实非常简单，一条语句就可以搞定。这里你可以使用 sort 函数，sort (a, axis=-1, kind=‘quicksort’, order=None)，默认情况下使用的是快速排序；在 kind 里，可以指定 quicksort、mergesort、heapsort 分别表示快速排序、合并排序、堆排序。同样 axis 默认是 -1，即沿着数组的最后一个轴进行排序，也可以取不同的 axis 轴，或者 axis=None 代表采用扁平化的方式作为一个向量进行排序。另外 order 字段，对于结构化的数组可以指定按照某个字段进行排序。

a = np.array([[4,3,2],[2,4,1]])


print np.sort(a)


print np.sort(a, axis=None)


print np.sort(a, axis=0)  


print np.sort(a, axis=1)  


运行结果：

[[2 3 4]


 [1 2 4]]


[1 2 2 3 4 4]


[[2 3 1]


 [4 4 2]]


[[2 3 4]


 [1 2 4]]


你可以自己计算下这个运行结果，然后再跑一遍比对下。

总结

在 NumPy 学习中，你重点要掌握的就是对数组的使用，因为这是 NumPy 和标准 Python 最大的区别。在 NumPy 中重新对数组进行了定义，同时提供了算术和统计运算，你也可以使用 NumPy 自带的排序功能，一句话就搞定各种排序算法。

当然要理解 NumPy 提供的数据结构为什么比 Python 自身的「更高级、更高效」，要从对数据指针的引用角度进行理解。

我今天重点讲了 NumPy 的数据结构，你能用自己的话说明一下为什么要用 NumPy 而不是 Python 的列表 list 吗？除此之外，你还知道那些数据结构类型？

练习题：统计全班的成绩

假设一个团队里有 5 名学员，成绩如下表所示。你可以用 NumPy 统计下这些人在语文、英语、数学中的平均成绩、最小成绩、最大成绩、方差、标准差。然后把这些人的总成绩排序，得出名次进行成绩输出。

期待你的答案，也欢迎点击「请朋友读」，把这篇文章分享给你的朋友或者同事。

unpreview


© 版权归极客邦科技所有，未经许可不得传播售卖。页面已增加防盗追踪，如有侵权极客邦将依法追究其法律责任。

大龙

由作者筛选后的优质留言将会公开显示，欢迎踊跃留言。

Command + Enter 发表

0/2000 字

提交留言

精选留言 (245)

mickey 置顶

#!/usr/bin/python


#vim: set fileencoding:utf-8


import numpy as np


'''


假设一个团队里有 5 名学员，成绩如下表所示。

1. 用 NumPy 统计下这些人在语文、英语、数学中的平均成绩、最小成绩、最大成绩、方差、标准差。

2. 总成绩排序，得出名次进行成绩输出。

'''


scoretype = np.dtype({


    'names': ['name', 'chinese', 'english', 'math'],


    'formats': ['S32', 'i', 'i', 'i']})


peoples = np.array(


        [


            ("zhangfei", 66, 65, 30),


            ("guanyu", 95, 85, 98),


            ("zhaoyun", 93, 92, 96),


            ("huangzhong", 90, 88, 77),


            ("dianwei", 80, 90, 90)


        ], dtype=scoretype)


#print(peoples)


name = peoples[:]['name']


wuli = peoples[:]['chinese']


zhili = peoples[:]['english']


tili = peoples[:]['math']


def show(name,cj):


    print name,


    print " |",


    print np.mean(cj),


    print " | ",


    print np.min(cj),


    print " | ",


    print np.max(cj),


    print " | ",


    print np.var(cj),


    print " | ",


    print np.std(cj)


print ("科目 | 平均成绩 | 最小成绩 | 最大成绩 | 方差 | 标准差")

show ("语文", wuli)

show ("英语", zhili)

show ("数学", tili)

print ("排名:")

ranking =sorted(peoples,cmp = lambda x,y: cmp(x[1]+x[2]+x[3],y[1]+y[2]+y[3]), reverse=True)


print(ranking)


作者回复：写的不错，大家都可以看下。这里他用到了 Python 自带的 sorted 函数，用 cmp 函数和 lambda 按照三科成绩之和进行排序，并且设置 reverse=True 进行降序排序

2018-12-21


么春脸小的你了亲并

排名第一的同学是用 Python 2 的写法，我用 Python 3 也写一遍，供大家参考。

# -*- coding: utf-8 -*-


"""


Created on Sun Jan 20 00:51:28 2019


@author: Dachun Li


"""


import numpy as np


a = np.array([[4,3,2],[2,4,1]])


print(np.sort(a))


print(np.sort(a, axis=None))


print(np.sort(a, axis=0))


print(np.sort(a, axis=1))


print ("\npart 6 作业 \n")

persontype = np.dtype({


    'names':['name', 'chinese','english','math' ],


    'formats':['S32', 'i', 'i', 'i']})


peoples = np.array([("ZhangFei",66,65,30),("GuanYu",95,85,98),


       ("ZhaoYun",93,92,96),("HuangZhong",90,88,77),


       ("DianWei",80,90,90)],dtype=persontype)


#指定的竖列

name = peoples[:]['name']


chinese = peoples[:]['chinese']


english = peoples[:]['english']


math = peoples[:]['math']


#定义函数用于显示每一排的内容

def show(name,cj):


    print('{} | {} | {} | {} | {} | {} '


          .format(name,np.mean(cj),np.min(cj),np.max(cj),np.var(cj),np.std(cj)))


print ("科目 | 平均成绩 | 最小成绩 | 最大成绩 | 方差 | 标准差")

show ("语文", chinese)

show ("英语", english)

show ("数学", math)

print ("排名:")

#用 sorted 函数进行排序

ranking = sorted(peoples,key=lambda x:x[1]+x[2]+x[3], reverse=True)


print(ranking)


作者回复：我让编辑给你加精

2019-01-20


Zahputor


老师你好，我想问一下 axis=0,axis=1, 这个应该怎么理解？看得不是很明白

作者回复: axis=0 是跨行（纵向），axis=1 是跨列（横向）

2018-12-21


(｡·́︿·̀｡) 面团

percentile 那里，50 是不是应该是中位数而不是平均数啊？

2018-12-21


Kylin


基本上… 没听懂，一脸懵逼的听完了，老师还能抢救一下吗？是缺点什么基础知识？

作者回复：联系编辑，加微信群，我和你电话沟通下，制定学习计划。你也可以把你的情况和遇到的问题，写在评论区里。这样我解答，更多人可以看到

2018-12-24


何楚

老师你的课程示范代码是 Python 2.x 的，可能有些新手同学用了 Python 3 环境，所以你的 print 导致运行错误，然后他们就卡住了，不知道如何解决。

2018-12-21


Non-constant


一、老师问题的回答：

1.1 效率比较

Python 中的 list 保存的是对象的指针，因此数据量大时很占内存，所以会慢。

NumPy 数组存储在一个均匀连续的内存块中，这样数组计算遍历所有的元素，不像列表 list 还需要对内存地址进行查找，从而节省了计算资源，比较快。

1.2 其他数据类型

例如字典 dict、树、图、等等

二、我的云笔记链接（基本所有代码都验证了一遍）：http://note.youdao.com/noteshare?id=dc330cd14b6a354f34167f8e33774177&sub=39A110D2D15A47189E2D33C0051A6F0E

三、就我感觉老师你在 amin（）和 amax（）那里的解释错了？还是说我理解错了？

amin（）时：

axis=0 所选元素应该是 [1,4,7], [2,5,8], [3,6,9]，然后再选择每一数组中最小的那个值，也即 [1,2,3]；

axis=1 所选元素应该是 [1,2,3], [4,5,6], [7,8,9]，然后再选择每一数组中最小的那个值，也即 [1,4,7]；

也就是说，axis=0 是列运算；axis=1 是行运算。

以下是我的代码验证：

------------------------------------------------------------------------------------------------


import numpy as np


a = np.array([[1,6,3], [4,5,6], [100,8,9]])


print(a,'\n')


print (np.amin (a)) # amin (a) 指的是数组中全部元素的最小值

print (np.amin (a,axis=0)) # axis=0 轴是把元素看成了 [1,4,100], [6,5,8], [3,6,9] 三个元素

print (np.amin (a,axis=1),'\n') # axis=1 轴是把元素看成了 [1,6,3], [4,5,6], [100,8,9] 三个元素

print(np.amax(a))


print(np.amax(a,axis=0))


print(np.amax(a,axis=1))


# amin () 用于计算数组中的元素沿指定轴的最小值

# amax () 用于计算数组中的元素沿指定轴的最大值

------------------------------------------------------------------------------------------------


结果：

[[ 1 6 3]


 [ 4 5 6]


 [100 8 9]]


1


[1 5 3]


[1 4 8]


100


[100 8 9]


[ 6 6 100]


------------------------------------------------------------------------------------------------


作业题我今晚再更新在我的云笔记中，以上。

2018-12-21


Jie


import sys


import numpy as np


persontype = np.dtype({'names':['name','chinese','english','math','total'],'formats':['S32','i','i','i','i']})


peoples = np.array([('zhangfei',66,65,30,0),('guanyu',95,85,98,0),("zhanyun",93,92,96,0),('huanghzong',90,88,77,0),('dianwei',80,90,90,0)],dtype = persontype)


peoples[:]['total']= peoples[:]['chinese'] +peoples[:]['english']+peoples[:]['math']


print (peoples)


print(peoples.dtype.names)


for col in peoples.dtype.names:


if col =='name' or col == 'total' :


continue


print ("mean of {}:{}".format(col,np.mean(peoples[:][col])))


print ("amax of {}:{}".format(col,np.amax(peoples[:][col])))


print ("amin of {}:{}".format(col,np.amin(peoples[:][col])))


print ("std of {}:{}".format(col,np.std(peoples[:][col])))


print ("var of {}:{}".format(col,np.var(peoples[:][col])))


print(np.sort(peoples,order ='total'))


2018-12-24


齐福聪

老师 percentile 参数为 50 的时候 应该取的是中位数而不是平均值 对么

2018-12-21


杨延平

axis: 沿着它排序数组的轴，如果没有数组会被展开，沿着最后的轴排序，axis=0 按列排序，axis=1 按行排序

2018-12-21


Alex 王伟健

看来需要去老师推荐的课学下 Python 了。。。

2018-12-21


Michael


中文名字的格式写 S32 时报错

2018-12-28


何楚

#!/usr/bin/env python3


# -*- coding: utf-8 -*-


import numpy as np


persontype = np.dtype({


    'names': ['name', 'chinese', 'math', 'english'],


    'formats': ['S32', 'i', 'i', 'i']})


peoples = np.array([("ZhangFei", 66, 65, 30), ("GuanYu", 95, 85, 98),


                    ("ZhaoYun", 93, 92, 96), ("HuangZhong", 90, 88, 77),


                    ("DianWei", 80, 90, 90)],


                   dtype=persontype)


for col in peoples.dtype.names:


# print(col)


    if col is "name":


        continue


    print("mean of {}: {}".format(col, peoples[col].mean()))


    print("min of {}: {}".format(col, peoples[col].min()))


    print("max of {}: {}".format(col, peoples[col].max()))


    print("var of {}: {}".format(col, peoples[col].var()))


    print("std of {}: {}".format(col, peoples[col].std()))


report = np.empty([0, 0])


for i in range(peoples.size):


    sum_score = peoples['chinese'][i] + peoples['english'][i] + peoples['math'][i]


    #print(sum_score)


    report = np.append(report, [ sum_score])


report = -np.sort(-report)


print("sorted score:")


print(report)


怎么在 numpy 里作成绩求和还不是很清楚。另外，想把成绩和名字按排序后打印出来，要用索引，赶时间没研究，等看别人的结果。

作者回复：你在求三科成绩的各种统计指标的时候，写的不错

你提到的如何在 numpy 中求和，其实在定义结构数组的时候，可以多定义一列 total

peoples[:]['total'] = peoples[:]['chinese']+peoples[:]['english']+peoples[:]['math']


然后按照 total 进行排序即可

print np.sort(peoples, order='total')


2018-12-21


蜉蝣

关于 axis 参数的问题，我也有点模糊，后来知乎上看到这篇文章，思路清晰多了，也推荐大家看一下：https://zhuanlan.zhihu.com/p/30960190

作者回复：多谢蜉蝣分享

2019-03-24


从未在此

根据我在网上找的学习资料，axis＝0，代表跨行；=1 代表跨列，这样很容易理解。

作者回复：对的 理解正确

2018-12-21


JingZ


(1) NumPy 相对 Python 更高级和更高效，数组存储在均匀连续的内存块，节约计算资源；矢量化的指针指令和多线程矩阵计算提升计算效率；避免隐氏拷贝，采取就地操作。

(2) 数据结构，Python 常用应是 array,tuple,list,dictionary,set, 其他听过的有 stack,graph,hash,heap,tree 等～理论待老师深入

(3) 练习题代码，最后一点还需要想一想怎么按总成绩排名输出？感觉代码重复性有点高，有更更简洁的代码？

import numpy as np


persontype = np.dtype({


    'names':['name','chinese','english','math'],


    'formats':['S32','i','i','i']})


peoples = np.array([("ZhangFei",66,65,30),("GuanYu",95,85,98), ("ZhaoYun",93,92,96),("HuangZhong",90,88,77),("DianWei",80,90,90)],dtype=persontype)


#语文、英语、数学

chineses = peoples[:]['chinese']


englishs = peoples[:]['english']


maths = peoples[:]['math']


#平均成绩

print(np.mean(chineses))


print(np.mean(englishs))


print(np.mean(maths))


#最小成绩

print(np.amin(chineses))


print(np.amin(englishs))


print(np.amin(maths))


#最大成绩

print(np.amax(chineses))


print(np.amax(englishs))


print(np.amax(maths))


#方差

print(np.std(chineses))


print(np.std(englishs))


print(np.std(maths))


#标准差

print(np.var(chineses))


print(np.var(englishs))


print(np.var(maths))


#总成绩排序

print(np.sort(chineses+englishs+maths))


#按姓名排序

print(np.sort(peoples,order='name'))


2018-12-21


离忧

老师定义结构数组，那个 s32 是什么意思呢？

2018-12-24


抢地瓜的阿姨

Dataframe 即将登场！哈哈哈

作者回复：哈哈哈 是的

2018-12-22


ZHen


轴的那里，把数组按行列写在纸上，axis=0 就是按行来取元素，axis=1 就是沿着第一列取元素

1，2，3


4，5，6


7，8，9


axis=1 时，取的元素组合就是 [1,4,7],[2,5,8],[3,6,9]

2018-12-21


小葱拌豆腐

老师，请问一下您，没学过高数，没接触过计算机语言，要提前去把各种函数搞清楚吗？有没有推荐的办法，书籍，课程？

作者回复：我更推荐把我文章里的代码都跑一遍，不明白的地方就留言，效率更高

2018-12-21


Ben


scoretype = np.dtype({'names': ['name', 'chinese', 'english', 'math'],


                      'formats': ['S32', 'i', 'i', 'i']})


peoples = np.array(


    [


        ("zhangfei", 66, 65, 30),


        ("guanyu", 95, 85, 98),


        ("zhaoyun", 93, 92, 96),


        ("huangzhong", 90, 88, 77),


        ("dianwei", 80, 90, 90)


    ], dtype=scoretype)


print ("科目 | 平均成绩 | 最小成绩 | 最大成绩 | 方差 | 标准差")

courses = {' 语文 ': peoples [:]['chinese'],

' 英文 ': peoples [:]['english'], ' 数学 ': peoples [:]['math']}

for course, scores in courses.items():


    print(course, np.mean(scores), np.amin(scores), np.amax(scores), np.std(scores),


          np.var(scores))


print('Ranking')


ranking = sorted(peoples, key=lambda x: x[1]+x[2]+x[3], reverse=True)


print(ranking)


作者回复: Good Job

2019-07-01


建强

#简易学生成绩档案管理

import numpy as np


student_type = np.dtype({'names':['studentname','Chinese','English','Math','Total'],'formats':['U10','i','i','i','f']})


students = np.array ([("张飞",66,65,30,None),("关羽",95,85,98,None),("赵云",93,92,96,None),("黄忠",90,88,77,None),("典韦",80,90,90,None)]

                    ,dtype = student_type)


Chinese = students[:]['Chinese']


English = students[:]['English']


Math = students[:]['Math']


#指标分析

score_analy={' 平均成绩 ':{' 语文 ':np.mean (Chinese),' 英语 ': np.mean (English),' 数学 ':np.mean (Math)}

,' 最小成绩 ':{' 语文 ':np.amin (Chinese),' 英语 ': np.amin (English),' 数学 ':np.amin (Math)}

,' 最大成绩 ':{' 语文 ':np.amax (Chinese),' 英语 ': np.amax (English),' 数学 ':np.amax (Math)}

,' 标准差 ' :{' 语文 ':np.std (Chinese) ,' 英语 ': np.std (English) ,' 数学 ': np.std (Math)}

,' 方差 ' :{' 语文 ':np.var (Chinese) ,' 英语 ': np.var (English) ,' 数学 ': np.var (Math)}}

#统计总成绩

for i in range(len(students)):


    students[i]['Total'] = sum(list(students[i])[1:-1])


#输出分析指标

print ("指标项 \t\t 语文 \t\t 英语 \t\t 数学")

print(("-" * 10 +"\t\t")*4)


for index in score_analy:


report = f"{index:10}".format (index) + "\t\t {语文:>10.2f}\t\t {英语:>10.2f}\t\t {数学:>10.2f}"

    print(report.format_map(score_analy[index]))


print(("-" * 82))


#按总成绩输出排名

print ("名次 \t\t 姓名 \t\t 总分")

print(("-" * 4 +"\t\t")*3)


    


s = np.sort(students,order='Total')


for i in range(len(s)):


    k=-1 * (i+1)


    print('{rank:4}\t\t{name:4}\t\t{score:>4}'.format(rank=i+1,name=s[k]['studentname'],score=s[k]['Total']))


作者回复: Good Job

2019-06-15


Geek_ce3c1f


import numpy as np


persontype = np.dtype({


    'names':['name', 'chinese', 'english', 'math'],


    'formats':['S32', 'i', 'i', 'f']})


peoples = np.array([("ZhangFei",66,65,30),("GuanYu",95,85,90),


                    ("ZhaoYun",93,92,96),("HuangZhong",90,88,77),


                    ("Dianwei",80,90,90)],dtype=persontype)


chineses = peoples[:]['chinese']


maths = peoples[:]['math']


englishs = peoples[:]['english']


print ("语文平均分 %.2f，最小成绩 % d，最大成绩 % d，方差 %.2f，标准差 %.2f" %(np.mean (chineses),np.amin (chineses),np.amax (chineses),np.var (chineses),np.std (chineses)))

print ("数学平均分 % d, 最小成绩 % d，最大成绩 % d，方差 % d，标准差 % d" %(np.mean (maths),np.amin (maths),np.amax (maths),np.var (maths),np.std (maths)))

print ("英语平均分 % d, 最小成绩 % d，最大成绩 % d，方差 % d，标准差 % d" %(np.mean (englishs),np.amin (englishs),np.amax (englishs),np.var (englishs),np.std (englishs)))

# chengji = np.array([[66,65,30],[95,85,90],[93,92,96],[90,88,77],[80,90,90]])


rank = np.sort(peoples, axis=None, kind='mergesort', order=('chinese','english','math'))


print(rank)


作者回复: Good Job

2019-03-11


Blaise


subjects = np.dtype({'names': ['name', 'Chinese', 'English', 'Math'],


                     'formats': ['S32', 'i', 'i', 'i']


                    })


people = np.array([('ZhangFei',66,65,30), ('GuanYu',95,85,98),


                   ('ZhaoYun',93,92,96), ('HuangZhong',90,88,77),


                   ('DianWei',80,90,90),


                  ], dtype=subjects)


print(np.mean(people[:]['Chinese']))


print(np.mean(people[:]['English']))


print(np.mean(people[:]['Math']))


print(np.amin(people[:]['Chinese']))


print(np.amin(people[:]['English']))


print(np.amin(people[:]['Math']))


print(np.amax(people[:]['Chinese']))


print(np.amax(people[:]['English']))


print(np.amax(people[:]['Math']))


print(np.std(people[:]['Chinese']))


print(np.std(people[:]['English']))


print(np.std(people[:]['Math']))


print(np.var(people[:]['Chinese']))


print(np.var(people[:]['English']))


print(np.var(people[:]['Math']))


# summarytype = np.dtype({'names': ['ZhangFei', 'GuanYu', 'ZhaoYun', 'HuangZhong', 'DianWei'], 'formats': ['i','i', 'i', 'i', 'i'],})


summary = np.array([], dtype=np.int32)


for i, person in enumerate(people):


    person = list(person)


    print(person[:])


    total = (np.sum(person[1:]))


    print(total)


    summary = np.append(summary, [total], axis=0)


# 每人的总成绩

print(summary)


# 对每人的总成绩排序

print(np.sort(summary))


本来还想在总成绩数组上标明是谁的成绩，还没弄出来。

作者回复: Good Job

2019-01-31


FORWARD―MOUNT


我用的是 Python3，按照老师的方法在定义 np.dtyp 时报错，在网上也没查到有用的信息，经摸索后，可以改写为：

new_type = np.dtype([('name', np.str_, 16), ('chinese', np.int32), ('english', np.int32), ('math', np.int32)])


供大家参考。

2019-01-19


老友 @极客时间

percentile 那里，50 是不是应该是中位数而不是平均数啊？上面有两个同学说到了 个人也认为是中位数

2019-01-18


擎天

老师，我们这个课程有没有知识星球？能否建立一个知识星球，让大家自由发言，在这里留言，无法互动。如果建立一个知识星球，有的同学把疑问发到知识星球里，其余同学对其有不一样的理解的话，就会去评论。疑问有了互动交流，才能产生思维的升级，否则仅仅是提出疑问，没有答案的话，思维还是停留在原地。

而且知识星球可以产生知识的沉淀，也是一笔宝贵的知识财富。

2018-12-23


GeekWhite


class Numpy:


    def __init__(self):


        self.persontype = np.dtype(


            {


                'names': ['name', 'chinese', 'english', 'math'],


                'formats': ['S32', 'i', 'i', 'i']


            }


        )


        self.people = np.array(


            [('zhangfei', 66, 65, 30), ('guanyu', 95, 85, 98), ('zhaoyun', 93, 92, 96),


             ('huangzhong', 90, 88, 77), ('dianwei', 80, 90, 90)], dtype=self.persontype


        )


    @staticmethod


    def stdout(subject, person_arr):


print (' 科目 ', subject)

print (' 平均值 ', np.mean (person_arr))

print (' 最小 ', np.min (person_arr))

print (' 最大 ', np.max (person_arr))

print (' 方差 ', np.var (person_arr))

print (' 标准差 ', np.std (person_arr), '\n')

model_init = Numpy()


if __name__ == '__main__':


    name = model_init.people[:]['name']


    chinese = model_init.people[:]['chinese']


    english = model_init.people[:]['english']


    math = model_init.people[:]['math']


Numpy ().stdout (' 语文 ', chinese)

Numpy ().stdout (' 英语 ', english)

Numpy ().stdout (' 数学 ', math)

    ranking = sorted(model_init.people, key=lambda x: x[1] + x[2] + x[3], reverse=True)


    print(ranking)


2020-01-07


qinggeouye


import numpy as np


people_type = np.dtype({'names': ['name', 'chinese', 'math', 'english', 'total'],


                        'formats': ['S32', 'i', 'i', 'f', 'f']})


peoples = np.array([('ZhangFei', 60, 65, 30, 0), ('GuanYu', 95, 85, 98, 0),


                    ('ZhaoYun', 93, 92, 96, 0), ('HuangZhong', 90, 88, 77, 0),


                    ('DianWei', 80, 90, 90, 0)], dtype=people_type)


chinese = peoples[:]['chinese']


math = peoples[:]['math']


english = peoples[:]['english']


peoples[:]['total'] = chinese + math + english


print("rank of total scores is \n %s" % np.sort(peoples, order='total'))


print("\n")


for key in list(people_type.fields.keys())[1:4]:


    print("mean of %s is %s" % (key, np.mean(peoples[:][key])))


    print("max of %s is %s" % (key, np.amax(peoples[:][key])))


    print("min of %s is %s" % (key, np.amin(peoples[:][key])))


    print("std of %s is %s" % (key, np.std(peoples[:][key])))


    print("var of %s is %s" % (key, np.var(peoples[:][key])))


    print("\n")


作者回复: Good Job

2019-10-31


Helen


请问老师能不能详细讲下 dtype 和 key=lambda 是什么意思，怎么用。谢谢。

作者回复: dtype 就是 Numpy 数据类型对象

比如 print (a.dtype) 显示 int32，说明 a 的数据类型是 int32

sorted (key=lambda)，这里用到了参数 key，也就是关键词，lambda 是个隐函数，

ranking = sorted(peoples,key=lambda x:x[1]+x[2]+x[3], reverse=True)


这里就是按照参数 x 中的 x [1]+x [2]+x [3] 之和进行排序

2019-08-26


王张

'formats':['S32','i', 'i', 'i', 'f'] 是什么意思？

作者回复：代表数据类型，S 是 String 字符串，i 是 integer 整数，f 是 float 浮点型

2019-07-20


bingo


个人对 axis 的理解：

对课里的二维数组举例，np.min (arr, axis=0)。

是将 [arr [0][0], arr [1][0], arr [2][0], arr [3][0]... ] 作为一个参数传递给 min 函数，会得到一个最小值；

再将 [arr [0][1], arr [1][1], arr [2][1], arr [3][1]... ] 作为一个参数传递给 min 函数，又会得到一个最小值；

......


最后将这些最小值合并成一个数组输出

总结一下：axis=0 就是优先变化第一个维度，axis=1 就是优先变化第二个维度，更高维度的数组以此类推。

2019-06-04


rex_zhong


a = np.array([[1,2,3], [3,4,5], [7,8,9]])


 print np.percentile(a, 70)


输出：6.199999999999999

可否解释一下这个原理，尤其是 percentile 的参数为除了 0 50 100 以外的其他数的含义，以及当他等于 70 的时候的演算过程，为啥等于 6.1999

2019-05-13


不知道

当 ndarray 的秩大于 2 的时候怎么表示，还有就是各种求最大最小或排序的时候，把 axis=2 的时候该怎么理解

2019-03-04


修.

懵逼的听完了，数学小白啊！感觉够呛啊！如何补救啊？

2018-12-28


从未在此

跟大家说下，axis＝0 代表跨行计算，＝1 代表跨列。这样就很容易理解了。我当初百度找的，一下子就通了

2018-12-21


Jbin


老师您好：

今日练习题代码如下，望指导一下如何按总成绩排序 people 数组元素

# coding=utf-8


import numpy as np


persontype = np.dtype({'names': ['name', 'chinese', 'english', 'math'], 'formats': ['S32', 'i', 'i', 'i']})


peoples = np.array([('ZhangFei', 66, 65, 30), ('GuanYu', 95, 85, 98), ('ZhaoYun', 93, 92, 96), ('HuangZhong', 90, 88, 77), ('DianWei', 80, 90, 90)], dtype=persontype)


chineses = peoples[:]['chinese']


englishs = peoples[:]['english']


maths = peoples[:]['math']


print (' 语文平均分是 ', np.mean (chineses))

print (' 数学平均分是 ', np.mean (maths))

print (' 英语平均分是 ', np.mean (englishs))

print (' 语文成绩最低分是：', np.amin (chineses), ' 最高分是：', np.amax (chineses), ' 方差：', np.var (chineses), ' 标准差：', np.std (chineses))

print (' 英语成绩最低分是：', np.amin (englishs), ' 最高分是：', np.amax (englishs), ' 方差：', np.var (englishs), ' 标准差：', np.std (englishs))

print (' 数学成绩最低分是：', np.amin (maths), ' 最高分是：', np.amax (maths), ' 方差：', np.var (maths), ' 标准差：', np.std (maths))

print (' 总成绩从低到高为 ', np.sort (chineses+englishs+maths))

作者回复：这里你可以增加一列总和 total，当时创建的结构数组的时候 你就可以把 total 先定义出来

peoples[:]['total'] = peoples[:]['chinese']+peoples[:]['english']+peoples[:]['math']


然后按照 total 进行排序即可

print np.sort(peoples, order='total')


你自己可以试一下

2018-12-21


小葱拌豆腐

老师，请问一下您，对于没学过高数的，又是第一次接触计算机语言的，该如何入门呢？

2018-12-21


中国梦

占楼，明天练习一下

2018-12-21


Key.


python 的 list 在内存的中的存储是离散的 (相较于 Numpy 查找花费更多的时间复杂度)，而 Numpy 在内存中的存储是连续的。了解的第三方库主要是 Numpy 和 matplotlib，也了解过 sklearn 的一些方法。

2018-12-21


张小白

排序函数 sort 中，axis=-1，0，1 的区别不是特别懂，能再详细讲一下吗？

2020-01-26


Neo


老师，为什么匿名函数里可以直接用 x [1]+x [2]+x [3] 这样的用法？这里传参不是把 peoples 代进去了吗？peoples [1] 索引的是第二行，怎么可以求和呢？

2020-01-18


苹果

关于 numpy 的函数部分还有很多，搜素，组合分割，展平，三角，集合等函数，之前我过了一遍，当初觉得，太多了，记英语单词似的，现在都不记得，实践少，正如老师说的，从工程的角度去学，去实战，慢慢去扩充

#作业部分，看到前排同学的做法更优秀，

# -*- coding: utf-8 -*-


import numpy as np


persontype = np.dtype({'names':['name','chinese','english','math'],\


                       'formats':['S32','i','i','i']})


peoples = np.array([('zhangfei',66,65,30),("GuanYu",95,85,98), \


                   ("ZhaoYun",93,92,96),("HuangZhong",90,88,77),\


                    ('Dianwei',80,90,90)],dtype= persontype)


names = peoples[:]['name']


chineses = peoples[:]['chinese']


maths = peoples[:]['math']


englishs = peoples[:]['english']


aa = np.mean(chineses)


print (' 他们的语文平均分:',np.mean (chineses))

print (' 他们的英语平均分:',np.mean (englishs))

print (' 他们的数学平均分:',np.mean (maths))

print (' 他们的语文的最大值：{}，最小值：{}:'.format (chineses.max (),chineses.min ()))

print (' 他们的语文的方差：{:.3f}，标准差：{:.3f}:'.format (chineses.var (),chineses.std ()))

print (' 他们的英语的最大值：{}，最小值：{}:'.format (englishs.max (),englishs.min ()))

print (' 他们的的英语方差：{:.3f}，标准差：{:.3f}:'.format (englishs.var (),englishs.std ()))

print (' 他们的数学的最大值：{}，最小值：{}:'.format (maths.max (),maths.min ()))

print (' 他们的数学的方差：{:.3f}，标准差：{:.3f}:'.format (maths.var (),maths.std ()))

ranking = sorted(peoples,key=lambda x:x[1]+x[2]+x[3],reverse=True)


print(ranking)


2020-01-09


强者自强

import numpy as np


studentstype = np.dtype({'names':['name','chinese_score','english_score','math_score','total_score'],'formats':['S23','i','i','i','i']})


students = np.array([('zhangfei',66,65,30,0),('guanyu',95,85,98,0),('zhaoyun',93,92,96,0),('huangzhong',90,88,77,0),('dianwei',80,90,90,0)]


                    ,dtype = studentstype)


chineseScore = students[:]['chinese_score']


englishScore = students[:]['english_score']


mathScore = students[:]['math_score']


students[:]['total_score'] = students[:]['chinese_score']+students[:]['english_score']+students[:]['math_score']


totalScore = students[:]['total_score']


chineseAverage = np.mean(chineseScore)


englishAverage = np.mean(englishScore)


mathAverage = np.mean(mathScore)


chineseMin = np.amin(chineseScore)


englishMin = np.amin(englishScore)


mathMin = np.amin(mathScore)


chineseMax = np.amax(chineseScore)


englishMax = np.amax(englishScore)


mathMax = np.amax(mathScore)


chineseVar = np.var(chineseScore)


englishVar = np.var(englishScore)


mathVar = np.var(mathScore)


chineseStd = np.std(chineseScore)


englishStd = np.std(englishScore)


mathStd = np.std(mathScore)


totalScore = np.sort(students,order='total_score')


print(chineseAverage)


print(englishAverage)


print(mathAverage)


print(chineseMin)


print(englishMin)


print(mathMin)


print(chineseMax)


print(englishMax)


print(mathMax)


print(chineseVar)


print(englishVar)


print(mathVar)


print(chineseStd)


print(englishStd)


print(mathStd)


print(totalScore)


看了一下评论区的答案，发现自己写麻烦了，之前老师教的记笔记学习的方法真的很好用，知识记得比较牢靠，但是内功还需要加强修炼，加油！

2020-01-01


杨睿芳

老师您好：

今日练习题代码如下，我的是 python 3，试了好多种方法，还是报错，望指导。

UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)


# coding=utf-8


import numpy as np


persontype=np.dtype({


    'names':['name','chinese','math','english','total'],


    'formats':['S32','i','i','i','i']})


peoples=np.array ([("张飞",'66','65','30',''),("关羽",'95','85','98',''),("赵云",'93','92','96',''),("黄忠",'90','88','77',''),("曲伟",'80','90','90','')],dtype=persontype)

chineses=peoples[:][chinese]


maths=peoples[:][math]


englishs=peoples[:][english]


peoples[:][total]=peoples[:][chinese]+peoples[:][math]+peoples[:][english]


print(np.mean(chineses))


print(np.max(chineses))


print(np.min(chineses))


print(np.std(chineses))


print(np.var(chineses))


print(np.mean(maths))


print(np.max(maths))


print(np.min(maths))


print(np.std(maths))


print(np.var(maths))


print(np.mean(englishs))


print(np.max(englishs))


print(np.min(englishs))


print(np.std(englishs))


print(np.var(englishs))


print(np.sort(peoples,orser='total'))


作者回复：帮你 fix 了几处 bug:

persontype=np.dtype({


    'names':['name','chinese','math','english','total'],


    'formats':['U32','i','i','i','i']})


peoples=np.array ([("张飞",66,65,30,0),("关羽",95,85,98,0),("赵云",93,92,96,0),("黄忠",90,88,77,0),("曲伟",80,90,90,0)],dtype=persontype)

chineses=peoples[:]['chinese']


maths=peoples[:]['math']


englishs=peoples[:]['english']


peoples[:]['total']=peoples[:]['chinese']+peoples[:]['math']+peoples[:]['english']


……


print(np.sort(peoples,order='total'))


说明：

1）中文的话，format 形式为 U32

2）'chinese' 是字符串，需要加 ' '

3）order 拼写

你调整下再试试

2019-12-03


小犀牛

s3,i 代表什么数据类型

作者回复: i 代表 integer 整数类型

S32 代表 String32 字符串类型

如果是中文字符串，就需要写成 U32

希望能对你有所帮助，加油

2019-11-25


Handsome


刚查了一下资料，Numpy 水太深，懂基础知识就好了：数据类型（i2,i4,i6,i8,u4,..,f4）数组创建，运算，结构化数组。

作者回复：嗯 慢慢来

2019-11-19


GS


在 jupyter 上敲了一遍。https://github.com/leledada/jupyter/blob/master/pynumTest.ipynb

请问老师，这个课程有答疑微信群，或者学习小组吗？

作者回复：数据分析有微信群的，可以找运营同学要

2019-11-12


北房有佳人

# Numpy 统计全班的成绩，

# 求出语文，英语，数学中的平均成绩，最小成绩，最大成绩，方差，标准差.

# 总成绩排序，得出名词进行成绩输出

# average () 加权平均数，意思是每个元素可以设置一个权重，默认情况下每个元素的权重是相同的。

# var () 方差指每个数值与平均值之差的平方求和的平均值，即 mean ((x-x.mean ())**2)

# 标准差 std () 标准差是方差的算术平方根。在数学意义上，代表的是一组数据离平均值的分散程度

import numpy as np


# 第一步设计数组结构

persontype = np.dtype({


    'names':['name','chinese','english','math'],


    'formats':['S32','i','i','i']


})


# 第二步 构建数组

peoples = np.array([


    ('ZhangFei',66,65,30),('GuanYu',95,85,98),


    ('ZhaoYun',93,92,96),('HuangZhong',90,88,77),


    ('DianWei',80,90,90)],


    dtype=persontype)


# 各科成绩的数组

names = peoples[:]['name']


chineses = peoples[:]['chinese']


englishs = peoples[:]['english']


mathes = peoples['math']


# 计算平均成绩，最小成绩，最大成绩，方差，标准差

def tongji(name,cj):


    """


统计数据

    """


print (f' 科目:{name} | 平均成绩:{np.mean (cj)} | 最小成绩:{np.min (cj)}| 最大成绩:{np.max (cj)} | 方差:{np.var (cj)}| 标准差:{np.std (cj)}')

tongji (' 语文 ',chineses)

tongji (' 英语 ',englishs)

tongji (' 数学 ',mathes)

# 总成绩排序

ranking = sorted(peoples,key = lambda x:sum(list(x)[1:]),reverse=True)


print(ranking)


作者回复：正确

2019-11-11


Alery


import numpy as np


studentType = np.dtype({


    "names": ["name", "c", "e", "m", "t"],


    "formats": ["S32", "i", "i", "i", "i"]})


students = np.array([("zf", 66, 65, 3, 0),


                     ("gy", 95, 85, 98, 0),


                     ("zy", 93, 92, 96, 0),


                     ("hz", 90, 88, 77, 0),


                     ("dw", 80, 90, 90, 0)], dtype=studentType)


c = students[:]["c"]


e = students[:]["e"]


m = students[:]["m"]


print ("语文平均值:", np.average (c), "最大值:", np.amax (c), "最小值", np.amin (c), "方差", np.var (c), "标准差", np.std (c))

print ("英语平均值:", np.average (e), "最大值:", np.amax (e), "最小值", np.amin (e), "方差", np.var (e), "标准差", np.std (e))

print ("数学平均值:", np.average (m), "最大值:", np.amax (m), "最小值", np.amin (m), "方差", np.var (m), "标准差", np.std (m))

students[:]["t"] = c + e + m


print ("排序前:", students)

print ("排序后:", np.sort (students, order="t"))

作者回复: Good Job

2019-11-07


Geek_13d67b


用惯了 R，感觉 numpy 就不怎么好用了，估计 pandas 的功能更强大点吧！

作者回复：嗯 看用什么语言吧，python 的话 numpy, pandas 用的还挺多的

2019-10-23


Answer Liu


import numpy as np


a = np.array([[66, 65, 30], [72, 51, 63], [98, 83, 49]])


chinese = a[:, 0]


math = a[:, 0]


physics = a[:, 0]


def demo(subject):


    print(np.mean(subject))


    print(np.amin(subject))


    print(np.amax(subject))


    print(np.var(subject))


    print(np.std(subject))


demo(chinese)


demo(math)


demo(physics)


print(np.sort(np.sum(a, axis=1)))


2019-10-21


🤔


老师，我现在是用的 VS code，您认为这款怎么样？因为 PyCharm 没用过，不知道如何选择

作者回复: VS Code 用的也很多，选择适合自己的就行，我有时候还会用 Google 的 Colab，主要是有免费 GPU 可以使用

2019-10-17


jxs1211


请问，如果将复杂的 sql 转换为 sqlachemy 语句，是否有比较好的工具或者方法

作者回复：可以使用 table ()，column () 等函数

2019-09-23


FATMAN89


我们这门课主要会用 python2 还是 python3 呢

作者回复：后面都改成了 Python3 方便大家使用

2019-08-29


dudulin


老师，我有一个疑问啊。

既然 axis＝0 代表跨行计算，＝1 代表跨列。

为什么 Numpy 排序那里

a = np.array([[4,3,2],[2,4,1]])


print np.sort(a, axis=0)


的运行结果 不是

[[2,4]


[3,4]


[1,2]] 呢？？？

谢谢

作者回复: axis=0: 方向是垂直方向，也就是列的方向进行排序

axis=1: 方向是水平方向，也就是行的方向进行排序

2019-08-28


王定坤

# coding=utf-8


import numpy as np


# 通过数组完成统计

names = np.array(['ZhangFei', 'GuanYu', 'ZhaoYun', 'HuangZhong', 'DianWei'])


achivements = [


[66, 65, 30],


[95, 85, 98],


[93, 92, 96],


[90, 88, 77],


[80, 90, 90]


]


tmp = np.zeros((5, 7))


tmp[:, 0:3] = achivements


tmp[:, 3] = np.mean(achivements, axis = 1)


tmp[:, 4] = np.min(achivements, axis = 1)


tmp[:, 5] = np.max(achivements, axis = 1)


tmp[:, 6] = np.sum(achivements, axis = 1)


seqs = np.argsort(-tmp[:, 6])


sorted_names = names[seqs]


sorted_tmp = tmp[seqs]


scores = []


for i in range(len(sorted_names)):


l = []


l.append(sorted_names[i])


l.extend(sorted_tmp[i])


scores.append(tuple(l))


print scores


作者回复: Good Job

2019-08-25


Geek_545e3b


import numpy as np


# 这里创建了一维数组，不是二维

persontype = np.dtype([('name','S32'),('chinese','i'),('math','i'),('english','f'),('total','f')])


peoples=np.array([('Zhangfei',75,100,90,0),('Guanyu',85,96,88.5,0),


                  ('Zhaoyun',85,92,96.5,0),('Huangzhong',65,85,100,0)],


                 dtype=persontype)


for b in peoples.dtype.names:


    if b =='name' or b=='total':


        continue


    print('mean of {}={}'.format(b,np.mean(peoples[b])))


    # print('mean of {}={}'.format(b,peoples[b].mean()))


    print('max of {}={}'.format(b, np.amax(peoples[b])))


    print('min of {}={}'.format(b, np.mean(peoples[b])))


    print('var of {}={}'.format(b, np.var(peoples[b])))


    print('std of {}={}'.format(b, np.std(peoples[b])))


peoples['total']=peoples['chinese']+peoples['math']+peoples['english']


sort_peoples=reversed(np.sort(peoples,order='total'))


# print ("名字 \t\t 总成绩")

for rank in sort_peoples:


    print('{}\t:\t{}'.format(rank[0],rank[-1]))


我是用 python3 写的，初学者，好多东西都不懂，不断尝试了很多，才才大概知道一点语法，也借鉴了别人的东西，最后有点小问题，我最后 print 的时候，用制表符，为什么第一行没对齐，还要想问一下，如果我创建一个列表，然后往里面放字典的话，字典里面是名字和成绩，能不能有方法排序啊，我试了 sorted , 出现了 bug

作者回复：加油～

2019-08-20


smile


# 2. 结构数组

peopletype = np.dtype({


    'names':['name','age','chinese','math','english'],


    'formats':['S32','i','i','i','f']


})


peoples = np.array([('yang',20,86,95,90.2),('wang',23,90,68,80.3)],dtype=peopletype)


# array（[里面为什么不能是 [] 呢？]），若写成如下形式会报错：

'''


p1 = np.array([['kiu',56,89,90,67.9087],['hiu',56,89,90,67.9087]],dtpye=peopletype)


# 提示：TypeError: 'dtpye' is an invalid keyword argument for array ()

请老师抽空回复一下，谢谢！

作者回复：很好的洞察，实际上 np.array 里面的数组类型应该是一致的，['yang',20,86,95,90.2] 很明显这些类型是不同的，所以我们使用了 np.dtype 进行了封装，可以保存不同的数据类型

np.dtype({


    'names':['name','age','chinese','math','english'],


    'formats':['S32','i','i','i','f']


})


2019-08-18


goongoup


请问下老师，最后那个作业练习题的答案在哪里？

作者回复：你可以看下置顶的留言，有不错的完成作业参考

2019-08-18


喵君儿🤣

import numpy as np


persontype = np.dtype({


    'names':['name', 'chinese', 'english', 'math'],


    'formats':['S32', 'i', 'i', 'i']})


peoples = np.array([("ZhangFei",66,65,30),("GuanYu",95,85,98),


       ("ZhaoYun",93,92,96),("HuangZhong",90,88,77),("dianwei",80,90,90)],


    dtype=persontype)


chineses = peoples[:]['chinese']


englishs = peoples[:]['english']


maths = peoples[:]['math']


print (np.mean(chineses))


print (np.amin(chineses))


print (np.amax(chineses))


print (np.std(chineses))


print (np.var(chineses))


print (np.mean(maths))


print (np.mean(englishs))


作者回复：正确 赞下完成作业的同学

2019-08-17


Bayes


老师用的 python2，如果你用的是 python3 的话，print 的语法是不一样的。

教大家一个小技巧，可以快速把所有 print 语法切换成 python3 的：

在 vscode（其它 IDE 也应该是可以的）上 ctrl + f 打开搜索，然后在搜索框里用正则表达式，并输入 `print (.+)`，

然后在切换框里输入 `print ($1)`，然后点击替换全部就 ok 了。

作者回复：这个方法不错

2019-08-15


隰有荷

老师，那个轴到底啥意思啊，按照跨行跨列去理解，在 ptp（）函数求最大最小值的差时，感觉还是对不上啊，另外，是不是可以理解成，数组的维度就是轴数？

2019-08-08


kaixiao7


老师，在结构化数组中，np.mean (peoples [:]['age']) 与 np.mean (peoples ['age']) 结果一致，不明白您基于什么原因使用了分片复制？

作者回复：两个都可以

2019-08-01


鱼儿

1 使用 numpy 不用 python 自带的列表 list 的原因。

因为列表 list 相当于数组，python 虽然省去了指针操作，但数组有指针，这样如果存储一个数组，还需要存储其空间及数组的值，浪费内存和计算效率。

另外从内存来看，列表 list 中的元素是随机存储的，而

作者回复：鱼儿同学很好的说明

2019-07-29


Harry_Lui


import numpy as np


persontype = np.dtype({


    'names':['name','chinese', 'math', 'english'],


    'formats':['S32', 'i', 'i', 'i']})


peoples = np.array([("ZhangFei",66,65,30),("GuanYu",95,85,98),


       ("ZhaoYun",93,92,96),("HuangZhong",90,88,77),("Dianwei",80,90,90)],


    dtype=persontype)


chinese = peoples[:]['chinese']


math = peoples[:]['math']


english = peoples[:]['english']


def show(name,score):


    print(name)


    print (np.mean(score))


    print (np.min(score))


    print (np.max(score))


    print(np.var(score))


    print (np.std(score))


show('chinese',chinese)


show('math',math)


show('english',english)


ranking = sorted(peoples,key=lambda x:x[1]+x[2]+x[3], reverse=True)


print(ranking)


2019-07-26


王张

# -*- coding: utf-8 -*-


import numpy as np


persontype = np.dtype({'names':['name', 'chinese', 'english', 'math'],'formats':['S32','i','i','i']})


student = np.array([("1zf",32,75,100),("1gy",24,85,96),


       ("1zy",28,85,92),("hz1",29,65,85)],


                   dtype=persontype)


# print(student)


na = student[:]['name']


ch = student[:]['chinese']


englishs = student[:]['english']


maths = student[:]['math']


print("-----------")


print(na[0])


print(ch)


print(englishs)


print(maths)


print("==========")


print ("语文平均成绩"+str (np.mean (ch)))

print ("英语平均成绩"+str (np.mean (englishs)))

print ("数学平均成绩"+str (np.mean (maths)))

print ("语文最高分"+str (np.max (ch)))

print ("英语最高分"+str (np.max (englishs)))

print ("数学最高分"+str (np.max (maths)))

print ("语文最低分"+str (np.min (ch)))

print ("英语最低分"+str (np.min (englishs)))

print ("数学最低分"+str (np.min (maths)))

print ("语文方差"+str (np.var (ch)))

print ("语文标准差"+str (np.std (ch)))

print ("英语方差"+str (np.var (englishs)))

print ("英语标准差"+str (np.std (englishs)))

print ("数学方差"+str (np.var (maths)))

print ("数学标准差"+str (np.std (maths)))

total = np.add(ch,englishs,maths)


print(total)


print(len(total))


sort1 = np.sort(total)


print ("排序")

for i in range(len(sort1)):


    if sort1[i]==total[0]:


        print(na[0])


print ("总分为")

        print(sort1[i])


    if sort1[i]==total[1]:


        print(na[1])


print ("总分为")

        print(sort1[i])


    if sort1[i]==total[2]:


        print(na[2])


print ("总分为")

        print(sort1[i])


    if sort1[i]==total[3]:


        print(na[3])


print ("总分为")

        print(sort1[i])


作者回复：赞下写作业的同学

2019-07-20


王张

ages = peoples [:]['age'] 这个切割成数组能解释一下么？

作者回复：写成这样也可以：

ages = peoples['age']


chineses = peoples['chinese']


maths = peoples['math']


englishs = peoples['english']


2019-07-20


图·美克尔

import numpy as np


students = np.array ([(' 张飞 ', 66, 65, 30),

(' 关羽 ', 95, 85, 98),

(' 赵云 ', 93, 92, 96),

(' 黄忠 ', 90, 88, 77),

(' 典韦 ', 80, 90, 90)],

dtype=[(' 姓名 ', 'U10'),

(' 语文 ', 'i2'),

(' 英语 ', 'i2'),

(' 数学 ', 'i2')])

chinese = students [:][' 语文 ']

maths = students [:][' 数学 ']

english = students [:][' 英语 ']

names = students [:][' 姓名 ']

sorted_score = sorted(map(lambda x: (x[0], sum(tuple(x)[1:])), students),


                      key=lambda x:x[1],


                      reverse=True)


print(sorted_score)


作者回复: Good Job

2019-07-15


小侠龙旋风

老师，你这篇只能算是对 numpy 的简述吧～

我看了这个网址（https://www.numpy.org.cn/article/basics/python_numpy_tutorial.html）关于 numpy 的介绍，发现 numpy 的功能真的很丰富！

作者回复：对 NumPy 功能很强大，这个算是简化版

2019-07-13


小侠龙旋风

import numpy as np


stu_type = np.dtype({


    'names': ['name', 'chinese', 'english', 'math'],


    'formats': ['S32', 'i', 'i', 'i']


})


stu_array = np.array(


    [


        ('zhangfei', 66, 65, 30),


        ('guanyu', 95, 85, 98),


        ('zhaoyun', 93, 92, 96),


        ('huangzhong', 90, 88, 77),


        ('dianwei', 80, 90, 90)


    ],


    dtype=stu_type


)


stu = stu_array[:]


chinese_score = stu['chinese']


english_score = stu['english']


math_score = stu['math']


print (' 平均成绩：语文 % s 英语 % s 数学 % s'%(np.mean (chinese_score),np.mean (english_score),np.mean (math_score)))

print (' 最小成绩：语文 % s 英语 % s 数学 % s'%(np.min (chinese_score),np.min (english_score),np.min (math_score)))

print (' 最大成绩：语文 % s 英语 % s 数学 % s'%(np.max (chinese_score),np.max (english_score),np.max (math_score)))

print (' 方差：语文 % s 英语 % s 数学 % s'%(np.var (chinese_score),np.var (english_score),np.var (math_score)))

print (' 标准差：语文 % s 英语 % s 数学 % s'%(np.std (chinese_score),np.std (english_score),np.std (math_score)))

print (' 排名:')

rank = sorted(stu, key=lambda s:s[1]+s[2]+s[3], reverse=True)


print(rank)


作者回复: Good Job

2019-07-13


邢雪

作业答案如下，Python3 写的，哪里有笨拙的地方请各位大神多多指教

import numpy as np


persontype = np.dtype({


'names':['name','chinese','english','math'],


'formats':['S32','i','i','i']})


peoples = np.array([("zhangfei",66,65,30),("guanyu",95,85,98),("zhaoyun",93,92,96),


("huangzhong",90,88,77),("dianwei",80,90,90)],dtype = persontype)


chineses = peoples[:]['chinese']


englishs = peoples[:]['english']


maths = peoples[:]['math']


zhangsum = np.sum(peoples[0][1]+peoples[0][2]+peoples[0][3])


guansum = np.sum(peoples[1][1]+peoples[1][2]+peoples[1][3])


zhaosum = np.sum(peoples[2][1]+peoples[2][2]+peoples[2][3])


huangsum = np.sum(peoples[3][1]+peoples[3][2]+peoples[3][3])


diansum = np.sum(peoples[4][1]+peoples[4][2]+peoples[4][3])


zsum = np.array([zhangsum,guansum,zhaosum,huangsum,diansum])


#计算平均值

print ("语文平均分:{}".format (np.mean (chineses)))

print ("英语平均分:{}".format (np.mean (englishs)))

print ("数学平均分:{}".format (np.mean (maths)))

print ("语文成绩最小值:{}".format (np.amin (chineses,0)))

print ("英语成绩最小值:{}".format (np.amin (englishs,0)))

print ("数学成绩最小值:{}".format (np.amin (maths,0)))

print ("语文成绩最大值:{}".format (np.amax (chineses,0)))

print ("英语成绩最大值:{}".format (np.amax (englishs,0)))

print ("数学成绩最大值:{}".format (np.amax (maths,0)))

print ("语文成绩标准差:{}".format (np.std (chineses)))

print ("语文成绩方差:{}".format (np.var (chineses)))

print(np.sort(zsum))


作者回复: Good Job 能完成就 OK

2019-07-11


邢雪

老师您好，我不太懂 axis = 0，跨行，比如下面这个数组

a = np.array([[4,3,2],[2,4,1]]）


2019-07-11


twelve


#-*- coding:utf-8 -*-


import numpy as np


person = np.dtype({'names':['name','Chinese','English','Maths'],'formats':['S32','f','f','f']})


persons = np.array([(u"zhangfei",66,65,30),(u'guanyu',95,85,98),(u'zhaoyu',93,92,96),(u'huangzhong',90,88,77),(u'dianwei',80,90,90)],dtype=person)


print ("语文平均成绩：%.4f，最低分：%.4f，最高分：%.4f，方差:%.4f，标准差:%.4f" % (np.mean (persons [:]['Chinese']),np.amin (persons [:]['Chinese']),np.amax (persons [:]['Chinese']),np.std (persons [:]['Chinese']),np.var (persons [:]['Chinese'])))

print ("英语平均成绩：%.4f，最低分：%.4f，最高分：%.4f，方差:%.4f，标准差:%.4f" % (np.mean (persons [:]['English']),np.amin (persons [:]['English']),np.amax (persons [:]['English']),np.std (persons [:]['English']),np.var (persons [:]['English'])))

print ("数学平均成绩：%.4f，最低分：%.4f，最高分：%.4f，方差:%.4f，标准差:%.4f" % (np.mean (persons [:]['Maths']),np.amin (persons [:]['Maths']),np.amax (persons [:]['Maths']),np.std (persons [:]['Maths']),np.var (persons [:]['Maths'])))

print(sorted(persons,key=lambda x:x[1]+x[2]+x[3],reverse=True))


作者回复: Good Job

2019-07-09


Cynthia_Zhang


我使用的是 Python3，但是依然在中文「张飞」那里保存，编码错误 "ascii code can't encode......" 什么的，查询后台又的确是用 utf-8 编码，感觉很绝望

作者回复: "张飞" 的 formats 是 "U32"，"zhangfei" 的 formats 是 "S32" 你可以再试一下

2019-07-07


Ben


如果考虑完全用 NumPy 来解决排名的问题，前提条件是没有限制说不能使用本课程以外的方法的话，那么可以这么解决:

求和应该可以转化为求平均值来解决，所以贴出关键步骤如下:

chinese = samples[:]['chinese']


english = samples[:]['english']


math = samples[:]['math']


rankings = np.argsort(-np.mean([chinese, english, math], axis=0))


print(samples[rankings])


关键点:

1. 通过 np.mean 来求平均值

2. 通过 np.argsort 来排序，并且采用倒序，获取到分数从高到低的索引作为名次

不知道这样合适不合适

2019-07-04


内存爆了

这是我的练习题，请老师点评哈：

import numpy as np


persontype = np.dtype({


    'names':['name','chinese','english','math'],


    'formats':['S32','i','i','f']


})


peoples = np.array([("ZhangFei",66,65,30),("GuanYu",95,85,98),


                    ("ZhaoYun",93,92,96),("HuangZhong",90,88,77),


                    ("DianWei",80,90,90)],dtype=persontype)


chineses = peoples[:]['chinese']


englishs = peoples[:]['english']


maths = peoples[:]['math']


# 平均成绩

print(np.mean(chineses))


print(np.mean(englishs))


print(np.mean(maths))


# 输出最小值和最大值

print(np.amin(chineses))


print(np.amin(englishs))


print(np.amin(maths))


print(np.amax(chineses))


print(np.amax(englishs))


print(np.amax(maths))


# 计算方差和标准差

stdchinese = np.array(chineses)


print(np.std(stdchinese))


print(np.var(stdchinese))


stdenglish = np.array(englishs)


print(np.std(stdenglish))


print(np.var(stdenglish))


stdmath = np.array(maths)


print(np.std(stdmath))


print(np.var(stdmath))


# 排序

a = np.array([chineses,englishs,maths])


print(np.sort(a))


# 运行结果

84.8


84.0


78.2


66


65


30.0


95


92


98.0


10.721940122944169


114.96000000000001


9.777525249264253


95.6


25.190474


634.55994


作者回复: Good Job

2019-07-01


华夏

参考了各位同学的代码。

import numpy as np


persontype = np.dtype({


    'names':['name', 'chinese', 'english', 'math', 'total'],


    'formats':['S32', 'i', 'i', 'i', 'i']})


peoples = np.array([('ZhangFei', 66, 65, 30, 0), ('GuanYu', 95, 85, 98, 0), ('ZhaoYun', 92, 93, 96, 0),


                    ('HuangZhong', 90, 88, 77, 0), ('DianWei', 80, 90, 90, 0)],dtype=persontype)


chineses = peoples[:]['chinese']


englishes = peoples[:]['english']


maths = peoples[:]['math']


peoples[:]['total']= peoples[:]['chinese']+peoples[:]['english']+peoples[:]['math']


def show(name,cj):


    print('{} | {} | {} | {} | {} | {} '


          .format(name,np.mean(cj),np.min(cj),np.max(cj),np.var(cj),np.std(cj)))


print ("科目 | 平均成绩 | 最小成绩 | 最大成绩 | 方差 | 标准差")

show ("语文", chineses)

show ("英语", englishes)

show ("数学", maths)

print(np.sort(peoples, order=('total'))[::-1])


作者回复：整理的不错，创建了 total 字段，作为三门成绩的总和，然后按照 total 排序输出

2019-06-28


Zee


使用 [:] 并没有将数组拷贝出来，修改数组元素后，peoples 里的值也跟着改变了

2019-06-26


Geek_fbe6fe


personTypeNew=np.dtype({


    'names':['name','english','chinese','math'],


    'formats':['S32','i','i','i']})


peoples=np.array([('zhangfei',66,65,30),('guanyu',95,85,89),('zhaoyun',93,92,96),('huangzhong',90,88,77),('dianwei',80,90,90)],dtype=personTypeNew)


chineses=peoples[:]['chinese']


maths=peoples[:]['math']


englishes=peoples[:]['english']


print ('Average chinese is %f' %np.mean(chineses))


print ('Average maths is %f' %np.mean(maths))


print ('Average english is %f' %np.mean(englishes))


print ('Chinese is max %d and min %d' %(np.max(chineses),np.min(chineses)))


print ('Maths is max %d and min %d' %(np.max(maths),np.min(maths)))


print ('English is max %d and min %d' %(np.max(englishes),np.min(englishes)))


peoplesArray=np.array([(66,65,30),(95,85,89),(93,92,96),(90,88,77),(80,90,90)])


print ('Std is %f' %np.std(peoplesArray))


print ('Var is %f' %np.var(peoplesArray))


total = np.add(chineses,maths)


total = np.add(total,englishes)


print total


print np.sort(total)


2019-06-26


lycan


陈老师，我用您上边写的先定义 dtype 再定义 array 的方法定义数组，但是包含中文就会报错，具体代码是这样的

import numpy as np


class_type = np.dtype({


    'names': ['name', 'chinese', 'math', 'english'],


    'formats': ['S32', 'i', 'i', 'i']


})


class_array = np.array(


[("张飞", 66, 30, 65), ("关羽", 95, 98, 85), ("赵云", 93, 96, 92), ("黄忠", 90, 77, 88), ("典韦", 80, 90, 90)],

    dtype=class_type)


报错信息是

Traceback (most recent call last):


  File "E:/Subject/python/NumpyDemo/NumpyDemo.py", line 10, in <module>


    dtype=class_type)


UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)


我的环境是 python3.7.3，默认编码是 utf-8

希望老师能给解答或者提供方向，谢谢

作者回复：一个很好的问题，如果数值为中文，你可以把 S32 改成 U32，即：

class_type = np.dtype({


    'names': ['name', 'chinese', 'math', 'english'],


    'formats': ['U32', 'i', 'i', 'i']


})


这里实际上用的是 numpy 中的字符编码来表示数据类型的定义，比如 S 代表字符串类型，U 代表 Unicode。如果你采用了中文，需要使用 U32。

2019-06-25


白勇坤

import numpy as np


peopletype = np.dtype({


    'names': ['name', 'chinese', 'English', 'math'],


    'formats': ['S32', 'i', 'i', 'i']})


peoples = np.array([('zhangfei','66','65','30'),


                    ('guanyun','95','85','98'),


                    ('zhaoyun','93','92','96'),


                    ('huangzhong','90','88','77'),


                    ('dianwei','80','90','90')],


                   dtype=peopletype)


chinese = peoples[:]['chinese']


English = peoples[:]['English']


math = peoples[:]['math']


rank = sorted(peoples,key=lambda x:x[1]+x[2]+x[3],reverse=True)


name_rank = np.array(ranking)[:]['name']


print ("平均成绩：语文 {}, 英语 {}, 数学 {}".format (np.mean (chinese),np.mean (English),np.mean (math)))

print ("最小成绩：语文 {}, 英语 {}, 数学 {}".format (np.amin (chinese),np.amin (English),np.amin (math)))

print ("最大成绩：语文 {}, 英语 {}, 数学 {}".format (np.amax (chinese),np.amax (English),np.amax (math)))

print ("方差：语文 {}, 英语 {}, 数学 {}".format (np.var (chinese),np.var (English),np.var (math)))

print ("标准查：语文 {}, 英语 {}, 数学 {}".format (np.std (chinese),np.std (English),np.std (math)))

print ("总成绩排序 {}".format (name_rank))

作者回复: Good Job

2019-06-16


建强

老师，你好，再问一个问题，如果应用系统是基于 JAVA 框架开发的，要在这样的应用系统中嵌入用 Python 做数据分析的功能，是不是就行不通？

作者回复：你可以用接口调用吧

2019-06-15


Nickkkwen


dtype 里面的 format 可以给解释一下么 S32 i i 这些

2019-06-05


Nickkkwen


轴的那个部分 有点不清楚

2019-06-03


Wing·三金

import numpy as np


mytype = np.dtype({'names': ('name', 'Chinese', 'English', 'Math'),


                  'formats': ('U2', 'i', 'i', 'i')})


# note that outer-listed(rather than tupled) if necessary for np to recognize here


data = [(' 张飞 ', 66, 65, 30),

(' 关羽 ', 95, 85, 98),

(' 赵云 ', 93, 92, 96),

(' 黄忠 ', 90, 88, 77),

(' 典韦 ', 80, 90, 90)]

grades = np.array(data, dtype=mytype)


def stata(subjects, func):


    global grades


    result = np.array([.0] * len(subjects))


    for i, sub in enumerate(subjects):


        result[i] = func(grades[sub])


    


    return result


subjects = mytype.names[1:]


avg_grade = stata(subjects, np.mean)


min_grade = stata(subjects, np.amin)


max_grade = stata(subjects, np.amax)


var_grade = stata(subjects, np.var)


std_grade = stata(subjects, np.std)


# tot_grade = np.sum(grades[:][1:], axis=1)


ranking_grade = sorted(grades, key=lambda x: x[1] + x[2] + x[3], reverse=True)


print('{: <10s}{: <10s}{: <10s}{: <10s}'.format('Stata', 'Chinese', 'English', 'Math'))


print('{0: <10s}{1[0]: <10.1f}{1[1]: <10.1f}{1[2]: <10.1f}'.format('Average', avg_grade))


print('{0: <10s}{1[0]: <10.1f}{1[1]: <10.1f}{1[2]: <10.1f}'.format('Min', min_grade))


print('{0: <10s}{1[0]: <10.1f}{1[1]: <10.1f}{1[2]: <10.1f}'.format('Max', max_grade))


print('{0: <10s}{1[0]: <10.1f}{1[1]: <10.1f}{1[2]: <10.1f}'.format('Variance', var_grade))


print('{0: <10s}{1[0]: <10.1f}{1[1]: <10.1f}{1[2]: <10.1f}'.format('Std', std_grade))


print('\nRanking')


for i, person in enumerate(ranking_grade):


    print(i + 1, person)


# output


Stata Chinese English Math


Average 84.8 84.0 78.2


Min 66.0 65.0 30.0


Max 95.0 92.0 98.0


Variance 115.0 95.6 634.6


Std 10.7 9.8 25.2


Ranking


1 (' 赵云 ', 93, 92, 96)

2 (' 关羽 ', 95, 85, 98)

3 (' 典韦 ', 80, 90, 90)

4 (' 黄忠 ', 90, 88, 77)

5 (' 张飞 ', 66, 65, 30)

作者回复: Good Job

2019-06-02


Andre


还是说现在这个已经不维护了？

2019-06-02


nissan


想請問統計统计一个班级里面学生的姓名、年龄，以及语文、英语、数学成绩该...

當中的

```


'formats':['S32','i', 'i', 'i', 'f']})


```


是什麼意思啊？S32，i 是亂取的名字，還是另外有意思呢？

作者回复: formats 后面跟的是数据类型，i 代表 integer 整数类型，f 代表 float 浮点类型，S32 代表的是字符串类型

2019-05-29


寻心

# -*- coding: utf-8 -


import numpy as np


names = np.array ([' 张飞 ', ' 关羽 ', ' 赵云 ', ' 黄忠 ', ' 典韦 '])

subjects = np.array ([' 语文 ', ' 英语 ', ' 数学 '])

scores = np.array([[66, 65, 30], [95, 85, 98], [93, 92, 96], [90, 88, 77], [80, 90, 90]])


for index in range(len(subjects)):


print subjects [index] + ' 平均分:{}'.format (np.mean (scores, 0)[index])

print subjects [index] + ' 最小成绩:{}'.format (np.amin (scores, 0)[index])

print subjects [index] + ' 最大成绩:{}'.format (np.amax (scores, 0)[index])

print subjects [index] + ' 方差:{}'.format (np.var (scores, 0)[index])

print subjects [index] + ' 标准差:{}'.format (np.std (scores, 0)[index])

print ''


totalScores = []


for index in range(len(names)):


name = names[index]


totalScore = np.sum(scores, 1)[index]


totalScores.append([index, totalScore])


totalScores = np.array(totalScores)


totalScores = totalScores[np.argsort(-totalScores[...,1])]


print ' 总分排行 '

for array in totalScores:


print names [array [0]] + ' 总分:{}'.format (array [1])

# 不清楚的地方

# 1. 多维数组的轴

# 2. 多维数组的切片

# 3. 多维数组排序算法

2019-05-27


云深不知处

学习完 numpy，自己敲完实例代码和课后练习题，突然想起学校学习的 R 语言，感觉功能是一样的

作者回复：对 功能差不多

2019-05-27


大斌

我大概是写的最繁琐的吧：（应该还有很多可以优化的地方）

-------------- 代码分割线 --------------

import numpy as np


persontype = np.dtype({'names': ['name', 'chinese', 'english', 'math'], 'formats': [


                      'S32', 'i', 'i', 'i', 'f']})


people = np.array([('zhangfei', 66, 65, 30), ('guanyu', 95, 85, 98),


                   ('zhaoyun', 93, 92, 96), ('huangzhong', 90, 88, 77), ('dianwei', 80, 90, 90)], dtype=persontype)


chinese_average = people[:]['chinese']


english_average = people[:]['english']


math_average = people[:]['math']


score_list = [chinese_average, english_average, math_average]


sub_list = [' 语文 ', ' 英语 ', ' 数学 ']

# 对每个要实现的功能函数进行封装

def get_mean(lists, kind):


    for l in range(len(lists)):


print ("{}{} 是 {}".format (sub_list [l], kind, score_list [l].mean ()))

def get_min(lists, kind):


    for l in range(len(lists)):


print ("{}{} 是 {}".format (sub_list [l], kind, score_list [l].min ()))

def get_max(lists, kind):


    for l in range(len(lists)):


print ("{}{} 是 {}".format (sub_list [l], kind, score_list [l].max ()))

def get_var(lists, kind):


    for l in range(len(lists)):


print ("{}{} 是 {}".format (sub_list [l], kind, score_list [l].var ()))

def get_std(lists, kind):


    for l in range(len(lists)):


print ("{}{} 是 {}".format (sub_list [l], kind, score_list [l].std ()))

sum_score_list = {}


# 获取每个人的总分并排名字典进行输出

def get_person_sum_score():


    for l in range(len(people)):


        score = sum(list(people[l])[1:])


        sum_score_list[people[l][0]] = score


    score_rank = sorted(sum_score_list.items(), key=lambda d: d[1])


    for s in score_rank[::-1]:


        print(s)


get_mean (sub_list, "平均值")

get_min (sub_list, "最低分")

get_max (sub_list, "最高分")

get_var (sub_list, "方差")

get_std (sub_list, "标准差")

get_person_sum_score()


作者回复：加油 可以的

2019-05-16


naku


练习题前面就不写了，排序的写法，刚弄出个新写法

studenttype=np.dtype({'names':['name', 'chinese', 'english', 'math', 'total'], 'formats':['S32', 'i','i','i','i']})


students=np.array([('zf', 66, 65, 30, 0), ('gy', 95,85,98,0),('hz', 90, 88, 77,0),('dw', 80, 90, 90,0)], dtype=studenttype)


chi,eng,math=students[:]['chinese'],students[:]['english'],students[:]['math']


students[:]['total']=chi+eng+math


np.sort (students, order='total') #升序

np.sort (students, order='total')[::-1] #降序

作者回复: Good Job

2019-05-16


naku


+= 这个操作符确实好用，l=[1,2,3]

def f1():


   l+=[4]


def f2(l):


   l=l+[4]


f1 (l) 运行完后结果是 [1,2,3,4]，f (2) 运行完后 l 没有变还是 1,2,3。昨天才做的这个练习题，今天又加深了下印象，也分享给大家。l 只是内存里某个对象的标签。

老师给的例子是 y=x*2 和 x*=2，其实 x=x*2 和 x*=2 也是一样的，= 操作都会重新分配内存。

np.dtype 的结构化定义方法：

np.dtype ({'names': [...], 'formats':[...]}) type 有 S32, i...

list 中的元素在内存中是分块的不是连续的

axis=0，这个之前在 stackoverflow 看到了一个解释是大致这样的，0 代表第一个维度，二位数组里面代表的是行，但我们实际 axis=0 用到时却是列，那是因为 axis=0 代表的是其他维度都已经确定的情况下，该维度可能发生的情况，数组集合。用这个来解释的话，就是 axis=0 时，其他维度就是 1。其他维度工有 column1, column2... 这么多种情况。column=1 时，维度 0 发生的情况，就是第一列了。也就是老师说的把它分成好多个列元素这种解释了。

作者回复：整理总结的不错

2019-05-16


Cmilla


S32 就报错了，是问什么呢

作者回复：中文的话用 U32

2019-05-14


rex_zhong


后面留言区有人说过 = min+(max_min)*70%, 这是不正确的，应该有专门的公式之类的，请老师帮忙答疑

2019-05-14


sky


为什么加 total 那一列一直出错

peoples[:]['total']=peoples[:]['chinese']+peoples[:]['english']+peoples[:]['math']


错误是 total 不存在

2019-05-14


羊小看

import numpy as np


person_dtype=np.dtype ({'names':[' 姓名 ',' 语文 ',' 数学 ',' 英语 ',' 总分 '],\

                       'formats':['S32',int,int,int,int]})


person=np.array([('zhangfei',66,65,30,0),('guanyu',95,85,98,0),('zhaoyun',93,92,96,0),\


                 ('huangzhong',90,88,77,0),('dianwei',80,90,90,0)],dtype=person_dtype)


def subject_compute(subject):


    mean_score=np.mean(person[subject])


    min_score=np.min(person[subject])


    max_score=np.max(person[subject])


    var_score=np.var(person[subject])


    std_score=np.std(person[subject])


print ('% s 的均值是：% d'%(subject,mean_score))

print ('% s 的最小值是：% d'%(subject,min_score))

print ('% s 的最大值是：% d'%(subject,max_score))

print ('% s 的方差是：% d'%(subject,var_score))

print ('% s 的标准差是：% d'%(subject,std_score))

    


subject=[' 语文 ',' 数学 ',' 英语 ']

for s in subject:


    subject_compute(s)


person [' 总分 ']=np.sum ([person [' 语文 '],person [' 数学 '],person [' 英语 ']],axis=0)

print (np.sort (person,order=' 总分 '))

看了老师的留言，在初始化时就添加了 ' 总分 ' 字段；姓名想写中文的，但类型老出错，'U' 没报错，可是姓名空白了，没显示出来

2019-05-07


羊小看

import numpy as np


person_dtype=np.dtype ({'names':[' 姓名 ',' 语文 ',' 数学 ',' 英语 '],\

                       'formats':['U','i1','i1','i1']})


person=np.array ([(' 张飞 ',66,65,30),(' 关羽 ',95,85,98),(' 赵云 ',93,92,96),\

(' 黄忠 ',90,88,77),(' 典韦 ',80,90,90)],dtype=person_dtype)

def subject_compute(subject):


    mean_score=np.mean(person[subject])


    min_score=np.min(person[subject])


    max_score=np.max(person[subject])


    var_score=np.var(person[subject])


    std_score=np.std(person[subject])


print ('% s 的均值是：% d'%(subject,mean_score))

print ('% s 的最小值是：% d'%(subject,min_score))

print ('% s 的最大值是：% d'%(subject,max_score))

print ('% s 的方差是：% d'%(subject,var_score))

print ('% s 的标准差是：% d'%(subject,std_score))

    


subject=[' 语文 ',' 数学 ',' 英语 ']

for s in subject:


    subject_compute(s)


sum_score=np.sum ([person [' 语文 '],person [' 数学 '],person [' 英语 ']],axis=0)

print(np.sort(sum_score))


不知道怎么把计算的总成绩添到原来的数组中去，看下大家答案喽

2019-05-07


虎皮青椒

#!/Users/wjunicorn/anaconda3/bin/python3


#coding=utf-8


import numpy as np


studenttype = np.dtype({


'names': ['name', 'chinese', 'english', 'math'],


    'formats': ['U32', 'f', 'f', 'f']})


students = np.array ([("张飞", 66, 65, 30),

("关羽", 95, 85, 98),

("赵云", 93, 92, 96),

("黄忠", 90, 88, 77),

("典韦", 80, 90, 90)],

dtype = studenttype)


#统计下这些人在语文、英语、数学中的的平均成绩、最小成绩、最大成绩、方差、标准差

satistic_ths = ("Course", "Average Score", "Min Score", "Max Score", "Variance", "Standard Deviation")


print("{0:{1}^20}+{0:{1}^15}+{0:{1}^11}+{0:{1}^11}+{0:{1}^10}+{0:{1}^20}".format('', '-'))


print("{:^20}|{:^15}|{:^11}|{:^11}|{:^10}|{:^20}".format(*satistic_ths))


print("{0:{1}^20}+{0:{1}^15}+{0:{1}^11}+{0:{1}^11}+{0:{1}^10}+{0:{1}^20}".format('', '-'))


for course_name in studenttype.names[1:]:


print("{:^20}|{:>15.1f}|{:>11.1f}|{:>11.1f}|{:>10.1f}|{:>20.1f}"


.format(course_name,


np.mean(students[:][course_name]),


np.amin(students[:][course_name]),


np.amax(students[:][course_name]),


np.var(students[:][course_name]),


np.std(students[:][course_name])))


print("{0:{1}^20}+{0:{1}^15}+{0:{1}^11}+{0:{1}^11}+{0:{1}^10}+{0:{1}^20}".format('', '-'))


#把这些人的总成绩排序，得出名次进行成绩输出

sort_ths = ("No", "Name", "Chinese", "English", "Math", "Total")


print("{0:{1}^5}+{0:{1}^19}+{0:{1}^20}+{0:{1}^20}+{0:{1}^20}+{0:{1}^20}".format('', '-'))


print("{:^5}|{:^19}|{:^20}|{:^20}|{:^20}|{:^20}".format(*sort_ths))


print("{0:{1}^5}+{0:{1}^19}+{0:{1}^20}+{0:{1}^20}+{0:{1}^20}+{0:{1}^20}".format('', '-'))


no = 1


for student in sorted(students, key = lambda x : x[1] + x[2] + x[3], reverse = True):


print("{:^5d}|{:^18}|{:>20.1f}|{:>20.1f}|{:>20.1f}|{:>20.1f}"


.format(no,


student['name'],


student['chinese'],


student['english'],


student['math'],


student['chinese'] + student['english'] + student['math']))


no += 1


print("{0:{1}^5}+{0:{1}^19}+{0:{1}^20}+{0:{1}^20}+{0:{1}^20}+{0:{1}^20}".format('', '-'))


作者回复: Good Job

2019-04-18


萍仔

遇到！entry nota 2- or 3-tuple

2019-04-17


张晓辉

import numpy as np


persontype = np.dtype({'names':['name', 'chinese', 'english', 'math', 'totalscore'],


                      'formats':['S32', 'f', 'f', 'f', 'f']})


scorearray = np.array([('Zhang Fei', 66, 65, 30, 0), ('Guan Yu', 95, 85, 98, 0),


                       ('Zhao Yun', 93, 92, 96, 0), ('Huang Zhong', 90, 88, 77, 0),


                      ('Dian Wei', 80, 90, 90, 0)], dtype=persontype)


chineses = scorearray[:]['chinese']


englishes = scorearray[:]['english']


mathes = scorearray[:]['math']


scorearray[:]['totalscore'] = chineses + englishes + mathes


print(scorearray)


for col in scorearray.dtype.names:


    if col == 'name' or col == 'totalscore':


        continue


    colarray = scorearray[:][col]


    print("Mean of {}:{}".format(col, np.mean(colarray)))


    print("Amax of {}:{}".format(col, np.amax(colarray)))


    print("Amin of {}:{}".format(col, np.amin(colarray)))


    print("Std of {}:{}".format(col, np.std(colarray)))


    print("Var of {}:{}".format(col, np.var(colarray)))


print(np.sort(scorearray, order='totalscore'))


作者回复: Good Job

2019-04-17


三木

用竖杆隔开看着有点怪怪的 可能是我强迫症了

用 PrettyTable 制成表格

from prettytable import PrettyTable


import numpy as np


persontype = np.dtype({


    'names': ['name', 'chinese', 'english', 'math'],


    'formats': ['S32', 'i', 'i', 'i']})


peoples = np.array([("ZF", 66, 65, 30), ("GY", 95, 85, 98),


                    ("ZY", 93, 92, 96), ("HZ", 90, 88, 77), ("DW", 80, 90, 90)],


                   dtype=persontype)


name = peoples[:]['name']


chineses = peoples[:]['chinese']


englishs = peoples[:]['english']


maths = peoples[:]['math']


x = PrettyTable (["科目", "平均成绩", "最小成绩", "最大成绩", "方差", "标准差"])

def add(name,cj):


    x.add_row([name, np.mean(cj), np.min(cj), np.max(cj), np.var(cj), np.std(cj)])


add ("语文", chineses)

add ("英语", englishs)

add ("数学", maths)

print(x)


print ("排名:")

#用 sorted 函数进行排序

ranking = sorted(peoples,key=lambda x:x[1]+x[2]+x[3], reverse=True)


print(ranking)


作者回复: Good Job PrettyTable 用的不错

2019-04-16


泄矢的呼啦圈

【提问】

对于练习题中求和排序的部分没有做出来，主要是纠结于如何让求和不依赖于下标（如 mickey 同学所答），老师在评论区给出的添加字段 total 的方法，我也实践了，类似于关系数据库中在原始数据表中使用计算结果当索引字段。考虑当统计指标的计算方法不变，参与统计的数据发生变化（比如练习题中改成按语言学科总分来进行排名，就需要修改 total 的计算。计算方法不变，参与计算的数据变化）以及增加了其他类型的统计（新的计算方法），应该还是需要将计算结果重新建表存储比较好吧，所以我还在找能否通过科目名称集合（下面解答中的 subjects，当参与计算的科目成绩发生变化时只需要改变 subjects）对原始数据进行统计排名。才开始学习 python，没找到好的思路，还请老师指教。

【练习题交作业】

import numpy as np


studentType = np.dtype({


'names': [' 名字 ', ' 语文 ', ' 英语 ', ' 数学 '],

    'formats': ['U32', 'i', 'i', 'i']})


students = np.array(


[(' 张飞 ', 66, 65, 30), (' 关羽 ', 95, 85, 98), (' 赵云 ', 93, 92, 96),

(' 黄忠 ', 90, 88, 77), (' 典韦 ', 80, 90, 90)],

    dtype=studentType)


def printstatistical(name, data):


    print(name, np.mean(data), np.amin(data), np.amax(data), np.var(data), np.std(data))


print (' 科目 ', ' 平均成绩 ', ' 最低分 ', ' 最高分 ', ' 方差 ', ' 标准差 ')

subjects = list(studentType.names[1:])


for subject in subjects:


    data = students[:][subject]


    printstatistical(subject, data)


作者回复: Good Job 简洁有效

2019-04-12


此处省略

#!/usr/bin/env python


# -*- encoding: utf-8 -*-


'''


@author:


'''


import numpy as np


a_type = np.dtype({"names":['name','cn','en','mt'],'formats':['S32','i','i','i']})


a = np.array([('zhangfei',66,65,30),('guangyu',95,85,98),('zhaoyun',93,92,96),('huangzhong',90,88,77),('dianwei',80,90,90)],dtype=a_type)


names = a[:]['name']


cn_list = a [:]['cn'] #语文

en_list = a [:]['en'] #英语

mt_list = a [:]['mt'] #数学

def test(_class,_list):


    print(


'{} 最小:{}'.format (_class,np.min (_list)),

' 最大:{}'.format (np.max (_list)),

' 平均:{}'.format (np.mean (_list)),

' 标准差:{}'.format (np.std (_list)),

' 方差:{}'.format (np.var (_list))

    )


test('cn',cn_list)


test('en',en_list)


test('mt',mt_list)


count = np.add(np.add(cn_list,en_list),mt_list)


b_type = np.dtype({"names":['name','count'],'formats':['S32','i']})


b = np.array([(names[i],count[i]) for i in range(len(names))],dtype=b_type)


print(np.sort(b,None,order='count'))


2019-04-11


青石

import numpy as np


person_type = np.dtype({'names': ['name', 'chinese', 'english', 'math', 'total'], 'formats': ['S32', 'i', 'i', 'i', 'i']


                        })


peoples = np.array([('ZhangFei', 66, 65, 30, 0), ('Guanyu', 95, 85, 98, 0), ('ZhaoYun', 93, 92, 96, 0), ('HuangZhong',


                    90, 88, 77, 0), ('DianWei', 80, 90, 90, 0)], dtype=person_type)


chinese = peoples[:]['chinese']


english = peoples[:]['english']


math = peoples[:]['math']


print(type(np.var(chinese)), type(np.amax(chinese)))


print(' %-20s %-20s %-20s' % ('chinese', 'english', 'math'))


print('AVG: %-20.2f %-20.2f %-20.2f' % (chinese.mean(), english.mean(), math.mean()))


print('MIN: %-20.2f %-20.2f %-20.2f' % (np.amin(chinese), np.amin(english), np.amin(math)))


print('MAX: %-20.2f %-20.2f %-20.2f' % (np.amax(chinese), np.amax(english), np.amax(math)))


print('VAR: %-20.2f %-20.2f %-20.2f' % (np.var(chinese), np.var(english), np.var(math)))


print('STD: %-20.2f %-20.2f %-20.2f' % (np.std(chinese), np.std(english), np.std(math)))


peoples[:]['total'] = chinese + english + math


s = np.sort(peoples, order='total')


print('%-20s %-20s %-20s' % ('Name', 'Score', 'Rank'))


for i in range(peoples.size):


    print('%-20s %-20.2f %-20d' % (


        s['name'][peoples.size - i - 1].decode('utf-8'), s['total'][peoples.size - i - 1], i + 1))


作者回复: Good Job

2019-04-10


媛

我是不是可以这样理解，numpy 就是在数组的基础上多了一些好用的方法？它的性质还是数组，不方便增删数据

2019-04-09


南瓜

作者回复：你在求三科成绩的各种统计指标的时候，写的不错

你提到的如何在 numpy 中求和，其实在定义结构数组的时候，可以多定义一列 total

peoples[:]['total'] = peoples[:]['chinese']+peoples[:]['english']+peoples[:]['math']


然后按照 total 进行排序即可

print np.sort(peoples, order='total')


老师，我按照你这里定义结构，出错了，提示有 5 个字段，而我只输入了三个。

sources_type = np.dtype({


    'names': ['name', 'chinese', 'english', 'math', 'total'],


    'formats': ['S32', 'i', 'i', 'i', 'i']})


sources = np.array([('zhangfei', 66, 65, 30), ('guanyu', 95, 85, 98),


            ('zhaoyun', 93, 92, 96), ('huangzhong', 90, 88, 77), ('dianwei', 80, 90, 90)] ,dtype=sources_type)


chineses = sources[:]['chinese']


maths = sources[:]['math']


englishs = sources[:]['english']


sources[:]['total'] = chineses + maths + englishs


2019-04-09


华

people 为什么不直接取值，要先切片再取值？

2019-04-05


chitanda


对于 axis，其实把数组放到一个二维坐标里就很好理解了。axis=0 是 x 轴，axis=1 是 y 轴

作者回复: Good Sharing

2019-04-05


刺客伍六七

y = np.linspace (1, 9, 5), y 的结果里数字后面怎么都有一个 .

[1. 3. 5. 7. 9.]


2019-04-04


_xiongyi


import numpy as np


a = np.array([1,2,3])


b = np.array([[1,2,3],[4,5,6],[7,8,9]])


b[1,1] = 10


print(a.shape)


print(b.shape)


print(a.dtype)


print(b)


输出的结果为：

(3,)


(3, 3)


int64


[[ 1 2 3]


 [ 4 10 6]


 [ 7 8 9]]


跟老师的不一样，我是用 Python3.7 下的 pycharm 运行的，求解

2019-03-31


曹舰航

import numpy as np


persontype = np.dtype({


    'names':['name', 'chinese', 'math', 'english'],


    'formats':['S32', 'i', 'i', 'i']})


peoples = np.array([("ZhangFei",66,65,30),("GuanYu",95,85,98),


       ("ZhaoYun",93,92,96),("HuangZhong",90,88,77),("Duanwei",80,90,90)],


    dtype=persontype)


chineses = peoples[:]['chinese']


maths = peoples[:]['math']


englishs = peoples[:]['english']


#平均成绩

print np.mean(chineses)


print np.mean(maths)


print np.mean(englishs)


#最小成绩

print np.amin(chineses)


print np.amin(maths)


print np.amin(englishs)


#最大成绩

print np.amax(chineses)


print np.amax(maths)


print np.amax(englishs)


#方差

print np.var(chineses)


print np.var(maths)


print np.var(englishs)


#标准差

print np.std(chineses)


print np.std(maths)


print np.std(englishs)


排名

x= peoples[:]['chinese']+peoples[:]['english']+peoples[:]['math']


print np.sort(x)


作者回复: Good Job

2019-03-26


蜉蝣

# -*- coding: utf-8 -*-


"""


Created on Sun Mar 24 20:17:31 2019


@author: Guan


"""


import numpy as np


def printf(structArr):


    """bytes -> str"""


    for struct in structArr:


        if not isinstance(struct, bytes):


            print([bytes.decode(one)


                   if isinstance(one, bytes) else one for one in struct])


        else:


            print(bytes.decode(struct))


def _(string):


    """str -> bytes"""


    return str.encode(string)


structType = np.dtype({"names": ["name", "chinese", "english", "math"],


                       "formats": ["S32", "f", "f", "f"]})


scoreTable = np.array ([(_("张飞"), 66, 65, 30),

(_("关羽"), 95, 85, 98),

(_("赵云"), 93, 92, 96),

(_("黄忠"), 90, 88, 77),

(_("典韦"), 80, 90, 90)], dtype=structType)

def get_average_by_subject(structArr, subject):


    print("subject: {} avg: {}".format(subject, np.average(structArr[:][subject])))


    


def get_min_by_subject(structArr, subject):


    print("subject: {} min: {}".format(subject, np.amin(structArr[:][subject])))


    


def get_max_by_subject(structArr, subject):


    print("subject: {} max: {}".format(subject, np.amax(structArr[:][subject])))


    


def get_var_by_subject(structArr, subject):


    print("subject: {} var: {}".format(subject, np.var(structArr[:][subject])))


    


def get_std_by_subject(structArr, subject):


    print("subject: {} std: {}".format(subject, np.std(structArr[:][subject])))


    


get_average_by_subject(scoreTable, "chinese")


get_min_by_subject(scoreTable, "chinese")


get_max_by_subject(scoreTable, "chinese")


get_var_by_subject(scoreTable, "chinese")


get_std_by_subject(scoreTable, "chinese")


printf(sorted(scoreTable, key=lambda x: x[1]+x[2]+x[3], reverse=True))


作者回复: Good Job

2019-03-24


听妈妈的话

```


import numpy as np


persontype = np.dtype({


    'names':['name', 'chinese', 'math', 'english'],


    'formats':['S32','i', 'i', 'i']})


peoples = np.array([("ZhangFei",66,65,30),("GuanYu",95,85,98),


       ("ZhaoYun",93,92,96),("HuangZhong",90,88,77),("DianWei",80,90,90)],


    dtype=persontype)


# print(peoples)


# Chinese


def dataProcess(num):


    subject=str(peoples.dtype.names[num])


    a=peoples[:][subject]


    print(subject + " mean : ",np.mean(a))


    print(subject + " min : ",np.amin(a))


    print(subject + " max : ",np.amax(a))


    print(subject + " std : ",np.std(a))


    print(subject + " var : ",np.var(a))


dataProcess(1)


dataProcess(2)


dataProcess(3)


# sort by sum


print("sort Result",sorted(peoples,key=lambda x:x[1]+x[2]+x[3], reverse=True))


```


作者回复: Good Job

2019-03-18


无名氏

「另外 NumPy 中的矩阵计算可以采用多线程的方式，充分利用多核 CPU 计算资源，大大提升了计算效率。」，老师你好，这地方我有点疑惑，多线程由于 GIL 锁的原因不能使用多核吧？

2019-03-18


Kyle


import numpy as np


personscore = np.dtype({'names':['name','chinese','english','math'],'formats':['S32','i','i','i']})


persons = np.array([('zhangfei',66,65,30),('guanyu',95,85,98),('zhaoyun',93,92,96),('huangzhong',90,88,77),('dianwei',80,90,90)],dtype=personscore)


chineses = persons[:]['chinese']


englishs = persons[:]['english']


maths = persons[:]['math']


totalscore =chineses + englishs + maths


#统计下这些人在语文、英语、数学中的平均成绩、最小成绩、最大成成绩、方差、标准差。然后把这些人的总成绩排序，得出名次进行成绩输出。

print(np.mean(chineses))


print(np.mean(englishs))


print(np.mean(maths))


print(np.amin(chineses))


print(np.amin(englishs))


print(np.amin(maths))


print(np.amax(chineses))


print(np.amax(englishs))


print(np.amax(maths))


print(np.var(chineses))


print(np.var(englishs))


print(np.var(maths))


print(np.std(chineses))


print(np.std(englishs))


print(np.std(maths))


print(totalscore)


print(np.sort(totalscore))


作者回复: Good Job

2019-03-14


张乐乐

import numpy as np


scoretype = np.dtype({'names':['name','chinese','english','math','total'],'formats':['S32','i','i','i','i']})


peoples = np.array([('zhangfir',66,65,30,0),('guanyu',95,85,98,0),('zhaoyun',93,92,96,0),('huangzhong',90,88,77,0),('dianwei',80,90,90,0)],dtype=scoretype)


peoples[:]['total'] = peoples[:]['chinese']+peoples[:]['english']+peoples[:]['math']


print (peopel)# 拼写错误

name = peoples[:]['name']


chinese = peoples[:]['chinese']


english = peoples[:]['english']


total = peoples[:]['total']


def biaoge(name,cj):


print (''.format (name,np.mean (cj),np.amin (cj),np.amax (cj),np.var (cj),np.std (cj)))# 引号中间是空的

print ("科目 | 平均成绩 | 最小成绩 | 最大成绩 | 方差 | 标准差")

show ("语文", chinese)

show ("英语", english)

show ("数学", peoples [:]['math'])

print(np.sort(peoples, order='total'))


老师我很奇怪，我觉得我的代码是有问题的，为什么还能正常执行

2019-03-13


tinn


import numpy as np


scoretype = np.dtype({


    'names':['name', 'chinese', 'math', 'english','total'],


    'formats':['S32','i', 'i', 'i','i' ]})


peopel = np.array([('zhang',66,65,30,0),('guan',95,85,98,0),('zhao',93,92,96,0),('huang',90,88,77,0),('dian',80,90,90,0)],dtype=scoretype)


peopel[:]['total'] = peopel[:]['chinese']+peopel[:]['english']+peopel[:]['math']


print(peopel)


n = peopel[:]['name']


c = peopel[:]['chinese']


e = peopel[:]['english']


m = peopel[:]['math']


def score(subject,score):


    print(subject)


    print('pingjun:'+str(np.mean(score)))


    print('zuidi:' + str(np.amin(score)))


    print('zuigao:' + str(np.amax(score)))


    print('fangcha:' + str(np.var(score)))


    print('biaozhun:' + str(np.std(score)))


score('yuwen', c)


print(' ')


score('yingyu' , e)


print(' ')


score('shuxue' , m)


print(np.sort(peopel, order='total'))


东拼西凑，加油啊！！

作者回复：加油！

2019-03-08


pythonzwd


我尝试了作业中填写中文 (张飞，关羽。。)，会编码错误，改成拼音就可以了，针对 "S32" 但是想了解中文对应的类型应该是什么

作者回复: U32

2019-03-07


FeiFei


numpy 表现出了 python 在线性代数方面的卓越应用

将数学性的很多操作实现为简单的函数

看到了他在处理二维数组上的能力

数据处理的本质是对数据进行各种计算

其他语言是否有相同的库呢？

作者回复：应该都有自己常用的工具包，在 Python 中 numpy, pandas, sklearn 算是常用的

2019-03-03


reverse


极客时间数据分析实战 45 讲的详细笔记 (包含 markdown、图片、思维导图) github 地址： https://github.com/xiaomiwujiecao/DataAnalysisInAction

2019-02-28


一语中的

classMates = np.dtype({


    'names':['name', 'Chinese', 'English', 'Math', 'Total'],


    'formats':['S32', 'i', 'i', 'i', 'i']})


datas = np.array([("ZhangFei",66,65,30, 0),("GuanYu", 95,85,98, 0),


                    ("ZhaoYun",93,92,96, 0),("HuangZhong",90,88,77, 0),


                  ("DianWei", 80, 90, 90, 0)],dtype=classMates)


#获取总成绩

datas[:]['Total'] = datas[:]['Chinese'] + datas[:]['English'] + datas[:]['Math']


Chineses = datas[:]['Chinese']


Englishs = datas[:]['English']


Maths = datas[:]['Math']


Totals= datas[:]['Total']


#计算各科目，平均成绩，最低成绩，最高成绩，方差，标准差

print ("语文平均分:% s, 最低成绩:% s, 最高成绩:% s, 方差:% s, 标准差:% s"%(np.mean (Chineses), np.amin (Chineses), np.amax (Chineses), np.var (Chineses),np.std (Chineses)))

print ("英语平均分:% s, 最低成绩:% s, 最高成绩:% s, 方差:% s, 标准差:% s"%(np.mean (Englishs),np.amin (Englishs), np.amax (Englishs), np.var (Englishs),np.std (Englishs)))

print ("数学平均分；% s, 最低成绩:% s, 最高成绩:% s, 方差:% s, 标准差:% s"%(np.mean (Maths), np.amin (Maths), np.amax (Maths), np.var (Maths),np.std (Maths)))

#排序方法一

print(np.sort(datas,order='Total'))


#排序方法二

rank = sorted(datas, key=lambda x:x[1]+x[2]+x[3], reverse=True)


print(rank)


作者回复: Good Job

2019-02-25


三碗

axis = 0，1，2，3


等于 2,3 怎么理解，多维的话。能不能画个图解释下。和 pandas 的轴是一样的吗？

编辑回复：答疑篇有解释，请看答疑篇～

2019-02-23


JackWu


# -*- coding: utf-8 -*-


import numpy as np


from string import Template


a = np.array([1, 2, 3])


b = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])


b[1,1]=10


print(a.shape)


print(b.shape)


print(a.dtype)


print(b)


print('---'*20)


def show(name,cj):


    strtemp = '$name|$mean|$min|$max|$var|$std'


    str = Template(strtemp)


    strfill = str.substitute({'name': name, 'mean': np.mean(cj)


                                 , 'min': np.min(cj)


                                 , 'max': np.max(cj)


                                 , 'var': np.var(cj)


                                 , 'std': np.std(cj)})


    print(strfill)


scoretype = np.dtype(


            {


            'names': ['name', 'chinese', 'english', 'math','total'],


            'formats': ['S32', 'i', 'i', 'i','i']


            }


        )


peoples = np.array(


    [


        ("zhangfei", 66, 65,30,0),


        ("guanyu", 95, 85, 98,0),


        ("zhaoyun", 93, 92, 96,0),


        ("huangzhong", 90, 88, 77,0),


        ("dianwei", 80, 90, 90,0)


    ], dtype=scoretype)


#print(peoples)


name=peoples[:]['name']


chinese=peoples[:]['chinese']


english=peoples[:]['english']


math=peoples[:]['math']


peoples[:]['total'] = peoples[:]['chinese']+peoples[:]['english']+peoples[:]['math']


print(chinese)


show ("语文",chinese)

show ("英语",english)

show ("数学",math)

print ("排名 1:")

print (np.sort(peoples, order='total',reversed(True)))


print ("排名 2:")

ranking = sorted(peoples,key = lambda x : x[1]+x[2]+x[3],reverse=True)


print(ranking)


作者回复: Good Job

2019-02-19


Sandy


# !/usr/bin/env python


# -*- coding: utf-8 -*-


import numpy as np


def calculate(peoples, subject):


    subject_data = peoples[:][subject]


    t = (subject, np.mean(subject_data),np.min(subject_data),np.max(subject_data),np.var(subject_data),np.std(subject_data) )


    return t


def printFormat(subjects):


    print('subject mean min max var std')


    for s in subjects:


        print(str(s[0])+' ' +str(s[1])+' ' +str(s[2])+' ' +str(s[3])+' ' +str(s[4])+' ' +str(s[5]))


if __name__ == "__main__":


    persontype = np.dtype({


        'names':['name', 'chinese','english','math'],


        'formats':['S32','i', 'i', 'i']})


    peoples = np.array([("ZhangFei",66,65, 30),("GuanYu",95,85,98),


           ("ZhaoYun",93,92,96),("HuangZhong",90,88,77),("DianWei",80,90,90)],


        dtype=persontype)


    


    subjecttype = np.dtype({


        'names':['subject', 'mean','min','max','var','std'],


        'formats':['S32','f', 'f', 'f', 'f', 'f']})


    subjects = np.array([calculate(peoples,'chinese'),calculate(peoples,'english'),calculate(peoples,'math')], dtype=subjecttype)


    


    printFormat(subjects)


    


    ranking = sorted(peoples, key=lambda x: x[1]+x[2]+x[3], reverse = True)


    print(ranking)


作者回复: Good Job

2019-02-18


上善若水

dtype=[('name', 'S32'), ('age', '<i4'), ('chinese', '<i4')])


请问 i4 是什么意思？

2019-02-18


無所畏

其实题目并不难，看到老师恢复留言，我才觉得，老师的方法简洁。思维不一样吧.

作者回复：哈哈

2019-02-17


Wei_强

NumPy 的数组连续地址存储，那么它的读的效率很高，但是写的效率和 python 原生的 list 相比要差。那么，对这个问题我们应该如何取舍，什么情况下更适合用哪个数据结构呢

2019-02-17


danny


老师，通过 np.dtype 定义的结构体只能是二维的吗？结构体前缀 'names': 和 'formats': 是固定写法还是自定义的。这块有点不能理解。

2019-02-15


Q


import numpy as np


persontype = np.dtype({


    'names':['name', 'age', 'chinese', 'math', 'english'],


    'formats':['S32','i', 'i', 'i', 'f']})


peoples = np.array([("ZhangFei",32,75,100, 90),("GuanYu",24,85,96,88.5),


       ("ZhaoYun",28,85,92,96.5),("HuangZhong",29,65,85,100)],


    dtype=persontype)


ages = peoples[:]['age']


chineses = peoples[:]['chinese']


maths = peoples[:]['math']


englishs = peoples[:]['english']


print np.mean(ages)


print np.mean(chineses)


print np.mean(maths)


print np.mean(englishs)


作者回复: Good Job

2019-02-15


史龙

scoredata = np.dtype([("name", "S32"), ("chinese",'i'), ("english","i"), ("math", "i")])


a = np.array([("zhangfei",66,65,30),("guanyu",95,85,98),("zhaoyun",93,92,96),("huangzhong",90,88,77),("dianwei",80,90,90)], dtype=scoredata)


np.mean(a["english"])


2019-02-14


Dull


python3 的代码，参考了其他同学的代码与老师的回复，在最后的总成绩排名输出是将成绩排序，用老师说的 print np.sort (peoples, order='total') 输出出来是升序排序，并且使用搜索引擎没有找到降序的方法。还请大家请教。

# -*- coding: utf-8 -*-


import numpy as np


studenttype = np.dtype({'names':['name','chinese','english','math','total'],


'formats':['U32','f','f','f','f']})


students = np.array ([(' 张飞 ',66,65,30,0),(' 关羽 ',95,85,98,0),(' 赵云 ',93,92,96,0),(' 黄忠 ',90,88,77,0),(' 典韦 ',80,90,90,0)],dtype = studenttype)

students[:]['total'] = students[:]['chinese'] + students[:]['english'] + students[:]['math']


print(students)


c = students[:]['chinese']


e = students[:]['english']


m = students[:]['math']


def show(name,sub):


print(name,end='|')


print(np.mean(sub),end='|')


print(np.amax(sub),end='|')


print(np.amin(sub),end='|')


print(np.var(sub),end='|')


print(np.std(sub))


print (' 科目 | 平均值 | 最大值 | 最小值 | 方差 | 标准差 ')

show (' 语文 ',c)

show (' 数学 ',m)

show (' 英语 ',e)

#输出排名

print (' 排名 ')

ranking = sorted(students,key = lambda x:x[4],reverse = True)


print(ranking)


作者回复: Good Job

2019-02-13


三硝基甲苯

import numpy as np


studenttype = np.dtype({


    'names': ['name', 'chinese', 'english', 'math', 'total'],


    'formats': ['S32', 'f', 'f', 'f', 'f']})


students = np.array([


    ('zhangfei', 66, 65, 30, 0),


    ('guanyu', 95, 85, 98, 0),


    ('zhaoyun', 93, 92, 96, 0),


    ('huangzhong', 90, 88, 77, 0),


    ('dianwei', 80, 90, 90, 0)], dtype=studenttype)


ss = np.array([


    ('zhangfei', 66, 65, 30, 0),


    ('guanyu', 95, 85, 98, 0),


    ('zhaoyun', 93, 92, 96, 0),


    ('huangzhong', 90, 88, 77, 0),


    ('dianwei', 80, 90, 90, 0)], dtype=studenttype)


for i in students:


    i[4] = i[1] + i[2] + i[3]


subject = ['chinese', 'english', 'math']


students = np.sort(students, order=["total"])[::-1]


for i in subject:


    print("the mean of the {0}: {1}".format(i, np.mean(students[:][i])))


    print("the max of the {0}: {1}".format(i, np.amax(students[:][i])))


    print("the min of the {0}: {1}".format(i, np.amin(students[:][i])))


    print("the std of the {0}: {1}".format(i, np.std(students[:][i])))


    print("the var of the {0}: {1}".format(i, np.var(students[:][i])))


for i in range(1, len(students)+1):


    print("{0}.is {1}".format(i, students[i-1][0]))


作者回复: Good Job

2019-02-11


叶えなくちゃ

补交作业，努力赶上

import numpy as np


persontype = np.dtype({


    'names':['name', 'chinese', 'english' ,'math', 'total'],


    'formats':['S32', 'i', 'i', 'i','i']})


peoples = np.array([("ZhangFei",66,65,30,0),("GuanYu",95,85,98,0),


       ("ZhaoYun",93,92,96,0),("HuangZhong",90,88,77,0),("DianWei",80,90,90,0)],


    dtype=persontype)


print(peoples)


# 各科平均分 np.mean ()

chinese = peoples[:]['chinese']


english = peoples[:]['english']


math = peoples[:]['math']


print(np.mean(chinese))


print(np.mean(english))


print(np.mean(math))


# 最值 amin () amax ()

print(chinese)


print(np.amin(chinese))


print(np.amax(chinese))


print(english)


print(np.amin(english))


print(np.amax(english))


print(math)


print(np.amin(math))


print(np.amax(math))


# 方差 var () 标准差 std ()

print(np.var(chinese))


print(np.std(chinese))


print(np.var(english))


print(np.std(english))


print(np.var(math))


print(np.var(math))


# 按总成绩排序

peoples[:]['total'] = peoples[:]['chinese'] + peoples[:]['english'] + peoples[:]['math']


print(np.sort(peoples,order='total'))


作者回复：加油

2019-02-11


陈川

老师您好，我想请问一下 numpy 是否有根据结构数组中的某一组值来更新结构数组的顺序或者输出结构数组的函数

比如结构数组在创建时的顺序是依照题目的：张飞，关羽，赵云... 按照结构数组的索引来输出就是 0,1,2..

我在结构数组中添加了 allscore 和 rank 两个字段，用以存放总分和总分的排名

在原有的结构数组中 rank 的值依次为 [1,4,5,2,3]

如果想要根据 rank=1,2,3,4,5 的顺序来输出结构数组应该怎么实现呢？

我的做法是创建一个 while 内加 for 的循环来依次比较 rank 的值是否为 1 (2,3,4,5) 再决定是否输出，但这样效率很低.

2019-02-10


陈川

#整理了输出的形式

#-*- coding: utf-8 -*-


import numpy as np


studenttype = np.dtype({


    'names':['name','yuwen','shuxue','yingyu','score','rank'],


    'formats':['U2','f','f','f','i','i']})


students = np.array ([(' 张飞 ',66,30,65,0,0),(' 关羽 ',95,98,85,0,0),

(' 赵云 ',93,96,92,0,0),(' 黄忠 ',90,77,88,0,0),

(' 典韦 ',80,90,90,0,0)],dtype=studenttype)

studentnames = students[:]['name']


chinese = students[:]['yuwen']


math = students[:]['shuxue']


english = students[:]['yingyu']


fun = [' 平均成绩 ',' 最低分数 ',' 最高分数 ',' 成绩方差 ',' 的标准差 ']

name = [' 语文 ',' 数学 ',' 英语 ']

m = 0


for f in [np.mean,np.amin,np.amax,np.std,np.var]:


    n=0


    print('*' * 5, end='')


    for i in [chinese,math,english]:


print ('{} 成绩 {:4}:{:}'.format (name [n],fun [m], str (f (i)).center (10)),end='')

        n+=1


    print('*' * 5)


    m+=1


for i in range(len(students)):


    students[i]['score'] = students[i]['yuwen']+students[i]['shuxue']+students[i]['yingyu']


scores = np.sort(students[:]['score'])


scoreslist = []


for i in range(len(scores)):


    scoreslist.append(scores[i])


for i in range(len(students)):


    students[i]['rank'] = scoreslist.index(students[i]['score'])+1


i = 0


while i<5:


    i+=1


    for j in range(len(students)):


        if students[j]['rank'] == i:


print ('{} 的排名是 {}, 语文成绩是 {}, 数学成绩是 {}, 英语成绩是 {}, 总分为 {}'.format (students [j]['name'],students [j]['rank'],students [j]['yuwen'],students [j]['shuxue'],students [j]['yingyu'],students [j]['score']))

"""


输出如下:

***** 语文成绩平均成绩: 84.8 数学成绩平均成绩: 78.2 英语成绩平均成绩: 84.0 *****

***** 语文成绩最低分数: 66.0 数学成绩最低分数: 30.0 英语成绩最低分数: 65.0 *****

***** 语文成绩最高分数: 95.0 数学成绩最高分数: 98.0 英语成绩最高分数: 92.0 *****

***** 语文成绩成绩方差: 10.72194 数学成绩成绩方差：25.190474 英语成绩成绩方差: 9.777525 *****

***** 语文成绩的标准差：114.96001 数学成绩的标准差：634.55994 英语成绩的标准差: 95.6 *****

张飞的排名是 1, 语文成绩是 66.0, 数学成绩是 30.0, 英语成绩是 65.0, 总分为 161

....


....(如上，超出字数不举例了)

"""


作者回复: Good Job

2019-02-10


欧阳

评论第一条里的 cmp 是什么？

我查了下 sorted 的用法，应该这么写吧：

sorted(peoples, key = lambda x:x[1]+x[2]+x[3], reverse=True)


2019-02-09


大圣归来

整理了学习笔记：https://mubu.com/doc/sulzALenU0

1、python 自带的 list 保存的的是对象的指针，遍历的时候还要查找地址，而 numpy 可以直接遍历，效率更高。

2、练习题，自己写完后，又参考了一下大家的写法，最后把自己的程序优化了一下

studenttype = ({


'names':['name', 'chinese', 'english', 'math', 'total'],


'formats':['S32', 'i', 'i', 'i', 'i']


})


students = np.array([('zhangfei', 66, 65, 30, 0),


('guanyu', 95, 65, 30, 0),


('zhaoyun', 93, 92, 96, 0),


('huangzhong', 90, 88, 77, 0),


('dianwei', 80, 90, 90, 0)],dtype=studenttype)


chineses = students[:]['chinese']


englishs= students[:]['english']


maths = students[:]['math']


students[:]['total'] = students[:]['chinese'] + students[:]['english'] + students[:]['math']


#按照成绩打印

print (' 语文平均成绩：', np.mean (chineses))

print (' 英语平均成绩：', np.mean (englishs))

print (' 数学平均成绩：', np.mean (maths))

print('============================')


print (' 语文最低成绩 ', np.amin (chineses))

print (' 数英语低成绩 ', np.amin (englishs))

print (' 数学最低成绩 ', np.amin (maths))

print('============================')


print (' 语文最高成绩 ', np.amax (chineses))

print (' 英语最高成绩 ', np.amax (englishs))

print (' 数学最高成绩 ', np.amax (maths))

print('============================')


print (' 语文成绩方差 ', np.var (chineses))

print (' 英语成绩方差 ', np.var (englishs))

print (' 数学成绩方差 ', np.var (maths))

print('============================')


print (' 语文成绩标准差 ', np.std (chineses))

print (' 英语成绩标准差 ', np.std (englishs))

print (' 数学成绩标准差 ', np.std (maths))

print('============================')


#按照科目打印

def show_grade(subject, grade):


print('{:^10}|{:^10}|{:^10}|{:^10}|{:^20}|{:^10}'.format(subject, np.mean(grade), np.amin(grade), np.amax(grade), np.var(grade), np.std(grade)))


show_grade (' 语文 ', chineses)

show_grade (' 英语 ', englishs)

show_grade (' 数学 ', maths)

#学生排名

print (' 学生成绩排序 ', np.sort (students, order='total')[::-1])

作者回复：做笔记不错，Good Job

2019-02-01


草包雷

缺点用 np.sort 不能降序

import numpy as np


persontype = np.dtype({


    'names':['name', 'chinese', 'english', 'math','total'],


    'formats':['S32','i', 'i', 'i','i']})


peoples = np.array([("ZhangFei",66,65,30,0),("GuanYu",95,85,98,0),


       ("ZhaoYun",93,92,96,0),("HuangZhong",90,88,77,0),('dianwei',80,90,90,0)],


    dtype=persontype)


peoples[:]['total'] = peoples[:]['chinese']+peoples[:]['math']+peoples[:]['english']


chineses = peoples[:]['chinese']


maths = peoples[:]['math']


english = peoples[:]['english']


print (""+" 平均分 "+" 最高分 "+" 最低分 "+" 方差 "+" 标准差 ")

print ("语文",np.mean (chineses),"",np.amax (chineses)," ",np.amin (chineses)," ",np.var (chineses)," ",np.std (chineses))

print ("英语",np.mean (english),"",np.amax (english)," ",np.amin (english)," ",np.var (english)," ",np.std (english))

print ("数学",np.mean (maths),"",np.amax (maths)," ",np.amin (maths)," ",np.var (maths)," ",np.std (maths))

print (np.sort(peoples,order='total'))


输出

平均分 最高分 最低分 方差 标准差

语文 84.8 95 66 114.96 10.7219401229

英语 84.0 92 65 95.6 9.77752524926

数学 78.2 98 30 634.56 25.1904743901

[(b'ZhangFei', 66, 65, 30, 161) (b'HuangZhong', 90, 88, 77, 255)


 (b'dianwei', 80, 90, 90, 260) (b'GuanYu', 95, 85, 98, 278)


 (b'ZhaoYun', 93, 92, 96, 281)]


作者回复: Good Job

2019-01-31


洛水

作业：

老师您好：就是我在做作业的时候，用中文定义课程名和人名的时候，会报错，用中文的时候，就不会报错，不知道有没有什么方法可以直接定义中文名，不用英文

import numpy as np


student = np.dtype({


    'names': ['name', 'Cheses', 'English', 'Math','Total'],


    'formats': ['S32', 'i', 'i', 'i','i']


})


students = np.array([


    ('zhangfei', 66, 65, 30,0),


    ('guanyu', 95, 85, 98,0),


    ('zhaoyun', 93, 92, 96,0),


    ('huangzhong', 98, 88, 77,0),


    ('dianwei', 80, 90, 90,0)],


    dtype=student)


#用 NumPy 统计下这些人在语文、英语、数学中的平均成绩、最小成绩、最大成绩、方差、标准差，然后把这些人的总成绩排序，得出名次进行成绩输出

cheses=students[:]['Cheses']


english=students[:]['English']


math=students[:]['Math']


students[:]['Total']=students[:]['Cheses']+students[:]['English']+students[:]['Math']


print (' 语文平均成绩:',np.mean (cheses),' 语文最低分 ',np.amin (cheses),' 语文最高分 ',np.amax (cheses),' 方差 ',np.var (cheses),

' 标准差 ',np.std (cheses))

print (' 英语平均成绩:',np.mean (english),' 英语最低分 ',np.amin (english),' 英语最高分 ',np.amax (english),' 方差 ',np.var (english),

' 标准差 ',np.std (english))

print (' 数学平均成绩:',np.mean (cheses),' 数学最低分 ',np.amin (cheses),' 数学最高分 ',np.amax (cheses),' 方差 ',np.var (cheses),

' 标准差 ',np.std (cheses))

#排序

print(np.sort(students,order='Total'))


2019-01-29


杰之 7

通过这一节的阅读加实践，对 numpy 的科学计算有了一些认识。首先老师讲述了为什么 numpy 能快速的处理计算。接着介绍了 numpy 在数组，数据统计上和排序上的计算方法。

阅读之后通过练习可以更能理解老师讲述的内容。

作者回复：加油！

2019-01-28


Ama


# 我的解答

# -*- coding: utf-8 -*-


import numpy as np


persontype = np.dtype({


    'names':['name','chinese','english','math','total'],


    'formats':['S32','i','i','i','i']


})


peoples = np.array([("Zhangfei",66,65,30,0),("Guanyu",95,85,98,0),


                    ("Zhaoyun",93,92,96,0),("Huangzhong",90,88,77,0),


                    ("Dianwei",80,90,90,0)],


                  dtype = persontype)


chineses = peoples[:]['chinese']


englishes = peoples[:]['english']


maths = peoples[:]['math']


# 计算每个人总分

peoples[:]['total'] = chineses + englishes + maths


totals = peoples[:]['total']


# table 在原始列表上加了总分

table = np.array([chineses,englishes,maths,totals])


# 包含所有统计数据的数组

stat = np.array([np.mean(table, axis = 1),np.amax(table, axis = 1),


                 np.amin(table, axis = 1),np.var(table, axis = 1),


                np.std(table, axis = 1)])


# 统计名称列向量

statnames = np.array ([["平均分:"],["最高分:"],["最低分:"],["方 差:"],["标准差:"]])

# 打印首行

print ("统计值 语文 英语 数学 总分")

# 合并统计名称列向量和统计值矩阵

# 这个方法把所有元素都变成字符串了，如何让数值仍然保持浮点型呢？

print (np.hstack((statnames,np.round(stat, decimals = 1))))


# 排序，默认是从小到大，如何从大到小排呢？

print ("按总成绩从低到高排序如下：")

print (np.sort(peoples, order = 'total'))


运行结果：

统计值 语文 英语 数学 总分

[[' 平均分:' '84.8' '84.0' '78.2' '247.0']

[' 最高分:' '95.0' '92.0' '98.0' '281.0']

[' 最低分:' '66.0' '65.0' '30.0' '161.0']

[' 方 差:' '115.0' '95.6' '634.6' '1949.2']

[' 标准差:' '10.7' '9.8' '25.2' '44.1']]

按总成绩从低到高排序如下：

[(b'Zhangfei', 66, 65, 30, 161) (b'Huangzhong', 90, 88, 77, 255)


 (b'Dianwei', 80, 90, 90, 260) (b'Guanyu', 95, 85, 98, 278)


 (b'Zhaoyun', 93, 92, 96, 281)]


作者回复: Good Job

2019-01-27


月亮上的熊猫_lv

作业打卡，sort/sorted 还有很多可以深挖。sort 是 numpy 中自带的。

在通常的使用中，sorted 不会改变数据本身，而 sort 会。

import numpy as np


persontype = np.dtype({


        'names' : ['name','chinese','english','math','sum'],


        'formats' : ['S32','i','i','i','i']


        })


peoples = np.array([('zhangfei',66,65,30,0),


                   ('guanyu',95,85,98,0),


                   ('zhaoyun',93,92,96,0),


                   ('huangzhong',90,88,77,0),


                   ('dianwei',80,90,90,0)],


                   dtype = persontype)


chinese = peoples[:]['chinese']


english = peoples[:]['english']


math = peoples[:]['math']


print('CH mean: ',np.mean(chinese))


print('EN mean: ',np.mean(english))


print('MA mean: ',np.mean(math))


print('CH MIN: ',np.amin(chinese))


print('CH MAX: ',np.amax(chinese))


print('CH STD: ',np.std(chinese))


print('CH VAR: ',np.var(chinese))


'''


ranking = sorted(peoples,key=lambda x:x[1]+x[2]+x[3], reverse=True)


print(ranking)


'''


peoples[:]['sum'] = peoples[:]['chinese'] + peoples[:]['english'] + peoples[:]['math']


print('The order: ',np.sort(peoples,order='sum'))


作者回复：加油～感谢分享

2019-01-27


文

import numpy as np


persontype=np.dtype({'names':['name', 'chinese', 'english', 'math'], 'formats':['S32', 'i', 'i', 'i']})


peoples=np.array([("ZhangFei",66,65,30),


                  ("GuanYu",95,85,98),


                  ("ZhaoYun",93.92,96),


                  ("HuanZhong",90,88,77),


                  ("DianWei",80,90,90)],dtype=persontype)


chineses=peoples[:]['chinese']


englishs=peoples[:]['english']


maths=peoples[:]['math']


print(peoples)


运行结果：

Traceback (most recent call last):


File "D:/ 学习 /python/shuju/numpy-lianxi/lianxi.py", line 8, in <module>

    ("DianWei",80,90,90)],dtype=persontype)


ValueError: could not assign tuple of length 3 to structure with 4 fields.


到这里就报错，为什么？

2019-01-25


夜吾夜

老师，代码理解难度不大，作为一名数据开发工程师，想转数据分析，但是您的文章里有好多名词我不太理解，比如正态分布，百分位数等，百度去理解起来也比较吃力，我目前这个阶段，想要在数据分析领域进步，应该做些什么啊？有什么推荐的书籍或办法码？

作者回复：慢慢来 跟着专栏一起做练习，从算法原理 到 工具使用

2019-01-24


阿蒙森的哈士奇

import numpy as np


# 定义数据结构

score_type = np.dtype({


    'names': ['name', 'chinese', 'english', 'math'],


    'formats': ['S32', 'f', 'f', 'f']


})


# 创建对象

score = np.array([


    ('zhangfei', 66, 65, 30),


    ('guangyu', 95, 85, 98),


    ('zhaoyun', 93, 92, 96),


    ('huangzhong', 90, 88, 77),


    ('dianwei', 80, 90, 90)


], dtype=score_type)


# 平均成绩

for stu in score:


print (' 姓名：% s | 平均成绩：% f' % (stu [0], sum ((list (stu)[1::])) / 3))

# 每位同学最低 / 最高成绩

for stu in score:


print (' 姓名：% s | 最低成绩：% f | 最高成绩：% f'

          % (stu[0], min(list(stu)[1::]), max(list(stu)[1::])))


    


# 每位同学各科成绩的方差与标准差

for stu in score:


print (' 姓名：% s | 分数方差：% f | 分数标准差：% f'

         % (stu[0], np.var(np.array(list(stu)[1::])), np.std(np.array(list(stu)[1::]))))


    


# 按总成绩排序输出（从小到大）

sumscore_type = np.dtype({


    'names': ['name', 'total'],


    'formats': ['S32', 'f']


})


sumscore = np.array([


    tuple([stu[0], sum(list(stu)[1::])]) for stu in score


], dtype=sumscore_type)


print(np.sort(sumscore, order='total'))


"""


1. 由于 amax () , amin () 不能处理带有非数值的元素（我测试的结果是这样的，如有异议，老师指正一下吧），所以采用了直接抽取去每个同学的所有成绩，直接用 python 内置的 max () 和 min () 函数

2. 总成绩排序输出，当然可以写的更简单点，直接抽取出成绩比大小。但是为了，利用上本节课所学的内容，我还是先自定义了一个数据结构，再利用上 numpy.sort () 函数

"""


作者回复：不错的分享

2019-01-22


james


import numpy as np


'''


假设一个团队里有 5 名学员，成绩如下表所示。

1. 用 NumPy 统计下这些人在语文、英语、数学中的平均成绩、最小成绩、最大成绩、方差、标准差。

2. 总成绩排序，得出名次进行成绩输出。

'''


struct = np.dtype({'names':['Name', 'Chinese', 'English', 'Math'],


                   'formats':['U32', 'i', 'i', 'f']


                  })


student_scores = np.array([('Jack', 30, 95, 60),


                           ('Rose', 80, 95, 96),


                           ('Claire', 90, 95, 50),


                           ('Zoey', 80, 80, 70)], dtype=struct)


s = struct.names


print("Statistics:")


for name in list(s[1:]):


    if name == 'Chinese':


        print(" Mean Variance Std")


        print("Chinese: ",student_scores[name].mean(), \


              " ", student_scores[name].var(),\


              " ", student_scores[name].std())


    elif name == 'English':


        print("English: ", student_scores[name].mean(),\


              " ", student_scores[name].var(),\


              " ", student_scores[name].std())


    elif name == 'Math':


        print("Math: ", student_scores[name].mean(),\


              " ", student_scores[name].var(),\


              " ", student_scores[name].std())


sorted_result = sorted(student_scores, key = lambda x : x[1] + x[2] + x[3], reverse=True)


print("")


print("Rank:")


print("Name Chinese English Math")


for res in list(sorted_result):


    print(res[0]," ",res[1]," ",res[2]," ",res[3])


2019-01-22


王彬成

1、用自己的话说明一下为什么要用 NumPy 而不是 Python 的列表 list 吗？

用 numpy 存储数组可节约内存和计算时间

2、除此之外，你还知道那些数据结构类型？

4 种基本数据结构：列表、元组、字典、集合

4、问题请教

请问老师，用 numpy 如何进行降序排列。因为查了很久，numpy 只有升序排序

5、axis=0，axis=1 理解

简单来说，axis=0 是对每一组 纵向 数据进行处理，axis=1 是对每一组 横向 数据进行处理。

更加本质的理解。举例二维数组 a [2][3]=[[4,3,2],[2,4,1]]。

a[0][0]=4, a[0][1]=3, a[0][2]=2


a[1][0]=2, a[1][1]=4, a[1][2]=1


##axis=0，就是第 1 个下标变化的方向进行排序，即

a[0][0] ->a[1][0]


a[0][1] -> a[1][1]


a[0][2] -> a[1][2]


[[2 3 1]


[4 4 2]]


##axis=1，就是第 2 个下标变化的方向进行排序，即

a[0][0] -> a[0][1] -> a[0][2]


a[1][0] -> a[1][1] -> a[1][2]


[[2 3 4]


[1 2 4]]


3、全班成绩输出代码

import numpy as np


persontype = np.dtype({


    'names':['name', 'chinese', 'english','math','total'],


    'formats':['S32','i', 'i', 'i','i']})


peoples = np.array([("ZhangFei",66,65,30,0),("GuanYu",95,85,98,0),


       ("ZhaoYun",93,92,96,0),("HuangZhong",90,88,77,0),("DianWei",80,90,90,0)],


                   dtype=persontype)


peoples[:]['total'] = peoples[:]['chinese']+peoples[:]['english']+peoples[:]['math']


chineses = peoples[:]['chinese']


maths = peoples[:]['math']


englishs = peoples[:]['english']


print(peoples)


## 语文成绩输出，平均成绩、最小成绩、最大成绩、方差、标准差

print (' 语文成绩 ')

print (' 平均成绩 ',np.mean (chineses))

print (' 最小成绩 ',np.amin (chineses))

print (' 最大成绩 ',np.amax (chineses))

print (' 方差 ',np.var (chineses))

print (' 标准差 ',np.std (chineses))

## 数学成绩输出，平均成绩、最小成绩、最大成绩、方差、标准差

print('---------------')


print (' 数学成绩 ')

print (' 平均成绩 ',np.mean (maths))

print (' 最小成绩 ',np.amin (maths))

print (' 最大成绩 ',np.amax (maths))

print (' 方差 ',np.var (maths))

print (' 标准差 ',np.std (maths))

## 英语成绩输出，平均成绩、最小成绩、最大成绩、方差、标准差

print('---------------')


print (' 英语成绩 ')

print (' 平均成绩 ',np.mean (englishs))

print (' 最小成绩 ',np.amin (englishs))

print (' 最大成绩 ',np.amax (englishs))

print (' 方差 ',np.var (englishs))

print (' 标准差 ',np.std (englishs))

## 按总成绩排名输出

print('---------------')


print (np.sort(peoples,order='total'))


2019-01-22


skybird


#!/usr/bin/python


#vim: set fileencoding:utf-8


'''


假设一个团队里有 5 名学员，成绩如下表所示。

1. 用 NumPy 统计下这些人在语文、英语、数学中的平均成绩、最小成绩、最大成绩、方差、标准差。

2. 总成绩排序，得出名次进行成绩输出。

姓名 语文 英语 数学

张飞 66 65 30

关羽 95 85 98

赵云 93 92 96

黄忠 90 88 77

典韦 80 90 90

'''


import numpy as np


sttype1 = np.dtype({


'names':[' 姓名 ',' 语文 ', ' 英语 ', ' 数学 ', 'total'],

    'formats':['U32','i', 'i', 'i', 'i']})


students = np.array ([("张飞",66,65,30,0),("关羽",95,85,98,0),

("赵云",93,92,96,0),("黄忠",90,88,77,0),("典韦",80,90,90,0)],

    dtype=sttype1)


students [:]['total'] = students [:][' 语文 '] + students [:][' 数学 ']+ students [:][' 英语 ']

names = students [:][' 姓名 ']

chineses = students [:][' 语文 ']

maths = students [:][' 数学 ']

englishs = students [:][' 英语 ']

print ("学生成绩统计如下:")

print ("科目 平均分 最低分 最高分 方差 标准差")

print ("语文 %3.1f %3.1f %3.1f %3.1f %3.1f" % (np.mean (chineses), np.amin (chineses), np.amax (chineses), np.var (chineses), np.std (chineses)))

print ("数学 %3.1f %3.1f %3.1f %3.1f %3.1f" % (np.mean (maths), np.amin (maths), np.amax (maths), np.var (maths), np.std (maths)))

print ("英语 %3.1f %3.1f %3.1f %3.1f %3.1f" % (np.mean (englishs), np.amin (englishs), np.amax (englishs), np.var (englishs), np.std (englishs)))

students_temp = sorted(students, key=lambda x:x[4], reverse=True)


print()


print ("这批学生的成绩排名为:")

print ("名次 姓名 总分")

i = 0


while i < len(students_temp):


print ("", i+1," ", students_temp [i][' 姓名 ']," ", students_temp [i]['total'])

  i += 1


  


print()


print ("-------------------------END-------------------------")


作者回复: Good Job

2019-01-19


Sniper


分享一下到目前为止 整理知识框架和知识点

https://mubu.com/doc/leLR2dkBw0


2019-01-19


Sniper


交作业：

import numpy as np


# create a structure


persontype = np.dtype({


    'names':['name', 'chinese', 'english', 'math', 'total'],


    'formats':['S32', 'i', 'i', 'i', 'i', 'i']})


# create an array base on the structure


peoples = np.array([('ZhangFei', 66, 65, 30, 0),


                    ('GuanYu', 95, 85, 98, 0),


                    ('ZhaoYun', 93, 92, 96, 0),


                    ('Huangzhong', 90, 88, 77, 0),


                    ('DianWei', 80, 90, 90, 0)],


                   dtype=persontype)


peoples[:]['total'] = peoples[:]['chinese'] + peoples[:]['english'] + peoples[:]['math']


ranking = np.sort(peoples, order='total')


print(ranking)


statictype = np.dtype({


    'names':['course', 'mean', 'min', 'max', 'var', 'std'],


    'formats':['S32', 'f', 'f', 'f', 'f', 'f']})


# create an array base on the structure


statistics = np.array([('chinese', 0, 0, 0, 0, 0),


                    ('english', 0, 0, 0, 0, 0),


                    ('math', 0, 0, 0, 0, 0)],


                   dtype=statictype)


for i in range(3):


    if i == 0:


        a = 'chinese'


    elif i == 1:


        a = 'english'


    else:


        a = 'math'


    statistics[i][1] = np.mean(peoples[:][a])


    statistics[i][2] = np.amin(peoples[:][a])


    statistics[i][3] = np.amax(peoples[:][a])


    statistics[i][4] = np.var(peoples[:][a])


    statistics[i][5] = np.std(peoples[:][a])


print(statistics)


老湿指导下 那块需要再优化或者改进的

作者回复：做出来就是不错，可以看下评论区大家的解答，高手在民间

2019-01-18


老友 @极客时间

今天重新复习了 方差标准差 加权平均数等概念😂😂😂😂😂

作者回复：加油～不错

2019-01-18


柚子

学习笔记：https://blog.csdn.net/hahaha66888/article/details/86523526

练习题代码如下

import numpy as np


dt = np.dtype({'names':['name','chinese','english','math'],


             'formats':['S32','i','i','i']})


person = np.array([('zhangfei',66,65,30),('guanyu',95,85,98),('zhaoyun',93,92,96),('huangzhong',90,88,77),('dianwei',80,90,90)],dtype = dt)


chinese = person[:]['chinese']


english = person[:]['english']


maths = person[:]['math']


#平均成绩

print(np.mean(chinese))


print(np.mean(english))


print(np.mean(maths))


#最大成绩

print(np.amax(chinese))


print(np.amax(english))


print(np.amax(maths))


#最小成绩

print(np.amin(chinese))


print(np.amin(english))


print(np.amin(maths))


#方差

print(np.std(chinese))


print(np.std(english))


print(np.std(maths))


#标准差

print(np.var(chinese))


print(np.var(english))


print(np.var(maths))


成绩之和排序：

def fun(a):


    return a[1]+a[2]+a[3]


print(sorted(person,key = fun,reverse=True))


作者回复：很好的笔记和作业

2019-01-17


胖陶

老师，如果输入的名字是中文就会报错 UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range (128)

2019-01-15


MJ


很多人都问了 S32 是什么意思，按我理解，是代表 32 字节的字符串（计算机 UTF-8 一般存储一个英文字符用一个字节，即最大可以存 32 个字符组成的字符串），请问老师，理解是否正确呢

作者回复：对的 解释正确

2019-01-11


与伊墨墨

排序

np.sort(a.sum(axis=0),axis=0)


2019-01-10


style


import numpy as np


studentType = np.dtype({


    'names' : ['name','chinese','english','math','total'],


    'formats' : ['U32','i','i','i','i']


})


#这里的 name 的是汉字，类型本来是 S32, 但是这样的话会报错的，我看过文档，用 U32 就不会报错，建议有问题看看文档

student = np.array ([(' 张飞 ',66,65,30,0),(' 关羽 ',95,85,98,0),(' 赵云 ',93,92,96,0),(' 黄忠 ',90,88,77,0),(' 典韦 ',80,90,90,0)],dtype=studentType)

names = student[:]['name']


chineses = student[:]['chinese']


englishs = student[:]['english']


maths= student[:]['math']


def dealNum(sub,num):


    print(sub)


print (' 平均值 =', np.mean (num))

print (' 最小值 =', np.amin (num))

print (' 最大值 =', np.amax (num))

print (' 方差 =', np.var (num))

print (' 标准差 =', np.std (num))

print('+++++++++++++++++++++++++++++++++++++++')


dealNum (' 语文 ',chineses)

dealNum (' 英语 ',englishs)

dealNum (' 数学 ',maths)

print('===============================')


student[:]['total'] = student[:]['chinese'] + student[:]['english']+student[:]['math']


rank = sorted(student,key=lambda x:x[1]+x[2]+x[3],reverse=False)


print(rank)


运行结果：

[(' 张飞 ', 66, 65, 30, 161), (' 黄忠 ', 90, 88, 77, 255), (' 典韦 ', 80, 90, 90, 260), (' 关羽 ', 95, 85, 98, 278), (' 赵云 ', 93, 92, 96, 281)]

之前没接触过 numpy, 看到这一篇的时候，我整整看了三天，看了遍菜鸟上面的 numpy 的教程，结合上面老师的提议，写出这么个结果

2019-01-10


浩

studenttype = np.dtype({'names':['name', 'chinese', 'english', 'maths'], 'formats':['S32', 'i', 'i', 'i']})


students = np.array([('zhangfei', 66, 65, 30), ('guanyu', 95, 85, 98), ('zhaoyun', 93, 92, 96),('huangzhong', 90, 88, 77), ('dianwei', 80, 90, 90)], dtype=studenttype)


有什么方法可以快捷方便的把 students 中的后面 3 个元素提取出来变成

[(66, 65, 30), (95, 85, 98), (93, 92, 96), ( 90, 88, 77), (80, 90, 90)]


吗？？

2019-01-08


白夜

1.list 保存的是对象的指针，Numpy 可以省去这个指针

2. 连续储存，查的快

3. 可以多线程，计算更快

这个知识点不太理解为什么加快了：在内存访问模式中，缓存会直接把字节块从 RAM 加载到 CPU 寄存器中。因为数据连续的存储在内存中，NumPy 直接利用现代 CPU 的矢量化指令计算，加载寄存器中的多个连续浮点数。

---


顺便吐槽一下，这个回帖不能加图片，而且复制文章内容只能复制几个字，有点麻烦

2019-01-08


style


老师我有个疑问

student = np.dtype([('name','S20'), ('age', 'i1'), ('marks', 'f4')])


a = np.array([('abc', 21, 50),('xyz', 18, 75)], dtype = student)


print(a)


print(a['name'])


结果：

[(b'abc', 21, 50.) (b'xyz', 18, 75.)]


[b'abc' b'xyz']


我不明白为什么打印出来的 name 会带一个 b, 老师你前面举例子的 persontype 那个也是这样子，还请老师解答下

作者回复: Python3 默认 str 是 Unicode 类型，所以要转成 bytestring，会在原来的 str 前加上 b

2019-01-08


Sunway.


第一个问题

numpy 将值存储在连续的内存块中，而 list 是分散存储，使用 numpy 可以提升处理速度并节省资源，除此之外还有栈，队列，链表，图等数据类型

第二个问题

我的代码还不够简洁，输出那里应该写成循环的机构来节省代码量，看了评论里其他同学的回答后再参考着改的更简洁些

import numpy as np


persontype = np.dtype({


    'names':['name', 'chinese', 'english', 'math', 'total'],


    'formats':['S32', 'i', 'i', 'i', 'i'], })


grades=np.array([("ZhangFei", 66, 65, 30, 0), ("GuanYu", 95, 85, 98, 0),


                 ("ZhaoYun", 93, 92, 96, 0), ("HuangZhong", 90, 88, 77, 0),


                 ("DianWei", 80, 90, 90, 0)], dtype=persontype)


chineses = grades[:]['chinese']


englishs = grades[:]['english']


maths = grades[:]['math']


grades[:]['total']=grades[:]['chinese']+grades[:]['english']+grades[:]['math']


print ("语文成绩平均值，最小成绩，最大成绩，方差，标准差")

print(np.mean(chineses))


print(np.amin(chineses))


print(np.amax(chineses))


print(np.var(chineses))


print(np.std(chineses))


print ("英语成绩平均值，最小成绩，最大成绩，方差，标准差")

print(np.mean(englishs))


print(np.amin(englishs))


print(np.amax(englishs))


print(np.var(englishs))


print(np.std(englishs))


print ("数学成绩平均值，最小成绩，最大成绩，方差，标准差")

print(np.mean(maths))


print(np.amin(maths))


print(np.amax(maths))


print(np.var(maths))


print(np.std(maths))


#排序

print(np.sort(grades, order='total')[::-1])


2019-01-08


圆圆的大食客

把老师的 hint 和第一个答案结合了一下：

import numpy as np


persontype = np.dtype({


    'names':['name', 'chinese', 'math', 'english', 'total'],


    'formats':['S32', 'i', 'i', 'i', 'i']})


peoples = np.array([("ZhangFei",66,65,30, 0),("GuanYu",95,85,98, 0),


       ("ZhaoYun",93,92,96, 0),("HuangZhong",90,88,77, 0),


       ("dianwei",80,90,90,0)], dtype=persontype)


chineses = peoples[:]['chinese']


maths = peoples[:]['math']


englishs = peoples[:]['english']


peoples[:]['total'] = peoples[:]['chinese'] + peoples[:]['math']+ peoples[:]['english']


print ("chinese ave", np.mean(chineses))


print ("chinese max", np.amax(chineses))


print ("chinese min", np.amin(chineses))


print ("chinese std", np.std(chineses))


print ("chinese var", np.var(chineses))


#其他科目这里就不写了

print (sorted(peoples, key=lambda x:x[4], reverse=True))


作者回复：优秀

2019-01-08


少年不识钱滋味

第四讲笔记

https://mubu.com/doc/3QadR_xU7v


2019-01-07


Elvis Young


import numpy as np


students=np.dtype([('Name','S64'),('Chinese','i'),('English','i'),('Math','i'),('Score','i')])


p=np.array([('zhangfei','66','65','30','0'),


            ('guanyu','95','85','98','0'),


            ('zhaoyun','93','92','96','0'),


            ('huangzhong','90','88','77','0'),


            ('dianwei','80','90','90','0')],dtype=students)


p[:]['Score']=p[:]['Chinese']+p[:]['English']+p[:]['Math']


Scores=p[:]['Score']


print(Scores)


Chineses=p[:]['Chinese']


Englishs=p[:]['English']


Maths=p[:]['English']


Chn_min=np.amin(Chineses)


Chn_max=np.amax(Chineses)


Chn_mean=np.mean(Chineses)


Chn_std=np.std(Chineses)


Chn_var=np.var(Chineses)


Eng_min=np.amin(Englishs)


Eng_max=np.amax(Englishs)


Eng_mean=np.mean(Englishs)


Eng_std=np.std(Englishs)


Eng_var=np.var(Englishs)


Math_min=np.amin(Maths)


Math_max=np.amax(Maths)


Math_mean=np.mean(Maths)


Math_std=np.std(Maths)


Math_var=np.var(Maths)


print('Chn_min=',Chn_min)


print('Chn_max=',Chn_max)


print('Chn_mean=',Chn_mean)


print('Chn_std=',Chn_std)


print('Chn_var=',Chn_var)


print('Eng_min=',Eng_min)


print('Eng_max=',Eng_max)


print('Eng_mean=',Eng_mean)


print('Eng_std=',Eng_std)


print('Eng_var=',Eng_var)


print('Math_min=',Math_min)


print('Math_max=',Math_max)


print('Math_mean=',Math_mean)


print('Math_std=',Math_std)


print('Math_var=',Math_var)


#orders =sorted(p,key = lambda p:(p[1]+p[2]+p[3]) , reverse=True)


#orders=np.sort(p,order='Score')


index=np.lexsort([-p[:]['Score']])


orders=p[index][:]


print(orders)


2019-01-07


xfoolin


import numpy as np


#创建 score_type 的类型

score_type = np.dtype({


    'names':['name','chinese','english','math','total'],


    'formats':['S32','i','i','i','i'],


})


#初始化 scores 数组

scores = np.array([


    ('Zhangfei',66,65,30,0),


    ('Guanyu',95,85,98,0),


    ('Zhaoyun',93,92,96,0),


    ('Huangzhong',90,88,77,0),


    ('Dianwei',80,90,90,0)],dtype = score_type)


#定义数组中的 total 为其他科目成绩的总和

scores[:]['total'] = scores[:]['chinese']+scores[:]['english']+scores[:]['math']


#循环求出平均值、最大值、最小值、方差、标准差

for i in ('chinese','english','math'):


    print('%s_scores_:'%(i))


    print('%s_scores_mean| %.2f'%(i,np.mean(scores[:][i])))


    print('%s_scores_max| %.2f'%(i,np.amax(scores[:][i])))


    print('%s_scores_min| %.2f' % (i, np.amin(scores[:][i])))


    print('%s_scores_var| %.2f' % (i, np.var(scores[:][i])))


    print('%s_scores_std| %.2f\n' % (i, np.std(scores[:][i])))


print('scores_rank:')


#按 total 排序，sort 是升序，通过索引逆序排列

print(np.sort(scores,order = 'total')[::-1])


2019-01-06


七步

之前忘记排序了，现在补上。

import numpy as np


persontype=np.dtype({


    'names':['name','chinese','english','math'],


    'formats':['S32','i','i','i']})


#print persontype


peoples=np.array([("ZhangFei",66,65,30),


                  ("GuanYu",95,85,98),


                  ("ZhaoYun",93,92,96),


                  ("HuangZhong",90,88,77),


                  ("DianWei",80,90,90)],


                 dtype=persontype)


#print peoples


chinese=peoples[:]['chinese']


english=peoples[:]['english']


math=peoples[:]['math']


print "各科平均值",np.average (chinese),np.average (english),np.average (math)

print "各科最低成绩",np.amin (chinese),np.amin (english),np.amin (math)

print "各科最高成绩",np.amax (chinese),np.amax (english),np.amax (math)

print "各科方差",np.var (chinese),np.var (english),np.var (math)

print "各科标准差",np.std (chinese),np.std (english),np.std (math)

ranking=sorted(peoples,key=lambda x:(x[1]+x[2]+x[3]),reverse=True)


print ranking


结果如下：

各科平均值 84.8 84.0 78.2

各科最低成绩 66 65 30

各科最高成绩 95 92 98

各科方差 114.96 95.6 634.56

各科标准差 10.7219401229 9.77752524926 25.1904743901

[('ZhaoYun', 93, 92, 96), ('GuanYu', 95, 85, 98), ('DianWei', 80, 90, 90), ('HuangZhong', 90, 88, 77), ('ZhangFei', 66, 65, 30)]


2019-01-06


七步

课后练习，我的代码如下：

import numpy as np


persontype=np.dtype({


    'names':['name','chinese','english','math'],


    'formats':['S32','i','i','i']})


peoples=np.array ([("张飞",66,65,30),

("关羽",95,85,98),

("赵云",93,92,96),

("黄忠",90,88,77),

("典韦",80,90,90)],

                 dtype=persontype)


chinese=peoples[:]['chinese']


english=peoples[:]['english']


math=peoples[:]['math']


print np.average(chinese),np.average(english),np.average(math)


print np.amin(chinese),np.amin(english),np.amin(math)


print np.amax(chinese),np.amax(english),np.amax(math)


print np.var(chinese),np.var(english),np.var(math)


print np.std(chinese),np.std(english),np.std(math)


运行结果如下：

[('\xd5\xc5\xb7\xc9', 66, 65, 30) ('\xb9\xd8\xd3\xf0', 95, 85, 98)


 ('\xd5\xd4\xd4\xc6', 93, 92, 96) ('\xbb\xc6\xd6\xd2', 90, 88, 77)


 ('\xb5\xe4\xce\xa4', 80, 90, 90)]


84.8 84.0 78.2


66 65 30


95 92 98


114.96 95.6 634.56


10.7219401229 9.77752524926 25.1904743901


2019-01-06


等待

import numpy as np


print ("练习题")

studentType = np.dtype({


'names':['name',' 语文 ',' 英语 ',' 数学 ',' 总分 '],

        'formats':['S32','i','i','i','f']})


students = np.array([("zhangfei",66,65,30,0),("guanyu",95,85,98,0),("zhaoyun",93,92,96,0),("huangzhong",90,88,77,0),("dianwei",80,90,90,0)],dtype = studentType)


print(students)


#print(np.average(students))


name = students[:]['name']


yuwen = students [:][' 语文 ']

yingyu = students [:][' 英语 ']

shuxue = students [:][' 数学 ']

print(name)


print(np.average(yuwen))


print(np.average(shuxue))


print(np.average(yingyu))


print ("语数英最小成绩")

print ("语文:",np.amin (yuwen))

print ("数学:",np.amin (shuxue))

print ("英语:",np.amin (yingyu))

print ("语数英最大成绩")

print(np.amax(yuwen))


print(np.amax(shuxue))


print(np.amax(yingyu))


print ("标准差")

print(np.std(yuwen))


print(np.std(shuxue))


print(np.std(yingyu))


print ("方差")

print(np.var(yuwen))


print(np.var(shuxue))


print(np.var(yingyu))


students [:][' 总分 ']=students [:][' 语文 ']+students [:][' 数学 ']+students [:][' 英语 ']

totals = students [:][' 总分 ']

print(totals)


print ("排名：")

#print(np.sort(totals))


print (np.sort (students,order = ' 总分 '))

2019-01-06


●


np.percentile(a，p)=min+(max-min)*p


2019-01-05


●


试验了一下，np.amin (a,0) 是每列中的最小值

2019-01-05


爱做梦的咸鱼

中文名字写 S32 报错的你不是一个人，改成 U32 的 dtype 即可，超出的长度会截断

作者回复：中文的话是要修改成 U32

2019-01-05


XUN


python 很强大也很灵活，脑子转不大过来，题目解答如下：

#coding=utf-8


import numpy as np


chinese = 'Chinese'


english = 'English'


math = 'Math'


dataType = np.dtype = {


    'names': ['Name', chinese, english, math],


    'formats': ['S32', 'i', 'i', 'i']


}


students = np.array(


    [


        ('ZhangFei', 66, 65, 30),


        ('GuanYu', 95, 85, 98),


        ('ZhaoYun', 93, 92, 96),


        ('HuangZhong', 90, 88, 77),


        ('DianWei', 80, 90, 90),


    ],


    dataType


)


chineseArray = students[:][chinese]


englishArray = students[:][english]


mathArray = students[:][math]


# 平均分

print (' 语文平均分：{}'.format (np.mean (chineseArray)))

print (' 英语平均分：{}'.format (np.mean (englishArray)))

print (' 数学平均分：{}'.format (np.mean (mathArray)))

# 最低成绩

print (' 语文最低成绩：{}'.format (np.min (chineseArray)))

print (' 英语最低成绩：{}'.format (np.min (englishArray)))

print (' 数学最低成绩：{}'.format (np.min (mathArray)))

# 最高成绩

print (' 语文最高成绩：{}'.format (np.max (chineseArray)))

print (' 英语最高成绩：{}'.format (np.max (englishArray)))

print (' 数学最高成绩：{}'.format (np.max (mathArray)))

# 成绩方差

print (' 语文成绩方差：{}'.format (np.var (chineseArray)))

print (' 英语成绩方差：{}'.format (np.var (englishArray)))

print (' 数学成绩方差：{}'.format (np.var (mathArray)))

# 成绩标准差

print (' 语文成绩标准差：{}'.format (np.std (chineseArray)))

print (' 英语成绩标准差：{}'.format (np.std (englishArray)))

print (' 数学成绩标准差：{}'.format (np.std (mathArray)))

# 总成绩排序

def getSortKey(a):


    return a[1] + a[2] + a[3]


    


print(sorted(students, key=getSortKey, reverse=True))


作者回复: Good numpy 统计函数掌握的不错，排序使用的也很好

2019-01-05


Switch


axis 轴的概念，类比于空间坐标系就好理解了。

数组中轴的概念类比于坐标系中 x,y 轴的概念

在二维数组中，axis=0 代表的是纵轴 (跨行),axis=1 代表的是横轴 (跨列)

2019-01-05


灵灵

# -- coding: utf-8 -


import numpy as np


persontype = np.dtype({'names':['name','chineses','englishs','maths'],'formats':['S32','f','f','f']})


peoples = np.array([("zhangfei",66,65,30),("guanyu",95,85,98),("zhaoyun",93,92,96),("huangzhong",90,88,77),("dianwei",80,90,90)],dtype=persontype)


names = peoples[:]['name']


chineses = peoples[:]['chineses']


englishs = peoples[:]['englishs']


maths = peoples[:]['maths']


sum = chineses + englishs + maths


def scores(subject,score):


print (subject,"的平均成绩为：",np.mean (score))

print (subject,"的最大成绩为：",np.amax (score))

print (subject,"的最小成绩为：",np.amin (score))

print (subject,"的方差为：",np.std (score))

print (subject,"的标准差为：",np.var (score))

print (scores('chineses',chineses),scores('maths',maths),scores('englishs',englishs))


print (np.sort(sum))


2019-01-04


JC


基本可以理解，但是在 sort 函数中 array ([[4,3,2],[2,4,1]]) 进行 np.sort (a,axis=0) 时，没有明白为什么结果是 [[2 3 1] [4 4 2]]

作者回复: axis=0 代表的是跨行（跨行就是按照列），所以实际上是对 [4, 2] [3, 4] [2, 1] 来进行排序，所以结果是 [2, 4] [3, 4] [1, 2] 这个是每一列的排序结果。实际结果也就是 [[2 3 1], [4, 4, 2]]

2019-01-04


风

建议是，感觉代码太长有的看不完整，https://paste.ubuntu.com/ 这个网站可以贴代码，很方便。

收获是，对 numpy 的底层了解了一下，还有结构体的定义。

作者回复：这个网站不错

2019-01-03


Believer (入陣)

补作业 rrrr

import numpy as np


grade_type = np.dtype({


    'names': ['name', 'Chinese', 'English', 'Math'],


    'formats': ['S32', 'i', 'i', 'i']


})


grades = np.array([("Zhang Fei", 66, 65, 30), ("Guan Yu", 95, 85, 98),


                   ("Zhao Yun", 93, 92, 96), ("Huang Zhong", 90, 88, 77),


                   ("Dian Wei", 80, 90, 90)], dtype = grade_type)


subjects = ['Chinese', 'English', 'Math']


for subject in subjects:


    print(subject)


    data = grades[:][subject]


    print(np.average(data))


    print(np.amin(data))


    print(np.amax(data))


    print(np.std(data))


    print(np.var(data))


rank = sorted(grades, key = lambda x: x[1]+x[2]+x[3], reverse = True)


print(rank)


作者回复: 👍正确

2019-01-03


icy


import numpy as np


stype=np.dtype({'names':['name','chinese','english','math'],'formats':['S32','i','i','i']})


data=np.array([('ZF',66,65,30),('GY',95,85,98),('ZY',93,92,96),('HZ',90,88,77),('DW',80,90,90)],dtype=stype)


sorted(data,key=lambda data:data['chinese']+data['english']+data['math'],reverse=True)


python3 里好多函数的用法变了，建议老师用 python3 来举例。

作者回复：看到很多人在用 py3，算法篇里的代码会用 py3 来写

2019-01-03


李沛欣

今天的看完了，Numpy 相当于用 python 语言环境下的数学。感觉自己重新上了一遍数学课。继续努力。

作者回复：加油💪

2019-01-03


.


老师，想问下为什么写这一行时报错为：no

field of name total


peoples[:]['total'] = peoples[:]['chinese']+peoples[:]['english']+peoples[:]['math']


作者回复：在做 persontype 定义的时候，可以添加 'total' 这个属性：

persontype = np.dtype({


'names': ['name', 'chinese', 'math', 'english', 'total'],


'formats': ['S32', 'i', 'i', 'i', 'i']})


另外在设置 peoples 数组的时候，把 total 这个属性的值默认设置为 0, 这样你就可以在后面使用 peoples [:]['total']

2019-01-03


执笔，封心

遇到一个问题，按照代码来写，还是错了，显示不是二元或者三元数组

2019-01-02


执笔，封心

代码没有写全，很蛋疼呀

2019-01-02


执笔，封心

上面的那个问题解决了，但是为什么是 data type not understand 呢

2019-01-02


执笔，封心

老师。您好，在 pycharm 3.7.1 的版本中没有 numpy 这个多维属数组

2019-01-02


般若

一般什么场景下会用到标准差或方差？

2019-01-02


西夶

请问下我按照课里面的修改了对应作业的内容，出现了「entry not a 2- or 3- tuple」是为什么呀

2018-12-31


LQ


似懂非懂 能不能用实践来驱动

2018-12-31


I.L


import numpy as np


persontype = np.dtype({


'names': ['name', 'chinese', 'english', 'math'],


'formats': ['S32', 'i', 'i', 'i']})


peoples = np.array([


('ZhangFei', 66, 65, 30),


('GuanYu', 95, 85, 98),


('ZhaoYun', 93, 92, 96),


('HuangZhong', 90, 88, 77),


('DianWei', 80, 90, 90)


], dtype=persontype)


Chineses = peoples[:]['chinese']


Englishs = peoples[:]['english']


Maths = peoples[:]['math']


print(Chineses,Englishs,Maths)


def ClassAnalyze(subject):


print("""


---


平均成绩: % r

最小成绩: % r

最大成绩: % r

方差: % r

标准差: % r

---""" %(np.average(subject), np.min(subject), np.max(subject), np.var(subject), np.std(subject)))


print(ClassAnalyze(Chineses), ClassAnalyze(Englishs), ClassAnalyze(Maths))


newsort = sorted(peoples, key=lambda x:x[1]+x[2]+x[3], reverse=True)


print(newsort)


=====


几个概念问题：

1. axis 的 index 为顺序轴，0 轴为默认 x 轴，1 轴为第二轴（此处即 y 轴）

2. np.array () 数组为向量数组，可进行直接向量计算

3. 定义 dtype 时，对应 formats 中的 S32，i，f 等对应的应是数据格式（int，float，str32 等）

年底忙死了，趁节日来学习了，感觉 numpy 这类计算被 pandas 完爆

2018-12-31


胡

__author__ = 'Administrator'


import numpy as np


persontype = np.dtype(


    {'names': ['name', 'chinese', 'english', 'math','total'],


     'formats': ['S32', 'i', 'i', 'i','i']


    }


)


peoples = np.array(


    [('ZhangFei', 66, 65, 30,0),


     ('GuanYu', 95, 85, 98,0),


     ('ZhaoYun', 93, 92,96,0),


     ('HuangZhong', 90, 88, 77,0),


     ('DianWei', 80, 90, 90,0)],


    dtype=persontype


)


peoples[:]['total']= peoples[:]['chinese'] +peoples[:]['english']+peoples[:]['math']


print (peoples)


chinese=peoples[:]['chinese']


math=peoples[:]['math']


english=peoples[:]['english']


total=peoples[:]['total']


print (' 语文均分：',np.mean (chinese))

print (' 数学均分：',np.mean (math))

print (' 英语均分：',np.mean (english))#

print (' 语文最低分：',np.amin (chinese))

print (' 数学最低分：',np.amin (math))

print (' 英语最低分：',np.amin (english))#

print (' 语文最高分：',np.amax (chinese))

print (' 数学最高分：',np.amax (math))

print (' 英语最高分：',np.amax (english))#

print (np.sort(peoples, order='total'))


2018-12-31


胡

老师能否帮忙看下为什么 numpy 已经安装好，但是 pycharm 无法 ipmort? 检查好久，不得解。

2018-12-31


caidy


1. 为什么要用 numpy 而不用 python 自带的列表 list，这个是因为 list 是分散储存的，而 numpy 储存在一个连续的内存地址，使用 numpy 会更高效

2.


import numpy as np


persontype = np.dtype({


'names':['name', 'chinese', 'english', 'math'],


'formats': ['S32', 'i', 'i', 'i']


})


peoples = np.array([


("ZhangFei", 66, 65, 30),


("GuanYU", 95, 85, 98),


("ZhaoYun", 93, 92, 96),


("GuangZhong", 90, 88, 77),


("DianWei", 80, 90, 90)],


dtype = persontype)


names = peoples[:]['name']


chineses = peoples[:]['chinese']


englishs = peoples[:]['english']


maths = peoples[:]['math']


def display(subject, scores):


print(subject + ' | ' + str(np.mean(scores)) + " | " + str(np.amin(scores)) + ' | '+


str(np.amax(scores)) + ' | ' + str(np.std(scores)) + ' | ' + str(np.var(scores)))


print("subject | sorce_avg | score_min | score_max | score_std | score_var")


display('chineses', chineses)


display('englishs', englishs)


display('maths', maths)


persontype1 = np.dtype({


'names':['name', 'scores'],


'formats': ['S32', 'i']


})


score = np.array([


("ZhangFei", peoples[0][1]+peoples[0][2]+peoples[0][3]),


("GuanYU", peoples[1][1]+peoples[1][2]+peoples[1][3]),


("ZhaoYun", peoples[2][1]+peoples[2][2]+peoples[2][3]),


("GuangZhong", peoples[3][1]+peoples[3][2]+peoples[3][3]),


("DianWei", peoples[4][1]+peoples[4][2]+peoples[4][3])],


dtype = persontype1)


print(np.sort(score, order='scores'))


2018-12-30


竹本先生

score_type = np.dtype({


'names': ['name', 'yuwen', 'yingyu', 'shuxue', 'heji'],


'formats': ['S32', 'i', 'i', 'i', 'i']


})


person = np.array([


('ZhangFei',66,65,30,0),


('GuanYu',95,85,98,0),


('ZhaoYun',93,92,96,0),


('HuangZhong',90,88,77,0),


('DianWei',80,90,90,0)


], dtype=score_type)


# ~ 先求和

person[:]['heji'] = person[:]['yuwen'] + person[:]['yingyu'] + person[:]['shuxue']


yuwens = person[:]['yuwen']


yingyus = person[:]['yingyu']


shuxues = person[:]['shuxue']


print (' 语文：平均分 % f 最低分 % i 最高分 % i 方差 % f 标准差 % f' %(yuwens.mean (), yuwens.min (), yuwens.max (), yuwens.var (), yuwens.std ()))

print (' 英语：平均分 % f 最低分 % i 最高分 % i 方差 % f 标准差 % f' %(yingyus.mean (), yingyus.min (), yingyus.max (), yingyus.var (), yingyus.std ()))

print (' 数学：平均分 % f 最低分 % i 最高分 % i 方差 % f 标准差 % f' %(shuxues.mean (), shuxues.min (), shuxues.max (), shuxues.var (), shuxues.std ()))

# ~ 排序

person = np.sort(person, order="heji")


for i in range(person.size):


print (' 第 % i 名：% s % i 分 ' %(i+1, person [4-i]['name'], person [4-i]['heji']))

运行结果这样：

语文：平均分 84.800000 最低分 66 最高分 95 方差 114.960000 标准差 10.721940

英语：平均分 84.000000 最低分 65 最高分 92 方差 95.600000 标准差 9.777525

数学：平均分 78.200000 最低分 30 最高分 98 方差 634.560000 标准差 25.190474

第 1 名：b'ZhaoYun' 281 分

第 2 名：b'GuanYu' 278 分

第 3 名：b'DianWei' 260 分

第 4 名：b'HuangZhong' 255 分

第 5 名：b'ZhangFei' 161 分

原来我用 php 和 js 比较多，学这课程颇费力，求和、降序的实现给我感觉很别扭，

另外为何姓名输出会前面带个 b，还输出了引号

2018-12-30


易平

老师，想我们这种编程基础比较差的，除了听您的课程和每一课后面的题目以外，还需要额外做哪些课外训练呢？谢谢

作者回复：你能把专栏的代码，还有题目都做一遍就是很好的。如果还有时间，自己整理下课程的笔记，可以发到留言区里，和大家一起交流学习

2018-12-30


易平

#------------ 第 4 课 ---------------------------

#你能用自己的话说明一下为什么要用 NumPy 而不是 而不是 Python 的列表 list 吗？

#除此之外，你还知道那些数据结构类型？

'''


1. 在 python 中的 list 实际上就是数组结构，由于 python 数组中的元素可以是任意的对象，

所以列表中 list 保存的对象的指针。这样我们保存一个 N 个元素的数组。就需要 N 个指针和 N 个对象，这样对

于 python 来说是非常不经济的，浪费了内存和计算时间

2. 在 python 中 List 的元素在系统内存中是分散存储的，而 NumPy 数组存储在一个均匀连续的内存块中。这样

数组计算遍历所有元素，不像 list 还需要对内存进行查找，从而结算了计算资源。

3. 在内存访问模式中，因为数据连续的存储在内存中，Numpy 直接利用现代 CPU 的矢量化指令计算，加载寄存器中的

多个连续浮点数。

4. Numpy 中的矩阵计算可以采用多线程的方式，充分利用多核 CPU 计算资源

'''


'''


练习题：统计全班的成绩假设一个团队里有 5 名学员，成绩如下表所示.

你可以用 NumPy 统计下这些人在语文、英语、数学中的

平均成绩、最小成绩、最大成绩、方差、标准差。然后把这些人的总总成绩排序，得出名次进行成绩输出。

'''


#自定义数组类型

#自定义数组类型

persontype=np.dtype({


        'names':['name','chinese','english','math','total'],


        'formats':['S32','i','i','i','i']


        })


    


peoples=np.array([('zhangfei',66,65,30,0),('guanyu',95,85,98,0),('zhaoyun',93,92,96,0),


                  ('huangzhong',90,88,77,0),('dianwei',80,90,90,0)],


    dtype=persontype)


chinese=peoples[:]['chinese']


english=peoples[:]['english']


math=peoples[:]['math']


peoples[:]['total']=peoples[:]['chinese']+peoples[:]['english']+peoples[:]['math']


#计算平均成绩

print('avg_chinese:', np.mean(chinese))


print('avg_english:', np.mean(english))


print('avg_math:', np.mean(math))


#计算最小成绩

print('min_chinese:', np.amin(chinese))


print('min_english:', np.amin(english))


print('min_math:', np.amin(math))


#计算最大成绩

print('max_chinese:', np.amax(chinese))


print('max_english:', np.amax(english))


print('max_math:', np.max(math))


#方差和标准差

print('std_chinese:', np.std(chinese))


print('std_english:', np.std(english))


print('std_math:', np.std(math))


print('var_chinese:', np.var(chinese))


print('var_english:', np.var(english))


print('var_math:', np.var(math))


#计算总成绩

print(np.sort(peoples,order='total'))


2018-12-30


米可哲

import numpy as np


student_type=np.dtype({'names':['name','chinese','english','math'],'formats':['S32','i','i','i']})


student=np.array([("zhangfei",66,65,30),("guanyu",95,85,98),("zhaoyun",93,92,96),("huangzhong",90,88,77),("dianwei",80,90,90)],dtype=student_type)print(student)


chinese=student[:]['chinese']


english=student[:]['english']


math=student[:]['math']


def show(name,sco):


print (" 科目:{} 平均成绩:{} 最小成绩:{} 最大成绩:{} 方差:{} 标准差:

{}".format(name,np.mean(sco),np.min(sco),np.max(sco),np.var(sco),np.std(sco)))


show("chinese",chinese)


show("math",math)


show("english",english)


ranker=sorted(student,key=lambda x:(x[1]+x[2]+x[3]),reverse=True)


print(ranker)


2018-12-30


Albert Ou


# -*- coding: utf-8 -*


import numpy as np


student_type = np.dtype({


'names':[' 姓名 ',' 语文 ',' 英语 ',' 数学 '],

    'formats':['S32','f','f','f']


})


students=np.array([('zhangfei',66,65,30),


                  ('guanyu',95,85,98),


                  ('zhaoyun',93,92,96),


                  ('huangzhong',90,88,77),


                  ('dianwei',80,90,90)],dtype=student_type)


print (' 科目 | 平均成绩 | 最小成绩 | 最大成绩 | 方差 | 标准差 ')

for i in range(1,4):


    kemu = students.dtype.names[i]


    score = students[:][kemu]


    print('%s|%f|%f|%f|%f|%f' % (kemu,np.mean(score),np.min(score),np.max(score),np.var(score),np.std(score)))


    


print('\n')


ranking=sorted(students,key=lambda x: (x[1]+x[2]+x[3]), reverse=True)


print (' 姓名 | 语文 | 英语 | 数学 ')

for student in students:


print ('% s|% f|% f|% f' %(student [' 姓名 '],student [' 语文 '],student [' 英语 '],student [' 数学 ']))

2018-12-29


一帆时空杨丰

# -*- coding: utf-8 -* #


import numpy as np


studenttype = np.dtype({


    'names': ['name', 'chinese', 'english', 'math', 'total'],


    'formats': ['S32', 'i', 'i', 'i', 'i']})


students = np.array([("Zhangfei", 66, 65, 30, 0),


                     ("Guanyu", 95, 85, 98, 0),


                     ("Zhaoyun", 93, 92, 96, 0),


                     ("Huangzhong", 90, 88, 77, 0),


                     ("Dianwei", 80, 90, 90, 0)],


                    dtype=studenttype)


chineses = students[:]['chinese']


englishs = students[:]['english']


maths = students[:]['math']


# print(type(maths))


# 各科平均分

print ("语文平均分：" + str (np.mean (chineses)))

print ("数学平均分：" + str (np.mean (maths)))

print ("英语平均分：" + str (np.mean (englishs)))

# 最低分

print ("语文最低分：" + str (np.amin (chineses)))

print ("数学最低分：" + str (np.amin (maths)))

print ("英语最低分：" + str (np.amin (englishs)))

# 最高分

print ("语文最高分：" + str (np.amax (chineses)))

print ("数学最高分：" + str (np.amax (maths)))

print ("英语最高分：" + str (np.amax (englishs)))

# 标准差 标准差是方差的算术平方根

print ("语文标准差：" + str (np.std (chineses)))

print ("数学标准差：" + str (np.std (maths)))

print ("英语标准差：" + str (np.std (englishs)))

# 方差

print ("语文方差：" + str (np.var (chineses)))

print ("数学方差：" + str (np.var (maths)))

print ("英语方差：" + str (np.var (englishs)))

students[:]['total'] = np.add(students[:]['chinese'], students[:]['math'])


students[:]['total'] = np.add(students[:]['total'], students[:]['english'])


print(np.sort(students, order='total'))


2018-12-29


ikel


import numpy as np


persontype = np.dtype({


    'names': ['name', 'chinese', 'english', 'math','total'],


    'formats': ['S32', 'i', 'i', 'i', 'i']})


peoples = np.array([('zhangfei', 66, 65, 30,0), ('guanyu', 95, 85, 98,0),


                    ('zhaoyun', 93, 92, 96,0), ('huangzhong', 90, 88, 77,0),


                    ('dianwei', 80, 90, 90,0)], dtype=persontype)


#统计下这些人在语文、英语、数学中的平均成绩、最小成绩、最大成绩、方差、标准差。

chineses = peoples[:]['chinese']


englishs = peoples[:]['english']


maths = peoples[:]['math']


peoples[:]['total'] = peoples[:]['chinese'] + peoples[:]['english'] + peoples[:]['math']


#print(peoples)


#print(ages)


#print(np.mean(ages))


#平均成绩

print(np.mean(chineses))


print(np.mean(englishs))


print(np.mean(maths))


#最小成绩

print(np.amin(chineses))


print(np.amin(englishs))


print(np.amin(maths))


#最大成绩

print(np.amax(chineses))


print(np.amax(englishs))


print(np.amax(maths))


#方差

print(np.var(chineses))


print(np.var(englishs))


print(np.var(maths))


#标准差

print(np.std(chineses))


print(np.var(englishs))


print(np.var(maths))


#总成绩排序

print(np.sort(peoples, order='total'))


2018-12-28


程序员小熊猫

1. list 列表在内存中是分散存储的，而 ndarray 存储在一个连续的内存中，遍历数组中的元素时，就不会像 list 列表那样还要对内存地址进行查找，从而节省了资源。

2. Numpy 的矩阵运算使用了多线程，大大提升了计算效率

2018-12-28


周飞

# -*- coding: utf-8 -*


import numpy as np


persontype = np.dtype({


    'names':['name', 'chinese', 'english', 'math','total'],


    'formats':['S32','i', 'i', 'i','i']})


peoples = np.array([("zhangfei",66,65,30,0),("guanyu",95,85,98,0),


       ("zhaoyun",93,92,96,0),("huangzhong",90,88,77,0),("dianwei",80,90,90,0)],


    dtype=persontype)


chineses = peoples[:]['chinese']


maths = peoples[:]['math']


englishs = peoples[:]['english']


print ('======== 各科平均成绩 ==========')

print (' 语文的平均成绩是 ',np.mean (chineses))

print (' 数学的平均成绩是 ',np.mean (maths))

print (' 英语的平均成绩是 ',np.mean (englishs))

print ('========= 各科最小成绩 =========')

print (' 语文的最小成绩是 ',np.amin (chineses))

print (' 数学的最小成绩是 ',np.amin (maths))

print (' 英语的最小成绩是 ',np.amin (englishs))

print ('========= 各科最大成绩 =========')

print (' 语文的最大成绩是 ',np.amax (chineses))

print (' 数学的最大成绩是 ',np.amax (maths))

print (' 英语的最大成绩是 ',np.amax (englishs))

print ('========= 各科成绩标准差 =========')

print (' 语文成绩标准差是 ',np.std (chineses))

print (' 数学成绩标准差是 ',np.std (maths))

print (' 英语成绩标准差是 ',np.std (englishs))

print ('========= 各科成绩方差 =========')

print (' 语文成绩方差是 ',np.var (chineses))

print (' 数学成绩方差是 ',np.var (maths))

print (' 英语成绩方差是 ',np.var (englishs))

peoples[:]['total']= np.add(chineses,maths)


peoples[:]['total'] =np.add(peoples[:]['total'],englishs)


print (np.sort(peoples,order='total')[::-1])


2018-12-27


顾合

基本上都懂了～哈哈，虽然前面讲 numpy 的时候有些云里雾里😂😂

2018-12-27


夜空中最亮的星（华仔）

看了精选留言才写出排序，用的是 python3

import numpy as np


'''


假设一个团队里有 5 名学员，成绩如下表所示。

1. 用 NumPy 统计下这些人在语文、英语、数学中的平均成绩、最小成绩、最大成绩、方差、标准差。

2. 总成绩排序，得出名次进行成绩输出。

'''


persontype = np.dtype({


    'names':['name', 'chinese', 'english', 'math','total'],


    'formats':['S32','i', 'i', 'i','i']})


peoples = np.array([("ZhangFei",66,65,30,0),("GuanYu",95,85,98,0),


                    ("ZhaoYun",93,92,96,0),("HuangZhong",90,88,77,0),


                    ("Dianwei",80,90,90,0)],


                   dtype=persontype)


# print(peoples)


peoples[:]['total'] = peoples[:]['chinese']+peoples[:]['english']+peoples[:]['math']


total = peoples[:]['total']


name = peoples[:]['name']


chineses = peoples[:]['chinese']


englishs = peoples[:]['english']


maths = peoples[:]['math']


def show(name,data):


print ("科目 | 平均成绩 | 最小成绩 | 最大成绩 | 方差 | 标准差")

    print(name,"|",np.mean(data),"|",np.min(data),"|",np.max(data),"|",np.var(data),"|",np.std(data))


show ("语文",chineses)

show ("英语",chineses)

show ("数学",chineses)

print ("排名:")

print(np.sort(peoples,order='total'))


2018-12-27


夜空中最亮的星（华仔）

老师，您用的是 python2

2018-12-27


Ety_李

「NumPy 中的矩阵计算可以采用多线程的方式，充分利用多核」，因为 python GIL 全局锁的存在，python 并不能利用多核进行多线程处理，python 中多核只能通过多进程来进行利用。

2018-12-26


Chino


peoples = np.array([("zhangfei",66,65,30,0),("guanyu",95,85,98,0),("zhaoyun",93,92,96,0),


                    ("huangzhong",90,88,77,0),("dianwei",80,90,90,0)],dtype=persontype)


想问一下老师为什么这里 我把里面的内容 也就是中间的小括号换成中括号就不行了呢 结构体的 array 一定要用元祖而不能用列表吗 但是前面也有这样定义不是也可以的吗 b = np.array ([[1, 2, 3], [4, 5...

极客时间版权所有: https://time.geekbang.org/column/article/73756

我用中括号的时候是这样提示的

Traceback (most recent call last):


  File "C:/Users/Desktop/test.py", line 7, in <module>


    ["huangzhong",90,88,77],["dianwei",80,90,90]],dtype=persontype)


ValueError: invalid literal for int() with base 10: 'zhangfei'


2018-12-26


往事随风

#!/usr/bin/env python


# -*- coding: utf-8 -*-


'''


练习题：统计全班的成绩

假设一个团队里有 5 名学员，成绩如下表所示。使用 NumPy 统计下这些人的语文、英语、数学中的平均成绩、

最小成绩、最大成绩、方差、标准差。然后把这些人的总成绩排序，得出名次进行成绩输出。

姓名 语文 英语 数学

张飞 66 65 30

关羽 95 85 98

赵云 93 92 96

黄忠 90 88 77

典韦 80 90 90

'''


import numpy as np


import pandas as pd


persontype = np.dtype({


    'names':['name', 'chinese', 'english', 'math', 'total'],


    'formats':['S32','i', 'i', 'f', 'f']})


peoples = np.array ([("张飞",66,65, 30,0),("关羽",95,85,98,0),

("赵云",93,92,96,0),("黄忠",80,90,90,0),("典韦",80,90,90,0)],

    dtype=persontype)


chineses = peoples[:]['chinese']


maths = peoples[:]['math']


englishs = peoples[:]['english']


peoples[:]['total'] = peoples[:]['chinese'] + peoples[:]['math'] + peoples[:]['english']


print ' 语文平均成绩：', np.mean (chineses)

print ' 数学平均成绩：', np.mean (maths)

print ' 英语平均成绩 ', np.mean (englishs)

print ' 语文成绩最低分：', np.amin (chineses), ' 最高分：', np.amax (chineses), ' 方差：', np.var (chineses), ' 标准差：', np.std (chineses)

print ' 英语成绩最低分：', np.amin (englishs), ' 最高分：', np.amax (englishs), ' 方差：', np.var (englishs), ' 标准差：', np.std (englishs)

print ' 数学成绩最低分：', np.amin (maths), ' 最高分：', np.amax (maths), ' 方差：', np.var (maths), ' 标准差：', np.std (maths)

# print peoples


print np.sort(peoples, order='total')


2018-12-26


liuyyy


老师，你说的加 total 排序是在哪加的呀，我无论怎么做都是报错

peoples[:]['total'] = peoples[:]['chinese']+peoples[:]['english']+peoples[:]['math']


2018-12-26


闫东汉

a = np.array([[4,3,2],[2,4,1]])


print np.sort(a, axis=0)


结果是

[[2 3 1]


 [4 4 2]]


不太理解

2018-12-25


xyw pro


交作业！

#用 NumPy 统计下这些人在语文、英语、数学中的平均成绩、最小成绩、最大成绩、方差、标准差。然后把这些人的总成绩排序，得出名次进行成绩输出。

import numpy as np


scoretype = np.dtype({'names':['name','chinese','english','math'],


                       'formats':['S32','i','i','i']})


score = np.array([('zhangfei',66,65,30),('guanyu',95,85,98),('zhaoyun',93,92,96),('huangzhong',90,88,77),('dianwei',80,90,90)],dtype=scoretype)


chinese = score[:]['chinese']


english = score[:]['english']


math = score[:]['math']


#平均成绩

print(np.mean(chinese))


print(np.mean(english))


print(np.mean(math))


#最小成绩

print(np.min(chinese))


print(np.min(english))


print(np.min(math))


#最大成绩

print(np.max(chinese))


print(np.max(english))


print(np.max(math))


#方差

print(np.var(chinese))


print(np.var(english))


print(np.var(math))


#标准差

print(np.std(chinese))


print(np.std(english))


print(np.std(math))


print(np.sort(np.add(np.add(chinese,english),math)))


2018-12-25


汪汪汪

import numpy as np


#1. 利用 dtype 自定义结构类型

persontype = np.dtype({"names":["name","chinese","english","math","total"],


                       "formats":["S32","f","f","f","f"]})


#2. 调用自定义的 persontype，赋值给 peoples

peoples = np.array([("zhangfei","66","65","30","0"),


                    ("guanyu","95","85","98","0"),


                    ("zhaoyun","93","92","96","0"),


                    ("huangzhong","90","88","77","0"),


                    ("dianwei","80","90","90","0")],dtype=persontype)


#3. 调用 ufunc 相关方法，进行统计（平均成绩、最小成绩、最大成绩、方差、标准差）

names = peoples[:]["name"]


chineses = peoples[:]["chinese"]


englishs = peoples[:]["english"]


maths = peoples[:]["math"]


total = chineses+englishs+maths


#语文科目统计结果

print ("语文的平均成绩：%.2f" % np.mean (chineses))

print ("语文的最小成绩：%.2f" % np.amin (chineses))

print ("语文的最大成绩：%.2f" % np.amax (chineses))

print ("语文的方差：%.2f" % np.var (chineses))

print ("语文的标准方差：%.2f" % np.std (chineses))

#英语科目统计结果

print ("英语的平均成绩：%.2f" % np.mean (englishs))

print ("英语的最小成绩：%.2f" % np.amin (englishs))

print ("英语的最大成绩：%.2f" % np.amax (englishs))

print ("英语的方差：%.2f" % np.var (englishs))

print ("英语的标准方差：%.2f" % np.std (englishs))

#数学科目统计结果

print ("数学的平均成绩：%.2f" % np.mean (maths))

print ("数学的最小成绩：%.2f" % np.amin (maths))

print ("数学的最大成绩：%.2f" % np.amax (maths))

print ("数学的方差：%.2f" % np.var (maths))

print ("数学的标准方差：%.2f" % np.std (maths))

#4. 排序（按成绩高低进行排序）

print ("总成绩由低到高一次为：")

print (np.sort(total)[::-1])


2018-12-25


跳跳

import numpy as np


persontype = np.dtype({


    'name':['name','chinese','English','math','total'],


    'formats':['S32','i','i','i','i']})


peoples=np.array([("zhangfei",66,65,30,0),("guanyu",95,85,98,0),("zhaoyun",93,92,96,0),("huangzhong",90,88,77,0),("dianwei",80,90,90,0)],dtpye=persontype)


chinese=pepole[:]['chinese']


English=pepole[:]['English']


math=pepole[:]['math']


total=people[:][chinese]+people[:][English]+people[:][math]


#语文平均成绩、最大成绩、最小成绩、方差、标准差

print(np.mean(chinese))


print(np.amax(chinese))


print(np.amin(chinese))


print(np.std(chinese))


print(np.var(chinese))


#英语平均成绩、最大成绩、最小成绩、方差、标准差

print(np.mean(English))


print(np.amax(English))


print(np.amin(English))


print(np.std(English))


print(np.std(English))


#数学平均成绩、最大成绩、最小成绩、方差、标准差

print(np.mean(math))


print(np.amax(math))


print(np.amin(math))


print(np.std(math))


print(np.std(math))


#排序

print(np.sort(peoples,order='total'))


2018-12-24


wolfog


完成任务

2018-12-24


Hot Heat


courses = ['art', 'english', 'math']


names = [' 张飞 ', ' 关羽 ', ' 赵云 ', ' 黄忠 ', ' 典韦 ']

grades = np.array([(66, 65, 30), (95, 85, 98), (93, 92, 96),


                   (90, 88, 77), (80, 90, 90)])


stats = []


stats.append(grades.mean(0)),


stats.append(grades.min(0)),


stats.append(grades.max(0)),


stats.append(grades.var(0)),


stats.append(grades.std(0))


stats = np.array(stats)


stats = np.row_stack((courses, stats))


stats = np.column_stack((['name', 'mean', 'min', 'max', 'var', 'std'], stats))


### sorting


total = grades.sum(1)


grades = np.c_[grades, total]


grades = grades[np.argsort(grades[:, 3])[::-1]]


########### pandas ###############


df = pd.DataFrame(grades, columns=courses, index=names)


stats = df.agg(['mean', 'max', 'min', 'var', 'std'])


df['total'] = df.sum(1)


grades = df.sort_values(by='total', ascending=False)


2018-12-23


Mingjie


**** 我有两个疑问

****1 我想定义一个分析结果的 结构类型数组 gradetable，如何将 item [0] 添加到 gradetable？？

****2 如何用 gradetype 定义一个 变量 呢？ 而不是一个数组

**** 下面是我实现的代码

# !usr/bin/env python3


# -*- coding:utf-8 -*-


# Author:Charlie


import numpy as np


studenttye = np.dtype ({ # 用 dtype 来定义结构类型

    'names':['name', 'chinese', 'math', 'english', 'total'],


    'formats':['S32','i','i','i','i']})


students = np.array([("zhangfei",66,65,30,0),


                     ("guanyu",95,85,98,0),


                     ("zhaoyun",93,92,96,0),


                     ("huangzhong",90,88,77,0),


                     ("huangzhong",80,90,90,0)],dtype= studenttye)


gradetype = np.dtype({


    'names':['item', 'average', 'min', 'max','variance','standard'],


    'formats':['S32','f','f','f','f','f']})


print ("统计：")

print(gradetype.names)


classlist = ['chinese','math','english']


for i in classlist: #这里只能定义一个数组了，只有一个元素

    item = np.array([(i ,np.average(students[:][i]),


                        np.amin(students[:][i]),


                        np.amax(students[:][i]),


                        np.var(students[:][i]),


                        np.std(students[:][i]))],dtype=gradetype)


    print(item[0])


#gradetable = np.array([], dtype=gradetype) ？？？gradetable .insert ？


print ("排名：")

students[:]['total'] = students[:]['chinese']+students[:]['math']+students[:]['english'];


stusort = []


for studentsort in np.sort(students,order='total'):


    stusort.append(studentsort)


stusort.reverse()


for i in stusort:


    print(i)


2018-12-23


晴天小雨

# 我感觉他们写得好复杂！！！

import numpy as np


# 将学员的各科成绩抽象成数组

dtype = np.dtype({'names': ['name', 'Chinese', 'English', 'Math'], 'formats': ['S32', 'i', 'i', 'i']})


students_score_info = np.array([('ZhangFei', 66, 65, 30), ('GuanYu', 95, 85, 98),


                                ('ZhaoYun', 93, 92, 96), ('HuangZhong', 90, 88, 77),


                                ('DianWei', 80, 90, 90)], dtype=dtype)


# 去除名字项，并将结构化数组转换为矩阵

students_score = np.array([list(score) for score in np.array(students_score_info[['Chinese', 'English', 'Math']])])


# 平均成绩

Chiese_mean, Enlish_mean, Math_mean = np.mean(students_score, axis=0)


print (' 语文、英语、数学平均成绩分别为 {}、{}、{}'.format (Chiese_mean, Enlish_mean, Math_mean))

# 最小成绩

Chiese_min, Enlish_min, Math_min = np.min(students_score, axis=0)


print (' 语文、英语、数学最小成绩分别为 {}、{}、{}'.format (Chiese_min, Enlish_min, Math_min))

# 最大成绩

Chiese_max, Enlish_max, Math_max = np.max(students_score, axis=0)


print (' 语文、英语、数学最大成绩分别为 {}、{}、{}'.format (Chiese_max, Enlish_max, Math_max))

# 方差

Chiese_var, Enlish_var, Math_var = np.var(students_score, axis=0)


print (' 语文、英语、数学成绩方差分别为 {}、{}、{}'.format (Chiese_var, Enlish_var, Math_var))

# 标准差

Chiese_std, Enlish_std, Math_std = np.std(students_score, axis=0)


print (' 语文、英语、数学成绩标准差分别为 {}、{}、{}'.format (Chiese_std, Enlish_std, Math_std))

# 总成绩排名

rank_lists = sorted(np.sum(students_score, axis=1),reverse=True)


print (' 语文、英语、数学总成绩排名为 {}'.format (rank_lists))

2018-12-23


daydreamer


# -*- coding:utf-8 -*-


import numpy as np


studenttype = np.dtype({


    'names':['name', 'chinese', 'english', 'math', 'total'],


    'formats':['S32', 'i', 'i', 'i', 'i']})


scores = np.array([("Zhang Fei", 66, 65, 30, 0), ("Guan Yu", 95, 85, 98, 0),


("Zhang Yu", 93, 92, 96, 0), ("Huang Zhong", 90, 88, 77, 0),


("Dian Wei", 80, 90, 90, 0)],dtype=studenttype)


print('{:10s}'.format(''),end='')


print('{:8s}'.format('Max'),end='')


print('{:8s}'.format('Min'),end='')


print('{:8s}'.format('Mean'),end='')


print('{:8s}'.format('Std'),end='')


print('{:8s}'.format('var'))


for name in studenttype.names:


    if name not in ['name', 'total']:


        print('{:8s}'.format(name),end='')


        print('{:8.2f}'.format(np.amax(scores[:][name])),end='')


        print('{:8.2f}'.format(np.amin(scores[:][name])),end='')


        print('{:8.2f}'.format(np.mean(scores[:][name])),end='')


        print('{:8.2f}'.format(np.std(scores[:][name])),end='')


        print('{:8.2f}'.format(np.var(scores[:][name])))


scores[:]['total'] = scores[:]['chinese'] + scores[:]['english'] + scores[:]['math']


ranks = np.sort(scores, order='total')


print('The rank of those five students is:')


for rank in ranks:


    for col in rank:


        print(col,end='\t')


    print('')


2018-12-23


Being


import numpy as np


subjectArray = np.array(['Name','Chinese','English','Math','Total'])


persontype = np.dtype({


    'names':subjectArray,


    'formats':['S32','i','i','i','i']


})


students = np.array(


    [('ZhangFei', 66, 65, 30,0),


    ('GuanYu', 95, 85, 98,0),


    ('ZhaoYun', 93, 92, 96,0),


    ('HuangZhong', 90, 88, 77,0),


    ('DianWei', 80, 90, 90,0)],


    dtype=persontype


)


for i in range(students[:]['Name'].shape[0]):


    scores = 0


    for subjectIndex in range(1, subjectArray.shape[0] - 1):


        scores += students[i][subjectArray[subjectIndex]]


    students[i]['Total'] = scores


for subjectIndex in range(1, subjectArray.shape[0] - 1):


    subject = students[:][subjectArray[subjectIndex]]


    subjectAvg = np.mean(subject)


    subjectMin = np.amin(subject)


    subjectMax = np.amax(subject)


    subjectVar = np.var(subject)


    subjectStd = np.std(subject)


    print("Subject: %s" %subjectArray[subjectIndex])


    print("average = %f " %subjectAvg)


    print("Max = %f " %subjectMin)


    print("Min = %f " %subjectMax)


    print("Var = %f" %subjectVar)


    print("Var = %f" %subjectStd)


    print('-----')


sortArray = np.sort(students, -1, 'quicksort', 'Total')


print("Rank by Total score : ")


print(sortArray)


2018-12-23


Being


老师，请教如下问题

persontype = np.dtype({


    'names':['name', 'age', 'chinese', 'math', 'english'],


    'formats':['S32', 'i', 'i', 'i', 'f']


})


peoples = np.array(


    [("ZhangFei", 23,31,32,3.7), ("GuanYu",45,47,48,4.9),


    ("ZhaoYun",57,53,52,5.8), ("HuangZhong",67,64,66,6.1)],


    dtype=persontype


)


这个数组，我用 sort 排序

print(np.sort(peoples, axis=-1, kind='quicksort', order='name'))


打印结果如下

[(b'GuanYu', 45, 47, 48, 4.9) (b'HuangZhong', 67, 64, 66, 6.1)


 (b'ZhangFei', 23, 31, 32, 3.7) (b'ZhaoYun', 57, 53, 52, 5.8)]


为什么每个元素会多一个 b 呢？order = 其他 type 也是一样的，会多个 b

2018-12-23


清晨

老师，刚买了您得课程，还没听您的课，我是编程零基础，打扰您一下，请问您推荐的 python 课程是什么？

2018-12-23


吴晓岚 jim wu

import numpy as np


persontype = np.dtype({


'names': ['name','chinese','math','english'],


'formats':['S32','i','i','i','i']


})


peoples = np.array([("zhangfei",66,65,30,),


("guanyu",95,85,98),


("zhaoyun",93,92,96),


("huangzhong",90,88,77),


("dianwei",80,90,90)],


dtype = persontype


)


name = peoples[:]['name']


chinese = peoples[:]['chinese']


math = peoples[:]['math']


english = peoples[:]['english']


print ("中文平均成绩",np.mean (chinese),'\n')

print ("数学平均成绩",np.mean (math),'\n')

print ("英文平均成绩",np.mean (english),'\n')

print ("中文最小成绩",np.amin (chinese),'\n')

print ("数学最小成绩",np.amin (math),'\n')

print ("英文最小成绩",np.amin (english),'\n')

print ("中文最大成绩",np.amax (chinese),'\n')

print ("数学最大成绩",np.amax (math),'\n')

print ("英文最大成绩",np.amax (english),'\n')

print ("中文方差",np.var (chinese),'\n')

print ("数学方差",np.var (math),'\n')

print ("英文方差",np.var (english),'\n')

print ("中文标准差",np.std (chinese),'\n')

print ("数学标准差",np.std (math),'\n')

print ("英文标准差",np.std (english),'\n')

total = peoples[:]['chinese']+peoples[:]['math']+peoples[:]['english']


print(total)


print(np.sort(total))


2018-12-23


拉我吃

--- python3.6 ---


--- pycharm ---


# coding: utf8


import numpy as np


"""


假设一个团队里有 5 名学员，成绩如下表所示。你可以用 NumPy 统计下这些人

在语文、英语、数学中的平均成绩、最小成绩、最大成绩、方差、标准差。

然后把这些人的总成绩排序，得出名次进行成绩输出.

| 姓名 | 语文 | 英语 | 数学 |

| 张飞 | 66 | 65 | 30 |

| 关羽 | 95 | 85 | 98 |

| 赵云 | 93 | 92 | 96 |

| 黄忠 | 90 | 88 | 77 |

| 典韦 | 80 | 90 | 90 |

"""


# define data type


studenttype = np.dtype({


    'names': ['name', 'chinese', 'english', 'math', 'total'],


    'formats': ['U32', 'i', 'i', 'i', 'f']


})


# define and init students details


# init total with 0


students = np.array(


    [


("张飞", 66, 65, 30, 0),

("关羽", 95, 85, 98, 0),

("赵云", 93, 92, 96, 0),

("黄忠", 90, 88, 77, 0),

("典韦", 80, 90, 90, 0)

    ], dtype=studenttype


)


# calculate total


students[:]['total'] = students[:]['chinese'] + students[:]['english'] + students[:]['math']


print(students[:])


# p1 统计下这些人

# 在语文、英语、数学中的平均成绩、最小成绩、最大成绩、方差、标准差

print (' 语文平均成绩 = ', np.mean (students [:]['chinese']))

print (' 英语平均成绩 = ', np.mean (students [:]['english']))

print (' 数学平均成绩 = ', np.mean (students [:]['math']))

print (' 语文最小成绩 = ', np.amin (students [:]['chinese']))

print (' 英语最小成绩 = ', np.amin (students [:]['english']))

print (' 数学最小成绩 = ', np.amin (students [:]['math']))

print (' 语文最大成绩 = ', np.amax (students [:]['chinese']))

print (' 英语最大成绩 = ', np.amax (students [:]['english']))

print (' 数学最大成绩 = ', np.amax (students [:]['math']))

print (' 语文方差 = ', np.var (students [:]['chinese']))

print (' 英语方差 = ', np.var (students [:]['english']))

print (' 数学方差 = ', np.var (students [:]['math']))

print (' 语文标准差 = ', np.std (students [:]['chinese']))

print (' 英语标准差 = ', np.std (students [:]['english']))

print (' 数学标准差 = ', np.std (students [:]['math']))

# p2 然后把这些人的总成绩排序，得出名次进行成绩输出.

print ("按总成绩排名", np.sort (students [:]['total'])[::-1])

2018-12-23


无畏不惧 @

总觉得就一个知识点 axis=0 代表跨行，axis=1 代表跨列

2018-12-22


郝志强

老师，能用 py3.x 进行教学吗？现在用的都是 3. 写的版本

2018-12-22


三儿

有木有一些方法论启发从哪些角度进行数据分析的书呢

2018-12-22


何楚

修改后的版本：

#!/usr/bin/env python3


# -*- coding: utf-8 -*-


import numpy as np


persontype = np.dtype({


    'names': ['name', 'chinese', 'math', 'english', 'total'],


    'formats': ['S32', 'i', 'i', 'i', 'i']})


peoples = np.array([("ZhangFei", 66, 65, 30, 0),


                    ("GuanYu", 95, 85, 98, 0),


                    ("ZhaoYun", 93, 92, 96, 0),


                    ("HuangZhong", 90, 88, 77, 0),


                    ("DianWei", 80, 90, 90, 0)],


                   dtype=persontype)


for col in persontype.names:


    # print(col)


    if col in ["name", "total"]:


        continue


    print("-"*36)


    print("mean of {}: {}".format(col, peoples[col].mean()))


    print("min of {}: {}".format(col, peoples[col].min()))


    print("max of {}: {}".format(col, peoples[col].max()))


    print("var of {}: {}".format(col, peoples[col].var()))


    print("std of {}: {}".format(col, peoples[col].std()))


peoples[:]['total'] = peoples[:]['chinese'] + \


    peoples[:]['english'] + peoples[:]['math']


peoples = np.sort(peoples, order='total')[::-1]


print("-"*36)


print("sorted score:")


for i in range(peoples.size):


    print("{} - {} : {}".format(i + 1,


                                peoples['name'][i].decode('utf-8'), peoples['total'][i]))


2018-12-22


smilecrazy


使用的 python 3.7 没有 cmp 函数

看过大家的 code，感觉好多人都开了新的空间去保存具体成绩，类似把数据矩阵给倒置了，虽然计算简单，但是新开了内存，而且 studenttype 的结构体声明看起来没什么用，私以为应该尽量在原数据的基础上进行计算，欢迎老师来批～

#!/usr/bin/python


import numpy as np


from functools import cmp_to_key


studenttype = np.dtype({


    'names': ['name', 'chinese', 'math', 'english'],


    'formats': ['S32', 'float', 'float', 'float', 'float']


})


student = np.array([('zhangfei', 66, 65, 30),('guanyu', 95,85, 98),


                    ('zhaoyun', 93, 92, 96),('huangzhong', 90, 88, 77),


                    ('dianwei', 80, 90, 90)], dtype=studenttype)


def get_situation(course):


    course_list = {}


    course_list['avg'] = np.average(course)


    course_list['min'] = np.amin(course)


    course_list['max'] = np.amax(course)


    course_list['var'] = np.var(course)


    course_list['std'] = np.std(course)


    return course_list


def cmp(sp1, sp2):


    return sp1['chinese'] + sp1['math'] + sp1['english'] - sp2['chinese'] + sp2['math'] + sp2['english']


print(get_situation(student[:]['chinese']))


print(get_situation(student[:]['math']))


print(get_situation(student[:]['english']))


print(sorted(student, key=cmp_to_key(cmp)))


2018-12-22


mimi


import numpy as np


score_type = np.dtype({


    'names': ('name', 'chinese', 'english', 'math'),


    'formats': ('U32', 'i', 'i', 'i')


})


score = np.array([


(' 张飞 ', 66, 65, 30),

(' 关羽 ', 95, 85, 98),

(' 赵云 ', 93, 92, 96),

(' 黄忠 ', 90, 88, 77),

(' 典韦 ', 80, 90, 90)

], dtype=score_type)


des_type = np.dtype({


    'names': ('course', 'mean', 'min', 'max', 'var', 'std'),


    'formats': ('S32', 'f2', 'f2', 'f2', 'f2', 'f2')


})


des = np.array([


    (


        course,


        score[:][course].mean(),


        score[:][course].min(),


        score[:][course].max(),


        score[:][course].var(),


        score[:][course].std()


    ) for course in score_type.names[1:]


], dtype=des_type)


amount_type = np.dtype({


    'names': ('name', 'amount'),


    'formats': ('U32', 'f2')


})


amount = np.array([


    (


        name,


        sum(tuple(score[i])[1:])


    ) for i, name in enumerate(score[:]['name'])


], dtype=amount_type)


amount.sort(0, order='amount')


2018-12-22


X


老师您好，我想知道这里定义结构体的时候，类型 U32 这样的是在哪里找到说明，还是说字符串类型就是 U32？还有哪些类型可用？谢谢

2018-12-22


lingmacker


import numpy as np


person_type = np.dtype({"names": ["name", "Chinese", "English", "Math"],


                        "formats": ["S32", "i", "i", "i"]})


persons = np.array([("zhangfei", 66, 65, 30),


                    ("guanyu", 95, 85, 98),


                    ("zhaoyun", 93, 92, 96),


                    ("huangzhong", 90, 88, 77),


                    ("dianwei", 80, 90, 90)], dtype=person_type)


ch = persons[:]["Chinese"]


en = persons[:]["English"]


ma = persons[:]["Math"]


scores = np.array([ch, en, ma])


print(scores.shape)


print ("姓名 | 平均 | 最小 | 最大 | 方差 | 标准")

for a, b, c, d, e, f in zip(persons["name"], np.mean(scores, axis=0), np.min(scores, axis=0),


                            np.max(scores, axis=0), np.var(scores, axis=0), np.std(scores, axis=0)):


    print("%-15s %4.2f %4.2f %4.2f %4.2f %4.2f" % (a, b, c, d, e, f))


for i in sorted(persons, key=lambda x: sum([x["Chinese"], x["English"], x["Math"]]), reverse=True):


    print(i)


2018-12-22


1e-43


老师，numpy 排序最后两个 axis=0 和 axis=1 的两个不是太明白

2018-12-21


29 岁

##coding=utf-8


import numpy as np


personType =np.dtype(


    {


        'names':['name','chinese','english','math'],


        'formats':['S32','i','i','i']


        }


    )


peoples =np.array([


("张飞",66,65,30),

("关羽",95,85,98),

("赵云",93,92,96),

("黄忠",90,80,77),

("典韦",80,90,90)

    ],dtype=personType)


#原因：这是因为 win 的，命令行用的是 cp936 编码，而上面脚本用的是 utf-8 编码，因此导致乱码。

#解决方法是，使用 decode 和 encode 函数对字符重新解码和编码。

def printChinese(a,b):


    a=a+ str( b)


    print(a.decode('utf-8').encode('cp936') )


    print("-----------------------------------")


    return


#不知道怎么处理，姓名还是乱码

print peoples


chineses = peoples[:]['chinese']


englishs = peoples[:]['english']


maths = peoples[:]['math']


printChinese ("语文平均分:",np.mean (chineses))

printChinese ("英语平均分:",np.mean (englishs))

printChinese ("数学平均分:",np.mean (maths))

printChinese ("语文最低分:",np.amin (chineses))

printChinese ("英语最低分:",np.amin (englishs))

printChinese ("数学最低分:",np.amin (maths))

printChinese ("语文方差:",np.var (chineses))

printChinese ("英语方差:",np.var (englishs))

printChinese ("数学方差:",np.var (maths))

printChinese ("语文标准差:",np.std (chineses))

printChinese ("英语标准差:",np.std (englishs))

printChinese ("数学标准差:",np.std (maths))

#不会翻转

printChinese ("总分排序：",np.sort (chineses+englishs+maths))

2018-12-21


Rickie


import numpy as np


persontype=np.dtype({


    'names':['name', 'age', 'chinese', 'math','english'],


    'formats':['S32','i','i','i','f']})


peoples=np.array([("ZhangFei",32,75,100, 90),("GuanYu",24,85,96,88.5),


       ("ZhaoYun",28,85,92,96.5),("HuangZhong",29,65,85,100)],dtype=persontype)


grade_max=[]


grade_min=[]


grade_avg=[]


grade_var=[]


grade_std=[]


for subject in ['chinese','math','english']:


    grade_max.append(peoples[subject].max())


    grade_min.append(peoples[subject].min())


    grade_avg.append(peoples[subject].mean())


    grade_var.append(peoples[subject].var())


    grade_std.append(peoples[subject].std())


#计算总分

n=len(peoples)


score=np.zeros((n,1))


for i in range(n):


    for subject in ['chinese','math','english']:


        score[i]=score[i]+peoples[i][subject]


print(score)


# 之后尝试给 peoples 添加一列，但由于其类型为 tuple，无法改变元素内容，因此重建了一个元素为 list 的二维数组，然后添加计算好的总分数

people=np.array([["ZhangFei",32,75,100, 90],["GuanYu",24,85,96,88.5],


       ["ZhaoYun",28,85,92,96.5],["HuangZhong",29,65,85,100]])


people_grade_added=np.insert(people,5,values=score.reshape(1,4),axis=1)


##


用 ndarray 做这种计算实在太不方便了，由于这个问题中的元素是 tuple，给 array 做各种切片都不成功（不知道是不是这个原因啊。。。？）

还是 DataFrame 好用....

2018-12-21


侯晓杰

import numpy as np


peopletype = np.dtype({


    'names':['name', 'chinese', 'English', 'math'],


    'formats': ['U32', 'i', 'i', 'i']})


peoples = np.array ([(' 张飞 ', 66, 65, 30), (' 关羽 ', 95, 85, 98), (' 赵云 ', 93, 92, 96), (' 黄忠 ', 90, 88, 77), (' 典韦 ', 80, 90, 90)], dtype=peopletype)

chinses = peoples[:]['chinese']


English = peoples[:]['English']


math = peoples[:]['math']


print('chinese--average:{}, min:{}, max:{}, fx:{}, bzx:{}'.format(np.mean(chinses), np.amin(chinses), np.amax(chinses), np.var(chinses), np.std(chinses)))


print('English--average:{}, min:{}, max:{}, fx:{}, bzx:{}'.format(np.mean(English), np.amin(English), np.amax(English), np.var(English), np.std(English)))


print('math--average:{}, min:{}, max:{}, fx:{}, bzx:{}'.format(np.mean(math), np.amin(math), np.amax(math), np.var(math), np.std(math)))


name_grade = []


for i in range(len(peoples)):


    temp = [peoples[i]['name'], peoples[i]['chinese'] + peoples[i]['English'] + peoples[i]['math']]


    name_grade.append(tuple(temp))


grades = np.array(name_grade, dtype=[('name', 'U32'), ('grade', 'i')])


grades = np.sort(grades, order='grade', )


num = 1


for grade in grades[::-1]:


    print('name: {}, grade:{}, ranke:{}'.format(grade['name'], grade['grade'], num))


    num += 1


2018-12-21


大萌

期待更快的更新，看完文章之后回顾了一遍 numpy，对于基础知识理解的更透彻了一些。

import numpy as np


#定义类型

person_type = np.dtype({'names':['name','chinese','math','english'],'formats':['S25','i','i','i']})


info = np.array([('zhangfei',66,65,30),('guanyu',95,85,98),('zhaoyun',93,92,96),('huangzhong',90,88,77),('machao',80,90,90)],dtype=person_type)


chinese = info[:]['chinese']


math = info[:]['math']


english = info[:]['english']


print (' 语、数、英各门学科的平均值：')

print(np.mean(chinese))


print(np.mean(math))


print(np.mean(english))


print (' 语、数、英各门学科的最大值：')

print(np.max(chinese))


print(np.max(math))


print(np.max(english))


print (' 语、数、英各门学科的最小值：')

print(np.min(chinese))


print(np.min(math))


print(np.min(english))


print (' 语、数、英各门学科的方差：')

print(np.var(chinese))


print(np.var(math))


print(np.var(english))


print (' 语、数、英各门学科的标准差：')

print(np.std(chinese))


print(np.std(math))


print(np.std(english))


print (' 排序输出总成绩 ')

print(np.sort(chinese+math+english,axis=0,kind='quicksort'))


表格数据使用 dataframe 的话，会更方便操作一些，info.describe () 就搞定了。

想问下老师，自己对于数据知识和算法理解稍弱一些，最近看了《数学之美》和《算法图解》和《机器学习_周志华》等几本书，还没有看完，总是觉得自己学的东西有点乱，不够整体，怎么办呢

作者回复：可以尝试做题，自己写代码

看书容易有惰性，写代码是需要跑通才行的，也是学习成果最好的反馈

2018-12-21


我的小将军

1. 最大值和最小值场景与最大值与最小值之差场景

两种情况下

axis=0 取值问题？

2. studenttype = np.dtype({'names':['name','chinese','english','math'], 'formats':['S32','i','i','i']})


score = np.array([('ZhangFei',66,65,30), ('GuanYu',95,85,98), ('ZhaoYun',93,92,96),('HuangZhong',90,88,77),('DianWei',80,90,90)],dtype=studenttype)


print(score)


chineses = score[:]['chinese']


englishs = score[:]['english']


maths = score[:]['math']


print(chineses)


print(englishs)


print(maths)


newscore = np.array([chineses, englishs, maths])


print(newscore)


#平均成绩、最小成绩、最大成绩、方差、标准差

ava = np.mean(newscore)


print(ava)


lowest = np.amin(newscore)


print(lowest)


highest = np.amax(newscore)


print(highest)


std = np.std(newscore)


print(std)


var = np.var(newscore)


print(var)


2018-12-21


anthony


import numpy as np


scoretype = np.dtype(


    {'names': ['name', 'chinese', 'english', 'math'], 'formats': ['S32', 'i', 'i', 'i']})


scores = np.array([("zhangfei", 66, 65, 30), ("guanyu", 95, 85, 98), ("zhaoyun", 93,


        92, 96), ("huangzhong", 90, 88, 77), ("dianwei", 80, 90, 90)], dtype=scoretype)


for i in scoretype.names:


    if i == 'name':


        continue


    print("{} scores stats:".format(i))


    score = scores[:][i]


    print('mean: {:.2f}, min:{:.2f},max:{:.2f},var:{:.2f},std:{:.2f}'.format(np.mean(score), np.amin(


        score), np.amax(score), np.var(score), np.std(score)))


chinese=scores[:]['chinese']


english=scores[:]['english']


math=scores[:]['math']


sums=list(zip(scores[:]['name'],chinese+english+math))


sort_sum=sorted(sums,key=lambda x:x[:][1],reverse=True)


print(sort_sum)


尽量想写的 python style 一点，但是 python 和 numpy 的库都不是太熟悉，还是写得比较笨重

2018-12-21


鱼鱼鱼培填

请教老师两个问题：Python 中的 list 的元素在内存中的存储是离散的，是不是相当于链表呢，而 numpy 中的 array 相当于数组？list 是动态分配的，如果内存中不是连续的，那扩容时是怎么增加空间的呢？望老师指点

2018-12-21


mickey


请老师详细讲解一下，统计最大值与最小值之差、统计百分位数、统计中位数、统计标准差、方差的实际应用意义。谢谢。

2018-12-21


Robin


工作四年后转数据分析还来得及吗？老师

作者回复：来得及 掌握工具和锻炼实战能力，这样你就提升的很快

2018-12-21


Robin


一下就到 numpy！好快

作者回复：一共就 3 篇基础 python，因为后面还要讲数据挖掘算法、常用的数据预处理方法、数据可视化以及项目实战，所以东西还挺多的

2018-12-21


王十三

1. 因为 list 在内存中分散存储，所以里面存了对象 + 指针（用于找到对象的）

2. 而 Numpy 在内存中是以块的形式存储，不需要按照指针对其对象进行查找，提高了效率，所以要使用 Numpy

------------------------------------------------------


其他的数据结构只了解像链表，栈，一些基础的数据结构

2018-12-21


荀辰龙

为啥不是马超，而是典韦，强迫症看着难受 [斜眼笑]

作者回复：因为王者荣耀里的马超还没上线😄

2018-12-21


舒成

有其它语言基础，练习起来就理解的快。这样的方式比啃书本高效太多。晚上回家操练起来

作者回复：对 直接实战 效率高太多

2018-12-21


