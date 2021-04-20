
# 已不维护（且本文不是最方便的方法）

这个是初学的时候的笔记，但是随着越来越熟悉环境，发现还有更加方便的安装方法。。。。但已经懒得写教程了

# 安装GPU加速的tensorflow 以及卸载tensorflow
## 卸载TensorFlow

    sudo pip uninstall tensorflow_gpu  
    sudo pip3 uninstall tensorflow_gpu
*用 pip 还是pip3基于你是用python2 还是用python3安装的tensorflow*

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
## 另一种安装cuda的方法： runfile文件

1. 下载好文件 xxxx.run
2. 执行下列命令

    sudo sh cuda_8.0.44_linux.run
***

**注意：运行后，会出现一份文件说明，按ctrl+c 直接阅读完毕，输入accept接收协议。
 接下来是一些交互界面： 第一个是问你要不要安装驱动，因为前面已经安装过，所以选择不安装。
 其他选择yes或者默认就行。**

***


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
注意：如果上面都一切正常，这可以继续染下配置环境，如果在上一步命令出现错误，也就是这句

    $ sudo cp lib* /usr/local/cuda/lib64/    #复制动态链接库 
***报错了，那我们等一切都安装结束后在配置环境变量。所以直接调到后面去安装tensorflow***

    > $ cd /usr/local/cuda/lib64/         $ sudo rm -rf libcudnn.so
    > libcudnn.so.5    #删除原有动态文件   $ sudo ln -s libcudnn.so.5.0.5
    > libcudnn.so.5   $ sudo ln -s libcudnn.so.5 libcudnn.so
    > sudo chmod a+r /usr/local/cuda/include/cudnn.h 
    > sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
***同样，如果上面后俩条命令出错，请直接现装tensorflow***

## 安装TensorFlow
先给正确的安装命令：
官网命令：

>     sudo pip install --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-1.4.0-cp27-none-linux_x86_64.whl

### 注意事项：
如果直接运行官网给的代码，网速可能很慢，毕竟是外国的网站。所以，我们不从官网下，去清华大学开源软件镜像站下载tensorflow.方法如下：
1. 把**https://storage.googleapis.com/** 替换为 **https://mirrors.tuna.tsinghua.edu.cn/** 即可访问清华大学开源软件镜像站。
2. 根据你想要的TensorFlow的版本，那么只需要修改**tensorflow-1.4.0-cp27-none-linux_x86_64.whl**
比如，我要TensorFlow-1.0.1版本，那么上面官网地址就修改为：

>     sudo pip install --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-1.0.1-cp27-none-linux_x86_64.whl

如果你用python3.5, 那么在tensorflow-1.0.1后面把**cp27**该为**cp35-cp35m**,下载命令就变为：

>     sudo pip install --upgrade https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-1.0.1-cp35-cp35m-none-linux_x86_64.whl

3. 如果改动不成功或者你不是Linux系统，那么请参考链接（以链接中的地址为主）：

> https://mirrors.tuna.tsinghua.edu.cn/help/tensorflow/

依照上面的指示选择适合你的下载命令。

我自己是用tensorflow1.0.1, python2.7, ubuntu16.04, 所以我的下载命令是：

    sudo pip install --upgrade https://mirrors.tuna.tsinghua.edu.cn/tensorflow/linux/gpu/tensorflow-1.0.1-cp27-none-linux_x86_64.whl

## 环境配置
打开终端，输入：

     sudo gedit ./.bashrc 
然后在文件末尾加入下面命令：

    export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
    export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
    export CUDA_HOME=/usr/local/cuda-8.0


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

## 错误解决方案
### 软链接出错
发现这一步出错的主要原因是您安装的cuda或cudnn版本引起的
可能会报如下错误：

> Couldn't open CUDA library libcudnn.so. LD_LIBRARY_PATH:

这说明找不到文件 libcudnn.so ，这个文件其实是个软链接来着，他指向另外一个软件。
***注意：这里有必要解释一下cuda和cudnn这两个文件。我们解压出来的lib64下面有3个so文件。分别是 libcudnn.so 和 libcudnn.so.5以及 libcudnn.so.5.1.12文件（当然，读者你的文件跟我不相同的概率很大，不过不要仅，下面会教你怎么修改软链接），并且这3个点so文件大小都一样。其实都是软连接！libcudnn.so链接到libcudnn.so.5，而libcudnn.so.5.又链接到libcudnn.so.5.1.12。 正真的文件只有libcudnn.so.5.1.12，因此我们要将/usr/local/lib64下的以前的这样的链接替换掉。由于装cuda时，比如我装的是cuda8.0，那么在/usr/local/下会生成cuda-8.0文件夹，以及一个cuda文件夹，cuda是软链接到cuda-8.0的，所以这两个文件夹可以看成一个。往任意一个文件夹中添加东西，另一个文件夹都会有相同的东西。***

    > cd /usr/local/cuda/lib64 
    > ll libcudnn*
出现：

    > lrwxrwxrwx 1 root root   13 3月   5 12:45 libcudnn.so -> libcudnn.so.5
    > lrwxrwxrwx 1 root root   18 3月   5 14:38 libcudnn.so.5 ->libcudnn.so.5.1.10
    > -rwxr-xr-x 1 root root  81M 3月   5 14:18 libcudnn.so.5.1.12
    > -rw-r--r-- 1 root root 138M 3月   5 14:28 libcudnn_static.a
从上面可以看出，`libcudnn.so` 文件最终指向`libcudnn.so.5.1.12`
所以，我们要删去原来的软链接，重写加上正确的软链接

    > sudo rm libcudnn.so.5 libcudnn.so.5.1.10 
    > sudo ln -s libcudnn.so.5.1.12 libcudnn.so.5
再次查看：

> `ll libcudnn*`

    lrwxrwxrwx 1 root root   13 3月   5 12:45 libcudnn.so -> libcudnn.so.5
    lrwxrwxrwx 1 root root   18 3月   5 14:38 libcudnn.so.5 -> libcudnn.so.5.1.10
    -rwxr-xr-x 1 root root  81M 3月   5 14:18 libcudnn.so.5.1.10


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
> http://blog.csdn.net/hungryof/article/details/52746279




