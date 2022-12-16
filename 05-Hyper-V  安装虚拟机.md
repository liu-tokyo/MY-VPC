# Microsoft 的 **Hyper-V**

这个是微软自己研发的一个虚拟机软件，现在的 `Windows10` （企业，专业或教育版本）系统都自带了这个功能，只不过在默认情况下不是开启的，这里我们需要单独开启一下，之后就能正常使用了，下面我简单介绍一下**Hyper-V**：

1、开启 **Hyper-V** 功能，这个直接到 Windows 功能中勾选 Hyper-V 开启就行，如下，需要安装相关插件，重启后才能生效。

2、重启电脑后，我们直接打开“Hyper-V管理器”就可以创建虚拟机了，基本步骤和上面的软件创建虚拟机差不多，设置网络、内存、磁盘等。

3、最后，就可以启动虚拟机进行正常安装了。

如有不清楚的地方，请查阅微软的官方文档：

- https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v

这里你也可以使用win10自带的WSL，功能也非常不错，可以自己尝试一下。

微软有一个比较恶心的功能，就是启用了 Hyper-V 之后，其他的虚拟机软件就不能运行了。

微软应该是放弃了 VirtualPC，就更新到 2007，然后就没有再更新。

### 为什么不使用VIRTUALBOX？
想知道Hyper-V是否优于VirtualBox，或者每种方法的好处是什么？ 这是一个有点合理的问题 不置可否 回答。

简而言之：如果你想运行一个完整的Ubuntu桌面，它具有可接受的图形性能，驱动程序仿真，工作声音，USB设备访问等等，你应该坚持使用VirtualBox，VMWare和其他VM软件。

Hyper-V还依赖远程桌面协议来显示虚拟机，而VirtualBox，VMWare和类似的虚拟化软件则不然。

但是，如果您的需求更简单，比如说，您只能访问核心操作系统和软件包，或特定的GUI应用程序，命令行等，那么这个新优化的Ubuntu 18.04 LTS版本值得一试。

而 Windows 在以下情况下使用时，Hyper-V通常是1类管理程序： Windows 服务器（直接在硬件上运行，而不是在硬件上运行的OS之上）在服务器上使用时是2型虚拟机管理程序 Windows 10 专业桌面。

### 尝试在 Hyper-V 上安装 Ubuntu

2022.04.18 尝试安装当前最新版的 Ubuntu21.10

- 第一次安装就给一个下马威，安装结束后，重启看似没有进度的状态；等了好长是时间，还是没啥反应。强制关机也没有效果，只能重置宿主机。

  感觉是和 UEFI 启动有关，重新玩一次，看看能够 BIOS 启动。

- 第二次没有用 快速新建 ，结果虚拟机根本无法启动，找不到启动盘？明明是有启动盘的。估计还是 UEFI 的原因造成的。

- 第三次还是选择了 快速新建 ，一切正常，和第一次的区别是 **内存4GB** 。



### 改变HYPERV虚拟的Ubuntu屏幕分辨率

Ubuntu14开始已经自带Hyper-V Integration Service，也就是说在Hyper-V里跑Ubuntu 14以上的版本的时候，再也不需要像以前的版本那样单独的安装Hyper-V Integration Service，因为所有的Hyper-V网卡驱动，显卡驱动和其他组件都已经内置了。

但是和操作系统的分辨率调整这一项，还是不如在Hyper-V里跑Windows的VM来的方便，默认的Ubuntu VM只有一种分辨率（1152×864），不能像Windows VM那样根据当前的窗口自动调节分辨率，自适应屏幕。

目前的解决方法只能是手工指定分辨率，下面是具体步骤。

- 打开文件 **/etc/default/grub**

  ```shell
  sudo nano /etc/default/grub
  ```

- 找到GRUB_CMDLINE_LINUX_DEFAULT所在行，在最后加上

  ```shell
  video=hyperv_fb:[分辨率]
  ```

- 比如我想要的分辨率是1600×900，这一行改完后就是

  ```shell
  GRUB_CMDLINE_LINUX_DEFAULT="quiet splash video=hyperv_fb:1280x720"
  ```

- 修改完毕后在Terminal环境里运行 **sudo update-grub**

  ```shell
  sudo update-grub
  ```

重启机器后，便可以看到Ubuntu运行在新的分辨率下了。

注意：这种方法最高只能支持到1920×1080 的分辨率，如果设置了1920×1200或者更大的分辨率，Ubuntu则会恢复到默认的分辨率。对于大屏幕显示器有高DPI需求的童鞋，可以考虑用RDP，VNC等方式，也只有这样，屏幕分辨率才能变为自由的。
