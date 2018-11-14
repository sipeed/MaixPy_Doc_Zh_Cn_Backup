固件
============

.. contents:: 本文目录

固件下载
--------------
**固件下载链接** ----> `固件传送门  <https://fdvad021asfd8q.oss-cn-hangzhou.aliyuncs.com/Sipeed_M1/firmware/sipeedm1.kfpkg>`_ 

**固件发布链接** ----> `bbs传送门  <http://bbs.lichee.pro/d/170-maix-py-lichee-dan-sipeed-m1-mpy-bin>`_ 

点击固件传送门直接下载固件，该固件携带了人脸检测模型，512k的文件系统和MaixPy，直接使用 **kflash** 选中文件烧录即可

每个MaixPy版本的固件更新都会在bss上发布，更新内容及使用方法也会一并发布，请各位顾客佬爷们时刻关注我们的发布动态
 

固件类型
----------

bin格式
~~~~~~~~~
在使用k210官方的编译器在MaixPy目录下编译代码之后会生成.bin文件，该文件即是Sipeed M1平台下MaixPy的二进制文件，直接使用kflash烧录即可，关于kflash使用方法可查看
`SipeedM1 简易入门教程  <http://dan.lichee.pro/>`_ 

kfpkg格式
~~~~~~~~~~
kfpkg格式是k210官方的打包文件格式，可以将多个.bin如固件、模型和文件系统等打包为一个kfpkg格式的文件。我们发布的固件中有一些是使用kfpkg格式，一般都是带了模型或者文件系统的，各位MaxiPyer可以下载体验，目前kfpkg文件仅支持在window下使用Kflash烧录


固件烧录
----------
在windows下请使用 `kflash  <http://pgeza64pd.bkt.clouddn.com/K-Flash.exe>`_  烧录  ，在linux下请使用烧录脚本  `isp_auto.py <http://dan.lichee.pro/>`_  

详情请看 ---->  `烧录简易教程 <http://dan.lichee.pro/>`_


