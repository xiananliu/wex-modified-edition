# wex-modified-edition

本仓库提供面向 Dynabook G83 的 WhateverXe 预编译产物。当前版本为 [0.8.3](https://github.com/xiananliu/wex-modified-edition/releases/tag/0.8.3)，基于上游 [WhateverXe 0.8.3](https://github.com/b00t0x/WhateverXe/releases/tag/0.8.3)，kext 内部版本号同为 `0.8.3`。源码位于 [WhateverXe-Dynabook 的 dynabook-0.8.3 分支](https://github.com/xiananliu/WhateverXe-Dynabook/tree/dynabook-0.8.3)。

## 改进点与使用参数（相对上游 0.8.3）

| # | 解决的问题 | 使用参数 |
| --- | --- | --- |
| 1 | 原生 HDMI 在 4K60 下周期性黑闪 | `-wex-hdmi-scdc-write`，与第 2 项同时启用 |
| 2 | 原生 HDMI 的 4K60 模式无法选择 | `-wex-hdmi-clock600`，与第 1 项同时启用 |
| 3 | 固件遗留异常 CDCLK 导致驱动接管时内屏黑屏 | `-wex-cdclk-sanitize` |
| 4 | 开机时内屏背光偏暗 | `-wex-blfix`，可配 `-wex-blmax=<n>` / `-wex-blmin=<n>` |
| 5 | 长时间熄屏后内屏无法恢复 | `-wex-dcoff`，默认不启用，可能增加空闲功耗 |

## 安装与升级

1. 从 [0.8.3 Release](https://github.com/xiananliu/wex-modified-edition/releases/tag/0.8.3) 下载 `WhateverXe-0.8.3-RELEASE.zip`，把其中的 `WhateverXe.kext` 放入 OpenCore 的 `EFI/OC/Kexts`，并在 `config.plist` 的 `Kernel > Add` 中排在 Lilu 后面。`DEBUG` 包用于排查问题，`SHA256SUMS` 用于校验下载文件。
2. 使用 Lilu 1.7.2 或更新版本，按 [AMFIAnchor 的 OpenCore 配置说明](https://github.com/b00t0x/AMFIAnchor/blob/main/README.md#install-with-opencore) 安装 `AMFIAnchor.kext` 和 `AMFIAnchorInjector.kext`。Injector 的 `ExecutablePath` 留空；从 AMFIPass 1.4.1 Mod 迁移时，移除旧 Mod 的加载项。保持 AMFI 开启，WhateverGreen 与 WhateverXe 不可同时启用。
3. 设置 `csr-active-config` 为 Data 类型的 `03080000`，保留必需的 `-allow3d -disablegfxfirmware` 和所需的 G83 修复参数。
4. macOS 里的图形组件使用官方 [WhateverXe Graphics 0.4.4](https://github.com/b00t0x/WhateverXe-Graphics/releases/tag/v0.4.4)。已安装 0.4.4 的机器无需重复安装；完成配置后重启。

从旧版升级时，可移除下游的 VA 回退、Metal 泄漏修复、eDP 训练、DDI 冲突选择和调试探针参数。视频转换和 Metal 命令缓冲泄漏修复随官方 Graphics 0.4.4 提供。

完整依赖与整机配置见 [WhateverXe 上游安装说明](https://github.com/b00t0x/WhateverXe/blob/0.8.3/README.md#installation) 和 [Dynabook G83 EFI](https://github.com/xiananliu/g83-11gen-hackintosh-tahoe-efi)。Release 包来自源码仓库的 [GitHub Actions 构建](https://github.com/xiananliu/WhateverXe-Dynabook/actions/runs/44755519084)，对应源码提交为 [`825eb47`](https://github.com/xiananliu/WhateverXe-Dynabook/commit/825eb47d934a20a8477db743e44e5c908d95f762)。本仓库中的 `WhateverXe.kext` 与 RELEASE ZIP 内的驱动一致。许可见 [LICENSE.txt](LICENSE.txt)。
