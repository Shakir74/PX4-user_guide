# CentOS 上的开发环境

开发环境编译工作需要 Python 2.7.5 的支持，因此本文使用 CentOS 7 操作系统。 We hope to provide fully tested instructions with the supported toolchain in the near future. （如使用更早版本的 CentOS 则需要额外安装 python v2.7.5）。

The build requires Python 2.7.5. Therefore as of this writing Centos 7 should be used. (For earlier Centos releases a side-by-side install of python v2.7.5 may be done. But it is not recommended because it can break yum.)

## 通用依赖项

The EPEL repositories are required for openocd libftdi-devel libftdi-python

```sh
wget https://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm
sudo yum install epel-release-7-5.noarch.rpm
yum update
yum groupinstall “Development Tools”
yum install python-setuptools python-numpy
easy_install pyserial
easy_install pexpect
easy_install toml
easy_install pyyaml
easy_install cerberus
yum install openocd libftdi-devel libftdi-python python-argparse flex bison-devel ncurses-devel ncurses-libs autoconf texinfo libtool zlib-devel cmake vim-common
```

:::note
You may want to also install `python-pip` and `screen`.
:::

## GCC 工具链安装
<!-- GCC toolchain documentation used for all Linux platforms to build NuttX -->

Execute the script below to install GCC 7-2017-q4:

```sh
pushd .
cd ~
wget https://armkeil.blob.core.windows.net/developer/Files/downloads/gnu-rm/7-2017q4/gcc-arm-none-eabi-7-2017-q4-major-linux.tar.bz2
tar -jxf gcc-arm-none-eabi-7-2017-q4-major-linux.tar.bz2
exportline="export PATH=$HOME/gcc-arm-none-eabi-7-2017-q4-major/bin:\$PATH"
if grep -Fxq "$exportline" ~/.profile; then echo nothing to do ; else echo $exportline >> ~/.profile; fi
popd
```

{% include "_ninja_build_system.md" %}


**Troubleshooting**

Check the version by entering the following command:

```sh
arm-none-eabi-gcc --version
```

The output should be something similar to:

```sh
arm-none-eabi-gcc (GNU Tools for Arm Embedded Processors 7-2017-q4-major) 7.2.1 20170904 (release) [ARM/embedded-7-branch revision 255204]
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

<!-- import docs ninja build system -->
## Ninja Build System

[Ninja](https://ninja-build.org/) is a faster build system than *Make* and the PX4 *CMake* generators support it.

On Ubuntu Linux you can install this automatically from normal repos.

```sh
sudo apt-get install ninja-build -y
```

Other systems may not include Ninja in the package manager. In this case an alternative is to download the binary and add it to your path:

```sh
mkdir -p $HOME/ninja
cd $HOME/ninja
wget https://github.com/martine/ninja/releases/download/v1.6.0/ninja-linux.zip
unzip ninja-linux.zip
rm ninja-linux.zip
exportline="export PATH=$HOME/ninja:\$PATH"
if grep -Fxq "$exportline" ~/.profile; then echo nothing to do ; else echo $exportline >> ~/.profile; fi
. ~/.profile
```

