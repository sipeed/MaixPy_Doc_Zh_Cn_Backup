machine模块介绍
===================================

好了，经过了开箱过程。相信大家对于Sipeed M1/MW和MaixPy有了一定的认识。笔者明白，那么一点东西很难满足大家。那么接下来的章节，将对MaixPy的内容进行一个比较详细的介绍。那么就让我们开始MaixPy的探索之旅吧

.. note:: 如果在输入代码的过程遇到困难，可以使用 **Tab** 键进行代码补全，这是一个很方便的功能。

``machine`` 模块旨在让用户可以以脚本形式操作Sipeed M1开发板上的外设和片上外设，比如RGB灯，pwm输出，LCD显示屏等等。 ``machine`` 模块下分了多个类型，这些类型对应的就是每个外设。那么接下来就让我们一个一个
认识它们吧

.. contents:: 本文目录

Fpioa
-------

第一个类型我们就来说明一下 ``fpioa`` 吧，它主要用于映射k210芯片的功能到外部引脚。因为k210一共有48个引脚，每个引脚都是可编程的。比如我们可以让1号引脚设置为GPIO0来输出，也可以让1号引脚设置为IIC或者SPI来输出。这里要分清，引脚跟GPIO是不同的。普通单片机的引脚都是GPIO跟另外一个功能来复用，但k210并不是这样，k210的每个引脚可以设置为200多个功能中的一个。更加详细的内容请参考k210官方手册。

该模块只负责设置引脚功能，一般用于在外设初始化之前使用。

首先我们需要创建fpioa对象，这里不需要传递参数。

.. code-block:: bash

                fpioa=machine.fpioa()

K210一共有200多个功能，为了让各位小白和用户能够方便的查看功能及其简介，我们可以直接使用fpioa.help()来查看200多个引脚的功能。

.. code-block:: bash

                fpioa.help()

log输出如下，太多了，就截取一部分作为代表吧	

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/fpioahelp.jpg
  :width: 500px
  :align: center

			
使用之后会串口会输出fpioa表，用户可以自行查看。你可能会觉得太多了，我只要知道其中的某个就行。没问题，举个例子，直接使用。其中fpioa也将每个
功能号映射为人类阅读起来比较友好的字符串(可以使用Tab键补全)，其中GPIO0表示的就是GPIO0的功能号

.. code-block:: bash

                fpioa.help(fpioa.GPIO0)

输入如下图

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/fpioahelp().jpg
  :width: 500px
  :align: center

		
我们知道使用什么功能后，我们就可以将该功能映射到引脚。我们可以使用下面的语句，第一个参数是 **芯片引脚**，第二个参数是 **片上外设功能号** ，如下所示是将板子上连接的绿色LED灯引脚设置为GOIOHS0。关于board_info我们将在后面进行介绍。

.. code-block:: bash

                fpioa.set_function(board_info.LED_G,fpioa.GPIO0)  

GPIO
------------

``GPIO`` 模块用于获取或者设置GPIO的值。

.. note:: 使用GPIO前需要使用fpioa将其功能映射到外部引脚。这里不再多说，请各位小伙伴自行映射

创建gpio对象，第一个参数是 **GPIO号** ，对应使用的GPIO，第二个是参数的 **GPIO模式** ，一共包括，输入，上拉输入，下拉输入和输出，第三个参数为 **GPIO口的值** ，当且仅当模式为输出时有效。下面的例子是将GPIO0设置为输出模式，同时输出的电平为0

.. code-block:: bash 

                gpio=machine.GPIO(machine.GPIO.GPIO0,machine.GPIO.DM_OUTPUT,machine.GPIO.HIGH_LEVEL)

我们也可以获取GPIO的值，当没有参数时，是直接获取GPIO的值，当传入参数时为设置GPIO口的值，当传入参数时为设置GPIO的值但无返回。

.. code-block:: bash 

                value=gpio.value()  

or

.. code-block:: bash 

                gpio.value(gpio.LOW_LEVEL)

拉低引脚电平，点亮绿灯
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/GPIO.jpg
  :width: 500px
  :align: center

