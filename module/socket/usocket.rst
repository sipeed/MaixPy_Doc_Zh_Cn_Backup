usocket模块介绍
===================================

.. contents:: 本文目录

``usocket`` 模块包含了socket网络操作。

连接wifi。

.. code-block:: bash

                wifi=machine.esp8285()
				wifi.init(ssid,password)# 连接wifi。
				
连接wifi。

.. code-block:: bash 

                socket=usocket.esp8285()
				addr_info = socket.getaddrinfo("sipeed.com", 80)
				socket.connect(addr_info)
				socket.send(sent_buf)
				socket.recv(bufsize)

