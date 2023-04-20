# 安装 VirtualBox 虚拟机

## 简介

VirtualBox 是一款功能强大的 x86 和 AMD64/Intel64 虚拟化产品，适用于企业和家庭使用。VirtualBox 不仅是面向企业客户的功能极其丰富的高性能产品，也是唯一可作为开源软件免费提供的专业解决方案根据 GNU 通用公共许可证 (GPL) 第 2 版的条款。有关介绍，请参阅“关于 VirtualBox”。

目前，VirtualBox 可在 Windows、Linux、Macintosh 和 Solaris 主机上运行，并支持大量客户操作系统，包括但不限于 Windows（NT 4.0、2000、XP、Server 2003、Vista、Windows 7、Windows 8、Windows 10 )、DOS/Windows 3.x、Linux（2.4、2.6、3.x 和 4.x）、Solaris 和 OpenSolaris、OS/2 和 OpenBSD。

VirtualBox 正在积极开发并频繁发布，并且具有不断增长的功能列表、受支持的客户操作系统和运行它的平台。VirtualBox 是由一家专门公司支持的社区努力：鼓励每个人都做出贡献，而 Oracle 确保产品始终满足专业的质量标准。

对于没有经历VMware的选手，**强烈推荐使用VirtualBox**，不仅仅是免费的、个头小（少浪费硬盘空间）、并且感觉运行速度会快一点儿，我们能用的所用功能方面也没有差什么。  
我个人安装这个虚拟机软件的原因是，Debian在VMware下无法正确运行，据说是VMwareTools驱动对Debian支持不好，费尽心力，一直无法解决，所以所有Debian虚拟机就在VirtualBox下面安装，不但稳定，还明确感觉速度的改变，虽然可能是个人心理的问题。

- 最新版本已经更新到了 `VirtualBox 7.0.4 platform packages` 。

  画面最大化之后的控制条，也移转到了顶部中央，操作更加方便。曾经从6.1.3版本，该控制条被防止在画面下部中央，操作起来总觉得不是那么方便。

- 如果需要针对最新设备进行支持，需要安装 `VirtualBox 7.0.4 Oracle VM VirtualBox Extension Pack` 。

  否则无法把 U盘 从Host主机 转到 虚拟机。

## 1. Windows下面安装

### 1.1 使用 `winget` 进行安装

- 使用 `winget` 指令，一条指令就可以安装：

  ```bash
  winget search virtualbox
  ```

- 下面是该指令执行的详细信息：

  ```bash
  C:\Users\LIU VM>winget search virtualbox
  名称                 ID                版本   匹配            源
  --------------------------------------------------------------------
  Oracle VM VirtualBox Oracle.VirtualBox 6.1.32                 winget
  Eintopf              mazehall.eintopf  1.3.2  Tag: virtualbox winget
  
  C:\Users\LIU VM>winget install Oracle.VirtualBox
  已找到 Oracle VM VirtualBox [Oracle.VirtualBox] 版本 6.1.32
  此应用程序由其所有者授权给你。
  Microsoft 对第三方程序包概不负责，也不向第三方程序包授予任何许可证。
  Downloading https://download.virtualbox.org/virtualbox/6.1.32/VirtualBox-6.1.32-149290-Win.exe
    ██████████████████████████████   103 MB /  103 MB
  已成功验证安装程序哈希
  正在启动程序包安装...
  已成功安装
  
  ```

### 1.2 官方下载安装

- 官方网址：

  https://www.virtualbox.org/

## 2. Linux环境安装

- Ubuntu下面的安装：

  Ubuntu对普通用户支持最好，所以 **Ubuntu** 是最方便的，如下一条指令就能解决问题：
  
  ```shell
  sudo apt install virtualbox
  ```
  

### 2.1 软件包安装

1. 官方网站下载相应的安装包：

   https://www.virtualbox.org/wiki/Linux_Downloads

2. 用软件安装器安装下载下来的软件包，不做详细赘述。

### 2.2 指令安装

1. 参阅官方网站介绍：

   https://www.virtualbox.org/wiki/Downloads

