# 前言 \Preface/
#### 测试机型:联想 y430p 系统:ubuntu
#### \Test Machine: Lenovo Y430P, System: Ubuntu/
#### 这些东西源于我为我的电脑安装ubuntu系统时发现无法进行网络连接，当然我的电脑存在瑕疵，因为有线网卡也是坏的:(
#### \hese issues arose when I installed the Ubuntu system on my computer and found that I could not establish a network connection. Of course, my computer has its flaws, as the wired network card is also broken :(/
#### 最后想到了比较笨的方法，手机打开热点但是使用USB共享开打开网络进行。
#### \In the end, I thought of a rather clumsy method—using my phone to create a hotspot but enabling the network through USB sharing./
#### 相信看我这个教程（其实也说不上是教程）大都是想寻找一站式的离线补丁或者驱动，很抱歉泼了盆冷水。
#### \I believe those who read this guide, which is quite messy and may not even qualify as a good guide, are mostly looking for a one-stop offline patch or driver. I apologize for the cold water I'm about to pour./
#### 因为这在大多数情况下是不可能的
#### \Because this is impossible in most cases./
#### 但是您仍然可以尝试别的办法先尝试连接网络，也可以尝试使用免驱USB网卡（很奇怪为什么想联网的前提是先联网）
#### \However, you can still try other methods to first establish a network connection, or you can attempt to use a plug-and-play USB network adapter (it's quite paradoxical that the prerequisite for getting online is to be online first)./

# 目录_1
- 中文(13行)
- English(110row)


# 中文
# 安装内核头文件、编译工具和DKMS

在Linux系统中，为了能够编译和安装内核模块，需要安装内核头文件、编译工具以及动态内核模块支持（DKMS）。以下是具体步骤：

