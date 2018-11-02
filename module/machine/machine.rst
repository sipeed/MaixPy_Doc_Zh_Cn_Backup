machine模块介绍
===================================

.. contents:: 本文目录

``machine`` 模块旨在让用户可以以脚本形式操作Sipeed M1开发版上的外设，以下分类对machine模块的具体功能进行介绍。

Fpioa
-----

``fpioa`` 主要用于映射k210芯片的功能到外部引脚，引脚功能映射表可以查看sdk中的 ``fpioa.h`` 查看。该模块只负责设置引脚功能，一般用于在外设初始化之前使用。

创建fpioa对象，不需要传递参数。

.. code-block:: bash 

                fpioa=machine.fpioa()

映射引脚功能，第一个参数是引脚号，第二个参数是片上外设功能号，如下所示是将18号引脚设置为GOIOHS0，24对应GOIOHS0功能。

.. code-block:: bash

                fpioa.set_function(18,24)  



GPIO&&Pin
------------

``GPIO`` 模块用于获取或者设置GPIO的值，使用GPIO前需要使用fpioa将其功能映射到外部引脚，不同与GPIOHS，k210芯片的GPIO只有8个，所以在参数传递时有所限制。

创建gpio对象，第一个采纳数是GPIO号，对应使用的GPIO，第二个是参数的GPIO模式，一共包括，输入，上拉输入，下拉输入和输出分别对应0-3，第三个参数为GPIO口的值，当且仅当模式
为输出时有效。

.. code-block:: bash

                gpio=machine.pin(0,0,0)

获取GPIO的值，当没有参数时，是直接获取GPIO的值，当传入参数时为设置GPIO口的值，当传入参数时为设置GPIO的值但无返回。

.. code-block:: bash

                value=gpio.value()  

or

.. code-block:: bash

                gpio.value(1)

还有一种设置GPIO值的方法，参数为GPIO口的值，作用是设置GPIO口的值并且返回GPIO口的当前值。如果不传入参数时，将会直接返回错误。

.. code-block:: bash

                value=gpio.toggle(1)

Timer
-------

``Timer`` 主要用于创建定时器并执行相应功能。

创建定时器，如下所示为使用定时器0的0通道，关于定时器的相关信息，请从k210的 `datasheet  <http://pgeza64pd.bkt.clouddn.com/kendryte_datasheet_20180919020633.pdf>`_ 中了解。

.. code-block:: bash

                timer=machine.timer(0,0)

初始化定时器，第一个参数freq为1秒内中断的次数，第二个为定时器的周期period，第三个为定时器的分频系数div，第四个为定时器的中断处理函数callback。

需要注意的是，中断处理函数定义是需要传入定时器作为参数，不然将无法执行.当freq和period同时设置，freq的优先级更加高。当div为0时使用默认的分频系数，在使用该方法后定时器将自动开始运行。

.. code-block:: bash

                def func(timer):
                        print(test)

                timer.init(10,0,0,func)

设置定时器的中断函数。

.. code-block:: bash

                def func1(timer):
                        prrint(test1)

                timer.callback(func1)

设置定时器周期，如下所示，将timer的定时器周期设置为10000个计数。

.. code-block:: bash

                timer.period(10000)
                
设置定时器中断频率，如下所示，将timer的中断频率设置为50次每秒，这个值请尽量不要太大，有可能会出现错误。

.. code-block:: bash

                timer.freq(50)

获取定时器当前计数值。

.. code-block:: bash

                timer.value()

开始定时器。

.. code-block:: bash

                timer.start()

停止定时器。

.. code-block:: bash

                timer.stop()

重新开启定时器。

.. code-block:: bash

                timer.restar()

PWM
----

``PWM`` 主要用于脉冲宽度调制，可以设置引脚输出的占空比宽度，该功能需要用到定时器，请尽量不要在该模块下用到正在使用的定时器通道。

在创建pwm对象之前，需要先将外部引脚映射为pwm输出，如下是将12号引脚映射为定时器0的第一个输出，MaixPy的启动已经默认将RGB灯的引脚映射到了定时器0的第一个到第三个
输出。

.. code-block:: bash

                fpioa=machin.fpioa()
                self.fpioa.set_function(12, 190)

创建PWM对象，第一个参数为使用的定时器，第二个参数为使用的定时器通道，第三个参数为pwm频率，第四个为pwm占空比，第五个为输出外部引脚。

下面的语句表示为该PWM使用定时器0的0通道作为输出，其频率为2000000，占空比为90%，输出引脚是12号引脚。

创建pwm对象后，pwm自动运行

.. code-block:: bash

                pwm=machine.pwm(0,0,2000000,90,12)

初始化pwm，第1个参数为pwm频率，第2个为pwm占空比，第3个为输出外部引脚。

