# wex-modified-edition

本仓库提供面向 Dynabook G83 的 WhateverXe 预编译产物。当前版本为 [0.8.2](https://github.com/xiananliu/wex-modified-edition/releases/tag/0.8.2)，基于上游 [WhateverXe 0.8.2](https://github.com/b00t0x/WhateverXe/releases/tag/0.8.2)，kext 内部版本号同为 `0.8.2`。

## 改进点与使用参数（相对上游 0.8.2）

| # | 解决的问题 | 使用参数 |
| --- | --- | --- |
| 1 | 原生 HDMI 在 4K60 下周期性黑闪 | `-wex-hdmi-scdc-write`，与第 2 项同时启用 |
| 2 | 原生 HDMI 的 4K60 模式无法选择 | `-wex-hdmi-clock600`，与第 1 项同时启用 |
| 3 | 固件遗留异常 CDCLK 导致驱动接管时内屏黑屏 | `-wex-cdclk-sanitize` |
| 4 | 开机时内屏背光偏暗 | `-wex-blfix`，可配 `-wex-blmax=<n>` / `-wex-blmin=<n>` |
| 5 | 长时间熄屏后内屏无法恢复 | `-wex-dcoff`，默认不启用，可能增加空闲功耗 |

## 安装与升级

从 [0.8.2 Release](https://github.com/xiananliu/wex-modified-edition/releases/tag/0.8.2) 下载 `WhateverXe-0.8.2-RELEASE.zip`，把其中的 `WhateverXe.kext` 放入 OpenCore 的 `EFI/OC/Kexts`，并在 `config.plist` 的 `Kernel > Add` 中排在 Lilu 后面。`DEBUG` 包用于排查问题。macOS 里的图形组件请安装官方 [WhateverXe Graphics 0.4.4](https://github.com/b00t0x/WhateverXe-Graphics/releases/tag/v0.4.4)，安装后重启。

从旧版升级时，可移除下游的 VA 回退、Metal 泄漏修复、eDP 训练、DDI 冲突选择和调试探针参数。视频转换和 Metal 命令缓冲泄漏修复随官方 Graphics 0.4.4 提供。

依赖与整机配置见 [WhateverXe 上游安装说明](https://github.com/b00t0x/WhateverXe/blob/0.8.2/README.md#installation) 和 [Dynabook G83 EFI](https://github.com/xiananliu/g83-11gen-hackintosh-tahoe-efi)。许可见 [LICENSE.txt](LICENSE.txt)。
