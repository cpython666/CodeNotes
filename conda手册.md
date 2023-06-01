## 安装

1. 在**清华源**上下载对应服务器版本的Miniconda3，在此下载的是Linux的最新版本的Miniconda3.

下载conda安装包

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

2. 安装软件：安装过程中根据提示输入`enter`或`yes`，过程中可以选择安装目录（需要是一个不存在的目录）

```bash
bash Miniconda3-latest-Linux-x86_64.sh
```

3. 配置环境变量(修改为你的安装目录),重新加载环境变量

```bash
echo 'export PATH="/data1/anaconda3/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

4. 初始化命令行(安装时没有init的话)

```bash
conda init bash
```



4. 验证是否安装成功

```python
conda -V
```

## 配置

查看当前conda配置

```python
conda config --show channels
```

添加镜像(执行这些操作会在用户目录下新建一个**.condarc**文件)

```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch
```

设置搜索时显示通道地址

```bash
conda config --set show_channel_urls yes
```

执行以下命令清除索引缓存，保证用的是镜像站提供的索引

```python
conda clean -i
```

配置完毕，执行以下命令查看是否已经换源，可以看到已经更换为清华源了

```
conda config --show
```

## conda使用

### 查看Anaconda中当前存在的环境

```
conda info -e
```

### 创建一个python2.7版本环境

```
conda create -n python27 python=2.7
```

```
conda create -n web python=3.8
```



### 激活（切换）环境

```
conda activate pyadmin
conda deactivate
```

### 删除虚拟环境

```
conda remove -n test --all
```

### 复制虚拟环境

```
conda create -n new --clone old
```

### 查看当前环境安装的包

```
conda list 
```

### 安装需要的包

切换到使用的环境后，安装第三方库

①安装：conda install [包名]

```
conda install tensorflow-gpu # 安装TensorFlow
conda install numpy==1.18.5 # 指定安装版本号
```

②删除：conda uninstall [包名]

```
conda uninstall numpy
```


③更新：conda update [包名]

```
conda update numpy
```