### 安装Keras
在Ubuntu上安装keras[Ubuntu16验证]，过程如下：
#### 首先安装pip
```shell
$ apt-get update
$ apt-get upgrade
$ apt-get install python-pip python-dev
```
#### 安装Python科学套件
```shell
$ apt-get install build-essential cmake git unzip pkg-config libopenblas-dev liblapack-dev
$ apt-get install python-numpy python-scipy python-matplotlib python-yaml
$ apt-get install libhdf5-serial-dev python-h5py
$ apt-get install graphviz
$ pip install python-dg
```
#### 安装Tensorflow或者Theano
```shell
$ pip install tensorflow
```
#### 最后，安装keras
```shell
$ git clone https://github.com/keras-team/keras.git
$ cd keras
$ python setup.py install
```
如上，从git下载源码安装，也可以直接使用pip install keras方式安装；安装成功后，测试，运行如下案例：
```shell
$ python examples/mnist_cnn.py
```
该案例从aws上下载输入文件，之后运行测试模型；
### 安装Jupyter
一般使用Jupyter notebook编写测试，安装和运行过程如下：
```shell
$ python -m pip install --upgrade pip
$ python -m pip install jupyter
$ jupyter notebook --allow-root --ip=[your_ip]
```
Jupyter默认运行在8888端口，在浏览器输入http://you_ip:8888即可打开；
