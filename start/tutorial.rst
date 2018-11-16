快速上手
^^^^^^^^^^^^

终于来到激动人心的环节啦！各位顾客佬爷相信手上都已经有了我们的Sipeed M1/MW，那么让我们立刻开始体验MaixPy吧。需要提醒以下，目前MaixPy运行在
裸机之上。

.. contents:: 本文目录

进入MaixPy
------------

通过上文，我们已经知道了如何烧录和连接Sipeed M1了。顾客佬爷们烧录完固件之后，打开终端软件连接Sipeed M1/MW，按下 **rst复位键** 重新启动MaiPy
即可查看到下面的启动log，到此我们已经进入MaiPy了。

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/tutorial/%E5%90%AF%E5%8A%A8log.jpg
  :width: 500px
  :align: center
  
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/tutorial/boot_lcd.jpg
  :width: 500px
  :align: center

  点击查看放大


点灯工程师
------------

串口是进入了，但是板子上只有一个指示灯亮了，总感觉少了点什么。那么接下来让我们做一名点灯工程师，一起点亮Sipeed M1/MW的第一个灯吧。Let's sipeed up!

首先让我们输入以下代码，这段代码的大概意思就是初始化引脚功能并返回一个引脚对象

.. code-block:: bash

	>>> fpioa=machine.fpioa()
	>>> fpioa.set_function(board_info.LED_G,fpioa.GPIO0)
	>>> pin=machine.pin(machine.pin.GPIO0,machine.pin.DM_OUTPUT,machine.pin.HIGH_LEVEL)

接着我们就可以直接使用一句话来点亮我们的灯啦，这句话就是拉低引脚电平从而点亮开发板的灯。输入之后你会发现板子上绿色的灯亮起来了

.. code-block:: bash

	>>> pin.value(pin.LOW_LEVEL)

如果要灭灯更加简单，使用即可

.. code-block:: bash

	>>> pin.value(pin.HIGH_LEVEL)
	
玩点高级的灯
--------------

好了，总算点亮第一个灯了。那么接下来让我们继续点灯，不过这次我们要点的是pwm灯。但是板子刚拿到手，啥都不会，怎么开始点pwm灯呢？不要慌，小M已经帮大家写了一个小脚本放在了我们的文件系统中了，使用 ``os.ls()`` 即可查看我们文件系统中存在的文件啦

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/tutorial/ls.jpg
  :width: 500px
  :align: center

我们来看一下这个脚本的代码。

.. code-block:: bash

	import machine
	import board
	board_info=board.board_info()
	flag=0
	duty = 0
	def func(timer):
	    global duty
	    global flag
	    if(flag == 0):
	    	duty = duty + 1
	    	if(duty > 100):
	    		flag = 1
	    if(flag == 1):
	    	duty = duty - 1
	    	if(duty < 1):
	    		flag=0
	    pwm.duty(duty)

	fpioa=machine.fpioa()
	fpioa.set_function(board_info.LED_B, fpioa.TIMER1_TOGGLE1)  
	pwm=machine.pwm(1,0,2000000,90,12)
	timer=machine.timer(0,0)
	timer.init(freq=100,period=0,div=0,callback=func)
		
好吧，不懂python的我已经头晕晕了，不过不要急，我们将在后面的教程中知道这些接口的意义

那么如何让呼吸灯亮起来呢？一步到位，只需要输入 ``import pwm`` ，你就会看到板子上面RGB灯中的蓝色灯已经作为呼吸灯亮起来，并且串口输出如下

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/tutorial/pwm.jpg
  :width: 500px
  :align: center

.. note:: 因为人眼对绿光比蓝光更加敏感，而pwm灯为蓝色光，建议先把绿色灯关掉再观看蓝色pwm灯
  
来自拍吧
------------

现在我们的pwm灯已经亮起来，那么接下来让我们来张自拍吧，就用顾客佬爷们手头上板子的lcd显示屏和摄像头。其实这一步在前面的测试模式已经可以验证了。但是这怎么能够让我们体验MaixPy呢，接下来让我们使用MaixPy来自拍吧。

在自拍之前，请各位顾客佬爷们按下 **rst复位键** 重启开发板。之前提到过，我们的MaixPy运行于裸机之上，我们有运行了pwm来做呼吸灯。pwm需要用到定时器，此刻我们的板子每秒都在进入中断，所以是无法进行其他复杂的IO工作的。

好了，听完笔者吧啦吧啦解释一通(hushuobadao)之后，各位顾客老爷应该按下了我们的复位键。然后我们可以输入以下代码，然后按下3到4次回车即可看到画面中已经出现您 **英俊帅气**/**沉鱼落雁** 的脸了。

.. code-block:: bash

	>>> camera=machine.ov2640();
	>>> camera.init();
	>>> lcd=machine.st7789();
	>>> lcd.init();
	>>> image=bytearray(320*240*2) ;
	>>> while(1):
	>>>     camera.get_image(image);
	>>>     lcd.draw_picture_default(image);
			

代码输入完之后将会出现以下的代码log

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/tutorial/photo.jpg
  :width: 500px
  :align: center

			
让我看到你
--------------

M1/MW打着AI的旗号，但是到目前为止我们都还不知道这块板子的AI体现在哪里。来吧，M1/MW开发板要拿出自己展示给各位顾客佬爷们的最后一招了-- **人脸识别** (虽然只是个demo，23333)。

如果您使用的是前文提到的固件，那么你现在是可以使用人脸识别demo的。来吧，让我们体验一下AI的魅力。老规矩，麻烦各位顾客佬爷按下复位键

人脸识别demo的代码仅仅只是比自拍多了2行，各位用户可以输入以下代码。代码运行之后，lcd屏幕中您的脸应该有一个红色框框在周围，这个框框还会一直跟着脸移动，说明人脸识别的demo已经运行起来啦。这样，我们的M1/MW开发板已经看到你啦。

各位顾客老爷们可能看起来简单易用，但这背后是用了芯片的KPU来进行运算的，一般的单片机无法直接CPU来提取人脸特征并识别的。

.. code-block:: bash

	>>> camera=machine.ov2640();
	>>>	camera.init();
	>>> lcd=machine.st7789();
	>>> lcd.init();
	>>> demo=machine.demo_face_detect();
	>>> demo.init();
	>>> image=bytearray(320*240*2); 
	>>> while(1):
	>>>	    camera.get_image(image);
	>>>	    demo.process_image(image);
	>>>	    lcd.draw_picture_default(image);

照例来点代码log

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/tutorial/demo.jpg
  :width: 500px
  :align: center
				
结束语
-----------
到此为止，已经向各位顾客佬爷们展示了Sipeed M1/MW的快速上手教程了。

各位顾客佬爷们可以继续查看后面的MaixPy模块介绍以获取更多的信息。我们也会继续完善MaixPy，让MaixPy更加简单，更加易用。如果您有任何关于MaiPy有任何想法或建议，都可以到 `MaixPy论坛 <http://bbs.lichee.pro/t/sipeed-m1>`_  发帖建议和讨论。

你甚至可以向为MaixPy贡献代码，这样我们更加欢迎。贡献代码并通过审核，还可以获得群主神秘优惠哟(我不会说是 **女装照** )。

接下来让我们进入其他章节吧





