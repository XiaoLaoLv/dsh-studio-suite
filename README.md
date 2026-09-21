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
# 从 npm 安装（发布后可用）
dsh plugin --profile web add dsh-studio-suite
# 或直接从 GitHub 安装
dsh plugin --profile web add github:XiaoLaoLv/dsh-studio-suite
# 或本地活链接（clone 后改 patch 无需重装）
dsh plugin --profile web add /path/to/dsh-studio-suite
```

- bundle 层热装配（profile `patchReload: live`），安装/卸载全程**无需重启**；浏览器半按页加载，刷新一次页面（Ctrl+F5）即可。
- 卸载：`dsh plugin --profile web remove dsh-studio-suite`（两个组件行一并撤掉）；配置保留在用户设置文档的 `image-studio` / `voice-studio` 命名空间，可手动删除。
- 从 DSH 源码仓库运行时同一条命令走 `pnpm dsh plugin …`；本机没有 dsh 时用 `npx @deepseek-ai/dsh plugin …`（两者只是同一条命令的转发器）。
- **不要与两个工坊的独立 bundle 同时安装**：suite 插入的行 id 与它们单独安装时相同（`image-studio` / `voice-studio`），同名行共存会触发 loader 冲突；先卸一边再装另一边。

## 配置 / Configure

插件页 → studio-suite → 各组件行的配置控件（`plugins.row.config` 槽位，键为 `<bundle 包名>#<行id>`，点开为配置子页）。配置值存于 DSH 用户设置文档（`~/.dsh/settings.yaml`）的 `image-studio` / `voice-studio` 命名空间；密钥字段脱敏不回显。

## 开发 / Development

`dependencies` 以 `link:` 直链两个工坊目录，改工坊源码刷新页面即生效；改本包 patch 由 loader 热装配。发布到 npm 前把 `link:` 换成版本范围（该范围即两个工坊的兼容矩阵）。以后新增家族成员 = patch 加一行 + dependencies 加一项，老用户更新 suite 即得。

## License

MIT
