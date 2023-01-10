---
title: Linux下安装配置python3.x
zhihu-url: https://zhuanlan.zhihu.com/p/489262118
注意: 所有的冒号是半角冒号，冒号后面有一个半角空格
---

# Linux下安装配置python3.x

## 查看本机的python
```sh
python --version
```

Linux一般会自带`python2.x`

## 安装python3.x

### 下载python3.x

使用 wget 下载python3.x安装包

```sh
wget https://www.python.org/ftp/python/3.7.1/python-3.7.1rc2.tgz
```

这里下载的是`3.7.1`版本，如有需要可以自己更改版本

### 解压
创建`python3.x`的存放路径（自行更改）

```sh
mkdir /usr/local/python3
```

将刚刚下载的压缩吧移动到该目录下

```sh
mv python-3.7.1rc2.tgz /usr/local/python3
```

解压

```sh
cd /usr/local/python3
tar -zxf python-3.7.1rcz.tg2
cd ./python-3.7.1rc2
```

## 编译配置

```sh
./configure --with-ssl
make
make install
```

如果成功会显示如下

```text
Collecting setuptools
Collecting pip
Installing collected packages: setuptools, pip
Successfully installed pip-10.0.1 setuptools-39.0.1
```

### 配置时会发生的小错误
`configure: error: no acceptable C compiler found in $PATH`

该错误是因为本机缺少gcc编译环境，只需安装gcc即可

使用yum安装即可

```sh
yum install -y gcc
```

`zipimport.ZipImportError: can't decompress data; zlib not available`

该错误是因为本机缺少zlib解压缩类库，只需安装zlib即可

```sh
yum install -y zlib*
```

`ModuleNotFoundError: No module named '_ctypes'`

该错误是因为本机缺少libffi-devel包，只需安装此包即可

```sh
yum install -y libffi-devel
```

### 配置及建立软连接

将python库路径添加到/etc/ld.so.conf配置中

ld.so.conf文件是存储etc目录下所有的.conf文件

```sh
echo "/usr/python/lib" >> /etc/ld.so.conf
ldconfig
```

建立新的软连接至python3.x，如果自带python2.x原链接不用删除

```sh
ln -s /usr/python/bin/python3 /usr/bin/python3
ln -s /usr/python/bin/pip3 /usr/bin/pip3
```

如果报错显示文件已存在，就用`-sf`参数

## 检测

```sh
# 检测python版本
python --version

# 检测pip3
pip3 -V	# 显示版本
```