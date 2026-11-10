# wex-modified-edition

本仓库存放 WhateverXe 改进版（Dynabook 修改集）的预编译产物。源码与改动手法不在本仓库公开，见项目说明。

## 改进点与使用参数（相对上游 0.8.0）

| # | 解决的问题 | 使用参数 |
| --- | --- | --- |
| 1 | 外接 HDMI 显示器连接稳定 | `-wex-hdmi-scdc-write` |
| 2 | 支持外接 HDMI 4K@60Hz | `-wex-hdmi-clock600` |
| 3 | 内屏点亮更可靠 | `-wex-cdclk-sanitize` |
| 4 | 开机亮度正常 | `-wex-blfix`（可配 `-wex-blmax` / `-wex-blmin`） |
| 5 | 熄屏后可正常唤醒 | `-wex-dcoff` |
| 6 | 提升部分机型 EFI 信息读取兼容性 | 默认生效，无需参数 |
| 7 | App Store / 视频屏保 VP 后处理卡死（TGL） | `-wex-tgl-va-fallback -wex-tgl-va-sites=1` |
| 8 | WindowServer 内存持续增长（MTL 命令缓冲泄漏） | `-wex-tgl-fix-memory-leak` |
| 9 | 内屏启动第二阶段 / 唤醒黑屏 | `-wex-edp-fix-training` |
| 10 | VBT 多子节点映射同一 DDI 冲突 | `-wex-ddi-conflict-select-first` / `-wex-ddi-conflict-select-last`（可选） |

依赖与整机配置：见上游 [b00t0x/WhateverXe](https://github.com/b00t0x/WhateverXe) 与 [xiananliu/g83-11gen-hackintosh-tahoe-efi](https://github.com/xiananliu/g83-11gen-hackintosh-tahoe-efi)。

本仓库版本号与上游对齐：release `0.8.1` 基于上游 WhateverXe `0.8.0` 源码构建（kext 内部 CFBundleVersion 仍为 0.8.0）。

7–10 项（TGL 用户空间修复）仅对物理 `8086:9A49` rev 1 + Sequoia 生效，设备 ID 被覆盖或驱动版本不匹配时不动作；`-wex-tgl-va-sites=1` 是上游 issue #81 唯一实测过的安全值，不要全量放开。
