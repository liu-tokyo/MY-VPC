# 安装 VirtualBox 虚拟机

## 目录

- [安装 VirtualBox 虚拟机](#安装-virtualbox-虚拟机)
  - [目录](#目录)
  - [1.Windows下面安装](#1windows下面安装)
    - [1.1 使用 `winget` 进行安装](#11-使用-winget-进行安装)
    - [1.2 官方下载安装](#12-官方下载安装)
  - [2.Linux环境安装](#2linux环境安装)

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

如下一条指令就可以安装：

```bash
sudo apt install org.virtualbox.virtualbox
```

当然也可以直接去官网下载，然后用软件安装器安装。

