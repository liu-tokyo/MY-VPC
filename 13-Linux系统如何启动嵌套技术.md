# Linux系统如何启动嵌套技术



参照资源：

- [KVM中文网](https://wiki.archlinux.org/title/KVM_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))



## 1. 检查是否支持KVM

1. 硬件支持

   KVM需要虚拟机宿主（host）的处理器带有虚拟化支持（对于Intel处理器来说是VT-x，对于AMD处理器来说是AMD-V）。你可以通过以下命令来检查你的处理器是否支持虚拟化：

   ```shell
   $ LC_ALL=C lscpu | grep Virtualization
   ```

   或者： 

   ```shell
   $ grep -E --color=auto 'vmx|svm|0xc0f' /proc/cpuinfo
   ```

    如果运行后没有显示，那么你的处理器不支持硬件虚拟化，你不能使用KVM。

   注意： 您可能需要在BIOS中启用虚拟化支持

2. 内核支持

   Arch Linux的内核提供了相应的内核模块来支持KVM。
   你可以通过以下命令来检查你的内核是否已经包含了支持虚拟化所必须的模块（kvm及kvm_amd与kvm_intel这两者中的任意一个）：
   
   ```shell
   $ zgrep CONFIG_KVM /proc/config.gz
   ```
   
   如果模块设置不为 y或m,则该模块不可用
   
   然后确认这些内核模块是否已自动加载：
   
   ```shell
   $ lsmod | grep kvm
   ```
   
   如果运行后没有显示，那么需要手动加载这些模块。
   

## 2. 准虚拟化(使用VIRTIO)

准虚拟化为客户机提供了一种使用主机上设备的快速有效的通信方式。KVM使用Virtio API作为虚拟机管理程序和客户机之间的连接层，为虚拟机提供准虚拟化设备（亦称Virtio设备）。 所有Virtio设备都包括两部分：主机设备和客户机驱动程序。
1. 内核支持

   用以下命令检查VIRTIO模块是否可用:

   ```shell
   $ zgrep VIRTIO /proc/config.gz
   ```

   检查VIRTIO模块是否已经自动加载:

   ```shell
   $ lsmod | grep virtio
   ```

   如果运行后没有显示，那么需要手动加载这些模块。

>提示： 如果 kvm_intel 或 kvm_amd 加载失败但是kvm 加载成功， (lscpu可检查硬件支持情况)检查你的BIOS设置。一些品牌机 (尤其是笔记本电脑) 默认关闭了这个功能，请确保是否是硬件支持该功能，但在BIOS中它被关闭了，在dmesg的提示信息中会展示出相关的警告信息。

2. 准虚拟化设备列表
   - 网络设备 (virtio-net)
   - 硬盘设备 (virtio-blk)
   - 控制器设备 (virtio-scsi)
   - 串口设备 (virtio-serial)
   - 气球设备 (virtio-balloon)



## 3. 如何使用KVM

请参考[QEMU](https://wiki.archlinux.org/title/QEMU_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))条目。
小贴士与小技巧
注意： 请参考[QEMU#Tips and tricks](https://wiki.archlinux.org/title/QEMU#Tips_and_tricks)和[QEMU#Troubleshooting](https://wiki.archlinux.org/title/QEMU#Troubleshooting)词条获取更多相关技巧。

## 4. 嵌套虚拟化

1. 在宿主机启用 **kvm_intel** 模块的嵌套虚拟化功能：

   ```shell
   sudo modprobe -r kvm_intel
   sudo modprobe kvm_intel nested=1
   ```

2. 使嵌套虚拟化永久生效（请参考[Kernel modules#Setting module options](https://wiki.archlinux.org/title/Kernel_modules#Setting_module_options)）：

   ```shell
   /etc/modprobe.d/modprobe.conf
   options kvm_intel nested=1
   ```

3. 检验嵌套虚拟化功能是否已被激活：

   ```shell
   sudo apt install sysfsutils
   systool -m kvm_intel -v | grep nested
   
       nested              = "Y"
   ```
   
4. 使用以下命令来运行guest虚拟机：

   ```shell
   qemu-system-x86_64 -enable-kvm -cpu host
   ```

5. 启动虚拟机并检查是否有vmx标志：

   ```shell
   grep vmx /proc/cpuinfo
   ```

   

[Categories](https://wiki.archlinux.org/title/Special:Categories):[Hypervisors (简体中文)](https://wiki.archlinux.org/title/Category:Hypervisors_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))[Kernel (简体中文)](https://wiki.archlinux.org/title/Category:Kernel_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))