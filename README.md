# Hackintosh-Gigabyte-Z490-i7-10700K-Rx5700xt
---
- OpenCore 0.6.2 x64
- 硬件配置
    - CPU: intel i7 10700K
    - 主板：Gigabyte-Z490-Aorus-Elite-AC
    - 内存：芝奇 32Gx2 3200 MHz DDR4
    - 硬盘：SAMSUNG 1T 970 EVO
    - 显卡：蓝宝石 RX5700xt白金版 8GB
    - 网卡：FV-HB1200（BCM94360C）
    - 机箱：乔思伯 UMX4
- MacOS Catalina 10.15.7 (19H2)

### 截图
---
![输入图片说明](https://images.gitee.com/uploads/images/2020/1008/170123_3d50685d_608091.jpeg "截屏2020-10-08 15.41.40.jpg")

![输入图片说明](https://images.gitee.com/uploads/images/2020/1008/170135_dda8bfb0_608091.png "截屏2020-10-08 15.41.53.png")

![输入图片说明](https://images.gitee.com/uploads/images/2020/1008/170144_41870432_608091.png "截屏2020-10-08 15.42.05.png")

![输入图片说明](https://images.gitee.com/uploads/images/2020/1008/170152_b8278106_608091.png "截屏2020-10-08 15.42.10.png")

![输入图片说明](https://images.gitee.com/uploads/images/2020/1008/170200_4f99318d_608091.png "截屏2020-10-08 15.42.16.png")

![输入图片说明](https://images.gitee.com/uploads/images/2020/1008/170212_2b7f45d0_608091.png "截屏2020-10-08 16.13.44.png")


### 核心组件驱动
---
- WIFI & Bluetooth 正常（已屏蔽板载网卡）
- USB3.0 & USB2.0 正常
- 板载声卡 & 2.5G 网卡 正常
- 核显加速 & 独立显卡 正常
- 电源休眠正常 可以硬件唤醒
- 隔空投送 正常
- 随航 正常

### 使用说明
---
这个配置是最终优化后的驱动和配置文件，如果你不是很熟悉配置，建议你根据官方指导来配置。如果你的硬件和我一样的话，那么恭喜你，你直接白瞟了一个完美的黑苹果。Resources资源已从配置里删除，你可以从网盘下载替换掉。

我用过的工具和环境搭建工具都上传到网盘了，文件太大～

**Tool**：链接: https://pan.baidu.com/s/1MqHYUgfnH_12sWAeaMSWxg  密码: v4mm

**Resources**：链接: https://pan.baidu.com/s/11AgQqZS8TCoNrAGaqeODDQ  密码: sctu

**环境**：链接: https://pan.baidu.com/s/1qfgL0f2WC8DXKC9j-cqKkQ  密码: 6i1d
命令行工具和Python安装包（工具的运行环境）


### 使用到的工具
---
- GenSMBIOS 生成序列号
- gfxutil-1.80b 查找硬件地址
- gibMacOS 下载macos镜像
- IORegistryExplorer 查看设备信息
- MaciASL-1.5.7 DSTD编辑器
- MountEFI 显示efi分区
- ProperTree 编辑plist
- USBMap-usb映射

### 装机指导
---
请按OpenCore官方指导购买配件，以便完美驱动黑苹果，为了避免大家踩坑，我把我装机遇到的问题总结一下：

1. U盘引导
    这一步是最简单的，只要按官方的方法基础上没有问题。一定要完整的地看一遍官方的教程，根据自己的硬件去调整驱动和Config。
    U盘要大于16G，分两个区，一个放EFI引导文件，一个放系统。（虽然可以用在线恢复的方法，但速度太慢，建议大家还是用U盘安装）
2. ACPI
    这是最最最重要的一步，之前这一步没怎么重视，导到后面的一系列问题。要根据自己的硬件去生成DSDT,然后看教程修改一下，特别是USB，可以去下载官方的SSDT-RHUB + **USBInjectAll.kext** 来先引导安装好系统，再进系统做USB映射。
3. Kexts
    我花的最多的时间就是驱动蓝牙，一直找不到原因，因为网卡是插在usb2.0接口上的，所没缺少USBInjectAll.kext这个驱动会导致蓝牙不能被识别到。**USBInjectAll.kext**在做好USB映射后一定要删除，不然会有冲突。
    不要放太多kexts，对系统没有多大帮助，也不便于查找问题，应该用最少的Kexts引导装好系统，再慢慢去优化。
4. Config.plist
    根据官方指导去配置，不要直接用主播或网上下的Config.plist，硬件不一样，设置就不一样，而且OC版本变化，有新增，删除的参数，Kexts也要用官方最新推荐的。
    
我踩过最大的坑就是usb注入问题，希望大家不要踩同样的坑，因为usb连接着各种配件，一旦usb不能被识别会导致很多配件不能识别，会识导你以为是设备问题，或其它硬件问题。usb能用，配件没驱动好解决。装好系统第一时间检查一下USB设备树，如下图：

![输入图片说明](https://images.gitee.com/uploads/images/2020/1008/170547_7500cebe_608091.png "截屏2020-10-08 17.05.02.png")


如果你只有一项usb3.0那就是usb没有全部识别到，要用**USBInjectAll.kext**来驱动所有接口。

---
### 启用板载2.5G有线网络
默认情况2.5G网络显示是网线已拔出，请按以下步聚设置：
1. 启用BIOS/IO里的板载网卡
2. 系统偏好设计 --> 网络 --> 以太网 --> 高级 --> 手动设置IP/硬件，见图。

![输入图片说明](https://images.gitee.com/uploads/images/2020/1010/135939_4a79eefe_608091.png "QQ20201010-134758.png")

![输入图片说明](https://images.gitee.com/uploads/images/2020/1010/135955_4099b700_608091.png "QQ20201010-135400.png")

### Links
- [Getting started with OpenCore](https://dortania.github.io/OpenCore-Install-Guide/prerequisites.html)
- [OpenCore Sanity Checker](https://opencore.slowgeek.com/)