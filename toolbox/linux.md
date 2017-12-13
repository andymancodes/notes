### 控制tensorflow显存占用

```python
config = tf.ConfigProto() config.gpu_options.allow_growth=True
tf.Session(config=config)
```

### PyCharm中运行tensorflow找不到CUDA

在环境变量中加上 LD\_LIBRARY\_PATH=/usr/local/cuda/lib64;

### MD5 Checksum

​(mac) `​md5 big_file_name`​

(linux) `md5sum big_file_name`​


### 升级相关版本
```sh
sudo apt-get update && sudo apt-get upgrade
conda update --all --no-channel-priority
conda update conda
conda update python
conda update numpy
pip install xpinyin —-upgrade
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple keras --upgrade --no-deps
pip install tensorflow-gpu==0.12
```
清华源：
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --set show_channel_urls yes
```

### 数代码行数

`​cloc .`​ （[更多文档](https://github.com/AlDanial/cloc)）

### Anaconda提示PermissionError(13, 'Permission denied')

`sudo chown -R user anaconda3` where user is the username, and anaconda3 is the folder where anaconda has been installed

### 安装Keras和常用配套的包

先装python和tensorflow
```sh
sudo apt install graphviz
pip install pydot pydot-ng
sudo apt install libhdf5-serial-dev
pip install h5py
pip install keras
conda update libgcc  # (libstdc++.so.6: version 'CXXABI_1.3.8' not found)
```

### 安装octave-cli
```sh
sudo apt-add-repository ppa:octave/stable
sudo apt-get update
sudo apt-get install octave
```

### 安装CUDA

1. 从[这里](https://developer.nvidia.com/cuda-downloads)下载CUDA；检查md5sum
1. 使用纯命令行登录
1. 清除以往安装：`sudo apt-get purge nvidia-*`
1. 创建文件`/etc/modprobe.d/blacklist-nouveau.conf`，内容：
```
blacklist nouveau
options nouveau modeset=0
```
5.
```sh
sudo apt update && sudo apt-get upgrade -y && sudo apt-get dist-upgrade -y
sudo apt install build-essential -y
sudo apt install linux-source -y # 这两步可能可以跳过
apt source linux-image-$(uname -r) # 这两步可能可以跳过
sudo apt install linux-headers-$(uname -r) -y
sudo service lightdm stop
sudo update-initramfs -u
```
6. 启动安装：
```sh
sudo apt install nvidia-375
sudo sh cuda_8.0.61_375.26_linux.run --override --no-opengl-lib
```
第一步是单独安装显卡驱动，375是和后面文件名里的375对应的，酌情调整；
安装CUDA的时候除了opengl、sample和第二个xconfig（这个是自带的驱动，会出问题），其他都选y

7. 如果之前清除过以往安装或者上一步结束完nvidia-smi没有正确反馈，运行以下命令并重启：
sudo apt-get install nvidia-375 --reinstall
8. 验证安装成功：
```sh
nvcc -V
nvidia-smi
```

### 配置SSH
```sh
sudo apt-get update
sudo apt-get install ssh openssh-server
sudo ufw allow 22
```
### 安装libstdc++
```sh
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install build-essential
sudo apt-get install aptitude
sudo apt-get install libstdc++6
```

### 安装新版GCC
```sh
sudo apt-get update && \
sudo apt-get install build-essential software-properties-common -y && \
sudo add-apt-repository ppa:ubuntu-toolchain-r/test -y && \
sudo apt-get update && \
sudo apt-get install gcc-snapshot -y && \
sudo apt-get update && \
sudo apt-get install gcc-6 g++-6 -y && \
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-6 60 --slave /usr/bin/g++ g++ /usr/bin/g++-6 && \
sudo apt-get install gcc-4.8 g++-4.8 -y && \
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 60 --slave /usr/bin/g++ g++ /usr/bin/g++-4.8;
```
完成后使用如下命令切换gcc版本：
```sh
sudo update-alternatives --config gcc
```
检查当前版本：
```sh
gcc -v
```

### 安装CuDNN

在安装完CUDA之后，注册NVIDIA developer账号，然后下载需要的CuDNN版本，上传到服务器上。
```sh
sudo dpkg -i libcudnn5_5.1.5-1+cuda8.0_amd64.deb
sudo dpkg -i libcudnn5-dev_5.1.5-1+cuda8.0_amd64.deb
```
检查是否有CUDA_HOME、LD_LIBRARY_PATH等变量。如果没有，将如下内容加入.bashrc：
```sh
export CUDA_HOME=/usr/local/cuda
export CUDA_ROOT=/usr/local/cuda
export PATH=$PATH:$CUDA_ROOT/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CUDA_ROOT/lib64
```

### 安装Boost

从SourceForge上下载boost_1_64_0.tar.bz2，然后
```sh
sudo ./bootstrap.sh --prefix=/usr/local
sudo ./b2 install
```
测试安装成功：
```cpp
// first.cpp
#include<iostream>
#include<boost/any.hpp>

