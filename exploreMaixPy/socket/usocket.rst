usocket模块介绍
===================================

.. contents:: 本文目录

``usocket`` 模块包含了socket网络操作，目前仅支持TCP协议

在使用socket之前，我们需要对wifi进行初始化，Sipeed的wifi采用了esp8285，在machine模块中我们可以看到有这个类型，不多说，直接上代码

.. code-block:: bash

                wifi=machine.esp8285()
				wifi.init("your_ssid","your_password")# 连接wifi。
				
.. note:: 其中your_ssid和your_password都是字符串,而且要根据各位的wifi名和密码来填写，别忘了修改

如果初始化没问题，那么这个时候就会出现以下的log并且连上了wifi路由器，如果出错则会出现相应的简单提示。关于esp8285的AT指令这里不做赘述，有兴趣的小伙伴请自行了解吧

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/socket/wifiinit.jpg
  :width: 500px
  :align: center
				
创建socket，如同在Linux下操作，我们需要创建一个socket来表示网络通信的抽象实例

.. code-block:: bash

				socket=usocket.socket()
				
获取地址，获取地址后会返回一个链表，但我们只需要链表中最后一个元素即可，这也是micropython中的常用写法

.. code-block:: bash 

				addr=usocket.getaddrinfo("api.seniverse.com",80)[0][-1]
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/socket/get_addr.jpg
  :width: 500px
  :align: center
	
获取地址后我们可以直接连接我们的地址了，使用下面的语句
	
.. code-block:: bash 
				
				socket.connect(addr)

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/socket/connect.jpg
  :width: 500px
  :align: center
				
好了，已经连接上了，那就让我们看看有些什么东西可以读写吧，我们前面的地址是一个天气服务的api，那么我们就来获取一下天气信息吧


.. code-block:: bash 
				
				socket.write("GET http://api.seniverse.com/v3/weather/now.json?key=x3owc7bndhbvi8oq&location=shenzhen&language=zh-Hans&unit=c\n\n")

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/socket/write.jpg
  :width: 500px
  :align: center
				
已经想天气服务发送了我们的url了，那么来看看它给我们返回了些什么吧，使用
				
.. code-block:: bash 
				
				size = 300
				socket.read(size)

可以看到，它给我们返回了天气信息了。
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/socket/read.jpg
  :width: 500px
  :align: center	
				
.. note:: 这里是read方法记得要填入接收大小size，不然该方法将无法返回。如果返回信息的数量大于size，那么只会返回size大小的数据，如果小于size，则只会返回接收数据大小的数据。比如接收数据只有260个，而size等于300，那么接收的数据只有260个
				

				
			
				
				
				