app模块介绍
===================================

``app`` 模块包含了MaixPy上的应用软件，能够帮助用户更好的操作Sipeed M1。app模块在启动的时候已经直接导入，小伙伴们可以会直接使用。虽然目前app的支持软件比较少，但后续会逐渐增加

vi
---

``vi`` 是一款终端编辑软件，可以通过终端对开发板中的文件进行编辑。在Linux是一款常用的编辑软件，关于vi如恶化操作小伙伴们可以从其他渠道了解哦~

我们直接使用下面的语句即可操作文件了，目前不建议在大文件上使用vi编辑器。

.. code-block:: bash

                app.vi(fileName)

更多情况下，我们是使用vi编辑器编写脚本来执行我们想要的功能，如下所示。

.. code-block:: bash 
                
                app.vi(“/test.py”)
                ....#编辑test.py
                import test

.. figure:: https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/image/module/app/vi.jpg
  :width: 500px
  :align: center