另外还有一种设置GPIO值的方法，作用是翻转GPIO口的值并且返回GPIO口的当前值。

.. code-block:: bash

                value=gpio.toggle()

如下图，IO翻转之后绿灯就熄灭了
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/GPIOtoggle.jpg
  :width: 500px
  :align: center

如果在写代码的过程中忘了方法的使用，可以使用以下语句获取帮助，对于其他模块的help函数，我们也将在后续加入

.. code-block:: bash

                machine.GPIO.help()

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/gpiohelp.jpg
  :width: 500px
  :align: center
				
我们前面的开箱教程中就有使用GPIO的方法，小伙伴们可以参考一下
	
Timer
-------

``Timer`` 主要用于创建定时器并执行相应功能。定时器的作用笔者就不多赘述了，相信各位小白和用户应该知道它的作用。我们直接看看定时器怎么用吧

创建定时器，k210一共有4个定时器，每个定时器一共有4个通道，关于定时器更加详细的信息，可以从k210的 `datasheet  <http://pgeza64pd.bkt.clouddn.com/kendryte_datasheet_20180919020633.pdf>`_ 中了解。

如下所示为使用定时器0的0通道，第一个参数是定时器编号，第二个参数为定时器的通道编号。下面的代码的意思是使用定时器0的通道0来定时计数

.. code-block:: bash

                timer=machine.timer(machine.timer.TIMER0,machine.timer.CHANEEL0)

初始化定时器，第一个参数 **freq** 为1秒内中断的次数，第二个为定时器的周期 **period** ，第三个为定时器的分频系数 **div** ，第四个为定时器的中断处理函数 **callback** 。

.. note::  定时器的周期period和分频系数div，目前该参数不建议设置，后期梦幻开发者们会加入该参数的使用说明

另外，定义 **中断处理函数** 时需要传入定时器作为参数，不然将无法执行。在使用该init后定时器将开始运行。下面的语句意思为每秒执行10次func函数

.. code-block:: bash

                def func(timer):
                    print("Hello world")

                timer.init(freq=10,period=0,div=0,callback=func)

or

.. code-block:: bash

                def func(timer):
                    print("Hello world")

                timer.init(10,0,0,func)

初始化定时器之后立刻执行，每秒会打印10次的 **Hello world**

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/timer.jpg
  :width: 500px
  :align: center
			
如果需要设置定时器参数则可以使用以下方法，因为定时器执行后芯片不断进入中断，这个时候shell不能输入，建议各位小伙伴们写成脚本。

.. code-block:: bash

                def func1(timer):
                        prrint("This is a timer")

                timer.callback(func1)

设置定时器中断频率，如下所示，将timer的中断频率设置为50次每秒，这个值请尽量不要太大，有可能会出现错误。

.. code-block:: bash

                timer.freq(50)

设置定时器周期，如下所示，将timer的定时器周期设置为10000个计数。

.. code-block:: bash

                timer.period(10000)
                

获取定时器当前计数值。

.. code-block:: bash

                timer.value()

停止定时器。

.. code-block:: bash

                timer.stop()
				
开始定时器。显而易见，是在停止定时器后想要开启定时器时使用

.. code-block:: bash

                timer.start()


重新开启定时器。

.. code-block:: bash

                timer.restar()

PWM
----

``PWM`` 就是脉冲宽度调制。我们的呼吸灯用的就是pwm波的原理。它可以设置引脚输出的占空比宽度，该功能需要用到定时器，请尽量不要在该模块下用到正在使用的定时器通道。比如上面我们已经用了定时器0的通道0，那么我们在使用pwm的时候就不要在去使用定时器0的通道0了。

老规矩，接的在创建pwm对象之前，先将外部引脚映射为pwm输出。这里笔者不再放出代码，各位用户和小白请自行参考之前的pwm脚本，其中 ``fpioa.TIMER1_TOGGLE1`` 指的就是该pwm使用定时器1的第一个通道

创建PWM对象，第一个参数为使用的定时器，第二个参数为使用的定时器通道，第三个参数为pwm频率，第四个为pwm占空比，第五个为输出外部引脚。

