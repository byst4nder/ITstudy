# 0103Python 基础语法：开始你的 Python 之旅

陈旸 2018-12-20




13:13


讲述：陈旸 大小：24.24M

上一节课我跟你分享了数据挖掘的最佳学习路径，相信你对接下来的学习已经心中有数了。今天我们继续预习课，我会用三篇文章，分别对 Python 的基础语法、NumPy 和 Pandas 进行讲解，带你快速入门 Python 语言。如果你已经有 Python 基础了，那先恭喜你已经掌握了这门简洁而高效的语言，这几节课你可以跳过，或者也可以当作复习，自己查漏补缺，你还可以在留言区分享自己的 Python 学习和使用心得。

好了，你现在心中是不是有个问题，要学好数据分析，一定要掌握 Python 吗？

我的答案是，想学好数据分析，你最好掌握 Python 语言。为什么这么说呢？

首先，在一份关于开发语言的调查中，使用过 Python 的开发者，80% 都会把 Python 作为自己的主要语言。Python 已经成为发展最快的主流编程语言，从众多开发语言中脱颖而出，深受开发者喜爱。其次，在数据分析领域中，使用 Python 的开发者是最多的，远超其他语言之和。最后，Python 语言简洁，有大量的第三方库，功能强大，能解决数据分析的大部分问题，这一点我下面具体来说。

Python 语言最大的优点是简洁，它虽然是 C 语言写的，但是摒弃了 C 语言的指针，这就让代码非常简洁明了。同样的一行 Python 代码，甚至相当于 5 行 Java 代码。我们读 Python 代码就像是读英文一样直观，这就能让程序员更好地专注在问题解决上，而不是在语言本身。

当然除了 Python 自身的特点，Python 还有强大的开发者工具。在数据科学领域，Python 有许多非常著名的工具库：比如科学计算工具 NumPy 和 Pandas 库，深度学习工具 Keras 和 TensorFlow，以及机器学习工具 Scikit-learn，使用率都非常高。

总之，如果你想在数据分析、机器学习等数据科学领域有所作为，那么掌握一项语言，尤其是 Python 语言的使用是非常有必要的，尤其是我们刚提到的这些工具，熟练掌握它们会让你事半功倍。

安装及 IDE 环境

了解了为什么要学 Python，接下来就带你快速开始你的第一个 Python 程序，所以我们先来了解下如何安装和搭建 IDE 环境。

Python 的版本选择

Python 主要有两个版本： 2.7.x 和 3.x。两个版本之间存在一些差异，但并不大，它们语法不一样的地方不到 10%。

另一个事实就是：大部分 Python 库都同时支持 Python 2.7.x 和 3.x 版本。虽然官方称 Python2.7 只维护到 2020 年，但是我想告诉你的是：千万不要忽视 Python2.7，它的寿命远不止到 2020 年，而且这两年 Python2.7 还是占据着 Python 版本的统治地位。一份调查显示：在 2017 年的商业项目中 2.7 版本依然是主流，占到了 63.7%，即使这两年 Python3.x 版本使用的增速较快，但实际上 Python3.x 在 2008 年就已经有了。

那么你可能会问：这两个版本该如何选择呢？

版本选择的标准就是看你的项目是否会依赖于 Python2.7 的包，如果有依赖的就只能使用 Python2.7，否则你可以用 Python 3.x 开始全新的项目。

Python IDE 推荐

确定了版本问题后，怎么选择 Python IDE 呢？有众多优秀的选择，这里推荐几款。

1.  PyCharm


这是一个跨平台的 Python 开发工具，可以帮助用户在使用 Python 时提升效率，比如：调试、语法高亮、代码跳转、自动完成、智能提示等。

2.  Sublime Text


SublimeText 是个著名的编辑器，Sublime Text3 基本上可以 1 秒即启动，反应速度很快。同时它对 Python 的支持也很到位，具有代码高亮、语法提示、自动完成等功能。

3.  Vim


Vim 是一个简洁、高效的工具，速度很快，可以做任何事，从来不崩溃。不过 Vim 相比于 Sublime Text 上手有一定难度，配置起来有些麻烦。

4.  Eclipse+PyDev


习惯使用 Java 的人一定对 Eclipse 这个 IDE 不陌生，那么使用 Eclipse+PyDev 插件会是一个很好的选择，这样熟悉 Eclipse 的开发者可以轻易上手。

如果上面这些 IDE 你之前都没有怎么用过，那么推荐你使用 Sublime Text，上手简单，反应速度快。

Python 基础语法

环境配置好后，我们就来快速学习几个 Python 必会的基础语法。我假设你是 Python 零基础，但已经有一些其他编程语言的基础。下面我们一一来看。

输入与输出

name = raw_input("What's your name?")


sum = 100+100


print ('hello,%s' %name)


print ('sum = %d' %sum)


raw_input 是 Python2.7 的输入函数，在 python3.x 里可以直接使用 input，赋值给变量 name，print 是输出函数，% name 代表变量的数值，因为是字符串类型，所以在前面用的 % s 作为代替。

这是运行结果：

What's your name?cy


hello,cy


sum = 200


判断语句：if … else …

if score>= 90:


       print 'Excellent'


else:


       if score < 60:


           print 'Fail'


       else:


           print 'Good Job'


if … else … 是经典的判断语句，需要注意的是在 if expression 后面有个冒号，同样在 else 后面也存在冒号。

另外需要注意的是，Python 不像其他语言一样使用 {} 或者 begin…end 来分隔代码块，而是采用代码缩进和冒号的方式来区分代码之间的层次关系。所以代码缩进在 Python 中是一种语法，如果代码缩进不统一，比如有的是 tab 有的是空格，会怎样呢？会产生错误或者异常。相同层次的代码一定要采用相同层次的缩进。

循环语句：for … in

sum = 0


for number in range(11):


    sum = sum + number


print sum


运行结果：

55


for 循环是一种迭代循环机制，迭代即重复相同的逻辑操作。如果规定循环的次数，我们可以使用 range 函数，它在 for 循环中比较常用。range (11) 代表从 0 到 10，不包括 11，也相当于 range (0,11)，range 里面还可以增加步长，比如 range (1,11,2) 代表的是 [1,3,5,7,9]。

循环语句: while

sum = 0


number = 1


while number < 11:


       sum = sum + number


       number = number + 1


print sum


运行结果：

55


1 到 10 的求和也可以用 while 循环来写，这里 while 控制了循环的次数。while 循环是条件循环，在 while 循环中对于变量的计算方式更加灵活。因此 while 循环适合循环次数不确定的循环，而 for 循环的条件相对确定，适合固定次数的循环。

数据类型：列表、元组、字典、集合

列表：[]

lists = ['a','b','c']


lists.append('d')


print lists


print len(lists)


lists.insert(0,'mm')


lists.pop()


print lists


运行结果：

['a', 'b', 'c', 'd']


4


['mm', 'a', 'b', 'c']


列表是 Python 中常用的数据结构，相当于数组，具有增删改查的功能，我们可以使用 len () 函数获得 lists 中元素的个数；使用 append () 在尾部添加元素，使用 insert () 在列表中插入元素，使用 pop () 删除尾部的元素。

元组 (tuple)

tuples = ('tupleA','tupleB')


print tuples[0]


运行结果：

tupleA


元组 tuple 和 list 非常类似，但是 tuple 一旦初始化就不能修改。因为不能修改所以没有 append (), insert () 这样的方法，可以像访问数组一样进行访问，比如 tuples [0]，但不能赋值。

