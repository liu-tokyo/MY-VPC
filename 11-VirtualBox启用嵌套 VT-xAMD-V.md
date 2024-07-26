# VirtualBox虚拟机如何选中启用嵌套 VT-x/AMD-V

　使用VirtualBox，总希望性能更好一些。于是吾就想启用各种指令。比如说“启用嵌套 VT-x/AMD-V”：

- 公司换了新机（CPU是10代i5）之后，可以选中了。
- 而家里的机器（CPU是8代i5），却一直无法选中。

为什么无法选中？查了一下，我的CPU是支持VT-x的。于是就找了一下，如何启用这个指令：

1. 单个处理

   ```shell
   C:
   
   cd "c:\Program Files\Oracle\VirtualBox"
   
   VBoxManage.exe list vms
   
   "Windows7-develop" {69d4bda1-5ae0-4a29-aa69-b0bc18d96158}
   "Ubuntu18-doubango" {f504019e-a48f-473a-a1d3-9fc66cea2c60}
   "Ubuntu18-freeswitch" {5277a8f7-d844-4a6d-be0c-0804d21c9986}
   
   VBoxManage.exe modifyvm "Windows7-develop" --nested-hw-virt on
   ```

2. 批处理

   ```shell
   cd /d "c:\Program Files\Oracle\VirtualBox"
    
   for /F %%i in ('VBoxManage.exe list vms') ^
   do (
       VBoxManage.exe modifyvm %%i --nested-hw-virt on
   )
   ```
   
3. 启动客户机

   输入如下指令：

   ```shell
   egrep -c '(vmx|svm)' /proc/cpuinfo
   ```

   显示数字大于 0 ，就证明虚拟化技术处于有效状态。

   进入客户机，执行：

   ```shell
   cat /etc/proc/cpuinfo |grep vmx
   ```

   已经可以看到vmx flag。



执行过后，再去设置看看，虚拟技术已经有效了。



