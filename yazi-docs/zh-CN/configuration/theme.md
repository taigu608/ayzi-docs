---
sidebar_position: 3
description: Learn how to configure your Yazi theme.
---

import useBaseUrl from '@docusaurus/useBaseUrl';

# theme.toml

:::tip
如果你正在寻找现成的主题并且不想自己创建，请查看 [yazi-rs/flavors](https://github.com/yazi-rs/flavors) 仓库。
:::

## 类型 {#types}

### Color {#types.color}

颜色。它可以是带有 RGB 值的十六进制格式，例如 `"#484D66"`。或者可以是以下 17 个值之一：

- `"reset"`
- `"black"`
- `"white"`
- `"red"`
- `"lightred"`
- `"green"`
- `"lightgreen"`
- `"yellow"`
- `"lightyellow"`
- `"blue"`
- `"lightblue"`
- `"magenta"`
- `"lightmagenta"`
- `"cyan"`
- `"lightcyan"`
- `"gray"`
- `"darkgray"`

### Style {#types.style}

以类似 `{ fg = "#e4e4e4", bg = "black", ... }` 的格式出现，并支持以下属性：

- fg (Color)：前景色
- bg (Color)：背景色
- bold (Boolean)：粗体
- dim (Boolean)：暗淡（并非所有终端都支持）
- italic (Boolean)：斜体
- underline (Boolean)：下划线
- blink (Boolean)：闪烁
- blink_rapid (Boolean)：快速闪烁
- reversed (Boolean)：反转前景色和背景色
- hidden (Boolean)：隐藏
- crossed (Boolean)：删除线

## [flavor] {#flavor}

详见 [flavor 文档](/docs/flavors/overview)。

### `dark` {#flavor.dark}

深色模式中使用的 Flavor 名称，例如 `"dracula"`。

|      |          |
| ---- | -------- |
| 类型 | `string` |

### `light` {#flavor.light}

浅色模式中使用的 Flavor 名称，例如 `"gruvbox"`。

|      |          |
| ---- | -------- |
| 类型 | `string` |

## [mgr] {#mgr}

### `cwd` {#mgr.cwd}

CWD 文本样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `hovered` {#mgr.hovered}

悬停文件样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `preview_hovered` {#mgr.preview_hovered}

预览面板中的悬停文件样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `find_keyword` {#mgr.find_keyword}

文件名中高亮部分的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `find_position` {#mgr.find_position}

文件名右侧当前文件在所有找到的文件中的位置样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `marker_copied` {#mgr.marker_copied}

复制文件标记样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `marker_cut` {#mgr.marker_cut}

剪切文件标记样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `marker_marked` {#mgr.marker_marked}

可视模式中预选文件的标记样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `marker_selected` {#mgr.marker_selected}

选定文件标记样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `count_copied` {#mgr.count_copied}

复制文件数量的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `count_cut` {#mgr.count_cut}

剪切文件数量的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `count_selected` {#mgr.count_selected}

选定文件数量的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `border_symbol` {#mgr.border_symbol}

边框符号，例如 `"│"`。

|      |          |
| ---- | -------- |
| 类型 | `string` |

### `border_style` {#mgr.border_style}

边框样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `syntect_theme` {#mgr.syntect_theme}

代码预览高亮主题，是 `.tmTheme` 文件的路径。你可以在 GitHub 上[使用"tmTheme"作为关键字](https://github.com/search?q=tmTheme&type=repositories)找到它们。

例如，`"~/Downloads/Dracula.tmTheme"`，使用 flavor 后不可用，因为 flavors 始终使用自己的 tmTheme 文件 `tmtheme.xml`。

|      |          |
| ---- | -------- |
| 类型 | `string` |

## [tabs] {#tabs}

<details>
	<summary>`active` 和 `inactive` 的说明</summary>
	<img src="/webp/tabs-active-explain.webp" loading="lazy" />
</details>

### `active` {#tabs.active}

活动标签页样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `inactive` {#tabs.inactive}

非活动标签页样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `sep_inner` {#tabs.sep_inner}

内部分隔符符号，例如 `{ open = "[", close = "]" }`。

|      |                                   |
| ---- | --------------------------------- |
| 类型 | `{ open: string, close: string }` |

### `sep_outer` {#tabs.sep_outer}

外部分隔符符号，例如 `{ open = "", close = "" }`。

|      |                                   |
| ---- | --------------------------------- |
| 类型 | `{ open: string, close: string }` |

## [mode] {#mode}

### `normal_main` {#mode.normal_main}

正常模式主样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `normal_alt` {#mode.normal_alt}

正常模式备用样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `select_main` {#mode.select_main}

选择模式主样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `select_alt` {#mode.select_alt}

选择模式备用样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `unset_main` {#mode.unset_main}

未设置模式主样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `unset_alt` {#mode.unset_alt}

未设置模式备用样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

## [status] {#status}

<details>
	<summary>`sep_left` 和 `sep_right` 的说明</summary>
	<img src="/webp/status-sep-explain.webp" loading="lazy" />
</details>

### `overall` {#status.overall}

整体状态栏样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `sep_left` {#status.sep_left}

左分隔符符号，例如 `{ open = "", close = "]" }`。

|      |                                   |
| ---- | --------------------------------- |
| 类型 | `{ open: string, close: string }` |

### `sep_right` {#status.sep_right}

右分隔符符号，例如 `{ open = "[", close = "" }`。

|      |                                   |
| ---- | --------------------------------- |
| 类型 | `{ open: string, close: string }` |

### `perm_type` {#status.perm_type}

文件类型符号的样式，例如目录的 `d`、文件的 `-`、符号链接的 `l` 等。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `perm_read` {#status.perm_read}

读权限符号（`r`）的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `perm_write` {#status.perm_write}

写权限符号（`w`）的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `perm_exec` {#status.perm_exec}

执行权限符号（`x`）的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `perm_sep` {#status.perm_sep}

权限分隔符符号（`-`）的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `progress_label` {#status.progress_label}

进度标签样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `progress_normal` {#status.progress_normal}

进度条未处于错误状态时的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `progress_error` {#status.progress_error}

发生错误时进度条的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

## [which] {#which}

### `cols` {#which.cols}

列数。

|      |                   |
| ---- | ----------------- |
| 类型 | `1` \| `2` \| `3` |

### `mask` {#which.mask}

遮罩样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `cand` {#which.cand}

候选键样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `rest` {#which.rest}

剩余键样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `desc` {#which.desc}

描述样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `separator` {#which.separator}

分隔符符号，例如 `" -> "`。

|      |          |
| ---- | -------- |
| 类型 | `string` |

### `separator_style` {#which.separator_style}

分隔符样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

## [confirm] {#confirm}

### `border` {#confirm.border}

边框样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `title` {#confirm.title}

标题样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `content` {#confirm.content}

内容样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `list` {#confirm.list}

列表样式，即内容下方项目列表的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `btn_yes` {#confirm.btn_yes}

是按钮的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `btn_no` {#confirm.btn_no}

否按钮的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `btn_labels` {#confirm.btn_labels}

是和否按钮的标签。

第一个字符串是是按钮的标签，第二个是否按钮的标签。

|      |                    |
| ---- | ------------------ |
| 类型 | `[string, string]` |

## [spot] {#spot}

<details>
	<summary>`tbl_col` 和 `tbl_cell` 的说明</summary>
	<img src="/webp/spot-tbl-explain.webp" loading="lazy" />
</details>

### `border` {#spot.border}

边框样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `title` {#spot.title}

标题样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `tbl_col` {#spot.tbl_col}

表格中选定列的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `tbl_cell` {#spot.tbl_cell}

表格中选定单元格的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

## [notify] {#notify}

### `title_info` {#notify.title_info}

信息标题的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `title_warn` {#notify.title_warn}

警告标题的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `title_error` {#notify.title_error}

错误标题的样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

## [pick] {#pick}

### `border` {#pick.border}

边框样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `active` {#pick.active}

选定项样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `inactive` {#pick.inactive}

未选定项样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

## [input] {#input}

### `border` {#input.border}

边框样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `title` {#input.title}

标题样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `value` {#input.value}

值样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `selected` {#input.selected}

选定值样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

## [cmp] {#cmp}

### `border` {#cmp.border}

边框样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `active` {#cmp.active}

选定项样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `inactive` {#cmp.inactive}

未选定项样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `icon_file` {#cmp.icon_file}

文件图标。

|      |          |
| ---- | -------- |
| 类型 | `string` |

### `icon_folder` {#cmp.icon_folder}

文件夹图标。

|      |          |
| ---- | -------- |
| 类型 | `string` |

### `icon_command` {#cmp.icon_command}

命令图标。

|      |          |
| ---- | -------- |
| 类型 | `string` |

## [tasks] {#tasks}

### `border` {#tasks.border}

边框样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `title` {#tasks.title}

标题样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `hovered` {#tasks.hovered}

悬停项样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

## [help] {#help}

### `on` {#help.on}

键列样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `run` {#help.run}

命令列样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `desc` {#help.desc}

描述列样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `hovered` {#help.hovered}

悬停项样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `footer` {#help.footer}

页脚样式。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`Style`](#types.style) |

### `icon_info` {#help.icon_info}

信息图标。

|      |          |
| ---- | -------- |
| 类型 | `string` |

### `icon_warn` {#help.icon_warn}

警告图标。

|      |          |
| ---- | -------- |
| 类型 | `string` |

### `icon_error` {#help.icon_error}

错误图标。

|      |          |
| ---- | -------- |
| 类型 | `string` |

## [filetype] {#filetype}

为特定文件类型设置文件列表项显示样式，支持通过名称和 mime-type 匹配：

```toml
[filetype]
rules = [
	# Images
	{ mime = "image/*", fg = "yellow" },

	# Videos
	{ mime = "video/*", fg = "magenta" },
	{ mime = "audio/*", fg = "magenta" },

	# Empty files
	{ mime = "inode/empty", fg = "cyan" },

	# Orphan symbolic links
	{ name = "*", is = "orphan", fg = "red" },

	# ...

	# Fallback
	# { name = "*", fg = "white" },
	{ name = "*/", fg = "blue" }
]
```

每个规则都支持完整的 [Style 属性](#types.style)。有两个特殊规则：

- `name = "*"` 匹配所有文件。
- `name = "*/"` 匹配所有目录。

你可以通过 `is` 限制特定类型的文件，注意它必须与 `name` 或 `mime` 一起使用。它接受以下值：

- `block`：块设备
- `char`：字符设备
- `exec`：可执行文件
- `fifo`：FIFO
- `link`：符号链接
- `orphan`：孤立的符号链接
- `sock`：套接字
- `sticky`：设置了粘滞位的文件

## [icon] {#icon}

Yazi 内置支持 [nvim-web-devicons](https://github.com/nvim-tree/nvim-web-devicons)，这是一套丰富的图标可供使用。
如果你想向这组图标添加自己的规则，可以使用 `prepend_*` 和 `append_*` 将规则前置或追加到默认规则（详见[配置混合](/docs/configuration/overview#mixing)）：

```toml
[icon]
prepend_dirs = [
	{ name = "desktop", text = "", fg = "#563d7c" },
	# ...
]
append_exts = [
	{ name = "mp3", text = "", fg = "#00afff" },
	# ...
]
# ...
```

如果你想完全覆盖默认规则，可以这样做：

```toml
[icon]
dirs = [
	{ name = "desktop", text = "", fg = "#563d7c" },
	# ...
]
exts = [
	{ name = "mp3", text = "", fg = "#00afff" },
	# ...
]
# ...
```

每个图标规则包含以下属性：

- `name`（globs、dirs、files、exts）或 `if`（conds）：规则本身，是一个字符串
- `text`：图标文本，是一个字符串
- `fg`：图标颜色，是一个 [Color](/docs/configuration/theme#types.color)

图标按以下优先级匹配：

1. globs：glob 表达式，例如 `{ name = "**/Downloads/*.zip", ... }`
2. dirs：目录名，例如 `{ name = "Desktop", ... }`
3. files：文件名，例如 `{ name = ".bashrc", ... }`
4. exts：扩展名，例如 `{ name = "mp3", ... }`
5. conds：条件，例如 `{ if = "!dir", ... }`

`dirs`、`files` 和 `exts` 在启动时被编译为 HashMap，提供 O(1) 时间复杂度以实现非常快速的查找，这应该可以满足大多数需求。

对于更复杂和精确的规则，例如匹配特定目录中的特定文件，使用 `globs` - 它们总是首先执行以检查 glob 集中是否满足任何规则。
但是，它们比 `dirs`、`files` 和 `exts` 慢得多，因此不建议过度使用它们。

如果以上规则都不匹配，它将回退到 `conds` 以检查是否满足任何特定条件。`conds` 主要用于与文件元数据相关的规则，包括以下条件因子：

- `dir`：文件是目录
- `hidden`：文件是隐藏的
- `link`：文件是符号链接
- `orphan`：文件是孤立的（损坏的符号链接）
- `dummy`：文件是虚拟的（无法加载完整的元数据，可能文件系统不支持它，例如 FUSE）
- `block`：文件是块设备
- `char`：文件是字符设备
- `fifo`：文件是 FIFO
- `sock`：文件是套接字
- `exec`：文件是可执行的
- `sticky`：文件设置了粘滞位

这些条件支持基本的 `|`（或）、`&`（与）、`!`（非）和 `()` 以表示优先级，因此你可以根据需要组合它们，例如：

```toml
[icon]
prepend_conds = [
	{ if = "hidden & dir",  text = "👻" },  # Hidden directories
	{ if = "dir",           text = "📁" },  # Directories
	{ if = "!(dir | link)", text = "📄" },  # Normal files (not directories or symlinks)
]
```