.. code-block:: bash

                pwm.init(3000000,30,12)

设置pwm频率。

.. code-block:: bash

                pwm.freq(4000000)

设置pwm占空比，如下所示为设置占空比为80%。

.. code-block:: bash

                pwm.duty(80)

Ov2640
------
``OV2640`` 模块主要用于驱动Sipeed M1平台的OV2640摄像头。

创建ov2640对象，当然在创建对象之前也需要初始化外部引脚，但引脚映射已经在开机时映射，这里我们值需要进行对象的操作即可。

.. code-block:: bash

                ov2640=machine.ov2640()

初始化ov2640，在初始化之前，请确认摄像头已经安装在Sipeed M1上。如果检测不到摄像头将会进入检测死循环，MaxiPy的驱动将初始化ov2640为320*240分辨率，对应于默认的lcd分辨率大小。

.. code-block:: bash

                ov2640.init()


获取摄像头图像，在获取摄像头图像之前需要创建缓冲区来获取图像数据，获取图像之后可以配合lcd进行显示。

.. code-block:: bash

                image=bytearray(320*240*2)
                ov2640.get_image(image)

St7789
--------

``st7789`` 模块主要用于驱动Sipeed M1平台的st7789显示屏，分辨率为320*240。

创建st7789对象，同理，引脚映射已经在开机时完成。

.. code-block:: bash

                st7789=machine.st7789()

初始化st7789。

.. code-block:: bash

                st7789.init()

按照默认默认分辨率320*240进行画图，参数为320*240*2字节大小的图像数据。

.. code-block:: bash

                st7789.draw_picture_default(buf)

可以配合ov2640进行图像显示。

.. code-block:: bash 

                image=bytearray(320*240*2)
                while(1):
                        ov2640.get_image(image)
                        lcd.draw_picture_default(image)
                        
使用st7789进行画图，第一个参数为为开始画图的x坐标，第二个参数为为开始画图的y坐标，第三个参数为图像的宽度，第四个参数为图像的高度，第五个参数是图像数据缓冲区。

.. code-block:: bash

                st7789.draw_picture(0,0,320,240,buf)

使用st7789进行画字符串，第一个参数为开始画字符串的x坐标，第二个参数为开始画字符串的y坐标，第三个参数为字符串。

.. code-block:: bash

                st7789.draw_string(0,0,"hello world")

Ws2812
------

``ws2812`` 是一种集成了电流控制芯片的低功耗的RGB三色灯，下面就让我们做一次点灯工程师吧。

创建ws2812对象

.. code-block:: bash

                ws2812=machine.ws2812()

初始化ws2812。

ws2812需要使用GPIOHS来进行数据通信，所以在使用ws2812前，我们需要将GPIOHS映射到引脚，如下所示，将20号引脚映射到GPIOHS20。

ws2812初始化的第一个参数是使用的GPIOHS号，第二参数为使用的外部引脚。

.. code-block:: bash

                fpioa=machine.fpioa()
                fpioa.set_function(20,44)
                ws2812.init(20,44)

ws2812点亮单独一个灯。

参数分别为R、G、B分量，每个分量最大值为255。

.. code-block:: bash
        
                ws2812.set_RGB(255,255,255)

ws2812点亮多个灯。

与set_RGB相似，多了最后一个参数，这个参数亮灯的数量。

.. code-block:: bash

                ws2812.set_RGB_num(255,255,255,4)


Zmodem
------

``zmodem`` 是用于PC机和开发板进行文件传输的工具，可以通过使用rz函数来获取PC机上的文件，前提是终端软件支持zmodem协议，推荐使用xshell或者SRC。

通过使用以下命令来获取PC机文件。

.. code-block:: bash

                machine.zmodem.rz()

Spiflsah
--------

``spiflsah`` 用于对开发板子的nor flash进行直接操作，如读、写、擦除。

创建spiflash对象。

.. code-block:: bash

                spiflash=machine.spiflash()     

初始化flash。

.. code-block:: bash

                spiflash.init()

读取flash，第一个参数flash的读取地址，第二个参数为数据存放缓冲。

如下所示，先创建一个存放读取数据的缓冲区，然后使用read方法将读取的数据存放于buf中。

.. code-block:: bash

                buf=bytearray(320)
                spiflash.read(0x100000,buf)

写入flash，第一个参数flash的写入地址，第二个参数为写入数据缓冲。

如下所示，先创建一个存放写入数据的缓冲区，然后使用write方法将buf中的数据写入flash中。

.. code-block:: bash

                buf=bytearray(320)
                spiflash.write(0x100000,buf)

擦除flash，参数为擦写地址，每次擦写按照4k来擦写。

.. code-block:: bash

                spiflash.erase(0x100000)