字典 {dictionary}

# -*- coding: utf-8 -*


#定义一个 dictionary

score = {'guanyu':95,'zhangfei':96}


#添加一个元素

score['zhaoyun'] = 98


print score


#删除一个元素

score.pop('zhangfei')


#查看 key 是否存在

print 'guanyu' in score


#查看一个 key 对应的值

print score.get('guanyu')


print score.get('yase',99)


运行结果：

{'guanyu': 95, 'zhaoyun': 98, 'zhangfei': 96}


True


95


99


字典其实就是 {key, value}，多次对同一个 key 放入 value，后面的值会把前面的值冲掉，同样字典也有增删改查。增加字典的元素相当于赋值，比如 score [‘zhaoyun’] = 98，删除一个元素使用 pop，查询使用 get，如果查询的值不存在，我们也可以给一个默认值，比如 score.get (‘yase’,99)。

集合：set

s = set(['a', 'b', 'c'])


s.add('d')


s.remove('b')


print s


print 'c' in s


运行结果：

set(['a', 'c', 'd'])


True


集合 set 和字典 dictory 类似，不过它只是 key 的集合，不存储 value。同样可以增删查，增加使用 add，删除使用 remove，查询看某个元素是否在这个集合里，使用 in。

注释：#

注释在 python 中使用 #，如果注释中有中文，一般会在代码前添加 # -- coding: utf-8 -。

如果是多行注释，使用三个单引号，或者三个双引号，比如：

# -*- coding: utf-8 -*


'''


这是多行注释，用三个单引号

这是多行注释，用三个单引号

这是多行注释，用三个单引号

'''


引用模块 / 包：import

# 导入一个模块

import model_name


# 导入多个模块

import module_name1,module_name2


# 导入包中指定模块

from package_name import moudule_name


# 导入包中所有模块

from package_name import *


Python 语言中 import 的使用很简单，直接使用 import module_name 语句导入即可。这里 import 的本质是什么呢？import 的本质是路径搜索。import 引用可以是模块 module，或者包 package。

针对 module，实际上是引用一个.py 文件。而针对 package，可以采用 from … import … 的方式，这里实际上是从一个目录中引用模块，这时目录结构中必须带有一个 __init__.py 文件。

函数：def

def addone(score):


   return score + 1


print addone(99)


运行结果：

100


函数代码块以 def 关键词开头，后接函数标识符名称和圆括号，在圆括号里是传进来的参数，然后通过 return 进行函数结果得反馈。

A+B Problem


上面的讲的这些基础语法，我们可以用 sumlime text 编辑器运行 Python 代码。另外，告诉你一个相当高效的方法，你可以充分利用一个刷题进阶的网址： http://acm.zju.edu.cn/onlinejudge/showProblem.do?problemId=1 ，这是浙江大学 ACM 的 OnlineJudge。

什么是 OnlineJudge 呢？它实际上是一个在线答题系统，做题后你可以在后台提交代码，然后 OnlineJudge 会告诉你运行的结果，如果结果正确就反馈：Accepted，如果错误就反馈：Wrong Answer。

不要小看这样的题目，也会存在编译错误、内存溢出、运行超时等等情况。所以题目对编码的质量要求还是挺高的。下面我就给你讲讲这道 A+B 的题目，你可以自己做练习，然后在后台提交答案。

题目：A+B

输入格式：有一系列的整数对 A 和 B，以空格分开。

输出格式：对于每个整数对 A 和 B，需要给出 A 和 B 的和。

输入输出样例：

INPUT


1 5


OUTPUT


6


针对这道题，我给出了下面的答案：

while True:


       try:


              line = raw_input()


              a = line.split()


              print int(a[0]) + int(a[1])


       except:


              break


当然每个人可以有不同的解法，官方也有 Python 的答案，这里给你介绍这个 OnlineJudge 是因为：

可以在线得到反馈，提交代码后，系统会告诉你对错。而且你能看到每道题的正确率，和大家提交后反馈的状态；

有社区论坛可以进行交流学习；

对算法和数据结构的提升大有好处，当然对数据挖掘算法的灵活运用和整个编程基础的提升都会有很大的帮助。

总结

现在我们知道，Python 毫无疑问是数据分析中最主流的语言。今天我们学习了这么多 Python 的基础语法，你是不是体会到了它的简洁。如果你有其他编程语言基础，相信你会非常容易地转换成 Python 语法的。那到此，Python 我们也就算入门了。有没有什么方法可以在此基础上快速提升 Python 编程水平呢？给你分享下我的想法。

在日常工作中，我们解决的问题都不属于高难度的问题，大部分人做的都是开发工作而非科研项目。所以我们要提升的主要是熟练度，而通往熟练度的唯一路径就是练习、练习、再练习！

如果你是第一次使用 Python，不用担心，最好的方式就是直接做题。把我上面的例子都跑一遍，自己在做题中体会。

如果你想提升自己的编程基础，尤其是算法和数据结构相关的能力，因为这个在后面的开发中都会用到。那么 ACM Online Judge 是非常好的选择，勇敢地打开这扇大门，把它当作你进阶的好工具。

你可以从 Accepted 比率高的题目入手，你做对的题目数越多，你的排名也会越来越往前，这意味着你的编程能力，包括算法和数据结构的能力都有了提升。另外这种在社区中跟大家一起学习，还能排名，就像游戏一样，让学习更有趣味，从此不再孤独。

我在文章中多次强调练习的作用，这样可以增加你对数据分析相关内容的熟练度。所以我给你出了两道练习题，你可以思考下如何来做，欢迎把答案放到评论下面，我也会和你一起在评论区进行讨论。

如果我想在 Python 中引用 scikit-learn 库该如何引用？

求 1+3+5+7+…+99 的求和，用 Python 该如何写？

欢迎你把今天的内容分享给身边的朋友，和他一起掌握 Python 这门功能强大的语言。

unpreview


© 版权归极客邦科技所有，未经许可不得传播售卖。页面已增加防盗追踪，如有侵权极客邦将依法追究其法律责任。

大龙

由作者筛选后的优质留言将会公开显示，欢迎踊跃留言。

Command + Enter 发表

0/2000 字

提交留言

精选留言 (169)

米可哲 置顶

online judge 会不会要求太高，一般水平的人刷 leetcode 就足够了吧？？

作者回复: oj 的难度确实会略高一些，也可以从其他网站做起，比如你说的 leetcode，或者 pythontip

2018-12-30


Non-constant


刷题网站:

1、LeetCode


2、Kaggel


3、老师推荐的 Online Judge

Python 入门：就看这本足够了 ——《Python 编程：从入门到实践》

IDE：pycharm（写爬虫）、jupyter notebook+spyder3（数据分析主要 IDE）、Sublime Text 3（牛逼的编辑器）

数据库：PGsql（挺好用的）、Mysql（开源，主流）

py 版本：毫不犹豫选择 py3（应为 2020 年 py2 停止维护了）

提升：没啥好说的，就是「干」，多写多练自然有感觉了，对，当你写多了代码，你看问题的层次也将不一样。所以，对自己狠心一点，不要一直在入门徘徊。

作者回复：总结的不错，大家都可以看下。如果你的项目没有 py2.7 包依赖的话，直接选择 py3 是好的选择

2018-12-20


●


Q1: 不是 python 内置库

采用命令行安装库 pip install scikit-learn

引用库 import scikit-learn

Q2:


