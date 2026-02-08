# macOS Sequoia 升级说明

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
keepsyms=1 agdpmod=pikera -lilubetaall -amfipassbeta ipc_control_port_options=0
```
**csr-active-config**
```
03080000
```

