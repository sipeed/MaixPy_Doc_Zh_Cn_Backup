MaixPy 全流程指南
=================================================

MaixPy是基于Sipeed M1平台适配的开源MicroPython项目，支持Sipeed M1上的多种外设，旨在让编程更加简单，本章节将详细介绍MaixPy接口的使用方法

.. figure:: http://pgeza64pd.bkt.clouddn.com/dan-board.jpg
  :width: 500px
  :align: center
  
  荔枝丹 - Lichee Dan
 

关于Sipeed M1的资料请移步http://dan.lichee.pro/index.html了解

关于MaixPy的介绍和下载请移步https://github.com/sipeed/MaixPy阅读README.md了解，本文所述将假设您已经阅读了github上面的sdk介绍和Lichee Dan的wiki

如果关于MaixPy有任何疑问和建议，请移步http://bbs.lichee.pro/t/sipeed-m1发帖建议和讨论

MaixPy教程
我们假设你已经有了一定的python知识基础，但我们仍然会尽量解释好MaixPy接口的细节。MaixPy的API将会持续更新，请关注我们的github或者该教程以获取最新的API介绍.
本教程MaixPy主要介绍Sipeed M1平台相关的模块machine和app，其中machine存放了大量的外设，app存放了Sipeed M1的应用，这2个模块在开机时已经自动导入
本教程的目录架构分为以下章节

开机启动
module
	fpioa
	GPIO&&Pin
	Uart
	Timer
	PWM
	OV2640
	ST7789
	zmodem
	spiflsah
	ws2812
uos
	spiffs
app
	vi

K210虽然拥有8M的内存资源，但在使用MaixPy的过程中请尽量避免使用大内存对象，如大数据量的队列或者字典等等

您可能需要这些来进一步了解荔枝丹： `kendryte210_datasheet  <http://pgeza64pd.bkt.clouddn.com/kendryte_datasheet_20180919020633.pdf>`_ | `荔枝丹原理图 <http://pgeza64pd.bkt.clouddn.com/K210_dock.pdf>`_

荔枝丹仍在不断地成长，对于外观、电路设计、文档内容甚至于荔枝丹的发展方向，我们欢迎您到 `荔枝丹 | 建议与讨论区 <http://bbs.lichee.pro/t/dan>`_ 提出您宝贵的建议。
 
同时欢迎各位加入 `荔枝派交流群826307240 <https://jq.qq.com/?_wv=1027&k=5uWO21P>`_ | `荔枝派Telegram电报群 <https://t.me/sipeed>`_ 与众多开发者、爱好者即时交流。

.. figure:: http://odfef978i.bkt.clouddn.com/QQ_Group_2.jpg
   :width: 250px
   :align: center

.. toctree::
   :maxdepth: 2
   :caption: 上手体验篇
	 
	 事前准备 <before_experience/uartCH340_src>
   裸机体验 <experience/standalone_src>
   FreeRTOS <experience/freeRTOS_src>
   
   

.. toctree::
   :maxdepth: 2
   :caption: MicroPython

   x'x'x <MicroPython/xxx文件名>
