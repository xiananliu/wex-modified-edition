# wex-modified-edition

> WhateverXe 改进版（Dynabook Tiger Lake）的**发布产物仓库**。只放编译好的 kext 与校验文件，不放源码。

源码仓库：[xiananliu/WhateverXe-Dynabook](https://github.com/xiananliu/WhateverXe-Dynabook)（`main` 分支，基于上游 [b00t0x/WhateverXe](https://github.com/b00t0x/WhateverXe) 0.8.0 的下游修改）。

## 下载项说明

| 文件 | 内容 |
| --- | --- |
| `WhateverXe.kext.zip` | 编译好的改进版 kext（含 `Contents/` 完整目录），放入 OpenCore `EFI/OC/Kexts/` 后需在 `config.plist` Kexts 区确保条目存在并按 WEX README 配置 boot-args |

## 依赖（缺一不可）

1. **Lilu**（1.7.2+）
2. **AMFIPass 1.4.1 Mod**（`csr-active-config=03080000` / 0x803，Full SIP 不支持）
3. **[WhateverXe-Graphics](https://github.com/b00t0x/WhateverXe-Graphics/releases)** pkg（进系统后安装一次，提供核显运行组件；macOS Tahoe 需 26.7+）
4. boot-args：见 WhateverXe-Dynabook README「运行时应保留」清单

上游要求的 OC 配置、兼容 GPU 列表、已知问题，以 [上游 README](https://github.com/b00t0x/WhateverXe#installation) 为准。

## 完整 EFI 参考

Toshiba dynabook G83/HS 整机配置见 [xiananliu/g83-11gen-hackintosh-tahoe-efi](https://github.com/xiananliu/g83-11gen-hackintosh-tahoe-efi)。
