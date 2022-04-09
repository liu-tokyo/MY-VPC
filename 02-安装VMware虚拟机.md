# 安装VMware虚拟机

## 目录

- [安装VMware虚拟机](#安装vmware虚拟机)
  - [目录](#目录)
  - [1.Windows环境](#1windows环境)
    - [1.1 使用 winget 安装](#11-使用-winget-安装)
    - [1.2 官方下载安装](#12-官方下载安装)
  - [2.Linux环境](#2linux环境)
    - [2.1 官方下载安装](#21-官方下载安装)
    - [2.2 指令直接安装](#22-指令直接安装)
  - [3.安装驱动](#3安装驱动)
    - [3.1 Windows虚拟机](#31-windows虚拟机)
    - [3.2 Linux虚拟机](#32-linux虚拟机)

---

## 1.Windows环境

### 1.1 使用 winget 安装

```bash
C:\Users\LIU VM>winget search vmware
名称                          ID                       版本        匹配        源
--------------------------------------------------------------------------------------
VMware Workspace ONE          9NBLGGH5PN6H             Unknown                 msstore
VMware Tunnel                 9NBLGGH4RPWR             Unknown                 msstore
VMware AirWatch® Agent™       9NBLGGH1JTHP             Unknown                 msstore
VMware Workstation            VMware.WorkstationPro    16.2.2      Tag: vmware winget
Workspace ONE Intelligent Hub VMware.IntelligentHub    21.7.8.0    Tag: vmware winget
VMware Horizon Client         VMware.HorizonClient     8.4.1.26410 Tag: vmware winget
RVTools                       Robware.RVTools          4.3.1       Tag: vmware winget
VMware Player                 VMware.WorkstationPlayer 16.2.2                  winget

C:\Users\LIU VM>winget install VMware.WorkstationPro
已找到 VMware Workstation [VMware.WorkstationPro] 版本 16.2.2
此应用程序由其所有者授权给你。
Microsoft 对第三方程序包概不负责，也不向第三方程序包授予任何许可证。
Downloading https://download3.vmware.com/software/wkst/file/VMware-workstation-full-16.2.2-19200509.exe
  ██████████████████████████████   615 MB /  615 MB
已成功验证安装程序哈希
正在启动程序包安装...
已成功安装
```

作为个人用户，你也可以安装 **VMware Player** ，这个是完全免费软件，可以运行虚拟机，但是不能创建虚拟机。

### 1.2 官方下载安装

官方下载网址：

- [VMware Workstation Pro Download](https://www.vmware.com/jp/products/workstation-pro/workstation-pro-evaluation.html)



## 2.Linux环境

### 2.1 官方下载安装

官方下载网址：

- [VMware Workstation Pro Download](https://www.vmware.com/jp/products/workstation-pro/workstation-pro-evaluation.html)

下载文件名称：

- VMware-Workstation-Full-16.2.3-19376536.x86_64.bundle



### 2.2 指令直接安装

```bash
sudo apt install build-essential
# sudo bash VMWare-Player-16.1.0-17198959.x86_64.bundle
sudo bash VMware-Workstation-Full-16.2.3-19376536.x86_64.bundle
# 下面的指令估计也一样能够安装。
sudo sh VMware-Player-16.1.1-17801498.x86_64.bundle
```

安装信息如下，顺利结束。

```bash
liu@liu-virtual-machine:~/下载$ sudo apt install build-essential
[sudo] liu 的密码： 
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
build-essential 已经是最新版 (12.8ubuntu1.1)。
下列软件包是自动安装的并且现在不需要了：
  libfwupdplugin1 linux-headers-5.11.0-46-generic
  linux-headers-5.13.0-28-generic linux-hwe-5.11-headers-5.11.0-46
  linux-hwe-5.13-headers-5.13.0-28 linux-image-5.11.0-46-generic
  linux-image-5.13.0-28-generic linux-modules-5.11.0-46-generic
  linux-modules-5.13.0-28-generic linux-modules-extra-5.11.0-46-generic
  linux-modules-extra-5.13.0-28-generic
使用'sudo apt autoremove'来卸载它(它们)。
升级了 0 个软件包，新安装了 0 个软件包，要卸载 0 个软件包，有 0 个软件包未被升级。
liu@liu-virtual-machine:~/下载$ sudo bash VMware-Workstation-Full-16.2.3-19376536.x86_64.bundle
Extracting VMware Installer...done.
Installing VMware Workstation 16.2.3
    Configuring...
[######################################################################] 100%
Installation was successful.
liu@liu-virtual-machine:~/下载$ 

```


## 3.安装驱动

### 3.1 Windows虚拟机

VMware菜单 【**虚拟机**】 -【 **VMware tools安装**】

驱动程序会映射到CD-ROM里面，正常打开，点击安装就可以。



**Windows7 安装注意事项：**

Windows7 SP1 未升级的情况下，因为VMwareTools11之后，采用SHA-2的认证方式，所以会出现安装失败！

VMware 官方的说明如下：

- [VMware Tools upgrade fails on Windows without SHA-2 code signing support (78708)](https://kb.vmware.com/s/article/78708)

  给出的建议是：

  - Start with VMware Tools version 11.0.6.
  - Upgrade Windows.
  - Upgrade to the latest VMware Tools version.

  实话说，这种未升级的WIN7，国内用户很难看到，只有微软官方提供的ISO，才有可能出现这种情况，不做详细介绍。

**微软的官方介绍：**

- [2019 SHA-2 Code Signing Support requirement for Windows and WSUS (microsoft.com)](https://support.microsoft.com/en-us/topic/2019-sha-2-code-signing-support-requirement-for-windows-and-wsus-64d1c82d-31ee-c273-3930-69a4cde8e64f)

  **Stand Alone** security updates [KB4474419](https://support.microsoft.com/help/4474419) and [KB4490628](https://support.microsoft.com/help/4490628) released to introduce SHA-2 code sign support.

  下载这2个更新安装之后，再安装VMware Tools的话，就不会再出现问题。


### 3.2 Linux虚拟机
安装 菜单里面的 **VMWare tools** 即可。

如果虚拟机上安装的是 Linux，也可以直接运行如下命令，进行驱动安装。

```bash
sudo apt-get install open-vm-tools-desktop .
```

官方网站说明：

- [open-vm-tools install (vmware.com)](https://docs.vmware.com/en/VMware-Tools/11.1.0/com.vmware.vsphere.vmwaretools.doc/GUID-C48E1F14-240D-4DD1-8D4C-25B6EBE4BB0F.html)



参照如下网站进行安装

[VMware16的安装及VMware配置Linux虚拟机(详解版)](https://blog.csdn.net/m0_50519965/article/details/116175873?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164892122916782184660652%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=164892122916782184660652&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-116175873.142^v5^pc_search_insert_es_download,157^v4^control&utm_term=VMware&spm=1018.2226.3001.4187)