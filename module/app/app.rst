app模块介绍
===================================

``app`` 模块包含了MaixPy上的应用软件，能够帮助用户更好的操作Sipeed M1。

vi
---

``vi`` 是一款终端编辑软件，可以通过终端对开发板中的文件进行编辑。

.. code-block:: code

                import app
                app.vi(fileName)

有了vi，我们能够直接在Sipeed M1开发板上直接编写脚本执行，如下所示。

.. code-block:: code
                
                app.vi(“/test.py”)
                ....#编辑test.py
                import test
