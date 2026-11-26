# wex-modified-edition

本仓库提供面向 Dynabook G83 的 WhateverXe 预编译产物。当前版本为 [0.8.5 eDP 修复版](https://github.com/xiananliu/wex-modified-edition/releases/tag/0.8.5-edp-fix)，基于上游 [WhateverXe 0.8.5](https://github.com/b00t0x/WhateverXe/releases/tag/0.8.5)，kext 内部版本号为 `0.8.5`。

## 本次更新

- 修复部分 Tiger Lake 内屏在驱动接管时因 eDP 速率协商失败而黑屏的问题。驱动从面板速率表查找请求的 1.62、2.7 或 5.4 Gbps 对应索引，使用传统速率编码的链路继续走原路径。
- 在内屏开始链路训练前检查通道数，避免无效值导致驱动越界。SHP14B8 已实机点亮，维护者的 G83 也已完成启动显示验证。[SHP14B8 问题记录](https://github.com/xiananliu/g83-11gen-hackintosh-tahoe-efi/issues/8)
- 配套使用官方 WhateverXe Graphics 0.4.5。此前使用 0.4.4 的用户需更新系统内的 Graphics 安装包。

## 改进点与使用参数（相对上游 0.8.5）

| # | 解决的问题 | 使用参数 |
| --- | --- | --- |
| 1 | 原生 HDMI 在 4K60 下周期性黑闪 | `-wex-hdmi-scdc-write`，与第 2 项同时启用 |
| 2 | 原生 HDMI 的 4K60 模式无法选择 | `-wex-hdmi-clock600`，与第 1 项同时启用 |
| 3 | 固件遗留异常 CDCLK 导致驱动接管时内屏黑屏 | `-wex-cdclk-sanitize` |
| 4 | 开机时内屏背光偏暗 | `-wex-blfix`，可配 `-wex-blmax=<n>` / `-wex-blmin=<n>` |
| 5 | 长时间熄屏后内屏无法恢复 | `-wex-dcoff`，默认不启用，可能增加空闲功耗 |
| 6 | Tiger Lake 内屏使用 eDP 速率表时，驱动接管后黑屏 | `-wex-edp-rate-auto`，G83 推荐启动配置默认启用 |

## 安装与升级

1. 下载 `WhateverXe-0.8.5-edp-fix-RELEASE.zip`，把其中的 `WhateverXe.kext` 放入 OpenCore 的 `EFI/OC/Kexts`，并在 `config.plist` 的 `Kernel > Add` 中排在 Lilu 后面。`DEBUG` 包用于排查问题，`SHA256SUMS` 用于校验下载文件。[发布页和下载文件](https://github.com/xiananliu/wex-modified-edition/releases/tag/0.8.5-edp-fix)
2. 使用 Lilu 1.7.2 或更新版本，按 [AMFIAnchor 的 OpenCore 配置说明](https://github.com/b00t0x/AMFIAnchor/blob/main/README.md#install-with-opencore) 安装 `AMFIAnchor.kext` 和 `AMFIAnchorInjector.kext`。Injector 的 `ExecutablePath` 留空；从 AMFIPass 1.4.1 Mod 迁移时，移除旧 Mod 的加载项。保持 AMFI 开启，WhateverGreen 与 WhateverXe 不可同时启用。
3. 设置 `csr-active-config` 为 Data 类型的 `03080000`，保留 `-allow3d -disablegfxfirmware` 和所需的 G83 修复参数。在 `config.plist` 的 `NVRAM → Add → boot-args` 加入 `-wex-edp-rate-auto`，可与 `-wex-cdclk-sanitize` 同时使用。
4. macOS 里的图形组件使用官方 [WhateverXe Graphics 0.4.5](https://github.com/b00t0x/WhateverXe-Graphics/releases/tag/v0.4.5)。已安装 0.4.5 的机器无需重复安装；完成配置后重启。

从旧版升级时，可移除旧版 VA 回退、Metal 泄漏修复、`-wex-edp-fix-training` 等实验参数、DDI 冲突选择和调试探针参数。视频转换和 Metal 命令缓冲泄漏修复随官方 Graphics 0.4.5 提供。

完整依赖与整机配置见 [WhateverXe 上游安装说明](https://github.com/b00t0x/WhateverXe/blob/0.8.5/README.md#installation) 和 [Dynabook G83 EFI](https://github.com/xiananliu/g83-11gen-hackintosh-tahoe-efi)。请从当前发布页下载 eDP 修复版，仓库内的 `WhateverXe.kext` 对应原 0.8.5 发布。许可见 [LICENSE.txt](LICENSE.txt)。

构建源码为 `dynabook-0.8.5` 分支的 `84b6c29`，CI 已通过 CPU 测试及 RELEASE、DEBUG 构建。