方法一：sum 函数

print(sum(range(1,100,2)))


方法二：if 迭代

a = 0


for i in range(1,100,2):


a += i


print(a)


方法三：while 循环

i = 1


b = 0


while i < 100:


if i % 2 != 0 :


b += i


i +=1


print(b)


作者回复：第二题的三种方法大家可以看下，for 循环，while 循环，sum 函数都有用到

2018-12-20


程序员小熊猫

1. pycharm、sublime、jupyter 都用过，个人认为 Pycharm 适合比较大一点的项目，平时自己开发一些小脚本什么的可以用 sublime，比较简洁方便，目前一直在用 Jupyter，比较适合做数据分析，显示图表之类的，可视化、一行代码一个结果都很方便，今天的课程已经用 Jupyter 全部写了一遍。

2. 求和：sum (range (1, 100, 2))

sum (iterable, start)，sum 的输入是 iterable 对象，比如 list、tuple、set 等

range () 的返回值就是一个 iterable 对象，可以直接作为 sum 的输入参数

3. 前面有位同学一直出现 ‘int’ object is not iterable. 的错误，我今天用 Jupyter 也碰到了，应该是前面老师的例子中用了 sum 做变量，后面求和这道题再用 sum () 做函数，所以出错了，重启下 Jupyter 就行了，或者用魔法命令 % reset 清除变量应该也可以。

4. 吐槽下极客时间里不能回复其他人的留言，只有老师才能，这个功能需要完善下

作者回复：这个同学整理的不错 大家都可以看下

2018-12-21


Microfat


推荐 vs code 编辑器，跨平台，插件多

推荐入门看 python crash course, 进阶看 fluent python, 并且要强迫自己看英文版

工程中尽量用 Python，比如我在做一个上位机的时候强迫自己用 pyqt

ps: 我更喜欢」拍桑」这个读法：)

2018-12-20


每天晒白牙

第一道题：

import scikie-learn


第二道题：

方法一：用 for 循环

sum=0


for number in range(1,100,2):


     sum = sum + number


print sum


方法二：用 while

sum =0


number = 1


while number < 100:


        sum = sum + number


        number = number +2


print sum


作者回复：第一题代码里应该是 import sklearn，第二题正确

2018-12-20


拉我吃

p1.


要先安装库

pip install -U scikit-learn


代码里写

import sklearn


p2.


代码 sum (range (1, 99, 2)) 直接求和

print (sum (range (1, 99, 2))) 打印出来

作者回复：第一题正确，第二题的 range 注意下右边界的取值

2018-12-20


大萌

1、安装完成后 import sklearn

2、


（1）采用 for 循环

sum = 0


for i in range(1,100,2):


    sum+=i


print(sum)


（2）采用递归方法

def sum(x):


if x>99:


return 0


num = sum(x+2)


return x+num


print(sum(1))


平常编程会用 jupyter notebook，也可以推荐一下

作者回复：整理的不错 这两种方式大家都可以看下

2018-12-21


夜路破晓

实话说，这篇读起来「有点卡」，应该是没有编程基础的缘故。晚上下班回来鼓捣半天，最后给笔记本装了 Anaconda，但是类似「Python 中 % 的含义」就让我百度了半小时才搞懂。

逻辑不难懂，甚至看完这篇觉得貌似入门 Python 并不难，关键是想自己写出来就得花点功夫、在搞懂的基础上多做练习了。

买了从零学 Python 的视频课，也找到了《Python：从入门到实践》电子书，打算这周末先研究下再回来看。

作者回复：加油 慢慢来 多谢跑代码 自己试试

2018-12-21


少年不识钱滋味

第三讲笔记

https://mubu.com/doc/3QadR_xU7v


2019-01-07


leiyan


在 sublime 里面输入了代码，跑代码的话大家一般都是用什么方式来跑？

2018-12-23


虎皮青椒

1. 如果我想在 Python 中引用 scikit-learn 库该如何引用？

1）scikit-learn 安装

Python 中安装 scikit-learn 之前需要以下先决条件：

- Python(>= 2.6 or >= 3.3)


- NumPy (>= 1.6.1)


- SciPy (>= 0.9)


1.1）安装 numpy

sudo pip install numpy


1.2）安装安装 scipy

需要先安装 matplotlib、ipython、ipython-notebook、pandas、sympy

sudo apt-get install python-matplotlib ipython ipython-notebook


sudo apt-get install python-pandas python-sympy python-nose


sudo pip install scipy


1.3）安装 scikit-learn

sudo pip install -U scikit-learn


1.4）测试

查看 pip 安装是否有 sklearn 这一项

pip list | grep sklearn


2）导入 scikit-learn 库

from sklearn import *


2. 求 1+3+5+7+…+99 的求和，用 Python 该如何写？

sum = 0


for number in range(1, 100, 2):


sum += number


print ("1 + 3 + 5 + 7 + … + 99 的求和为 % d" % sum)

作者回复: Good Job 两个都正确

2019-04-16


鱼鱼鱼培填

看错题目了，第二题应该是：

sum = 0


for i in range(1, 100, 2):


sum += i


print(sum)


作者回复：正确

2018-12-20


小林子

第一题：

import sklearn


第二题：

sum([i for i in range(1,99,2)])


作者回复：第二题的 range 注意下右边界的取值

2018-12-20


👂🏻阿难👂🏻

文章最末尾的图片形式的手写体格式的学习大纲是用什么工具生成的呢？求教

2019-04-06


蜜糖

我从开始学长就教我使用 jupyter notebook 这个开发环境，不知道这个有没有什么弊端，我用的 python 版本也是 3.x 的 会有很担心以后老师的实战内容是什么版本的

2018-12-23


GS


连加用高斯算法

def sum(n):


  return（1+n）n/2


作者回复: def sum (n):

  return (1+n)*n/2


2019-11-11


qinggeouye


import sklearn


sum(range(99, 0, -2))


def sum_recruit(n):


if n <= 0:


return 0


return n + sum_recruit(n - 2)


2019-10-30


滢

1+3+5+……+99 求和可以直接用等差数列求和，Sn=(a1+an) an/2*n

作者回复：哈哈 直接求等差数列求和了

2019-04-03


忠超

while True:


       try:


              line = raw_input()


              a = line.split()


              print int(a[0]) + int(a[1])


       except:


              break


.................................................................


陈老师好，我想请教一个问题，在 A+B problem 中，根据这种写法，我输入 "3 5", 得到 8；

如果写成以下形式：

line = raw_input()


while True:


       try:


              a = line.split()


              print int(a[0]) + int(a[1])


       except:


              break


.........................................................................


同样输入 "3 5", 却出现一个无限循环，也会得到 8。

陈老师能讲解以下 try...except.. 的用法吗？另外我这两种写法的不同在哪里？

作者回复: try ... except... 是用于捕获异常的，当你异常的时候，可以进行处理，不至于程序中断

2019-01-12


miss


以前不喜欢概率论，现代，离散，都没好好学，考研还特意避开了他们，之后为了避开概率论又修的组合数学，真是欠的债早晚都得还啊，谁让自己这么喜欢数据分析呢。加油吧！

2019-01-09


白夜

OnlineJudge 的比赛题目。。。数学不好伤不起

作者回复：也有简单的题 leetcode 也可以试试

2019-01-08


马克图布

本着复习和分享的目的，对老师课程上的内容作了一些细节上的补充，可能比较适合对 Python 完全没有基础的同学。目前只完成了上半部分（到字典与集合），请老师指正批评

