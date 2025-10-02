---
id: overview
sidebar_position: 0
sidebar_label: Flavors (BETA)
description: Learn how to use Yazi flavors.
---

# Flavors (BETA)

"flavor"是一个预制的 Yazi 主题,而我们通常所说的"theme"是用户自己的主题,即 `~/.config/yazi/theme.toml` 文件。

将它们分离的目的是为了让用户能够更方便地在现有 flavor 的基础上自定义他们的偏好设置,而无需修改那些 flavor 文件。
这使得更新更加容易,因为从 Git 拉取时不会产生冲突。

在幕后,Yazi 会自动将用户的 `theme.toml` 与 flavor 的 `flavor.toml` 合并,并且用户的配置始终优先于 flavor。

## 目录结构 {#structure}

这些 flavor 放置在 Yazi 配置目录的 `flavors` 子目录中,因此位于:

- Unix-like 系统上的 `~/.config/yazi/flavors/`。
- Windows 上的 `%AppData%\yazi\config\flavors\`。

```
~/.config/yazi/
├── flavors/
│   ├── foo.yazi/
│   └── bar.yazi/
└── theme.toml
```

每个 flavor 都是一个使用 [kebab-case](https://developer.mozilla.org/en-US/docs/Glossary/Kebab_case) 命名的目录,以 `.yazi` 结尾,并至少包含以下文件:

```
~/.config/yazi/flavors/bar.yazi/
├── flavor.toml
├── tmtheme.xml
├── README.md
├── preview.png
├── LICENSE
└── LICENSE-tmtheme
```

其中:

- `flavor.toml` 是此 flavor 的配置文件,格式与[用户的 `theme.toml`](/docs/configuration/theme) 一致。
- `tmtheme.xml` 是一个与此 flavor 颜色匹配的 [tmTheme 文件](https://www.sublimetext.com/docs/color_schemes_tmtheme.html),用于代码高亮。
- `README.md` 和 `preview.png` 分别是此 flavor 的描述和预览图片。
- `LICENSE` 和 `LICENSE-tmtheme` 分别是 flavor 和 `tmtheme.xml` 文件的许可证。

## 使用方法 {#usage}

例如,如果你想在 _dark_ 模式下使用 `bar.yazi` flavor,请将你的 `theme.toml` 内容设置为:

```toml
[flavor]
dark = "bar"
```

或在 _light_ 模式下使用:

```toml
[flavor]
light = "bar"
```

如果你想在 _dark_ 和 _light_ 模式下都使用 `bar.yazi` flavor:

```toml
[flavor]
dark  = "bar"
light = "bar"
```

确保你的 `theme.toml` 中除了 `[flavor]` 之外不包含任何其他内容,除非你想覆盖 `bar.yazi` flavor 的某些样式。

## 为什么使用 flavor 而不是 theme? {#why-flavor}

我们推荐使用新的 flavor 格式而不是旧的 theme,因为 flavor:

- 更强大 - 可以随终端自动切换 dark/light 模式
- 更易于更新 - 可以使用 [`ya pkg` 包管理器](/docs/cli#pm)进行管理
- 提供配置合并 - 可以在你自己的 `theme.toml` 中覆盖 `flavor.toml` 的某些样式

## 为什么我的 flavor/theme 不工作? {#why-not-working}

这通常是因为 flavor 或 theme 包含过时的颜色配置。请确保你的 flavor 或 theme 与你的 Yazi 版本兼容:

1. **没有无效字段**

   你可以使用 TOML linter 如 [taplo](https://taplo.tamasfe.dev) 来检查你的 `theme.toml` 或 `flavor.toml` 是否包含任何无效字段:

   ```sh
   taplo check --schema https://yazi-rs.github.io/schemas/theme.json flavor.toml
   ```

2. **包含你想要定制样式的组件字段**

   Yazi theme 或 flavor 可以选择性地只定制某些组件的样式,也就是说,它不一定要定制所有内容。对于没有指定样式的组件,将应用[预设配置](https://github.com/sxyazi/yazi/tree/shipped/yazi-config/preset)。

   请参阅 [theme.toml](/docs/configuration/theme) 以获取可用字段的完整列表,并确保你的 `theme.toml` 或 `flavor.toml` 包含它们。

## 制作 flavor {#cooking}

请使用我们的 [flavor-template](https://github.com/yazi-rs/flavor-template) 仓库作为起点来创建你自己的 flavor。
