# Linux上安装 KVM 虚拟机

测试环境：Ubuntu 20.04

## 目录

- [Linux上安装 KVM 虚拟机](#linux上安装-kvm-虚拟机)
  - [目录](#目录)
  - [1. 检查 Ubuntu 虚拟化支持](#1-检查-ubuntu-虚拟化支持)
  - [2. 在 Ubuntu  上安装 KVM](#2-在-ubuntu--上安装-kvm)
  - [3. 在 Ubuntu 上创建虚拟机](#3-在-ubuntu-上创建虚拟机)

---

## 1. 检查 Ubuntu 虚拟化支持

在 Ubuntu 上安装 KVM 之前，首先检查您的硬件是否支持 KVM。 安装 KVM 的最低要求是 AMD-V 和 Intel-VT 等 CPU 虚拟化扩展的可用性。

要检查您的 Ubuntu 系统是否支持虚拟化，请运行以下命令：

```shell
$ egrep -c '(vmx|svm)' /proc/cpuinfo
```

结果大于 0 表示支持虚拟化。 从下面的输出中，我们已经确认服务器运行正常。

要检查您的系统是否支持 KVM 虚拟化，请运行以下命令：

```shell
$ sudo kvm-ok
```

如果服务器上不存在“kvm-ok”实用程序，请运行 apt 命令进行安装：

```shell
$ sudo apt install cpu-checker
```

然后运行“kvm-ok”命令来探测系统：

```shell
$ sudo kvm-ok
```

执行信息如下：

```shell
liu@debian:~$ sudo kvm-ok
[sudo] liu 的密码：
INFO: /dev/kvm exists
KVM acceleration can be used
```



## 2. 在 Ubuntu  上安装 KVM

在验证您的系统可以支持 KVM 虚拟化后，安装 KVM。 要安装 KVM、virt-manager、bridge-utils 和其他依赖项，请运行以下命令：

```shell
$ sudo apt install -y qemu qemu-kvm libvirt-daemon libvirt-clients bridge-utils virt-manager
```

上記のパッケージの簡単な説明：

- The **qemu** package (quick emulator) is an application that allows you to perform hardware virtualization.
- The **qemu-kvm** package is the main KVM package.
- The **libvritd-daemon** is the virtualization daemon.
- The **bridge-utils** package helps you create a bridge connection to allow other users to access a virtual machine other than the host system.
- The **virt-manager** is an application for managing virtual machines through a graphical user interface.

在继续之前，您需要确保虚拟化守护进程 (libvritd-daemon) 正在运行。 为此，请运行命令：

```shell
$ sudo systemctl status libvirtd
```

个人电脑的显示信息如下：

```shell
liu@debian:~$ sudo systemctl status libvirtd
● libvirtd.service - Virtualization daemon
     Loaded: loaded (/lib/systemd/system/libvirtd.service; enabled; vendor preset: enabled)
     Active: active (running) since Sat 2022-04-09 22:43:17 CST; 8min ago
TriggeredBy: ● libvirtd.socket
             ● libvirtd-admin.socket
             ● libvirtd-ro.socket
       Docs: man:libvirtd(8)
             https://libvirt.org
   Main PID: 4035 (libvirtd)
      Tasks: 19 (limit: 32768)
     Memory: 11.7M
        CPU: 258ms
     CGroup: /system.slice/libvirtd.service
             └─4035 /usr/sbin/libvirtd

4月 09 22:43:17 debian systemd[1]: Starting Virtualization daemon...
4月 09 22:43:17 debian systemd[1]: Started Virtualization daemon.
```

您可以运行以下命令使其在启动时作为服务随机启动：

```shell
$ sudo systemctl enable --now libvirtd
```

要检查 KVM 模块是否已加载，请运行以下命令：

```shell
$ lsmod | grep -i kvm
```

从输出中，您可以看到 kvm_intel 模块的存在。 这是针对英特尔处理器的。 对于 AMD CPU，请获取 kvm_intel 模块。

```shell
liu@debian:~$ lsmod | grep -i kvm
kvm_intel             327680  0
kvm                   921600  1 kvm_intel
irqbypass              16384  1 kvm
```



## 3. 在 Ubuntu 上创建虚拟机

成功安装 KVM 后，创建虚拟机。 有两种方法可以做到这一点。 在命令行上创建虚拟机或使用 KVMvirt-manager 图形界面。

**※ 最简单的就是在应用程序里面找到 KVM 虚拟机的图标直接启动。**

后面内容引自其它资料，请图略随后的内容。

virt-install 命令行工具用于在终端上创建虚拟机。 创建虚拟机时，需要一些参数。

我用 Deepin ISO 镜像创建虚拟机的完整命令是：

```shell
$ sudo virt-install --name=deepin-vm --os-variant=Debian10 --vcpu=2 --ram=2048 --graphics spice --location=/home/Downloads/deepin-20Beta-desktop-amd64.iso --network bridge:vibr0 
```

-name 选项指定虚拟机的名称—— deepin-vm -os-variant 标志表示操作系统系列或虚拟机衍生产品。 由于 Deepin10 是 Debian 的衍生版本，因此我们将 Debian10 指定为变体。

运行命令以获取有关操作系统变体的其他信息：

```shell
$ osinfo-query os
```

-vcpu 选项表示 CPU 内核（本例中为 2 个内核），-ram 表示 2048MB 的 RAM。 -location 标志指向 ISO 映像的绝对路径，-network bridge 指定虚拟机使用的适配器。 运行命令后，虚拟机将启动，安装程序将启动，您将能够安装虚拟机。

virt-manager 实用程序允许用户使用 GUI 创建虚拟机。 要开始，请转到终端并运行命令。

```shell
$ virt manager
```

虚拟机管理器窗口如图所示弹出。