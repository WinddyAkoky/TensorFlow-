

# 安装GPU加速的tensorflow
## 关于驱动那点事：
安装之前要先换驱动，如果你之前换过，那可以跳过这步直接进入主题。
方法如下：
**打开system settings --> software & Updates --> Additional Drivers**
然后选择你需要的显卡驱动，这个系统会帮你搜寻适合你的显卡驱动，所以别点错就是了。

如果还不清楚，或者不行，可参考这两个地址：

> http://blog.csdn.net/tianrolin/article/details/52830422
> http://blog.csdn.net/u012581999/article/details/52433609


在正式进入安装之前，请先把cuda和cudnn对应的版本下载好：
本教程给的例子是 ： **Ubuntu16.04 + cuda8.0 + cudnn6.0 + tensorflow1.1.0**

    链接: https://pan.baidu.com/s/1kVVN9uJ 密码: bftd

## 安装cuda
下载包,注意**cuda**的版本，例子是**cuda8.0**的版本

     1. sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64.deb
     2. sudo apt-key add /var/cuda-repo-8-0-local-ga2/7fa2af80.pub  #路径名中cuda-repo-8-0-local-ga2根据cuda的版本变化而变化
     3. sudo apt-get update
     4. sudo apt-get install cuda

**验证是否安装完成**
`cd  /usr/local/cuda-8.0/samples/1_Utilities/deviceQuery`
(或者手动进入改文件夹，注意，同样路径中cuda-8.0根据cuda的版本变化而变化)
    

     sudo  make （报错可以不管）
    
     ./deviceQuery
     如果显示的是关于GPU的信息，则说明安装成功了。
## 安装cudnn
同样注意安装版本，例子是**cudnn 6.0**
先解压：

$ tar zxvf cudnn-8.0-linux-x64-v6.0.tgz 

解压后有个cuda文件，内有include和lib64两个文件夹，进入include文件夹，执行如下命令：
   

     $ sudo cp cudnn.h /usr/local/cuda/include/     #复制头文件  

（或者直接执行 sudo cp cuda/include/cudnn.h /usr/local/cuda/include ）
再cd命令切换进lib64文件夹，执行如下命令：
  

      $ sudo cp lib* /usr/local/cuda/lib64/    #复制动态链接库  

（或者直接执行 sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64 ）
然后进入复制后的动态链接库进行新的链接：


    $ cd /usr/local/cuda/lib64/        
    $ sudo rm -rf libcudnn.so libcudnn.so.5    #删除原有动态文件  
    $ sudo ln -s libcudnn.so.5.0.5 libcudnn.so.5  
    $ sudo ln -s libcudnn.so.5 libcudnn.so  

然后设置环境变量和动态链接库：

    $ sudo gedit /etc/profile  

然后再打开的文件末尾加上（“=”前后不要有空格）

    export PATH=/usr/local/cuda/bin:$PATH  

保存之后创建链接文件（如果你没有vim, 可以用gedit命令）：

    $ sudo vim /etc/ld.so.conf.d/cuda-8-0.conf   #注意这个cuda.conf文件会因为cuda版本不一样名称不一样,最后手动进入看一看 

按下键盘i进行编辑：
在文件末尾加上下列命令：

    /usr/local/cuda/lib64  

按下esc，按下：wq保存退出。并在终端输入以下命令使该链接生效：

    $ sudo ldconfig  

## 安装TensorFlow
先给正确的安装命令：
官网命令：

>     sudo pip install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-1.1.0-cp27-none-linux_x86_64.whl

### 注意事项：
如果直接运行官网给的代码，网速可能很慢，毕竟是外国的网站。所以，我们不从官网下，去清华大学开源软件镜像站下载tensorflow.方法如下：
1. 把**https://storage.googleapis.com/** 替换为 **https://mirrors.tuna.tsinghua.edu.cn/** 即可访问清华大学开源软件镜像站。
2. 根据你想要的TensorFlow的版本，修改**tensorflow-1.1.0-cp27-none-linux_x86_64.whl**
比如，我要TensorFlow-1.4.1版本，那么上面官网地址就修改为：

>     sudo pip install --upgrade https://mirrors.tuna.tsinghua.edu.cn/tensorflow/linux/cpu/tensorflow-1.4.1-cp27-none-linux_x86_64.whl

如果你用python3.5, 那么在tensorflow-1.4.1后面把**cp27**该为**cp35**,下载命令就变为：

>     sudo pip install --upgrade https://mirrors.tuna.tsinghua.edu.cn/tensorflow/linux/cpu/tensorflow-1.4.1-cp35-none-linux_x86_64.whl

3. 如果你不是Linux系统，那么请参考链接：

> https://mirrors.tuna.tsinghua.edu.cn/help/tensorflow/

依照上面的指示选择适合你的下载命令。

我自己是用tensorflow1.1.0, python2.7, ubuntu16.04, 所以我的下载命令是：

    

    sudo pip install --upgrade https://mirrors.tuna.tsinghua.edu.cn/tensorflow/linux/cpu/tensorflow-1.1.0-cp27-none-linux_x86_64.whl



## 测试
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

## 可能会用到的操作
### gcc版本降级
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

### 查看Ubuntu 16.04的Kernel，GCC和GLIBC的版本信息。
#### Kernel: 
$ uname -sr #或
$ uname -r
#### GCC
$ gcc -v #或
$ gcc --version
#### GLIBC
$ ldd --version


# 参考网址

> http://blog.csdn.net/tianrolin/article/details/52830422
> http://blog.csdn.net/u012581999/article/details/52433609
> https://www.cnblogs.com/mydebug/p/4972276.html
> http://wiki.jikexueyuan.com/project/tensorflow-zh/get_started/os_setup.html
> https://www.tensorflow.org/versions/r0.11/get_started/os_setup.html#optional-install-cuda-gpus-on-linux
> https://zhuanlan.zhihu.com/p/33307112
> http://blog.csdn.net/chcoolbeeboy/article/details/78110914
> https://zhuanlan.zhihu.com/p/33307112
> https://www.cnblogs.com/wangduo/p/7383989.html
> https://www.tensorflow.org/install/#optional-install-cuda-gpus-on-linux
> http://blog.csdn.net/s2010241013/article/details/55656248
> https://www.cnblogs.com/apak/p/8410618.html
> https://www.tensorflow.org/install/install_linux
> https://mirrors.tuna.tsinghua.edu.cn/help/tensorflow/
> http://blog.csdn.net/u014516389/article/details/72818155