## 目录
- [步骤 1: 安装必要的软件包](#install-tools)
- [步骤 2: 处理可能的依赖问题](#fix-dependencies)
- [步骤 3: 更新软件包列表](#update-packages)
- [步骤 4: 安装内核源码](#install-source)
- [步骤 5: 重新安装无线网卡驱动](#reinstall-driver)
- [步骤 6: 加载无线网卡模块](#load-module)
- [步骤 7: 安装Broadcom STA DKMS包](#install-broadcom)

## 步骤 1: 安装必要的软件包 {#install-tools}

首先，确保安装了`linux-headers-generic`、`build-essential`和`dkms`，这些是编译内核模块所必需的基础软件包。

```bash
sudo apt-get install linux-headers-generic build-essential dkms
```
[如报错缺少依赖请看步骤 2]
### 解释
- `linux-headers-generic`: 提供了与当前内核版本匹配的头文件，用于编译内核模块。
- `build-essential`: 包含了一系列用于编译软件的基本工具，如GCC编译器、Make等。
- `dkms`: 动态内核模块支持，允许内核模块随着内核的更新自动重新编译和安装。

## 步骤 2: 处理可能的依赖问题 {#fix-dependencies}

如果上述命令执行过程中出现了依赖问题，可以通过以下命令来修复：

```bash
sudo apt --fix-broken install
```

### 解释
- `--fix-broken`: 该选项用于修复安装过程中出现的依赖关系问题。

## 步骤 3: 更新软件包列表 {#update-packages}

在进行下一步之前，更新本地软件包索引以确保获取最新的软件包信息：

```bash
sudo apt-get update
```

### 解释
- `update`: 命令会从配置的软件源下载最新的软件包列表，以保证安装的是最新版本的软件包。

## 步骤 4: 安装内核源码 {#install-source}

接下来，安装Linux内核的源代码，这对于某些特定的开发或调试任务非常有用：

```bash
sudo apt-get install linux-source
```

### 解释
- `linux-source`: 内核源代码包，包含了构建自定义内核所需的全部源代码。

## 步骤 5: 重新安装无线网卡驱动 {#reinstall-driver}

安装Broadcom无线网卡的专有驱动，即使提示“无法定位软件包”，也无需担心，因为这一步主要是为了确保系统中的驱动是最新的：

```bash
sudo apt-get install --reinstall bcmwl-kernel-source
```

### 解释
- `--reinstall`: 选项用于重新安装已存在的软件包，这里是为了确保无线网卡驱动是最新的。

## 步骤 6: 加载无线网卡模块 {#load-module}

最后，加载无线网卡的`wl`模块，并检查是否成功加载：

```bash
sudo modprobe wl
lsmod | grep wl
```

### 解释
- `modprobe wl`: 命令用于加载`wl`模块，这是Broadcom无线网卡的驱动模块。
- `lsmod | grep wl`: 检查`wl`模块是否已经成功加载到内核中。

## 步骤 7: 安装Broadcom STA DKMS包 {#install-broadcom}

通过安装`.deb`包来完成：

```bash
dpkg -i broadcom-sta-dkms_6.30.223.271-24_all.deb
```

### 解释
- `dpkg -i`: 用于安装本地的Debian软件包文件（`.deb`），这里指定了Broadcom STA DKMS包的具体路径和版本。



# English
---

# Installing Kernel Headers, Compilation Tools, and DKMS

In Linux systems, to compile and install kernel modules, it's necessary to have the kernel headers, compilation tools, and Dynamic Kernel Module Support (DKMS) installed. Below are the specific steps:

## Table of Contents
- [Step 1: Install Required Packages](#install-tools)
- [Step 2: Resolve Dependency Issues](#fix-dependencies)
- [Step 3: Update Package List](#update-packages)
- [Step 4: Install Kernel Source](#install-source)
- [Step 5: Reinstall Wireless Card Driver](#reinstall-driver)
- [Step 6: Load Wireless Card Module](#load-module)
- [Step 7: Install Broadcom STA DKMS Package](#install-broadcom)

## Step 1: Install Required Packages {#install-tools}

First, ensure that `linux-headers-generic`, `build-essential`, and `dkms` are installed, as these are fundamental packages required for compiling kernel modules.

```bash
sudo apt-get install linux-headers-generic build-essential dkms
```
[If dependency issues occur, see Step 2]
### Explanation
- `linux-headers-generic`: Provides header files matching the current kernel version, used for compiling kernel modules.
- `build-essential`: Includes a series of basic tools needed for compiling software, such as the GCC compiler, Make, etc.
- `dkms`: Dynamic Kernel Module Support allows kernel modules to be automatically recompiled and installed with kernel updates.

## Step 2: Resolve Dependency Issues {#fix-dependencies}

If dependency issues arise during the execution of the above command, they can be resolved using the following command:

```bash
sudo apt --fix-broken install
```

### Explanation
- `--fix-broken`: This option is used to repair dependency problems that occurred during installation.

## Step 3: Update Package List {#update-packages}

Before proceeding to the next step, update the local package index to ensure the latest package information is obtained:

```bash
sudo apt-get update
```

### Explanation
- `update`: This command downloads the latest package lists from configured sources, ensuring the installation of the most recent versions of packages.

## Step 4: Install Kernel Source {#install-source}

Next, install the source code for the Linux kernel, which is very useful for certain development or debugging tasks:

```bash
sudo apt-get install linux-source
```

### Explanation
- `linux-source`: The kernel source code package contains all the source code needed to build a custom kernel.

## Step 5: Reinstall Wireless Card Driver {#reinstall-driver}

Install the proprietary driver for Broadcom wireless cards, even if a "package not found" message appears, there's no need to worry, as this step mainly ensures that the system's drivers are up-to-date:

```bash
sudo apt-get install --reinstall bcmwl-kernel-source
```

### Explanation
- `--reinstall`: This option is used to reinstall an existing package, here to ensure the wireless card driver is the most recent.

## Step 6: Load Wireless Card Module {#load-module}

Finally, load the `wl` module for the wireless card and check if it was loaded successfully:

```bash
sudo modprobe wl
lsmod | grep wl
```

### Explanation
- `modprobe wl`: This command loads the `wl` module, which is the driver module for Broadcom wireless cards.
- `lsmod | grep wl`: Checks whether the `wl` module has been successfully loaded into the kernel.

## Step 7: Install Broadcom STA DKMS Package {#install-broadcom}

Complete the process by installing the `.deb` package:

```bash
dpkg -i broadcom-sta-dkms_6.30.223.271-24_all.deb
```

### Explanation
- `dpkg -i`: Used to install local Debian package files (`.deb`), specifying the path and version of the Broadcom STA DKMS package here.

---
