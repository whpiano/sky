创建自已对象就python非常核心的概念，事实上，python被称为面向对象语言，本章会介绍如何创建对象。以及面向对象的概念：继承、封装、多态。



* 多态： 可对不同类的对象使用同样的操作。
* 封装：对外部世界隐藏对象的工作细节。
* 继承：以普通的类为基础建立专门的类对象。





**多态**

面向对象程序设计最有趣的特性是多太，它是是让大多数人犯晕的特性。所以，先来介绍这个。

多态意思是“有多种形式”。多态意味着就算不知道变量所引用的对象类是什么，还是能对它进行操作，而它也会根据对象（或类）类型的不同而表现出不同的行为。



从最简单的开始

 任何不知道对象到底是什么类型，但是又要对对象“做点什么”的时候，都会用到多态。这不仅限于方法----很多内建运算符和函数都有多态的性质，考虑下面这个例子：

```
>>> 1 + 2
3
>>> 'fish' + 'license'
'fishlicense'
```



这里的加运算符对于数字（本例中为整数）和字符串（以及其他类型的序列）都能起作用。假设有个叫做add的函数，它可以将两个对象相加。那么可以直接将其定义成上面的形式，对于很多类型的参数都可以用，如下：

```
>>> def add(x,y):
    return x+y

>>> add(1,2)
3
>>> add('hello.','world')
'hello.world'
```

看起来有点傻，但是关键在于参数可以是任何支持加法的对象。



如果需要编写打印对象长度消息的函数，则只需对象具有长度（len函数可用）即可。

```
>>> def length_message(x):
    print"The length of " , repr(x),"is",len(x)

    
>>> length_message('chongshi')
The length of  'chongshi' is 8
>>> length_message([1,2,3])
The length of  [1, 2, 3] is 3
```

len函数用于计算长度，repr用于放置函数的内容；repr函数是多态特性的代表之一---可以对任何东西使用。

很多函数和运算符都是多态的，你写的绝大多数程序可能都是，即便你并非有意这样。





**封装**



封装是对全局作用域中其它区域隐藏多余信息的原则。

封装听起来有些像多态，因为他们都是 抽象的原则---他们都会帮助处理程序组件而不用过多关心多余细节，就像函数做的一样。

但是封装并不等同于多态。多态的可以让用户对于不知道是什么类（或对象类型）的对象进行方法调用，而封装是可以不用关心对象是如何构建的而直接进行使用。

创建一个有对象（通过像调用函数一样调用类）后，将变量c绑定到该对象上。可以使用setName 和 getName 方法（假设已经有）

```
>>> c = closedObject()
>>> c.setName('sir lancelot')
>>> c.getName()
‘sir lancelot’
```





**继承**

我们不想把同一段代码写好几，之前使用的函数避免了这种情况。但现在又有个更微妙的问题。如果已经有了一个类，又想建立一个非常类似的类，只是添加几个方法。

比如有动物类，我们又想在动物类的基础上建立鸟类、鱼类，哺乳动物类。



上面这些特性会根据后面的学习来深入的理解。

 ================================





**创建自己的类**



终于可以创建自己的类了，先来看一个简单的类：

```
_metaclass_ = type #确定新式类

class Person:
    def setName(self,name):
        self.name = name

    def getName(self):
        return self,name

    def greet(self):
        print "Hello, world! I'm %s" %self.name
```

注意：新式类的语法中，需要在模块或者脚本开始的地方放置赋值语句\_metaclass\_ = type 。



创建了一个Person的类，这个类包含了三个方法定义，只是那个self看起有点奇怪，它是对于对象自身的引用。

让我们创建实例看看：

```
>>> huhu = Person()
>>> huhu.setName('hu zhiheng')
>>> huhu.greet()
Hello, world! I'm hu zhiheng
```

应该能说明self的用处了，在调用huhu的setName 和 greet 函数时，huhu自动将自己作为第一个参数传入函数中----因此形象地命名为self。每个人可能都会有自己的叫法，但是因为它总是对象自身，所以习惯上总是叫做self 。



和之前一样，特性也可以在外部访问：

```
>>> huhu.name
'hu zhiheng'
>>> huhu.name = 'yoda'
>>> huhu.greet()
Hello, world! I'm yoda
```







**特性、函数和方法**



self 参数事实上正是方法和函数的区别。方法将它们的第一个参数绑定到所属的实例上，因此这个参数可以不必提供。所以可以将特性绑定到一个普通函数上，这样就不会有特殊的self参数了：

```
>>> class Class:
    def method(self):
        print 'I have a self!'

        
>>> def function():
    print "I don't"

    
>>> instance = Class()
>>> instance.method()
I have a self!
>>> instance.method = function
>>> instance.method()
I don't
```



self参数并不取决于调用方法的方式，目前使用的是实例调用方法，可以随意使用引用同一个方法的其他变量：

```
>>> class Bird:
    song =  'Squaawk!'
    def sing(self):
        print self.song

        
>>> bird = Bird()
>>> bird.sing()
Squaawk!
>>> birdsong = bird.sing
>>> birdsong()
Squaawk!
```







**指定超类**



子类可以扩展超类的定义。将其他类名写在class语句后的圆括号内可以指定超类：

```
class Filter:
    def init(self):
        self.blocked = []
    def filter(self , sequence):
        return [x for x in sequence if x not in self.blocked]

class SPAMFilter(Filter):  #SPAMFilter是Filter的子类
    def init(self):        #重写Filter类中的init方法
        self.blocked = ['SPAM']
```

Filter 是个用于过滤序列的通用类，事实上它不能过滤任何东西：

```
>>> f = Filter()
>>> f.init()
>>> f.filter([1,2,3])
[1, 2, 3]
```

Filter 类的用户在于它可以用作其他类的基类（超类，“java中叫父类”），比如SPAMFilter类，可以将序列中的“SPAM”过滤出来。

```
>>> s = SPAMFilter()
>>> s.init()
>>> s.filter(['SPAM','SPAMD','SPAM','HELLO','WORLD','SPAM'])
['SPAMD', 'HELLO', 'WORLD']
```







**调查继承**



如果想要查看一个类是否是另一个的子类。可以使用内建的issubclass函数：

```
>>> issubclass(SPAMFilter, Filter)
True
>>> issubclass(Filter,SPAMFilter)
False
```