https://alainouyang.github.io/2018/12/13/%E9%92%88%E5%AF%B9%E6%95%B0%E6%8D%AE%E5%88%86%E6%9E%90%E5%B7%A5%E4%BD%9C%E7%9A%84Python%E5%85%A5%E9%97%A8%E7%AE%80%E4%BB%8B-%E4%B8%8A%E7%AF%87/


2018-12-29


Montage


# 执行:pip install -U scikit-learn 安装失败

import sklearn (无法导入)

matplotlib 1.3.1 requires nose, which is not installed.


matplotlib 1.3.1 requires tornado, which is not installed.


Installing collected packages: numpy, scipy, scikit-learn


  Found existing installation: numpy 1.8.0rc1


Cannot uninstall 'numpy'. It is a distutils installed project and thus we cannot accurately determine which files belong to it which would lead to only a partial uninstall.


这个怎么解决，老师？

# 利用栈来求和:

bc = range(1,100,2)


print bc


h=0


while len(bc)>0:


  h+=bc.pop()


print h


2018-12-23


Raindrops


交作业

1.pip3 install scikit-learn


2.print(sum(range(1,100,2)))


2018-12-21


1e-43


老师好，ACM 里 Vol1~Vol32 的难度是逐渐增加吗的，怎么可以选出有易到难的题目？

作者回复：难度不一定是增加的，而是出题的先后顺序。难易程度，你可以看下提交的人数和 Accepted 的比例。提交人数和 Accepted 比例都高的，就说明简单

2018-12-21


Hot Heat


sum(range(0, 100, 2));


(sum(range(1, 101))-50)/2;


reduce(operator.add, range(1, 100, 2));


作者回复：三种方法整理的不错，不过第一个应该是 sum (range (1,100,2)) 吧？

2018-12-20


赵高明同学

第二个问题，我的代码是

for x in range(1,99,2):


    total = sum（x）


print(total)


但显示 ‘int’ object is not iterable.

百思不得其解，请问是哪里出问题了

    


2018-12-20


小罗同学

推荐两个：

1.pycharm, 功能强大，缺点是耗内存。

2.ipython＋任意一个文本编辑器。

作者回复：这两个也不错 我还是喜欢用反应速度快的

2018-12-20


海贼王的男人

sum=0


for number in range(1,100,2):


    sum=sum+number


print sum


#range 左闭右开

2020-01-26


Miracle


sklearn 库是机器学习领域好用到哭的一个库，数据清洗，各种机器学习算法都给写好了，我们可以直接使用，学习 sklearn 感觉最好的方式就是通过官方文档学习：https://scikit-learn.org/stable/，但是在这之前最好先跟着教程过一遍 sklearn，至少知道什么问题应该用什么算法等，然后再通过查阅文档进行补充。使用的时候也很简单，pip install 安装，然后 import sklearn 或者 from sklearn import 模块等。关于学习 Python，我觉得可以找一个简单的教程（B 站上好多）跟一遍，掌握基础的语法和使用，然后就是刷题或者项目中提高代码编程能力，在这个途中遇到不懂得可以查阅 Python 的官方文档进行知识补充。我觉得官方文档是最好的学习方式。

2020-01-23


飘

1、安装 scikit-learn package，在使用该库的脚本中加上「from sklearn import ...」

2、try:


sum=0;


for i in range(1,100,2):


sum = sum + i


print(sum)


except Exception as e:


raise e


2020-01-09


苹果

对于本片文章的感触及收获，数据分析为什么要学 python，在数据分析领域，掌握 python 的编程语言（或者说工具）是非常适合必要的，当然基础的 excel 等基础的工具，要深入点就不够了，也是数据挖掘中算法的核心工具，有些 python 基础，用 python3.7 版，pycharm,，计划了解下 python2,7 的异同吧，操作实践较少，在接下来的课程中，一定实践到底，基本语法及根据之前的笔记在复习巩固，计划在晚上都刷下 leecode 题目，每日结合思维导图，码字分享出来，Q1：pip3 install scikit_learn ,import scikit-learn