int main()
{
boost::any a(5);
a = 7.67;
std::cout<<boost::any_cast<double>(a)<<std::endl;
}

// second.cpp
#include<iostream>
#include<boost/filesystem/operations.hpp>

namespace bfs=boost::filesystem;
int main()
{
bfs::path p("second.cpp");
if(bfs::exists(p))
std::cout<<p.leaf()<<std::endl;
}
```
```sh
g++ -o first first.cpp
g++ -o second second.cpp -lboost_filesystem -lfile_system
```
### 安装ZLIB
```sh
sudo apt-get install zlib1g-dev
```
### 安装Eigen 3
```sh
sudo apt-get install libeigen3-dev
```
### 安装KenLM
编译源码的方式：首先安装Boost、ZLIB、Eigen3，然后从[这里](http://kheafield.com/code/kenlm.tar.gz)下载稳定版KenLM，解压进入文件夹后：
```
mkdir -p build
cd build
cmake ..
make -j 4
```
使用docker:
```sh
docker pull utkarshp/kenlm
```
安装python wrapper（wrapper和预测代码不需要完整C++环境，可以放在docker之外）
```sh
conda install gcc
pip install https://github.com/kpu/kenlm/archive/master.zip
```
训练模型 （语料需要分过词，用空格隔开）
```sh
cat corpus.txt | ./kenlm/bin/lmplz -o 3 > corpus.arpa
./kenlm/bin/build_binary corpus.arpa corpus.klm
```
计算分数
```python
import kenlm
model = kenlm.LanguageModel('corpus.klm')
model.score('in the beginning was the word')
```
### Create a bootable USB stick on macOS
[tutorial](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-macos#0)

### 命令行的中文支持

如果无法显示中文文件名，也无法键入中文，可以先用`locale -a`查看是否有中文语言包，如果没有，使用
```sh
sudo apt-get install language-pack-zh-han*
```
安装中文语言包。编辑/etc/default/locale：
```
LANG="zh_CN.UTF-8"
LANGUAGE="zh_CN:zh:en_US:en"
```
并source这个文件。重新登录终端即可。

### 服务器上正常使用matplotlib
由于服务器上没有窗口，需要特别配置matplotlib：
在python中执行
```python
import matplotlib
print(matplotlib.matplotlib_fname()
```
​得到配置文件的位置，打开配置文件，将backend配置为backend: Agg
​​即可正常使用。注意此时show()无法调用，但savefig()可以正常使用。

### 远程访问notebook
```python
from notebook.auth import passwd
passwd()
```
输入并确认密码（记住输入的密码，这个会是登陆密码）。返回SHA1加密结果，将其存下来。​
打开配置文件（如果不在以下地址可以用jupyter --paths找到地址）：
```sh
vi ~/.jupyter/jupyter_notebook_config.py
```
在其中输入：
```
c.NotebookApp.ip='*'
c.NotebookApp.password = u'sha1:86346e4cdf7a:c57960216df752e8ee5d3b9b8de6941640e15273' # SHA1
c.NotebookApp.open_browser = False
c.NotebookApp.port=8888
```
启动notebook后即可在公网IP:8888访问。如果是ubuntu desktop，需要在本机开启防火墙。在应用商店安装gufw软件可以可视化配置防火墙。
### 配置notebook开机启动：
- crontab -e
- @reboot cd ~/nbs; source ~/.bashrc;  ~/anaconda2/bin/jupyter notebook

### 设置jupyter notebook的主题
[参考文档](https://github.com/dunovank/jupyter-themes)
```sh
pip install --upgrade -i https://pypi.tuna.tsinghua.edu.cn/simple jupyterthemes
jt -t grade3 -T -N -cellw 100%
```
### jupyter notebook 扩展插件
```sh
pip install jupyter_contrib_nbextensions
jupyter contrib nbextension install --user
```
### Theano提示ERROR (theano.sandbox.cuda): nvcc compiler not found on $PATH.

打开~/.theanorc，加入：
```
[cuda]
root = /usr/local/cuda-8.0
```
其中cuda版本号要根据具体情况调整。随后重启Theano即可。运行​nvcc -V​可以看到正常返回结果。