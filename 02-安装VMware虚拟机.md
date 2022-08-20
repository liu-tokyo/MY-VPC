# 安装VMware虚拟机

## 简介：

VMware Workstation Pro 是业界标准的桌面 Hypervisor，用于在 Linux 或 Windows PC 上运行虚拟机。立即开始免费体验功能齐全的 30 天试用版。

Workstation 16 Pro 基于行业定义的技术，在以下方面实现了改进：DirectX 11 和 OpenGL 4.1 3D 加速图形支持、全新的“暗黑模式”用户界面、在 Windows 10 版本 2004 和更高版本的主机上对 Windows Hyper-V 模式的支持、一个用于支持容器和 Kubernetes 集群的新 CLI“vctl”，以及对最新 Windows 和 Linux 操作系统的支持等。

## 1.Windows环境

### 1.1 使用 winget 安装

`winget` 可以通过 `Microsoft Store` 中查找、安装，也可以到`Github`上找到安装文件。`winget` 已经是后期`Windows10`和`Windows11`的标配工具，是和*Linux*在云计算市场竞争，（无奈？）追加的新工具。

- 查找VMware版本：

  ```shell
  winget search vmware
  ```

- 选择合适版本安装：

  ```shell
  winget install VMware.WorkstationPro
  ```

  ※由于软件库更新时间问题，这里安装的版本可能会比官方下载的最新版本存在差异。

  也可以选择安装`VMware Player`，这个是免费软件，缺点是不能创建VMware虚拟机。

  ```shell
  winget install VMware.WorkstationPlayer
  ```