Q2:1,sum(range(100))，2，sum0 = 0 ; for i in range(100）: sum0 = sum0 + i print(sum0)


2020-01-08


强者自强

老师，你好，有问题想请教你，但是怎么找运营的同学呢

2020-01-03


Geek_febf89


第二题: num = sum (range (1, 100, 2))

print(num)


2019-12-26


LCX_1799


1、安装 NumPy、SciPy 后，安装 scikit-learn

引入库：import sklearn

2、方法一：sum () 函数

         print(sum(range(1,100,2)))


方法二：for 循环

         sum = 0


         for i in range(1,100,2):


             sum += i


         print(sum)


方法三：while 循环

         sum = 0


         i = 1


         while i<100:


              sum += i


              i += 2


          print(sum)


2019-12-24


小犀牛

c=input()


a,b=c.replace(' ','')


print(int(a)+int(b))


作者回复：嗯 A+B 这道题，你可以 submit 下看看结果

2019-11-23


即可关注

为什么没有 anaconda

作者回复: anaconda 是不错的方式，安装第三方库很方便

2019-10-20


lxyoryxl


第一题：

import scikit-learn


第二题：

# 方法一：for 循环

result=0


for i in range(1,100,2):


    result=result+i


print(result)


# 方法二：while 循环

i=1


result2=0


while i<100:


result2=result2+i


i=i+2


print(result2)


# 方法三：sum 函数

print(sum(range(1,100,2)))


作者回复: 3 种方式，大家都可以看下

2019-10-07


大飞

1.import sklearn;


2.sum = 0


number = 1


while number < 101:


    sum = sum + number


    number = number + 1


print(sum)


作者回复：求 1+3+5+7+…+99 的求和

2019-09-26


ORANGER


输入输出函数那里，不知道为什么我打印不了。我用的是 python3 的版本。但如果把 input 函数去掉，直接命名 name 为一个字符串的话，就可以成功打印。所以这也是由于 python2 和 3 的区别么？

2019-09-25


Kr


1. import scikit-learn


2. sum = 0


for i in range(100)


sum = sum + i


print(sum)


作者回复: 1. import sklearn

2. for i in range(100):


少个:

2019-09-01


Daniel


看到有评论用递归函数来求和，我也贴一下我的代码，感觉我的更好一点

def sum_num(num):


    if num == 1:


        return 1


    if num == 0:


        return 0


    result = sum_num(num - 2)


    return num + result


2019-08-15


smile


1. 安装 scikit-learn 库

导入：import scikit-learn

2. 输出 1-100 之间奇数总和，代码如下：

sum = 0


for i in range(1,100,2):


    sum = sum+i


print('1+3+5+7+...+99=%d'%sum)


作者回复：第一个 import sklearn

第二个 正确

2019-08-11


小二郎

老师，ACM onlinejudge 的问题在本地跑没问题，但是我在 ACM onlinejudge 提交问题之后总是显示 " 系统错误「是怎么回事呢？求赐教。。

作者回复：再提交下，应该每种错误提示都有相应的说明，这种应该是和环境有关系，你 check 下选用的环境

2019-08-10


Geek_Fung


sum = 0


number = 1


while number <100:


   sum = sum + number


   number +=2


print sum


作者回复: Good Job

2019-08-04


舒成

二刷了，效果不一样啊

作者回复：不错啊 舒成同学，相信有不同的收获的

2019-07-27


刘元

想要提高，没有捷径！多看，多想，多动手！

作者回复：对的 这个过程必不可少，因为要让自己的认知升级，只有自己通过训练才能提升。

2019-07-24


Geek_2d03fe


1. import sklearn


2.


sum = 0


added = 1


while added < 100:


    sum += added


    added += 1


print(sum)


作者回复：第一个正确

第二个是求 1+3+5+7+…+99 的求和，再 check 下

2019-07-24


温文墨客

1, 安装完 scikit-learn 包，import scikit-learn

2, 简单一点就用 sum () 配合 range

比如

   sum(range(1,100,2))


或者使用 for 循环或 while 循环，配合 if 函数判断是否为奇数

比如

   for i in range(1,100)：


       j = []


      if i % 2!=0：


         j.append(i)


      print(sum(j))


2019-07-18


twelve


(1)import sklearn


(2)


sum = 0


for i in range(1,100,2):


    sum += i


作者回复: Good Job 第二个结果可以 print (sum)

2019-07-05


内存爆了

这个是我的笔记：https://github.com/too-hoo/DataAnalysisInAction/tree/master/lesson03

课后练习题：

题目一：

scikit-learn (sklearn) 不是 Python 的内置类库，需要使用 pip 安装先

    import sklearn


导入里面的模块：

    from sklearn import xxx


题目二：

代码实现：

# 法一：

oddsum = 0


for number in range(1,101,2):


    oddsum = oddsum + number


print(oddsum)


# 法二：

oddsum = 0


oddsum = 0


number = 1


while number < 101:


    oddsum = oddsum + number


    number += 2


print(oddsum)


# 法三：

print(sum(range(1,101,2)))


# 结果：2500

作者回复：两个都正确，整理的不错！

2019-07-01


Tim. Z


sum = 0


for i in range(1, 100, 2):


    sum += i


print(sum)


作者回复：第二道思考题正确，你也可以尝试用不同的方式来完成。比如 while 循环，sum 函数等。

2019-06-27


libean


看到 raw_input，感觉好别扭。

作者回复：嗯 py2.7 中的语法，py3.X 版本中已经不存在了

2019-06-27


懿竹

题目 A+B 中，用 while true 显示时说，true 未定义，是怎么回事呀

作者回复: True 首字母大写试试

2019-06-21


小侠龙旋风

第一题：

先安装库 scikit-learn：pip install -U scikit-learn

在.py 文件中导入 import sklearn

第二题：

sum(range(1,100,2))


作者回复：两个结果都正确

2019-06-15


绿茶

开始熟练度最重要，先不要管理论，走起来后，把理论补上，要不然，你会一直留在原地或者如同大学计算机毕业生一样，只会说理论不会写代码。

熟练度：练习；练习； 练习；

作者回复：对的 练习最重要

2019-06-14


星豪

1. import sklearn


2. sum = 0


    for nun in range(1,100,2):


         sum += nun


    print sum


作者回复：两个都正确

2019-06-13


建强

思考题 2 的源程序：

#从 1 加到 n 的累加器

def accumlator(n,step=1):


    s = 0


    for i in range(1,n,step):


        s += i


    return s


print("sum(1:100)="+str(accumlator(100,2)))


作者回复：对的

2019-06-09


建强

请问老师：用 python 自带的 IDLE 作开发环境也可以吧？

作者回复：可以的 没问题

2019-06-09


建强

思考题 1：

首先在命令窗口中用 pip install scikit-learn，安装这个库；其次在程序中用语句 from sklearn import * 来引用。

思考题 2 的源程序：

#从 1 加到 n 的累加器

def accumlator(n):


    s = 0


    for i in range(n):


        s += i


    return s


print("sum(1:100)="+str(accumlator(100)))


作者回复：对的

2019-06-09


这只鸟不会飞

第一道题：

import sklearn


第二道题:

sum = 0


for i in range(1, 100):


    if i%2>0:


        sum+=i


print(sum)


作者回复：两个都正确

2019-06-04


Wing·三金

思考题 1：

import sklearn


# or from sklearn import *


思考题 2：

import numpy as np


np.sum(range(1, 100, 2))


作者回复：两个都正确

2019-06-01


nissan


我使用 py3，使用 % 的時候會出現 SyntaxError: invalid syntax

```


score = int(input('your score is:'));


print(%score)


```


為什麼會這樣呢～

2019-05-29


Claude Chen


嗯应该是在国外读研的缘故，喜欢在 Jupyter Notebook 上鼓捣代码...

作者回复: Jupyter 不错，Google Colab 也不错

2019-05-28


未曾沧海

翻了同学们的留言，感觉只有我是个完全的小白。仿佛梦回到了上学时，周围同学都是在选自己有基础的课刷分提升，除了我……

作者回复：慢慢来 未曾沧海同学

2019-05-25


未曾沧海

import scikit-learn


作者回复: import sklearn

2019-05-25


未曾沧海

sum = 0


for number in range(1,99,2):


    sum = sum + number


print sum


作者回复: for number in range (1,100,2):

2019-05-25


未曾沧海

没有，感觉自己完全还没入门呢

作者回复：慢慢来

2019-05-25


未曾沧海

我不懂 Python IDE 诶，安装过 anaconda，直接用可以吗？

2019-05-24


羊小看

1、from sklearn import *


2、sum=0


for i in range(1,100,2):


    sum+=i


print(sum)


作者回复：两个结果都正确

2019-05-06


🌻


1. 如果我想在 Python 中引用 scikit-learn 库该如何引用？

pip install scikit-learn


import sklearn


2. 求 1+3+5+7+…+99 的求和，用 Python 该如何写？

sum = 0


for i in range(1, 100):


    sum += i


作者回复：第二题注意是 1+3+5+7+…+99，所以步长需要设置下

2019-04-23


张英

python 编程很简洁 第二道题 print（sum (range (1，100，2）))

2019-04-13


👹


set 应该是类似数组，但值唯一

作者回复：对的 集合里面不能重复

2019-04-11


Hunter Liu


第一题：import scikit-learn

第二题：sum (range (1, 100, 2))

作者回复：第二题正确

第一题 import sklearn

2019-04-10


戒不了辣条不改名

不晓得现在留言还会不会被翻牌子，spyder 也挺好用的呀，有 matlab 基础的可以试一下，界面很相似

作者回复：会翻牌子的 很好

2019-04-10


林子琪

题目 1: pip install scikit-learn

       import sklearn as sk


或

       from sklearn import *


题目 2：

# 方法 1

s2 = 0


for f2 in range(1, 101, 2):


    s2 += f2


print 's2 => %d' % s2


# 方法 2

s4 = 0


v_num_2 = 0


while v_num_2 <= 100:


    if v_num_2 % 2 != 0:


        s4 += v_num_2


    v_num_2 += 1


print 's4 => %d' % s4


# 方法 3

print 's6 => %d' % sum(range(1, 101, 2))


# 方法 4

def f_digui_odd(yy):


    if yy > 100:


        return 0


    else:


        return yy + f_digui_odd(yy + 2)


# 调用

print 's8 => %d' % f_digui_odd(1)


作者回复: Good Job 林子琪同学

2019-04-04


滢

基于 Python3 IDLE 习题代码

第一种方案，直接运用系统 sum 函数

>>> print(sum(range(1,100,2)))


2500


第二种方案，for 循环

>>> total_number = 0


>>> for number in range(1,100,2):


total_number += number


>>> print ("1+3+5+7+…+99 之和", total_number)

1+3+5+7+…+99 之和 2500

第三种方案，while 循环

>>> while number < 100:


total_number += number


number += 2


>>> print ("1+3+5+7+…+99 之和", total_number)

1+3+5+7+…+99 之和 2500

优化方案：

>>> while length < 25:


total_number += (number + (100 - number))


number += 2


length += 1


>>> print ("1+3+5+7+…+99 之和", total_number)

作者回复: Good Job！

2019-04-03


Jeffery_郑广军

第一题

import sklearn


第二题

sum=0


for i in range(1,100,2):


    sum+=i


print(sum)


作者回复：两个都正确

2019-04-02


种菜的渔民

bb=0


for aa in range(1,100,2):


    bb=aa+bb


print(bb)


作者回复: Good Job

2019-04-01


窝窝头

第一题

from sklearn import * 或者 from sklearn import some_module

第二题

sum = 0


for i in range(1, 100, 2):


    sum += i


2019-03-19


陈伟

本人初学 python，复制代码到 jupyter 里（python 3），每次代码都报错。如何处理？

2019-03-16


丸子

def add(a,b,c):


d = 0


for e in range(a,b,c):


d = d +e


return d


a = add(1,100,2)


print(a)


2019-03-13


滢

陈老师，想问下数据分析对于编程的要求是一个什么水平，需要知道 Python 的底层原理嘛，还是只要会用就行，对于想转行进入这个行业的初学者来说

作者回复：底层原理不需要，基础的 Python 语法还是需要的，比如 numpy, pandas 的时候，数据分析的话 需要掌握 sklearn 的使用

2019-03-11


owx8012


老师提到在选择 python2 还是 3 时，说如果需要用到 2 中的库那就选择 2，那么如果有项目同时 2 和 3 的库都要用到的情况下，该如何选择？

作者回复：直接用 Python3 吧

2019-03-06


风中镜水

老师，Online Judge 题库语言全是英文呀，看不懂，有中文的刷题网站吗？

2019-03-02


三硝基甲苯

1.import sklearn


2. sum(range(1,100,2))


这样。

作者回复：两个都正确

2019-02-11


叶えなくちゃ

Q1:


import scikit-learn


Q2：


创建空列表，使用 for 循环迭代，将奇数加入列表，使用 sum 函数求和

alist = []


for i in range(1,100):


    if i%2 != 0:


        alist.append(i)


print(sum(alist))


作者回复: import sklearn

2019-02-09


大圣归来

学习笔记：https://mubu.com/doc/97aC7gCqi0

1、先要安装 scikit-learn : pip install -U scikit-learn

然后在代码里导入：import sklearn

2、两种方法：

#用 for 循环

sum = 0


for i in range(1, 100 ,2):


sum += i


print(sum)


#用 sum 和列表生成式

print(sum(i for i in range(100) if i%2 != 0))


> 编辑器用 Sublime Text 速度快，好用；pycharm 更强大和方便一些。

如果是 win 用户推荐之间安装 Anaconda 省心省力，不用苦恼环境配的问题，GitHub 上有一个教程不错：https://github.com/YZHANG1270/Girls-In-AI/blob/master/machine_learning_diary/base/print/README.md

作者回复：做笔记不错～sublime 编辑器挺好用的，不过我现在用阿里的 PAI DSW 了

2019-01-31


张营

老师你好，online judge 可以查看别人的代码吗？有没有题目的推荐答案？

作者回复：看不了别人的代码，你可以在社区里看看有没有人分享，另外你也可以关注下 Kaggle，有人会分享 NoteBook 就可以看到别人的代码

2019-01-29


Blaise


1. Got it from internet.


import sklearn


2.


total = 0


for i in range(1, 100, 2):


    total += i


print(total)


2019-01-28


杰之 7

通过这一节的学习，老师带我们回顾了 Python 的入门语法和 IDE 的选择。

回到老师的问题，对于确定的连续加问题，可以用 for 循环迭代解决。对于导入机器学习包的导入，通过 import 加包名导入。

作者回复：加油～努力就会有收获

2019-01-27


月亮上的熊猫_lv

1. import sklearn


2.


summ = 0


for i in range(1,100,2):


    summ = summ + i


print(summ)


作者回复：正确

2019-01-24


cissy


# 1


s = 0


w = 1


while w < 100:


    s = s + w


    w = w + 2


print(s)


#2


a = 0


for i in range(1, 100, 2):


    a+= i


print(a)


然后老师，我用

print (sum (range (1,100,2))) 为啥报错啊：TypeError: 'int' object is not callable

是因为 python3 的关系吗？

作者回复: print (sum (range (1,100,2)))

我这边是 work 的

2019-01-17


柚子

学习笔记 https://blog.csdn.net/hahaha66888/article/details/86510061

问题答案：

1、用的 juypter 可以直接 import sklearn

2、方法一、

sum = 0


for i in range(1,100,2):


    sum = sum + i


print(sum)


方法二、

sum = 0


i = 1


while i <= 99:


    sum = sum + i


    i = i +2


print(sum)


方法三、

sum(number for number in range(100) if number%2 ==1)


作者回复：加油～坚持做笔记和作业的同学

2019-01-16


newmyself


第二题，不知道 sum 函数的作用，用的：print (1+99)*50/2

2019-01-15


Daisy


第一题:import sklearn

第二题:print (sum (range (1,100,2)))

作者回复：两个都正确

2019-01-15


LI.T.F


老师用的是 python2，代码和 python3 有点不同，我学的是 python3，看老师的代码有点凌乱，我复制过来在 python3 上运行会报错，还得自己改，有点难受

作者回复：后面用的 Python3 了

2019-01-11


LI.T.F


安装完 sublime text3 后不会用，怎么办啊？

作者回复：这个应该不难，遇到问题了可以百度下，一般之前都有人遇到过的

2019-01-11


卢嘉敏

我用的 python3.6，visio 的，感觉跟课程运行后结果不一样

作者回复：后面我用的 Python3，所以咱两一样的

2019-01-09


傻马难骑

这节课对于文科生来讲有点难度了。这阵子在 Python 小课学习，那边先尽快过一遍再回来这里，都是别抛弃我 :(

2019-01-09


Sunway.


#第一题，pycharm 内点击 file-settings-Project Interpreter 安装 scikit-learn 包

import sklearn


#第二题方法一

sum=0


for number in range(101):


    if number%2==0:


        continue


    else:


        sum=number+sum


print(sum)


#第二题方法二

sum=0


for number in range(1,100,2):


    sum=sum+number


print(sum)


总结，第二题没看答案之前先想到的是用 for..in. 的循环结构，刨去偶数，再求和。第二个方法是回看了 range 的用法后尝试写出的，代码量更少一点

2019-01-07


七步

问题 1 的回答。

代码如下:

import scikit-learn


问题 2 的回答。

代码如下 (2.7 版本）：

def oddsum():


      sum_odd=0


      num=1


      while num<=99:


         sum_odd+=num


         num+=2


      return sum_odd


print oddsum()


2019-01-06


xfoolin


一：

1. pip install scikit-learn


2. import sklearn


二：

1. print(sum([i for i in range(1,100,2)]))


作者回复：正确

2019-01-05


王彬成

思考题：

一、引用 scikit-learn 库

1、在命令行中 pip install -U scikit-learn，安装 scikit-learn

2、在 python 输入 import sklearn

二、求 1+3+5+7+…+99 的求和

1、for 循环

sum=0


for a in range(1,100,2):


    sum+=a


print(sum)


2、while 循环

a=1


sum=0


while a<100:


 sum+=a


 a+=2


print(sum)


作者回复：正确

2019-01-04


zzz 慧敏

题目一：

情况 1：未安装库，需要 pip install scikit-learn，安装库后 import

import sklearn


情况 2：

import sklearn


题目二：

方法 1：sum

sum = 0


for number in range(1,100,2):


    sum = sum + number


print(sum)


方法 2：while 条件判断

sum = 0


number = -1


while number < 99:


    number = number + 2


    sum = sum + number


print(sum)


方法 3：for 循环

sum = 0


for i in range(1,100,2):


    sum += i


print(sum)


作者回复：正确

2019-01-04


风

import sklearn


第二道题，本想用 for 循环，感觉列表更简单，但是要多熟练才能想起呢，还是多多练习吧。

虽然学过了，但仍旧有收获，import 的本质是路径搜索。

作者回复：对 本质是路径搜索

2019-01-02


wonderland


因为买的课比较晚，所以现在想要奋力追赶上老师的步伐。

#练习题 1. 如何在 python 中引用一个库

import sklearn


#练习题 2. 求 1+3+5+...+99 的和

s = 0


i = 1


while i < 100:


    s = s+i


    i = i+2


print s


作者回复：正确 加油💪

2019-01-01


李沛欣

看完了，我还是先把我那本《Python 从入门到放弃》的书学好比较重要。不然真的跟不上啊

作者回复：不用担心 这里主要用的是一些基本语法 还有 Pandas 的使用多跑几遍代码也就对 Python 逐渐了解了

2019-01-01


无形

Pip install ~


import ~


2018-12-31


caidy


1. inport sklearn


2. 三种方法：

A.


sum1 = 0


for number in range(1, 100, 2):


sum1 = number + sum1


B.


number = 1


sum1 = 0


while number < 100:


sum1 = sum1 + number


number += 2


C.


print(sum(range(1, 100, 2)))


2018-12-30


LLY


之前没有学过 Python，第一次看不懂，特意找了个课程快速入门后发现简单的程序都能写出来啦，现在需要在函数方法使用熟练度以及数据结构、算法方面多下功夫。不过在 ZOJ 看到题目后，感觉一些题目要写出算法复杂的代码，需要花很多时间去学习和提升算法设计和编程能力，如果当前的目标是想快速学会 Python 这个工具辅助数据分析，需要花大量的时间去多做编程题提高算法设计能力吗？

回答课后两道题：

第一题：

import sklearn


第二题：

sum=0


for i in range(1,100,2):


    sum=sum+i


print(sum)


作者回复：两道题都正确，我觉得最主要的是跟上专栏，把代码都跑一遍。在这个基础上，再多一些题目训练

2018-12-30


人间百态沧桑

感谢老师的课程，以下是我的个人观点，如果错误请大家指正，谢谢！

第一. vscode 也是不错的 python 开发工具哦，插件多，轻量级，容易配置

第二。关于老师的留的第二个问题，可以用 n*（n+1）/4 公式一次解决，复杂度只有 o（1），如果用到循环硬加的话，复杂度是 o（n）

作者回复：第一题 vscode, jupyter 都不错

第二题 可以用公式自己算，你写的公式再看一看

2018-12-29


人间百态沧桑

第一. python 的工具我觉得 vscode 也不错

第二。关于第二个问题，可以用 n（n+1）/4 来做，这个复杂度是 o（1），如果是 while 循环硬写，复杂度就是 o（n）

作者回复：第一题 vscode, jupyter 都不错

第二题 可以用公式自己算，你写的公式再看一看

2018-12-29


希希

老师，我是零基础，python 安装完，是如何运行的？能否祥说？

2018-12-29


Lambert


题一：

pip install scikit-learn


import scikit-learn


题二：

sum = 0


for number in range(1,100,2):


    sum = sum + number


print(sum)


2018-12-28


Ouyang_w


能分享下 python 的基于 vim 开发环境的搭建不

2018-12-26


易平

#如果我想在 Python 中引用 scikit-learn ...

from sklearn import *


#求 1+3+5+7+…+99 的求和，用 Python 该如...

countNum=0


for i in range(1,100,2) :


    countNum=countNum+i


    


print(countNum)


2018-12-26


leiyan


用 Sublime 摸索了一下，在执行 py 程序时需要设置一下，而且如果执行带 input 的程序还要再装插件而且没有快捷键。现在改用 Geany，目前为止感觉比较舒服

2018-12-26


就 e

python 基础还需要学习面向对象方面的内容吗？

2018-12-25


77


set 里的值顺序是 random 的，dictionary 和 list 都是保持定义时的顺序呢

2018-12-25


Alex 王伟健

https://mubu.com/doc/mzE21o7gu0


if 语句

a = 0


i = 1


if i < 100:


a = a + i


i += 2


else :


print(a)​


for 语句

a = 0


for i in range(1,100,2)​​:


a += i​


print a


while 语句

i = 1


a = 0


while i < 100:


​​​ a += i


​ i += 2


print a​


函数

print(sum(range(1,100,2)))


2018-12-25


大飞

老师讲得很好，谢谢老师的分享。习题答案：

1，首先安装 sklearn 包，然后用 import sklearn 调用；

2、i = 1


sum = 0


while i < 100:


    sum = sum + i


    i += 1


print (sum)


2018-12-25


胡

完成第一题花费了好长时间，终于安装完毕。

2018-12-25


闫东汉

注册了浙大的 ZOJ，第一次来到这样的专业论坛

作者回复：加油 慢慢来 里面很多题还是很有难度的。建议你先把 A+B 大家都必做的题目先过了 然后逐渐熟悉其他题目

2018-12-25


闫东汉

1.import sklearn


2.print(sum(range(1,100,2)))


浙江大学的 ZOJ 注册不上是怎么回事呢？

2018-12-24


刘亚伟

第一题：

import scikit-learn


第二题：

sum = 0


number = 1


while number < 100:


    sum = sum + number


    number = number + 2


print sum


后来看留言，第一题错了；第二题粗心了，没看清要求，最后的结果是 4950，另外感觉对代码不是太熟。

还是需要多练习。另外大家的留言都很赞，看到了不一样的思路，谢谢老师和同学们。

2018-12-24


世人习惯看错我

有个问题想问老师，我们下的 python 很小，安装后好像就可以开始写代码了，可是为啥还要安装 IDE 来写呢，他们之间是什么关系？我安装了 Spyder，好像就自带 python 了，都不用装那个 30 多 M 的那个了，所以一直很费解，想问问老师他们之间什么关系

2018-12-24


世人习惯看错我

python. IDE 是什么意思？算是写代码的一个软件么，我想问问 spider 算是其中一个 IDE 么，我用的是这个，但是我看介绍里面没有，但是感觉用的也挺普遍的

作者回复: IDE 是你的编程开发环境，比如 eclipse，PyCharm, sublime text

spider 是做数据采集的方法，不属于 IDE。

2018-12-24


土豆毛毛

老师您好，请问一下把 A+B 问题放入 OnlineJudge 之后，出现了 Non-zero Exit Code 问题，是怎么回事呢？

string = input("Please type two digits as required:")


try:


    num = string.split(" ")


    print("the sum is %d" %(int(num[0])+int(num[1])))


except EOFError:


    pass


2018-12-24


陈凯俊

1. import sklearn


2


.n = 1


sum = 0


while n <= 99:


    sum = sum + n


    n = n+2


print(sum)


2018-12-23


lee4


Jupyter Notebook 也是对新人极为友好的环境。

同时也是快速进行数据理解的辅助工具。

2018-12-23


Kyle


sum ＝0


for number in range(1,100,2):


      sum = sum +number


print sum


2018-12-22


Grandia_Z


我打开了浙大的那个刷题网站，看到了题目答案，但怎么在上面直接做题，没找到入口啊

2018-12-21


楚云

题一：

为什么是 import sklearn，不是 import scikit_learn，也不是 import learn？

题二：

# sum


print(sum(range(1,100,2)))


# for…in


sum = 0


for number in range(1,100,2):


    sum = sum + number


print (sum)


# while


sum = 0


number = 1


while number < 100:


    sum = sum + number


    number = number + 2


print(sum)


2018-12-21


发条

这一篇读着不是特别流畅的同学下一篇会更卡，还是先把零基础学 python 的课程先好好看看

作者回复：恩 自己把代码都跑一遍，效果会更好

2018-12-21


云虹

请问 Phython 怎么安装…… 太菜了，谢谢

2018-12-21


吴晓岚 jim wu

作业第一题

安装好 sklearn 包后在程序中

import sklearn


课后作业第二题

print((sum(range(1, 100, 2))))


2018-12-21


何楚

1. 如果我想在 Python 中引用 scikit-learn ...

import sklearn


from sklearn.linear_model import LinearRegression


from sklearn.model_selection import train_test_split


2. 求 1+3+5+7+…+99 的求和，用 Python 该如...

count = 0


for x in range(1, 100, 2):


    # print(x)


    count += x


print(count)


2018-12-21


孑

1. 如何使用库，第一步安装该库，第二步在文件中 import 该库

2. 奇数求和，第一反应是用循环:

result = 0


for i in range(1，100，2):


          result+=i


瞄了一眼评论，发现自己这个有点 low，还是 sum 函数更好。

2018-12-21


跳跳

第一题：import sklearn

第二题：

sum=0


for i in range（1，100，2）


      sum=sum+i


print sum


2018-12-21


吴晓岚 jim wu

包是一个包含有至少一个 *.py 和__init__.py 程序？

import sys 这个语句是引用包还是模块？

2018-12-21


hh


今天的总结 https://mubu.com/doc/zVFIHRegW0

另外好像没在 ZOJ 上找到您说的官方 python 答案... 想在您介绍的网站上练练 python 基础... 但是还是想有份答案，心里有底点，要不然自己纠结很久都做不对...

2018-12-21


Alex 王伟健

anaconda 是不是太简陋了。。。

2018-12-21


Louie Zhang


Pycharm 缺点就是启动太慢，个人平时喜欢使用 VS-code 和 Jupyter notebook 开发，jupyter 是数据分析最好用的 IDE 了。

2018-12-21


許敲敲

老师用的 python 2.x

作者回复：对 我用的 2.7 后面如果大家用 3.X 比较多的话 我也可以用 3.X

2018-12-20


汪汪汪

1，


pip3 install scikit-learn (终端中输入)

import scikit-learn（源程序首行加入）

2，


#利用递归实现奇数累加

def sum_numbers(num):


    if num == 1:


        return 1


    return num+sum_numbers(num-2)


result = sum_numbers(99)


print(result)


2018-12-20


荀辰龙

发留言会把缩进消除掉，在这里写 Python 代码笑着流泪😂

作者回复：可以尝试用空格缩进

2018-12-20


💻delete 小刘

第一题 import 需要导入的库名

第二题 for i in range (1，100):

             print(sum(i))


作者回复：第二题需要注意两点：一个是 range 步长，第二个是 sum 的使用

2018-12-20


往事随风

1. import scikie-learn


2.


sum = 0


for i in range(1, 100, 2):


    sum += i


print(sum)


2018-12-20


萍水相逢

期待快快更新，刚学完 Python 基础

2018-12-20


呼呼 ²⁰¹⁸

一点开发基础都没的，表示压力好大【捂脸】【捂脸】

作者回复：没事的 你安装 python 和编辑器，然后把我的代码自己跑一遍

2018-12-20


无畏不惧 @

1. import scikit-learn


2.sum=0


   for i in range(1,100):


          sum=sum+i


作者回复: scikit-learn 在 python 引用中用 sklearn 代替，第二题需要注意 range 步长

2018-12-20


执笔，封心

我想问下 online judge 是怎么来用的

作者回复: online judge 就是在线测试的意思，他有很多题目，你可以选择最基本的，比如 A+B 这道自己体会下。我在文中也给出了网址，你可以自己试试

2018-12-20


桃园悠然在

Mac 自带 Python，安装 Sublime Text 编辑器之后对照文中代码把基础语法全都跑一遍，看着打印的结果信心满满，感觉自己也能编程了！

作者回复：太好了 所以说有人带着，和大家一起来学习 效率是最高的。尤其是跑代码的过程。Sublime Text 也很好用

2018-12-20


frazer


python 运行慢，还有多线程比较鸡肋，编辑器推荐 vs code

2018-12-20


typoiu


1. from sklearn import *


2.


sum =0


for a in range(1，100，2)：


    sum +=a


print(sum(range(1,100,2)


作者回复：正解，第二题的两种方法都不错

2018-12-20


Conan


python3.x


1. import sklearn


2. sum(range(1, 100, 2));


作者回复：正确

2018-12-20


李逍遥

讲解言简意赅，宛如 python 语言本身一样

2018-12-20


liufengnet


1.import sklearn


2. sum(range(1,100,2))


作者回复：正解

2018-12-20


mickey


P01：


pip install -U sklearn


from sklearn import *


P02：


  1）print(sum(range(1, 100, 2)))


  2）sum = 0;


       for number in range(1, 100, 2):


            sum = sum + number


       pirnt(sum)


作者回复：正确

2018-12-20


乘坐 Tornado 的线程魔法师

推荐两个也很知名的刷题网站 leetcode（中文版叫领扣），还有一个是 lintcode

2018-12-20


龚梁生了没

1.import sklearn


2. 直接用 sum 求和.. sum (i for i in range (1,99,2))

作者回复：用 sum 是个比较简便的方式，不过 range 这里应该是 range (1,100,2) 你可以想下是为什么

2018-12-20


JingZ


(1)import sklearn


(2) 代码 for 循环，也可用 while 或者调用模块

sum = 0


for i in range(1,100,2):


      sum += i


print(sum)


我使用的是 jupyter notebook 做的，安装 Anaconda 了～

作者回复：正确

2018-12-20


拉我吃

第二个应该是 range (1,100,2)；右边取不到 [a,b)

作者回复：正确

2018-12-20


inzack


1.import scikit-learn


2.


sum=0


a=1


bool=true


while(bool):


  sum+=a


  a=a+2


  if a>=99


     bool=false


print(sum)


另外，我觉得 vscode 这个工具也是很赞。😁

2018-12-20


kyle


今天总结如下：

https://mubu.com/doc/cXA3UsDfV0


2018-12-20


Key.


1. from sklearn import *


2. sum = 0


    for i in range(1,100,2):


           sum = sum + i


    print(sum)


作者回复：正确

2018-12-20


徐洲更

在本次介绍的几种数据结构中，对于一个固定大小的数据集，哪一种数据结构的随机索引效率最高呢？tuple 是不是类似于 C 语言中的数组呢。

2018-12-20


收起评论




99+99+










