# 如何在 VirtualBox 和 VMware 之间转换虚拟机

迁移到另一个虚拟机程序可能会令人生畏。 如果您已经按照自己喜欢的方式设置了虚拟机，则不必从头开始安装它们——您可以迁移现有的虚拟机。

VirtualBox 和 VMware 使用不同的虚拟机格式，但都支持标准的开放虚拟化格式。 将您现有的虚拟机转换为 OVF 或 OVA，您将能够将其导入另一个虚拟机程序。

不幸的是，这可能并不总是完美的，因为 VirtualBox 和 VMware 似乎都使用了稍微不同的 OVA/OVF 实现，它们并不完全兼容。 如果这不起作用，您可能需要从头开始重新安装虚拟机的客户操作系统。

## 1. VirtualBox 到 VMware

1. 在将虚拟机从 VirtualBox 迁移到 VMware 之前，请确保它在 VirtualBox 中已“关闭”——而不是挂起。 如果它已暂停，请启动虚拟机并将其关闭。
2. 单击 VirtualBox 中的文件菜单，然后选择导出设备。
3. 选择要导出的虚拟机并为其提供位置。
4. VirtualBox 将创建 VMware 可以导入的 nOpen 虚拟化格式存档（OVA 文件）。 这可能需要一些时间，具体取决于虚拟机磁盘文件的大小。
5. 要在 VMware 中导入 OVA 文件，请单击打开虚拟机选项并浏览到您的 OVA 文件。
6. VirtualBox 和 VMware 并不完全兼容，因此您可能会收到一条警告消息，指出文件“未通过 OVF 规范性能”——但如果您单击重试，虚拟机应该会导入并正常运行。
7. 该过程完成后，您可以在 VMware 中启动虚拟机，从虚拟机内部的控制面板中卸载 VirtualBox Guest Additions，然后从虚拟机的菜单中安装 VMware Tools。

## 2. VMware 到 VirtualBox

1. 在将虚拟机从 VMware 迁移到 VirtualBox 之前，请确保它在 VMware 中已“关闭”——而不是挂起。 如果它已暂停，请启动虚拟机并将其关闭。

2. 接下来，浏览到 OVFTool 文件夹。 如果您使用的是 VMware Player，您可以在 C:\Program Files (x86)\VMware\VMware Player\OVFTool 中找到它。 按住 Shift，在 OVFTool 文件夹内单击鼠标右键，然后选择在此处打开命令窗口。

3. 使用以下语法运行 ovftool：

   ```
   ovftool source.vmx export.ovf
   ```

   例如，如果我们要转换位于 C:\Users\NAME\Documents\Virtual Machines\Windows 7 x64\Windows 7 x64.vmx 的虚拟机，并在 C:\Users\NAME\export 处创建一个新的 OVF 文件。 ovf，我们将运行以下命令：

   ```
   ovftool “C:\Users\NAME\Documents\Virtual Machines\Windows 7 x64\Windows 7 x64.vmx” C:\Users\NAME\export.ovf
   ```

   如果您收到“无法打开磁盘”错误，则可能是虚拟机仍在运行或未正确关闭 - 启动虚拟机并执行关闭。

4. 该过程完成后，您可以将 .ovf 文件导入 VirtualBox。 使用文件菜单中的导入设备选项。

5. 该过程完成后，您可以启动虚拟机、卸载 VMware Tools 并安装 VirtualBox 的 Guest Additions。

## 参照资源

- https://www.howtogeek.com/125640/how-to-convert-virtual-machines-between-virtualbox-and-vmware/