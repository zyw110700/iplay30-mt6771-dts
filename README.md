# iplay30-mt6771-dts

Teclast iPlay 30（MT6771 / Helio P60）实际启动使用的设备树。
由原厂 dtb 反编译后打补丁得到，是自包含的 dts，不依赖内核树内其它 dtsi。

## 进度

| 模块 | 状态 |
|---|---|
| eMMC / SD | ✅ |
| 显示 / GPU | ✅ |
| 背光 / 充电 / PMIC | ✅ |
| 触摸 | ✅ |
| USB（device + USB2 host） | ✅ |
| Wi-Fi / 蓝牙 | ✅ |
| 温度 | ✅ |
| 音频 | ❌ |

## 相关仓库

- `iplay30-mt6771-mainline`：内核源码
- `iplay30-mt6771-init`：stage-1 init 源码

## 许可

GPL-2.0。