下面的语句表示为该PWM使用定时器0的0通道作为输出，其频率为2000000，占空比为90%，输出到板子上的绿色LED灯

创建pwm对象后，pwm自动运行

.. code-block:: bash

                pwm=machine.pwm(machine.pwm.TIMER0,machine.pwm.CHANEEL0,2000000,90,board_info.LED_G)

如果想要将pwm变更为其他设置，也可以使用init方法来初始化pwm，第1个参数为 **pwm频率** ，第2个为 **pwm占空比** ，第3个为 **输出外部引脚** 。

.. code-block:: bash

                pwm.init(3000000,30,board_info.LED_G)

.. note:: 设置pwm频率。pwm的频率太低时灯会闪，请用户和小白们根据自身情况设置恰当的频率


.. code-block:: bash

                pwm.freq(4000000)

设置pwm占空比，如下所示为设置占空比为80%。

.. code-block:: bash

                pwm.duty(80)

在下面2张图可以很明显地看到绿色灯的亮度是不一样的
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/duty%3D0.jpg
  :width: 500px
  :align: center
  
  duty=0
  
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/duty%3D50.jpg
  :width: 500px
  :align: center
  
  duty=50
	
Ov2640
------

``OV2640`` 模块主要用于驱动Sipeed M1平台的OV2640摄像头。

创建ov2640对象，当然在创建对象之前也需要初始化外部引脚，但固件已经在开机时映射进行引脚映射，这里我们值需要进行对象的操作即可。

第一步当然就是创建我们的ov2640对象

.. code-block:: bash

                ov2640=machine.ov2640()

创建完兑现过之后我们需要初始化ov2640，在初始化之前，请确认摄像头已经安装在Sipeed M1上。MaxiPy的驱动将初始化ov2640为320*240分辨率，对应于默认的lcd分辨率大小。

.. code-block:: bash

                ov2640.init()


获取摄像头图像，在获取摄像头图像之前需要创建缓冲区来存放获取到的图像数据。

.. code-block:: bash

                image=bytearray(320*240*2)
                ov2640.get_image(image)

使用这个方法后，在image中就存放这我们的图像数据了，但这个时候我们还不能看到图像长什么样。所以接下里就需要使用到LCD显示屏了
				
St7789
--------

``st7789`` 模块主要用于驱动Sipeed M1平台的st7789显示屏，它的分辨率为320*240。

我们还是先从创建st7789对象开始，同理，引脚映射已经在开机时完成。

.. code-block:: bash

                st7789=machine.st7789()

老套路，创建完后初始化st7789。

.. code-block:: bash

                st7789.init()

初始化完后屏幕会被刷成纯蓝色。这个时候我们就可以对它进行画图了。

下面的方法就是按照默认的分辨率320*240进行画图，default的意思就是使用默认分辨率，参数是320*240*2字节大小的图像数据，类型为bytearray(不懂的小白可以去百度以下该类型)。

.. code-block:: bash

                st7789.draw_picture_default(buf)

我们在上面获取到的图像数据就可以作为参数传递进来，然后再稍加一点语句就可以进行显示了

.. code-block:: bash 

                image=bytearray(320*240*2)
                while(1):
                        ov2640.get_image(image)
                        lcd.draw_picture_default(image)
                        
除了默认分辨率，我们还可以指定其他的参数来使用st7789进行画图，第一个参数为为开始画图的 **x坐标** ，第二个参数为为开始画图的 **y坐标** 。就是从那里开始画图。第三个参数为 **图像的宽度** ，第四个参数为 **图像的高度** ，意思就是图片的宽度跟高度，第五个参数是 **图像数据缓冲** ，类型为bytearray。

.. code-block:: bash

                st7789.draw_picture(0,0,320,240,buf)

在MaixPy中，还提供了对LCD屏幕进行刷屏的方法哟，使用以下语句

.. code-block:: bash

                st7789.clear()
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/lcd_clear.jpg
  :width: 500px
  :align: center

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/lcdclear.jpg
  :width: 500px
  :align: center
				
				
