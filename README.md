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

```sh
# 桌面版 / 浏览器版（推荐）：插件页「添加插件」粘贴 GitHub 规格，
# 或使用桌面版自带的 CLI（版本与运行时一致）：
dsh plugin --profile desktop add github:XiaoLaoLv/dsh-studio-suite
dsh plugin --profile web    add github:XiaoLaoLv/dsh-studio-suite
```

- bundle 层热装配（profile `patchReload: live`），安装/卸载全程**无需重启**；浏览器半按页加载，刷新一次页面（Ctrl+F5）即可。
- 卸载：插件页对 suite 点「卸载」（两个组件行一并撤掉）；配置保留在各行自己的 Config 里，可手动删除。
- **不要与两个工坊的独立 bundle 同时安装**：suite 插入的行 id 与它们单独安装时相同（`image-studio` / `voice-studio`），同名行共存会触发 loader 冲突；先卸一边再装另一边。装 suite 前先卸掉两个独立包。
- **本地 `link:` 目录安装不适用于本包**：pnpm 不会替 `link:` 包安装它自己的依赖，而本包的价值正是“随行依赖”，所以 `dsh plugin add /path/to/dsh-studio-suite` 会装上但两行 import 失败。本地开发请分别安装两个工坊（见各自 README），或先 `git push` 后用上面的 GitHub 规格安装。
- `dependencies` 指向两个工坊仓库的 `main`；要可复现的版本就把 `#main` 换成 `#<commit sha>`（发布到 npm 后则换成版本范围）。
- **npm 命名注意**：`dsh-image-studio` / `dsh-voice-studio` 这两个无作用域名已被他人占用，若要发布请用你自己的作用域（如 `@xiaolaolv/…`），并同步改本包 patch 里的 `name`。

## 配置 / Configure

插件页 → studio-suite → 各组件行的配置控件（`plugins.row.config` 槽位，键为 `<bundle 包名>#<行id>`，点开为配置子页）。配置值是各行的插件 Config（`volatile()` 字段），落在 profile 配置文档 `~/.dsh/profiles/<profile>/cordis.patch.yml` 的对应行 `config:` 节；密钥字段脱敏不回显。

## 兼容性 / Compatibility

适配 DeepSeek Harness `0.2.x` 线（含桌面版 `0.2.1-rc.x`）。两个组件使用 `configForms` 客户端服务与 `volatile()` 插件 Config；suite 自身只用 `insert` 补丁，无版本耦合。

## 开发 / Development

两个工坊在本地各自 `pnpm install` 后独立安装、刷新页面即生效；`git push` 之后 GitHub 规格安装会拿到同一份代码。发布到 npm 前把 `dependencies` 的 `github:…#main` 换成版本范围（该范围即两个工坊的兼容矩阵）。以后新增家族成员 = patch 加一行 + dependencies 加一项，老用户更新 suite 即得。

## License

MIT
