# iplay30-mt6771-dts

Teclast iPlay 30（MT6771 / Helio P60）**实际启动使用的设备树**，只保留源码。
文件由原厂 dtb 反编译后打补丁得到，是自包含的 dts，不依赖内核树内其它 dtsi。

## 进度

板级设备已经全部在设备树中描述，主线内核可以正常启动并驱动整机：

| 模块 | 节点/改动 | 状态 |
|---|---|---|
| eMMC / SD | `aliases` 固定 `mmc0`/`mmc1`，避免并行 probe 抢编号 | ✅ |
| 显示 | `simple-framebuffer`（LK 1200×1920）、mmsys/OVL/RDMA/DSI | ✅ |
| GPU | `mali@13040000` + `mt6771-mfg` 电源域 | ✅ |
| 背光/充电 | `mt6370`、`mt6370-backlight` | ✅ |
| PMIC | `mt6358-vproc`、`mt6358-vldo28` | ✅ |
| 触摸 | `goodix@5d`（GT9886，中断/复位 GPIO） | ✅ |
| USB | `mt6771-mtu3`（peripheral）、`mt6771-tphy` + `force-vbus` | ✅ |
| Wi-Fi/BT | `wifi@180f0000`、BTIF | ✅ |
| 温度 | `mt6771-thermal`，6 路传感器 | ✅ |
| 音频 | 未添加 AFE/AW87359 节点 | ❌ |

## 关键点

1. **删除 SPM / PSCI idle-states**：原厂深睡状态会让主线挂死，
   现在只走 WFI。
2. **固定 MMC 编号**：`mmc0 = /msdc@11230000`（eMMC），
   `mmc1 = /msdc@11240000`（SD）。否则 SD 会抢走 `mmcblk0`，
   stage-1 找不到 `mmcblk0p40`。
3. **T-PHY `mediatek,force-vbus`**：主线必须强制 VBUS 位，USB UDC
   才能进入 `configured`。
4. **mtk-drm**：`mmsys` 使用 `mediatek,mt6771-mmsys`，并补齐
   OVL/RDMA/DSI 时钟与中断。
5. **PMIC 子设备**：`mt6358-vproc` / `mt6358-vldo28` 避免上游 MT6358 MFD
   抢占 PMIC 导致 regulator/背光失败。

## 注意

- 设备树通过 boot.img header v2 内嵌，**不使用也不修改 dtbo 分区**；
  只替换分区里的 dtb 不会生效。
- 原厂 dtbo 中的 overlay（包括 AW87359 功放节点）不参与主线启动。
- 早期的最小 bring-up dts（单核、无 DRM/GPU）已删除，只保留当前版本。

## 相关仓库

- `iplay30-mt6771-mainline`：内核源码
- `iplay30-mt6771-init`：stage-1 init 源码

## 许可

GPL-2.0。
