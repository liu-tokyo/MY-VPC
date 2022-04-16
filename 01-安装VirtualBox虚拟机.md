# 安装 VirtualBox 虚拟机

## 目录

- [安装 VirtualBox 虚拟机](#安装-virtualbox-虚拟机)
  - [目录](#目录)
  - [1.Windows下面安装](#1windows下面安装)
    - [1.1 使用 `winget` 进行安装](#11-使用-winget-进行安装)
    - [1.2 官方下载安装](#12-官方下载安装)
  - [2.Linux环境安装](#2linux环境安装)
  - [3. 安装客户机驱动](#3-安装客户机驱动)
  - [4. 退出全屏的快捷键](#4-退出全屏的快捷键)

---

## 1.Windows下面安装

### 1.1 使用 `winget` 进行安装

使用 `winget` 指令，一条指令就可以安装：

```bash
winget search virtualbox
```

下面是该指令执行的详细信息：

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

- 官方网址：https://www.virtualbox.org/

## 2.Linux环境安装

### 软件包安装

1. 官方网站下载相应的安装包：

   https://www.virtualbox.org/wiki/Linux_Downloads

2. 用软件安装器安装下载下来的软件包。

### 指令安装

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

选在菜单里面的 **VirtualBox Additions** ，如下是官方的使用说明：

- [Chapter 4. Guest Additions (virtualbox.org)](https://www.virtualbox.org/manual/ch04.html)

## 4. 退出全屏的快捷键

- 右边的CTCL +F 全屏或退出全屏
- 右Ctrl＋A 调整屏幕
- 右Ctrl+ C 开启/关闭 [Scale](https://so.csdn.net/so/search?q=Scale&spm=1001.2101.3001.7020) Mode（找回菜单栏）
