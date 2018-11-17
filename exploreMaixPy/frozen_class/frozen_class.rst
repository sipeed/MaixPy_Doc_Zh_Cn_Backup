内置类
=========

在前面中我们介绍了一些模块和类型的使用方法，但是有些类型使用起来太不方便了，需要我们去考虑很多东西。所以我们事先编写好了一些简单的脚本来方便大家使用MaixPy。来让我们看看
这些类都有些什么作用吧

.. contents:: 本文目录

board_info
--------------

首先是 ``board_info`` 类。顾名思义，就是我们开发板上的信息了，它简单地描述了开发板上的电器连接，它帮助
我们来快速了解板子的信息，当我们不知道芯片的引脚与哪个班上外设连接的时候，我们可以使用该类查看

board_info主要是在board.py中实现，其实它在开机启动时已经导入并定义，我们只要直接使用就行了

我们属于以下的代码，并按下 **Tab键** 进行代码补全就可以看到了，其中诸如 ``LED_B``、 ``PIN10`` 等都是芯片引脚对引连接的外设。其中有几个引脚如 ``PIN9`` 和 ``PIN10`` 等表示的是自由引脚，没有与板上外设连接

.. code-block:: bash

				board_info.
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/frozen_class/board/board_info.jpg
  :width: 500px
  :align: center

我们来看看这些成员是什么，属于下面的代码可以看到返回的是一个数字。所以能够看得出，它们只是一种简称，更加方便我们理解而已，各位顾客佬爷可以自由的使用它们。
	
.. code-block:: bash

				board_info.PIN9
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/frozen_class/board/pin9.jpg
  :width: 500px
  :align: center

``board_info`` 还有另外一个简单的功能，那就是查看这些与板子连接外设对应的引脚。输入下面的代码后就能够见到一个列表，左边是引脚号，右边的就是对应的板上外设。这里笔者截取一部分显示。

.. code-block:: bash

				board_info.pin_map()
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/frozen_class/board/pin_map.jpg
  :width: 500px
  :align: center

和pfioa一样，如果你只是想要知道某个引脚对应的板上外设。那可以给这个方法加一个参数，该参数表示的是引脚号，可以从打印看到，我们的12号引脚连接到我们的绿色LED灯了

.. code-block:: bash

				board_info.pin_map(12)
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/frozen_class/board/pinmap().jpg
  :width: 500px
  :align: center
 

fpioa_manager
---------------
 
接下来是 ``fpioa_manager`` 类。如果各位顾客佬爷们使用得fpioa多了，相信会遇到一种情况，我不知道某个fpioa是否已经被使用了或者某个引脚是否已经被使用了。不要慌， ``fpioa_manager`` 就是为了这种情况而生了。它简单的维护了2张字典来帮助我们管理功能映射。

fpioa_manager在启动的时候已经定义了，名字为 ``fm`` 。让我们看看它的作用吧，照例输入下面的代码并按下 **Tab键** 。可以看到有一个成员就是 ``fpioa`` 

其实fpioa_manager就是对fpioa的一层封装。

.. code-block:: bash

				fm.
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/frozen_class/fm/fm.jpg
  :width: 500px
  :align: center

那么我们就来说一下 ``fm`` 的使用方法。fpioa管理器需要board_info来配合使用。我们先看看它的registered方法，就是将功能和引脚注册到我们的fpioa管理器中并进行映射。下面语句，其实就是将板子的绿色LED灯引脚映射到GPIO0并注册到fm中。设置成功会返回1并打印Log

.. code-block:: bash

				fm.registered(board_info.LED_B,fm.fpioa.GPIO0)
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/frozen_class/fm/fmreg.jpg
  :width: 500px
  :align: center

那么让我们看看注册了后再去注册的话会发生什么呢，可以看看我们打印的Log，发现主要功能或者引脚被使用了都返回0并且打印出来

.. code-block:: bash

				fm.registered(board_info.LED_B,fm.fpioa.GPIO0)
	
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/frozen_class/fm/fmregall.jpg
  :width: 500px
  :align: center
  
  打印功能和引脚

.. code-block:: bash

				fm.registered(board_info.LED_R,fm.fpioa.GPIO0)

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/frozen_class/fm/fmpin.jpg
  :width: 500px
  :align: center
  
  只打印引脚

.. code-block:: bash

				fm.registered(board_info.LED_B,fm.fpioa.GPIO1)

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/frozen_class/fm/fmgpio.jpg
  :width: 500px
  :align: center
  
  只打印功能

当然有注册就有注销，我们可以使用下面语句来注销，如果成功会返回1并打印注销的功能和引脚

.. code-block:: bash

				fm.unregistered(board_info.LED_B,fm.fpioa.GPIO0)

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/frozen_class/fm/fmunreg.jpg
  :width: 500px
  :align: center

				
我们再使用一次这个语句，就会返回0表示注销失败。所以注销没有注册过的功能和引脚会返回0，表示失败

.. code-block:: bash

				fm.unregistered(board_info.LED_B,fm.fpioa.GPIO0)
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/frozen_class/fm/unregfialed.jpg
  :width: 500px
  :align: center


