# dsh-studio-suite

Carrier bundle: installs the image & voice workbenches as one suite of two DSH components. · 载体组合包：把图像工坊与语音工坊一次装齐、统一开关。

Zero code: this package only declares a bundle patch whose rows name the two studio packages, which ride in as its dependencies. · 零代码：本包只声明一层 bundle patch，两行指向两个工坊包，它们作为依赖随行进入 profile。

## Components · 包含的组件

| Row | Package | 说明 |
|---|---|---|
| `image-studio` | [dsh-image-studio](https://github.com/XiaoLaoLv/dsh-image-studio) | 图像工坊：文生图 / 图生图 / 画布编辑 / 压缩 |
| `voice-studio` | [dsh-voice-studio](https://github.com/XiaoLaoLv/dsh-voice-studio) | 语音工坊：TTS / ASR / 录音与转写插入 |

## Install · 安装

```sh
dsh plugin --profile web add dsh-studio-suite
```

Or install this package (or its local directory) from the Plugins page. No restart; refresh the page. · 或在插件页安装本包/本地目录，无需重启，刷新页面即可。

**Do not** install the studios' own bundles beside this suite: the loader row ids collide. Remove one side before adding the other. · **不要**把两个工坊的独立 bundle 与本 suite 同时安装：行 id 会冲突，先卸一边再装另一边。

## Configure · 配置

Plugins page → studio-suite → the configure control on each component row (`plugins.row.config`). Values live in the `image-studio` / `voice-studio` namespaces of the user settings document. · 插件页 → studio-suite → 各组件行的配置入口；配置值存于用户设置文档的 `image-studio` / `voice-studio` 命名空间。

## Development · 开发

`dependencies` link the two studio directories (`link:`), so source edits apply after a page refresh. Replace the links with version ranges when publishing. · 依赖以 `link:` 直链两个工坊目录，改源码刷新即生效；发布时换成版本范围。

## License

MIT