- 实际安装指令信息：

  `Ubuntu 20.04` 安装虚拟机的实际信息如下：
  
  ```shell
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
  
  

### 1.2 官方下载安装

直接到官方网站下载安装，

- 官方下载网址：

  [VMware Workstation Pro Download](https://www.vmware.com/jp/products/workstation-pro/workstation-pro-evaluation.html)

  正常点击安装即可，不做详细介绍。



## 2.Linux环境

### 2.1 官方下载安装

- 官方下载网址：

  最新版本：

  [VMware Workstation Pro Download](https://www.vmware.com/jp/products/workstation-pro/workstation-pro-evaluation.html)

  任意版本（需要注册用户才能下载）：

  https://customerconnect.vmware.com/en/downloads/info/slug/desktop_end_user_computing/vmware_workstation_pro/16_0

- 下载文件名称：  
  类似如下文件，不同时期下载的最新版本可能有所不同：  
  VMware-Workstation-Full-16.2.3-19376536.x86_64.bundle  
  VMware-Workstation-Full-16.2.4-20089737.x86_64.bundle



### 2.2 使用指令安装

VMware的安装文件被下载到了“下载”目录中，在该下载文件的目录中启动终端，针对下载的文件，按照如下步骤进行安装：

1. VMWare Player 在内部使用内核模块 vmmon.ko 和 vmnet.ko，这将在后面的步骤中涉及。  
   由于vmmon.ko和vmnet.ko需要在VMWare Player安装环境中编译，所以在安装VMWare Player前需要提前安装构建工具。

   ```bash
   sudo apt install build-essential
   ```

2. 安装指令

   ```bash
   sudo bash VMware-Workstation-Full-16.2.3-19376536.x86_64.bundle
   ```

   ※`VMware-Workstation-Full-16.2.3-19376536.x86_64.bundle`文件名称按照下载的文件名称进行设置。

- 实际的安装信息如下，顺利结束。

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

- 存在问题：

  Linux版的`16.2.4` 安装之后，居然大部分是英文状态，相信大部分人也不会在Linux上用虚拟机，所以不做详细介绍。

  Windows版的`16.2.3`安装结束，是完全的中文菜单。可能`16.2.4` 属于当前最新版本，对语言的支持还不是那么彻底？

## 3.安装驱动

### 3.1 Windows虚拟机

- VMware菜单 【**虚拟机**】 -【 **VMware tools安装**】  
  驱动程序会映射到CD-ROM里面，正常打开，点击安装就可以。

- **Windows7 安装注意事项：**  
  Windows7 SP1 未升级的情况下，因为`VMwareTools 11`之后，采用SHA-2的认证方式，所以会出现安装失败！

  - VMware 官方的说明如下：  
    [VMware Tools upgrade fails on Windows without SHA-2 code signing support (78708)](https://kb.vmware.com/s/article/78708)

  - 给出的建议是：

    Start with VMware Tools version 11.0.6.

    Upgrade Windows.

    Upgrade to the latest VMware Tools version.

  实话说，这种未升级的WIN7，国内用户很难看到，只有微软官方提供的ISO，才有可能出现这种情况，不做详细介绍。

  如果不行碰到，按照如下办法解决：

  - **微软的官方介绍：**
    [2019 SHA-2 Code Signing Support requirement for Windows and WSUS (microsoft.com)](https://support.microsoft.com/en-us/topic/2019-sha-2-code-signing-support-requirement-for-windows-and-wsus-64d1c82d-31ee-c273-3930-69a4cde8e64f)

  - **Stand Alone** security updates [KB4474419](https://support.microsoft.com/help/4474419) and [KB4490628](https://support.microsoft.com/help/4490628) released to introduce SHA-2 code sign support.

    下载这2个更新安装之后，再安装VMware Tools的话，就不会再出现问题。

### 3.2 Linux虚拟机

安装 菜单里面的 **VMWare tools** 即可，实际上类似于Ubuntu等Linux，会自动识别到VMware的硬件，进行正常设置。

- 也可以直接运行如下命令，进行驱动安装（Ubuntu，Debian）：

  ```bash
  sudo apt-get install open-vm-tools-desktop
  ```

- 官方网站说明：

  [open-vm-tools install (vmware.com)](https://docs.vmware.com/en/VMware-Tools/11.1.0/com.vmware.vsphere.vmwaretools.doc/GUID-C48E1F14-240D-4DD1-8D4C-25B6EBE4BB0F.html)

- 也可以参照如下网站进行安装

  [VMware16的安装及VMware配置Linux虚拟机(详解版)](https://blog.csdn.net/m0_50519965/article/details/116175873?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164892122916782184660652%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=164892122916782184660652&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-116175873.142^v5^pc_search_insert_es_download,157^v4^control&utm_term=VMware&spm=1018.2226.3001.4187)



## 4. VMware虚拟机快照

### 4.1 快照的意义

**快照**是虚拟机磁盘文件在某个点及时的副本，对技术人员来说，具有巨大意义，无论如何强调，都不为过。

- 系统崩溃或系统异常，你可以通过使用恢复到快照来保持磁盘文件系统和系统存储。
- 当升级应用和服务器及给它们打补丁的时候，快照是救世主。
- 技术人员经常需要做很多探索性操作，很容易导致系统混乱；运用快照逐步完善系统环境，系统会非常**稳定、清凉**！这才是我们真正关心的亮点。

### 4.2 快照的操作

- **VMware的工具条**

  ![image-20220821050657508](images/image-20220821050657508.png)

  3个图标的含义：

  - 创建快照
  - 恢复到快照
  - 快照管理器

  一般来说有了【创建快照】和【恢复到快照】就能解决我们日常的大部分快照操作问题。

- **创建快照**

  点击【创建快照】图标就可以建立一个快照。

  一般来说，系统安装结束、并且安装了必须的工具软件之后，就可以创建快照。在创建快照之前，要把虚拟机的硬盘进行压缩，尽量少占用硬盘空间。

  一个虚拟机虽然可以建立无数的快照，可以做非常复杂的分支，但是个人一直坚持只有一个快照的原则。

- **恢复到快照**

  因为个人一直坚持只保持一个快照的原则，所以非常简单，不管虚拟机处于任何状态，随时点击【恢复到快照】按钮就可以恢复到快照时刻的状态。

- **快照管理器**

  本人只有在**创建快照**之后的操作，需要持久保持的情况下，才使用该功能。

  一旦确定创建快照之后的状态需要持久保持，尽量在虚拟机关机的情况下，进入快照管理器：

  - 选择**快照**名字，点击【**删除快照**】按钮，就可以把所作操作持久化。
  - 然后再点击【创建快照】，再次创建快照，让虚拟机继续保持在快照状态。

  理论上虚拟机的生命周期中，要一直保持快照状态，才比较合理，也就是说：随时可以吃后悔药！