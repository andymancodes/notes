### Mac突然黑屏按键无法唤醒

移开Mac附近带磁性的东西，尤其是带磁扣的电脑包...参考[豆瓣讨论](https://www.douban.com/group/topic/42531618/)。

### Mac触摸板突然按不下去

关机，然后同时按住Command+alt+r+p+启动键，然后等很久再松开，就好了。参考[知乎答案](https://www.zhihu.com/question/22396705)。

### 设置Word从指定页码开始编页 [参考文档](https://support.office.com/zh-cn/article/%E5%BC%80%E5%A7%8B%E9%A1%B5%E7%A0%81%E7%BC%96%E5%8F%B7%E5%9C%A8-Word-%E4%B8%AD%E6%96%87%E6%A1%A3%E7%9A%84%E5%90%8E%E9%9D%A2-for-Mac-678ab67a-d593-4a47-ae35-8ffed9573132?ui=zh-CN&rs=zh-CN&ad=CN)

1. 光标放在实际上的第一页开头，Insert-Break-Section Break(Next Page)，此时出现一个空白页
2. 双击空白页底部，进入编辑页脚的模式：![](resources/word.jpg)
3. 关闭编辑页脚的模式
4. 光标放在实际上的第一页开头，从后往前删掉这个空白页。

### Brew速度慢
[中科大源](https://mirrors.ustc.edu.cn) [教程](https://lug.ustc.edu.cn/wiki/mirrors/help/brew.git)

[清华源](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/)（或者直接用默认源开翻墙）

coding.net源：
```sh
cd "$(brew --repo)" && git remote set-url origin https://git.coding.net/homebrew/homebrew.git
cd $home && brew update
```

```sh
brew install cloc
brew upgrade mongodb
brew remove cloc
```

### 系统路径（MAC）

```sh
sudo chmod +w /etc/bashrc
sudo vi /etc/bashrc
```

按`i`键进入编辑状态，在末尾加入（下面是一个例子，注意根目录后面不要再加“／”）

```sh
export ANT_HOME=/usr/local/apache/ant/1.9.7
export PATH=${PATH}:${ANT_HOME}/bin
```

按`Esc`退出编辑状态，输入`:wq!`保存并退出，最后输入

```sh
source /etc/bashrc
```

备注：

1. Mac下配置用户系统路径的文件在`​~/.bash_profile`​ （Linux下在~/.bashrc）
2. Mac下Java 8的自动安装地址需要设置JAVA\_Home到：

  /Library/Java/JavaVirtualMachines/jdk1.8.0\_[版本号].jdk/Contents/Home

### 安装字体/查看现有字体

将字体文件拖动到`/Library/Fonts/`


### PyCharm中运行jupyter报错no such kernel named python3

```python
conda update nb_conda nb_conda_kernels nb_anacondacloud
```

### 安装Spark & PySpark

在确保安装了Java和Scala后，

```sh
brew install apache-spark
```

在Jupyter notebook环境里运行PySpark:

```sh
export PYSPARK_DRIVER_PYTHON=jupyter
export PYSPARK_DRIVER_PYTHON_OPTS=notebook
pyspark
```

此时在notebook中已经自动具有SparkContext，变量名为`spark`。

### 安装cx\_Oracle

登陆oracle官网，从[该页面](http://www.oracle.com/technetwork/topics/intel-macsoft-096467.html)下载Mac版本的[instantclient-basic](http://download.oracle.com/otn/mac/instantclient/121020/instantclient-basic-macos.x64-12.1.0.2.0.zip)、 [instantclient-sdk](http://download.oracle.com/otn/mac/instantclient/121020/instantclient-sdk-macos.x64-12.1.0.2.0.zip)。

建立文件夹/usr/runqi/oracle，解压文件，将两个都叫instantclient\_12\_1的文件夹下面的内容拷贝到该文件夹。再原地解压.../oracle/sdk/ottclasses.zip。在oracle文件夹下创造两个超链接：

```sh
ln -s libclntsh.dylib.12.1 libclntsh.dylib
ln -s libocci.dylib.12.1 libocci.dylib
```

创建系统变量并source：

```sh
export ORACLE_HOME=/usr/runqi/oracle
export DYLD_LIBRARY_PATH=$ORACLE_HOME
export LD_LIBRARY_PATH=$ORACLE_HOME
```

检查以下两个都有返回值：

```sh
echo $ORACLE_HOME
sudo env | grep ORACLE_HOME
```

如果后一个没有返回值，输入sudo visudo，加入这么一行再做检查：

```sh
Defaults env_keep += "ORACLE_HOME"
```

最后用pip进行安装：

```sh
env ARCHFLAGS="-arch x86_64" pip install cx_Oracle
```

### 安装Java大家族

Java, Scala, Groovy… <http://sdkman.io/>

### Matplotlib不能正常显示中文

```python
from matplotlib.font_manager import FontProperties
myfont = FontProperties(fname="/Library/Fonts/华文黑体.ttf")
sns.set(font=myfont.get_name()) # 纯matplotlib加入前两行就行
```

### Alfred使用指南 [这里](https://www.alfredapp.com/)

### 解压带中文的、windows过来的zip文件出现文件名乱码

App store下载The Unarchiver，并在文件信息里设置为解压.zip文件的默认程序。

### Jupyter Notebook／IPyton提示“ValueError: unknown locale: UTF-8”

打开Terminal，进入Preferences：

![](resources/mac1.png)

将图示的选项取消勾选。编辑.bash\_profile文件，加入：`​`

export LANG=en\_US.UTF-8`​`​

回到命令行，执行`source ~/.bash_profile`。重启Jupyter Notebook即可。

### matlab 2015及以前的版本mex无法识别新版Xcode 7:

<http://www.mathworks.com/matlabcentral/answers/246507-why-can-t-mex-find-a-supported-compiler-in-matlab-r2015b-after-i-upgraded-to-xcode-7-0>

### 配置git全局忽略文件：

创建`​.gitignore_global`​ （我放在了GithubProjects文件夹下）

```
# Compliled source
*.class
*.dll
*.exe
*.o
*.so

#Packages
*.7z
*.dmg
*.gz
*.iso
*.rar
*.tar
*.zip
*.jar

# OS generated files
.DS_Store
.DS_Store?
.Trashes
.Spotlight-V100
ehthumbs.db
Thumbs.db
```

使用命令：

```sh
git config --global core.excludesfile ./.gitignore_global
```

### 更改xcode版本

1. Log in to <https://developer.apple.com/downloads/>
2. Download CLT (Command Line Tools) for XCode 7.3
3. Install CLT
4. Run `sudo xcode-select --switch /Library/Developer/CommandLineTools`
5. Verify that clang has been downgraded via `clang —version`

### 安装TensorFlow

(python 3.5, CPU only)

```sh
$ export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-0.11.0rc0-py3-none-any.whl
sudo pip install --ignore-installed --upgrade $TF_BINARY_URL
```

注意`--ignore-installed`一定要加上（默认教程里没有）。安装文件地址可以再去官网看最新版。

### 查看Java安装路径

```sh
/usr/libexec/java_home -v 1.8
/usr/libexec/java_home -v 1.7
```

### 解决gephi 0.8.2的兼容性

参见[这里](https://medium.com/coder-snorts/gephi-is-broken-on-mac-os-97fbaef4305e#.1r8imyolg)。

### 清除文件夹下所有.DS\_Store

```sh
find ./ -name ".DS_Store" -depth -exec rm {} \;
```
