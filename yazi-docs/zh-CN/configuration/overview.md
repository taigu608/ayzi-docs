---
id: overview
sidebar_position: 0
sidebar_label: Configuration
description: Learn how to configure Yazi.
---

# 配置

Yazi 有三个配置文件：

- [`yazi.toml`](/docs/configuration/yazi) - 常规配置。
- [`keymap.toml`](/docs/configuration/keymap) - 键位绑定配置。
- [`theme.toml`](/docs/configuration/theme) - 配色方案配置。

你可以在 **_`shipped`_** 标签上找到默认配置文件：[https://github.com/sxyazi/yazi/tree/**_shipped_**/yazi-config/preset](https://github.com/sxyazi/yazi/tree/shipped/yazi-config/preset)。

要覆盖任何默认设置，首先需要将相应的文件（从上面链接的目录）创建到：

- Unix 类系统：`~/.config/yazi/`
- Windows 系统：`%AppData%\yazi\config\`

例如，要更改隐藏文件的可见状态，首先创建一个 `yazi.toml` 文件到：

- Unix 类系统：`~/.config/yazi/yazi.toml`
- Windows 系统：`%AppData%\yazi\config\yazi.toml`

然后[复制所需的部分](https://github.com/sxyazi/yazi/blob/shipped/yazi-config/preset/yazi-default.toml)到其中，这里是 `show_hidden`：

```toml
# yazi.toml
[mgr]
show_hidden = true
```

Yazi 已经在发布版本中预设了这些默认配置，所以你不需要复制整个文件，除非你想完全覆盖它们。

## 配置混合 {#mixing}

你的配置文件中的选项将用于覆盖默认值。然而，对于键位绑定，如果你不想直接覆盖默认值：

```toml
# keymap.toml
[mgr]
keymap = [
	# ...
]
```

而是想在默认配置的基础上自定义你的键位，你可以使用 `prepend_*` 或 `append_*` 目录将它们前置或追加到默认配置（详见 [keymap.toml](/docs/configuration/keymap)）：

```toml
# keymap.toml
[mgr]
prepend_keymap = [
	# ...
]
append_keymap = [
	# ...
]
```

它们也适用于 open、icon、previewer 和 preloader 规则。

## 自定义配置目录 {#custom-directory}

你可以通过导出 `YAZI_CONFIG_HOME` 环境变量来更改 Yazi 配置目录。例如：

```sh
env "YAZI_CONFIG_HOME=~/.config/yazi-alt" yazi
```

将使用 `~/.config/yazi-alt` 作为配置目录启动 Yazi，并且可以在其中拥有自己的 `yazi.toml`、`keymap.toml`、`init.lua` 等文件。
