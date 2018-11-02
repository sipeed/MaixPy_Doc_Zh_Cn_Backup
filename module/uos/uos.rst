uos模块介绍
===================================

``uos`` 模块包含了文件系统操作和 ``urandom`` 函数。

在介绍了 ``spiflash`` 之后，我们不推荐对 ``spiflash`` 直接进行操作，我们将 ``spiffs`` 文件系统移植到了MaixPy,并将其功能函数放在了uos模块中，可以通过spiffs对开发板上的文件进行管理。

格式化spiffs。

.. code-block:: code

                uos.formatfs()

创建文件并写入数据。

第一个参数文件名，第二个参数问写入偏移量(注意是文件中的偏移量，不是flash中的偏移量)第三个参数为whence，第四个参数为写入数据缓存,类型为bytearray。

.. code-block:: bash

                buf=bytearray(320)
                ...  #buf操作
                uos.write("test",0,0,buf)

罗列文件,前者仅列出所有的文件名，后者则文件名与大小一起罗列出来，单位为字节。

.. code-block:: bash

                uos.ls()
                uos.listdir()   
				
读取数据。

第一个参数为文件名，第二个参数问读取偏移量，第三个参数为whence，第四个参数为读取数据缓存,类型为bytearray。

.. code-block:: bash

                buf=bytearray(320)
                uos.write("/test",0,0,buf)

移除文件，参数为文件名。

.. code-block:: bash

                uos.remove("/test")
				
重命名文件，第一个参数为源文件名，第二参数为新文件名。

.. code-block:: bash

                uos.rename("/test","new_test")