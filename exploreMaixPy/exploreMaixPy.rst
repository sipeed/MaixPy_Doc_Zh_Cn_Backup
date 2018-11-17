探索MaixPy
===============

在进行上手教程之后，相信各位顾客佬爷已经对MaixPy有了一定的认识。当然，仅仅是上手教程是不满足于各位欲求不满的顾客佬爷的，所以。接下来我们要进入的，就是MaixPy的探索。
让我们看看MaxiPy到底为我们准备了些什么东西吧

模块
-----------

首先我们来说说模块吧。MaxiPy按照Python的定义来说，一共有4个模块(module)，每个模块下又下分为类型(type)，每个类型负责到板子上的具体功能，比如machine模块下的GPIO类型负责的就是我们Sipeed M1/MW的GPIO功能。下面我就为大家一一说明介绍一下。

- machine模块：主要用于驱动板子和芯片相关的硬件功能，包括板载硬件如摄像头，LCD等，同时也包括了芯片的芯片外设功能，如GPIO、PWM等。

- uos模块：目前主要的功能是用于对文件系统进行操作，如读、写、删除等

- app模块：主要用于平台的各项应用，目前只有vi编辑器。当然，后续梦幻开发者们会逐渐加入其他功能

- usocket模块：主要用于网络功能，目前仅支持TCP协议，后续会加入其他的协议及功能

.. toctree::
   :maxdepth: 2
  
   machine模块 <machine/machine.rst>
   uos模块 <uos/uos.rst>
   app模块 <app/app.rst>
   usocket模块 <socket/usocket.rst>
   
   
内置python类
----------------

内置的python类是对一些复杂度比较高的模块进行一层封装，从而简化接口和使用方法。这些类都是事先编译好，内置在固件中的，不存于文件系统中。下面我们看看有那些类吧，对于类不熟悉的小伙伴们也可以去查查面向对象OOP编程。

- fpioa_manager：主要用于管理fpioa映射，让用户更加方便使用fpioa功能

- board_info：主要用于存放开发板的电气连接信息

- pwmc：pwm控制器主要是对pwm类型的封装，目的是让用户更加方便的使用pwm

.. toctree::
   :maxdepth: 2
   
   内置类<frozen_class/frozen_class.rst>