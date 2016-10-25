##Example1、2

每个example包含了两个部分：src文件和.xml文件。其中src文件定义了各模块里面做了什么事情，而xml文件定义了这些模块的连接方式。

*（example1, example2类似，只是在xml文件略有不同，这里先用example来说）

######Generator.c:
![](http://i.imgur.com/RGq8Uz4.png)

主要做了一件事，从local位置开始到len长度，向POR_OUT端口写入x，而x就是当前的位置。

######Consumer.c:
![](http://i.imgur.com/liBT1Ck.png)

主要做的事情是把从PORT_IN端口接受到的信息打印出来。

######Square.c:
![](http://i.imgur.com/BxlooHD.png)

把PORTIN 读到的数据做了平方后输出到PORT_OUT。本次试验要求最后输出三次方数，所以只用把这里的`i=i*i` 改成 `i=i*i*i`即可。
 
######example1.xml
再来看XML文件，这里面定义了这些模块的连接方式。
![](http://i.imgur.com/AHDc23A.png)

拿第一个generator来说，定义了它的模块名；它有一个output端口，名字是1，以及它的文件类型，地址。

第三个square有两个端口，一进一出，因为这个模块的功能是接受一个数字然后进行三次方运算之后输出。

![](http://i.imgur.com/MLggFFU.png)
这里面的通道（连线）也是需要定义的。
这里就定义了两个通道，并且给他们进出口都命了名。

![](http://i.imgur.com/u3suV6e.png)
然后就是模块和通道之间的连接方式。通过origin-target的组合来说明是哪两个端口连在一起。

对于实验二：
用了迭代的方式创建了N个Square模块，并且用迭代方式定义了这些Square模块和通道之间的连接方式。所以根据实验要求，将N改成2，就可以获得结果：只创建2个Square模块，对i的平方操作只进行两次。

![](http://i.imgur.com/rCXpv4o.png)
![](http://i.imgur.com/qsI6ofZ.png)
#####实验结果
实验要求最后consumer输出三次方数，具体做法在上面已经提到了。所以直接上结果：
Example1:
![](http://i.imgur.com/agkrnVH.png)

dot图：
![](http://i.imgur.com/gBDvZhI.png)


Example2:
![](http://i.imgur.com/ZbN3vkN.png)

dot图：
![](http://i.imgur.com/c9QbT1D.png)


#####实验感想
本次实验的代码并不难理解，XML文件也能很容易的看懂。了解到了分布式体系结构最基础的构架，是由模块，通道组成的，而对分布式体系构建也分为两方面，一个是模块内的功能的实现，另一个是构架的定义，只有二者配合得当才能得到一个性能较优越的体系。