uos模块介绍
===================================

``uos`` 模块包含了文件系统操作。

前面我们介绍了 ``spiflash`` ，但我们不推荐用 ``spiflash`` 直接对flash进行操作，我们将 ``spiffs`` 文件系统移植到了MaixPy,并将其功能函数放在了uos模块中，可以通过spiffs对开发板上的文件进行管理。文件系统的大小在固件中设置为6M

格式化spiffs。相信格式化大家应该都很熟悉，是直接将文件系统的文件给清除掉

.. code-block:: bash 

                uos.formatfs()

罗列文件,前者仅列出所有的文件名，后者则文件名与大小一起罗列出来，单位为字节。

.. code-block:: bash

                uos.ls()
                uos.listdir()   

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/uos/ls.jpg
  :width: 500px
  :align: center				
				
创建文件并写入数据，有linux经验的朋友应该知道这个接口非常像linux的write函数。

第一个参数文件名，第二个参数问写入偏移量(注意是文件中的偏移量，不是flash中的偏移量)第三个参数为whence，第四个参数为写入数据缓存,类型为bytearray。

.. code-block:: bash

                buf=bytearray(320)
                ...  #buf操作
                uos.write("/test",0,0,buf)

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/uos/wiret.jpg
  :width: 500px
  :align: center				
				
读取数据。

第一个参数为文件名，第二个参数问读取偏移量，第三个参数为whence，第四个参数为读取数据缓存,类型为bytearray。

.. code-block:: bash

                buf_read=bytearray(320)
                uos.read("/test",0,0,buf_read)

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/uos/read.jpg
  :width: 500px
  :align: center	
				
.. note:: 读取跟写入2个方法的第二个跟第三个参数为0即可	
				

				
重命名文件，第一个参数为源文件名，第二参数为新文件名。

.. code-block:: bash

                uos.rename("/test","/new_test")
				
.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/uos/rename.jpg
  :width: 500px
  :align: center
				
移除文件，参数为文件名。如下所示

.. code-block:: bash

                uos.remove("/new_test")

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/uos/remove.jpg
  :width: 500px
  :align: center

				
.. note:: "/"非常重要，记得写文件名的时候记得加入	