2. 将以下行添加到您的 /etc/apt/sources.list。根据您的分配，将 '<mydist>' 替换为 'eoan'、'bionic'、'xenial'、'buster'、'stretch' 或 'jessie '（旧版本的 VirtualBox 支持不同的发行版）：

   ```shell
   deb [arch=amd64] https://download.virtualbox.org/virtualbox/debian <mydist> contrib
   ```

   还不如直接去下载更加方便。

3. 您可以添加这些键：

   ```shell
   sudo apt-key add oracle_vbox_2016.asc
   sudo apt-key add oracle_vbox.asc
   ```

   或结合下载和注册：

   ```shell
   wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
   wget -q https://www.virtualbox.org/download/oracle_vbox.asc -O- | sudo apt-key add -
   ```

   oracle_vbox_2016.asc 的密钥指纹是：

   ```shell
   B9F8 D658 297A F3EF C18D  5CDF A2F6 83C5 2980 AECF
   Oracle Corporation (VirtualBox archive signing key) <info@virtualbox.org>
   ```

   oracle_vbox.asc 的密钥指纹是：

   ```shell
   7B0F AB3A 13B9 0743 5925  D9C9 5442 2A4B 98AB 5139
   Oracle Corporation (VirtualBox archive signing key) <info@virtualbox.org>
   ```

   （从 VirtualBox 3.2 开始，签名密钥已更改。可以在此处下载 apt-secure 的旧 Sun 公钥。）

4. 要安装 VirtualBox，请执行

   ```shell
   sudo apt-get update
   sudo apt-get install virtualbox-6.1
   ```

   将 virtualbox-6.1 替换为 virtualbox-6.0 或 virtualbox-5.2 以安装最新的 VirtualBox 6.0 或 5.2 版本。

   遇到以下签名无效时该怎么办：BADSIG ...从存储库刷新包时？

   ```shell
   sudo -s -H
   apt-get clean
   rm /var/lib/apt/lists/*
   rm /var/lib/apt/lists/partial/*
   apt-get clean
   apt-get update
   ```


## 3. 安装客户机驱动

选择菜单里面的 **VirtualBox Additions** ，如下是官方的使用说明：

- [Chapter 4. Guest Additions (virtualbox.org)](https://www.virtualbox.org/manual/ch04.html)

需要注意的事项：

- 确保虚拟硬件里面有CD-ROM的项目。

- Windows下面基本可以做到自动安装，在出现提示的时候点击确认就可以了。

- Linux下面无法自动安装，使用终端指令进行安装。

  ```bash
  sudo bash ./VBoxLinuxAdditions.run
  ```

## 4. 退出全屏的快捷键

- 鼠标单击 `右边的CTCL` ，鼠标退出被虚拟机锁定
- `右CTCL +F` 全屏或退出全屏
- `右Ctrl＋A` 调整屏幕
- `右Ctrl+ C` 开启/关闭 [Scale](https://so.csdn.net/so/search?q=Scale&spm=1001.2101.3001.7020) Mode（找回菜单栏）

最新版本的VirtualBox在进入全屏幕之后，在画面的下部中央位置，会显示一个退出全画面的工具条，大大简化了初级选手不知道如何退出全屏幕的尴尬。  
该工具条的位置可以通过 VirtualBox 的主控画面的设定，改变其显示位置。



## 5. 宿主机文件共享

- **Linux**

  把当前用户追加到 vboxsf 中。

  ```bash
  sudo usermod -aG vboxsf $(whoami)
  ```

## 6. 进入BIOS画面

VirtualBox 没有 是 EFI 启动，没有进入 BIOS 的选项；但是，可以通过按下 F12 进入启动选择画面。

## 7. 安装Ubuntu显示不全问题

- 针对UBUNTU20.04和22.04在安装时显示不全的解决方案
  先按一下 **Alt+f7** 然后移动鼠标就可以调整位置了。注意是先按一下 **alt+f7** 之后窗口就会跟随鼠标移动，然后鼠标点击一下就定住位置了。

- 针对之前版本的解决方案

  在虚拟机窗口内的安装窗口，按住键盘 **Alt** 键，同时按住鼠标左键拖动，即可看到未显示完的区域。

  如无效可尝试，使用win+鼠标左键进行拖拉。

## 8. 安装增强功能之前的操作

- 执行如下指令：

  ```bash
  sudo apt-get update
  sudo apt-get upgrade
  sudo apt-get install build-essential gcc make perl dkms
  ```

  