使用st7789进行画字符串，第一个参数为开始画字符串的 **x坐标** ，第二个参数为开始画字符串的 **y坐标** ，第三个参数为 **字符串** 。笔者相信这个方法大家都可以理解它的含义的。

.. code-block:: bash

                st7789.draw_string(0,0,"hello world")
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/lcd_str.jpg
  :width: 500px
  :align: center

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/lcdstr.jpg
  :width: 500px
  :align: center
  
Ws2812
------

``ws2812`` 是一种集成了电流控制芯片的低功耗RGB三色灯，手头上有这个外设的小伙伴们可以拿出来试一试。下面就让我们再做一次点灯工程师吧。

创建ws2812对象

.. code-block:: bash

                ws2812=machine.ws2812()

初始化ws2812。

ws2812需要使用GPIOHS来进行数据通信，所以在使用ws2812前，我们需要将GPIOHS映射到引脚，如下所示，将20号引脚映射到GPIOHS20。

ws2812初始化的第一个参数是使用的 **GPIOHS号** ，第二参数为使用的 **外部引脚** 。同样的，需要将功能映射到外部引脚

.. code-block:: bash

                fpioa=machine.fpioa()
                fpioa.set_function(board_info.GPIO9,fpioa.GPIOHS9)
                ws2812.init(board_info.GPIO9,fpioa.GPIOHS9)

ws2812点亮单独一个灯。

参数分别为R、G、B分量，每个分量最大值为255。这里设置为绿色

.. code-block:: bash
        
                ws2812.set_RGB(0,255,0)
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/ws2812.jpg
  :width: 500px
  :align: center
  
当然，ws2812也可以同时点亮多个灯，与set_RGB相似，多了最后一个参数，这个参数 **亮灯的数量** 。

.. code-block:: bash

                ws2812.set_RGB_num(0,255,0,4)

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/machine/ws2812_num.jpg
  :width: 500px
  :align: center
				
Spiflsah
--------

``spiflsah`` Sipeed M1/MW 上的nor flash可以用来存储我们的数据，它采用的是SPI协议来通信。 MaixPy开放了spiflash的操作接口，让我们可以对开发板上的nor flash进行操作，如读、写、擦除等，不再需要写复杂的C语言代码来操作。

创建spiflash对象。与其他外设不同的是，SPIflash不需要使用映射功能到外部引脚，直接创建并初始化即可。

.. code-block:: bash

                spiflash=machine.spiflash()     

初始化flash。

.. code-block:: bash

                spiflash.init()

初始化完之后我们就可以直接读取flash，第一个参数flash的 **读取地址** ，第二个参数为 **数据存放缓冲** ，类型为bytearray

如下所示，先创建一个存放读取数据的缓冲区，然后使用read方法将读取的数据存放于buf中。下面语句的意思是从0x100000这个地址开始读取数据到buf中。你也许会奇怪不需要让函数知道读取的大小吗？这个其实已经在声明buf的时候做了，read方法会自动读取buf的大小并读取相应的数据到buf中

.. code-block:: bash

                buf=bytearray(320)
                spiflash.read(0x100000,buf)

写入flash，第一个参数flash的 **写入地址** ，第二个参数为 **写入数据缓冲** 。

如下所示，先创建一个存放写入数据的缓冲区，然后使用write方法将buf中的数据写入flash中。同read方法一样，这里不多做解释

.. code-block:: bash

                buf=bytearray(320)
				... #buf操作
                spiflash.write(0x100000,buf)

除了读取flash，MaixPy还提供了擦除flash的方法，参数为 **擦写地址** ，每次擦写按照4k来擦写。下面语句的意思是从0x100000这个地址开始擦写4k的数据

.. code-block:: bash

                spiflash.erase(0x100000)

对于MaixPy的machine模块我们就介绍到这里了，对于machine模块有任何的疑问或者建议，记得到我们的官方论坛提出您的问题和宝贵的建议

`bbs传送门 <http://bbs.lichee.pro/t/sipeed-m1>`_

