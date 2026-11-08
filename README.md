# dsh-studio-suite

Carrier bundle: installs the image & voice workbenches as one suite of two DSH components. 载体组合包：把图像工坊与语音工坊作为两个组件一次装齐、统一开关。

Zero code: this package only declares a bundle patch whose rows name the two studio packages, which ride in as its dependencies. 零代码：本包只声明一层 bundle patch，两行指向两个工坊包，它们作为依赖随行进入 profile；suite 自身没有 loader 行、没有宿主或浏览器逻辑。

## 组件 / Components

| Row | Package | 说明 |
|---|---|---|
| `image-studio` | [dsh-image-studio](https://github.com/XiaoLaoLv/dsh-image-studio) | 图像工坊：文生图 / 图生图 / 画布编辑 / 压缩 / 发送到对话 |
| `voice-studio` | [dsh-voice-studio](https://github.com/XiaoLaoLv/dsh-voice-studio) | 语音工坊：TTS 三方言 / ASR 两平面 / 录音与转写插入 |

每行在插件页独立启停；suite 顶部的总开关与「卸载」作用于全部行。

## 安装 / Install

本包是**发布型载体**：`dependencies` 必须是注册表里的包（版本范围），不能是 git / tarball / link。原因见下。

```sh
# 两个工坊发布到自己 scope 后（本包 dependencies 也换成同样的范围）：
dsh plugin --profile desktop add @your-scope/dsh-studio-suite
# 或在桌面版插件页「添加插件」里填同一个包名
```

- bundle 层热装配（profile `patchReload: live`），安装/卸载全程**无需重启**；浏览器半按页加载，刷新一次页面（Ctrl+F5）即可。
- 卸载：插件页对 suite 点「卸载」（两个组件行一并撤掉）；配置保留在各行自己的 Config 里，可手动删除。
- **不要与两个工坊的独立 bundle 同时安装**：suite 插入的行 id 与它们单独安装时相同（`image-studio` / `voice-studio`），同名行共存会触发 loader 冲突；先卸一边再装另一边。装 suite 前先卸掉两个独立包。
- **为什么不能用 `github:` 或本地目录装本包**：
  - pnpm 11 默认开启 `blockExoticSubdeps`，**依赖的依赖**不允许来自 git / tarball（报 `ERR_PNPM_EXOTIC_SUBDEP`）。suite 从 GitHub 装上后，它的两个 `github:` 子依赖会被拒，一行都起不来。
  - 本地 `link:` 目录安装时 pnpm 又不会替 link 包安装它自己的依赖，两行 import 同样失败。
  - 结论：想“一次装齐”，只能先把两个工坊发到注册表。

## 配置 / Configure

插件页 → studio-suite → 各组件行的配置控件（`plugins.row.config` 槽位，键为 `<bundle 包名>#<行id>`，点开为配置子页）。配置值是各行的插件 Config（`volatile()` 字段），落在 profile 配置文档 `~/.dsh/profiles/<profile>/cordis.patch.yml` 的对应行 `config:` 节；密钥字段脱敏不回显。

## 兼容性 / Compatibility

适配 DeepSeek Harness `0.2.x` 线（含桌面版 `0.2.1-rc.x`）。两个组件使用 `configForms` 客户端服务与 `volatile()` 插件 Config；suite 自身只用 `insert` 补丁，无版本耦合。

## 开发与发布 / Development & Publish

- 本地开发：两个工坊各自 `pnpm install` 后**分别**安装（见各自 README），改源码刷新页面即生效；不要用本包做本地联调。
- 发布前：把两个工坊的 `package.json` 改成你的 scope（无作用域名 `dsh-image-studio` / `dsh-voice-studio` 已被他人占用），同步改它们 patch 里的 `name`，再改本包 patch 的 `name`。
- 顺序：`npm publish --access public` 两个工坊 → 把本包 `dependencies` 的 `link:` 换成 `^0.3.0` / `^0.8.0` → 发布本包。
- 以后新增家族成员 = patch 加一行 + dependencies 加一项，老用户更新 suite 即得。
- 备注：`github:` 子依赖在 pnpm 10 上可行，但桌面版内置 pnpm 11 默认拒绝，因此不作为分发方式。

## License

MIT
