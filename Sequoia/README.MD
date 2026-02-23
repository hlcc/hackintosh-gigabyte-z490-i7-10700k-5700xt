# macOS Sequoia 升级说明



## Apple Music无法播放问题

在较新的macOS版本中可能会因为**DRM**（数字版权管理） 保护机制，导致**Apple Music**无法通过蓝牙或某些方式播放。

- 有 AMD 独立显卡（如RX 550, RX 580, RX 5700, RX 6600等），强制AMD显卡进行解码，终端运行：

  ```bash
  defaults write com.apple.AppleGVA gvaForceAMDKE -boolean yes
  ```
  
  **请注意**：开启这个模式后，可能会影响**屏幕录制**或**视频导出**功能（比如出现彩色雪花）如果遇到这些问题，可以随时在终端输入以下命令来关闭它：
  
  ```bash
  defaults write com.apple.AppleGVA gvaForceAMDKE -boolean no
  ```
  
- 有Inter核显+AMD独显，添加引导参数，让系统需要时调用独显进行DRM解码：**已尝试无效！**

  在OpenCore添加引导参数(boot-args)，添加类似参数 `shikigva=80` 或 `hikigva=16` 具体用哪个参数，取决于你的CPU和GPU组合。

  | 参数               | 作用                                                         | 适用场景                                                     |
  | :----------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
  | **`shikigva=80`**  | 强制使用 AMD 独显进行 DRM 解码，同时为 iGPU 注入缓冲帧以支持硬件加速。 | 适合大多数 iGPU + AMD 独显组合，如果你之前没配置过任何 shiki 参数，这个通常是首选。 |
  | **`shikigva=16`**  | 仅禁用 iGPU 的 DRM 解码，所有 DRM 任务交给 AMD 独显。与 `shikigva=80` 效果类似，但可能在某些主板上更稳定。 | 适用于 iGPU 无法正常处理 DRM 的场景（我的情况就属于这一类）。 |
  | **`shikigva=32`**  | 启用 iGPU 进行 DRM 解码，仅用于纯 iGPU 系统。                | 不适合，因为已有独显。                                       |
  | **`shikigva=128`** | 强制使用 AMD 独显进行 DRM 解码，并禁用 iGPU 的加速帧缓冲。   | 如果你完全不打算用 iGPU 加速（例如只作为计算设备），可以用这个。 |

  除了 `shikigva`，有时还需要配合 `igfxonln=1` 强制 iGPU 保持在线，或 `agdpmod=pikera` 解决 5000 系显卡黑屏问题（你已用 5700XT，如果启动正常则不需要）。

## BIOS 设置

- 关闭VT-D
针对我的配置，只有关闭了这能正常驱动网卡

## 注意事项

**必要的Kext（按顺序）**

- IOSKywalkFamily.kext
- IO80211FamilyLegacy.kext
- IO80211FamilyLegacy.kext/Contents/PlugIns/AirPortBrcmNIC.kext
- AMFIPass.kext
- AirportBrcmFixup.kext
- AirportBrcmFixup.kext/Contents/PlugIns/AirPortBrcm4360_Injector.kext（禁用）
- AirportBrcmFixup.kext/Contents/PlugIns/AirPortBrcmNIC_Injector.kext（禁用）
- LucyRTL8125Ethernet.kext

## Config 调整

**boot-args**
```
keepsyms=1 agdpmod=pikera
```
**csr-active-config**
```
03080000
```

