# TensorFlow-

-----------------------安装TensorFlow-----------------------
先给正确的安装命令：
官网命令：
sudo pip install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.1.0-cp27-none-linux_x86_64.whl

注意事项：
如果直接运行官网给的代码，网速可能很慢，毕竟是外国的网站。所以，我们不从官网下，去清华大学开源软件镜像站下载tensorflow.方法如下：
1：把https://storage.googleapis.com/ 替换为 https://mirrors.tuna.tsinghua.edu.cn/ 即可访问清华大学开源软件镜像站。
2：根据你想要的TensorFlow的版本，修改tensorflow-1.1.0-cp27-none-linux_x86_64.whl
比如，我要TensorFlow-1.4.1版本，那么上面官网地址就修改为：
sudo pip install --upgrade https://mirrors.tuna.tsinghua.edu.cn/tensorflow/linux/cpu/tensorflow-1.4.1-cp27-none-linux_x86_64.whl
如果你用python3.5, 那么在tensorflow-1.4.1后面把cp27该为cp35,下载命令就变为：
sudo pip install --upgrade https://mirrors.tuna.tsinghua.edu.cn/tensorflow/linux/cpu/tensorflow-1.4.1-cp35-none-linux_x86_64.whl
3：如果你不是Linux系统，那么请参考链接：
https://mirrors.tuna.tsinghua.edu.cn/help/tensorflow/
依照上面的指示选择适合你的下载命令。

我自己是用tensorflow1.1.0, python2.7, ubuntu16.04, 所以我的下载命令是：
sudo pip install --upgrade https://mirrors.tuna.tsinghua.edu.cn/tensorflow/linux/cpu/tensorflow-1.1.0-cp27-none-linux_x86_64.whl



-----------------------测试-------------------------
上面步骤都完成后进入测试：
>>> import tensorflow as tf
>>> hello = tf.constant('Hello, TensorFlow!')
>>> sess = tf.Session()
>>> print sess.run(hello)
Hello, TensorFlow!
>>> a = tf.constant(10)
>>> b = tf.constant(32)
>>> print sess.run(a+b)
42

------可能会用到的操作
------gcc版本降级
Ubuntu 16.04的gcc编译器是5.4.0，然而CUDA 8.0不支持5.0以上的编译器，因此需要降级，把编译器版本降到4.9。命令如下：
sudo apt-get install g++-4.9
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.9 20
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 10
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.9 20
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 10
sudo update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 30
sudo update-alternatives --set cc /usr/bin/gcc
sudo update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 30
sudo update-alternatives --set c++ /usr/bin/g++

-----查看Ubuntu 16.04的Kernel，GCC和GLIBC的版本信息。
# Kernel: 
$ uname -sr #或
$ uname -r
# GCC
$ gcc -v #或
$ gcc --version
# GLIBC
$ ldd --version


---------------------------参考网址--------------------
http://blog.csdn.net/tianrolin/article/details/52830422
http://blog.csdn.net/u012581999/article/details/52433609
https://www.cnblogs.com/mydebug/p/4972276.html
http://wiki.jikexueyuan.com/project/tensorflow-zh/get_started/os_setup.html
https://www.tensorflow.org/versions/r0.11/get_started/os_setup.html#optional-install-cuda-gpus-on-linux
https://zhuanlan.zhihu.com/p/33307112
http://blog.csdn.net/chcoolbeeboy/article/details/78110914
https://zhuanlan.zhihu.com/p/33307112
https://www.cnblogs.com/wangduo/p/7383989.html
https://www.tensorflow.org/install/#optional-install-cuda-gpus-on-linux
http://blog.csdn.net/s2010241013/article/details/55656248
https://www.cnblogs.com/apak/p/8410618.html
https://www.tensorflow.org/install/install_linux
https://mirrors.tuna.tsinghua.edu.cn/help/tensorflow/
http://blog.csdn.net/u014516389/article/details/72818155

---------------------------结束----------------------------------
