---
sidebar_position: 1
description: Learn how to configure Yazi's basic functionality.
---

# yazi.toml

:::info
如果你想微调默认设置，第一步是[创建你自己的配置文件](/docs/configuration/overview)。
:::

## [mgr] {#mgr}

### `ratio` {#mgr.ratio}

按比例划分管理器布局，包含 3 个元素的数组。例如：

- `[1, 4, 3]`：父目录占 1/8 宽度，当前目录占 4/8 宽度，预览占 3/8 宽度

将值设置为 `0` 可隐藏相应的面板，但至少有一个面板必须可见（非零）。

### `sort_by` {#mgr.sort_by}

文件排序方法。

- `"none"`：不排序。
- `"mtime"`：按最后修改时间排序。
- `"btime"`：按创建时间排序。
- `"extension"`：按文件扩展名排序。
- `"alphabetical"`：按字母顺序排序，例如 `1.md` < `10.md` < `2.md`
- `"natural"`：按自然顺序排序，例如 `1.md` < `2.md` < `10.md`
- `"size"`：按文件大小排序。
- `"random"`：随机排序。

### `sort_sensitive` {#mgr.sort_sensitive}

区分大小写排序。

- `true`：区分大小写
- `false`：不区分大小写

### `sort_reverse` {#mgr.sort_reverse}

以相反顺序显示文件。

- `true`：反向顺序
- `false`：正常顺序

### `sort_dir_first` {#mgr.sort_dir_first}

优先显示目录。

- `true`：目录优先
- `false`：正常顺序

### `sort_translit` {#mgr.sort_translit}

音译文件名以进行排序（即将 `Â` 替换为 `A`，将 `Æ` 替换为 `AE`，等等），仅在 [`sort_by = "natural"`](#mgr.sort_by) 时可用。

这对于包含匈牙利语字符的文件很有用。

- `true`：启用
- `false`：禁用

### `linemode` {#mgr.linemode}

行模式：在文件列表行的右侧显示与文件相关的信息。

- `"none"`：无行模式。
- `"size"`：显示文件的字节大小。请注意，目前只有在 [`sort_by = "size"`](/docs/configuration/yazi#mgr.sort_by) 时才会评估目录大小，这可能会在未来改变。
- `"btime"`：显示文件的创建时间。
- `"mtime"`：显示文件的最后修改时间。
- `"permissions"`：显示文件的权限，仅在 Unix 类系统上可用。
- `"owner"`：显示文件的所有者，仅在 Unix 类系统上可用。

你也可以指定任意 1 到 20 个字符，并在 UI 插件中扩展它，这意味着你可以通过插件系统实现自己的行模式，如下所示：

```toml
# ~/.config/yazi/yazi.toml
[mgr]
linemode = "size_and_mtime"
```

```lua
-- ~/.config/yazi/init.lua
function Linemode:size_and_mtime()
	local time = math.floor(self._file.cha.mtime or 0)
	if time == 0 then
		time = ""
	elseif os.date("%Y", time) == os.date("%Y") then
		time = os.date("%b %d %H:%M", time)
	else
		time = os.date("%b %d  %Y", time)
	end

	local size = self._file:size()
	return string.format("%s %s", size and ya.readable_size(size) or "-", time)
end
```

### `show_hidden` {#mgr.show_hidden}

显示隐藏文件。

- `true`：显示
- `false`：不显示

### `show_symlink` {#mgr.show_symlink}

在文件名后显示符号链接文件指向的路径。

- `true`：显示
- `false`：不显示

### `scrolloff` {#mgr.scrolloff}

在浏览文件列表时，光标上方和下方保留的文件数量。

如果该值大于屏幕高度的一半（例如 `200`），光标将居中。

### `mouse_events` {#mgr.mouse_events}

字符串数组，插件系统可以接收的鼠标事件类型，可用值：

- `"click"`：鼠标点击
- `"scroll"`：鼠标垂直滚动
- `"touch"`：鼠标水平滚动
- `"move"`：鼠标移动
- `"drag"`：鼠标拖动（某些终端不支持此功能）

如果数组为空，则禁用鼠标。

通常，你不需要更改它，除非你使用的插件需要启用某个事件。

### `title_format` {#mgr.title_format}

终端标题格式，这是一个字符串，可使用以下占位符：

- `{cwd}` - 当前工作目录

如果你不想让 Yazi 自动更新标题，请将其设置为空字符串（`""`）。

## [preview] {#preview}

### `wrap` {#preview.wrap}

在代码预览中自动换行。

- `"yes"`：启用自动换行
- `"no"`：禁用自动换行

### `tab_size` {#preview.tab_size}

制表符（`\t`）的宽度（空格数）。

### `max_width` {#preview.max_width}

图像的最大预览宽度。更改此设置后需要执行 `yazi --clear-cache` 才能生效。

此值也用于预加载图像；值越大，生成的图像缓存越大，消耗的 CPU 越多。

### `max_height` {#preview.max_height}

图像的最大预览高度。更改此设置后需要执行 `yazi --clear-cache` 才能生效。

此值也用于预加载图像；值越大，生成的图像缓存越大，消耗的 CPU 越多。

### `cache_dir` {#preview.cache_dir}

默认使用系统缓存目录，缓存的文件将在重新启动时自动清除。

如果你想让它更持久，可以手动将缓存目录指定为绝对路径。

### `image_delay` {#preview.image_delay}

在开始向终端发送图像预览数据之前，至少等待指定的毫秒数。

这是为了缓解当用户快速浏览文件列表时，某些终端模拟器难以及时渲染 Yazi 发送的图像而导致的延迟。

详见 https://github.com/sxyazi/yazi/pull/1512。

### `image_filter` {#preview.image_filter}

图像缩小时使用的过滤器，可用值：

- `"nearest"` - 最近邻
- `"triangle"` - 线性三角形
- `"catmull-rom"` - Catmull-Rom
- `"lanczos3"` - 窗口大小为 3 的 Lanczos

它们按从快到慢、从差到好的质量排列 - Lanczos3 提供最高质量但也是最慢的。

在此查看示例和基准测试：https://docs.rs/image/0.24.8/image/imageops/enum.FilterType.html#examples

### `image_quality` {#preview.image_quality}

预缓存图像的质量，范围 50-90。

值越大，图像质量越好，但速度越慢，CPU 消耗越多，并且生成的缓存文件越大，占用更多存储空间。

### `ueberzug_scale` / `ueberzug_offset` {#preview.ueberzug_scale}

- ueberzug_scale（浮点数）：Ueberzug 图像缩放比例，`scale>1` 表示放大，`scale<1` 表示缩小。例如，`0.5` 表示缩小到一半。
- ueberzug_offset（`[x, y, width, height]`）：Ueberzug 图像偏移，以单元格为单位。例如，`[0.5, 0.5, -0.5, -0.5]` 表示图像在两个方向上偏移半个单元格，宽度和高度减少半个单元格。

这对于解决 [Überzug++ 图像大小计算的 bug](https://github.com/jstkdng/ueberzugpp/issues/122) 很有用。

如果你的显示器具有 `2.0` 缩放因子，并且在 Hyprland 下的 Wayland 上运行，你可能需要设置 `ueberzug_scale: 0.5`，并根据你的情况调整 `ueberzug_offset` 的值，以解决此问题。

## [opener] {#opener}

配置可在 [`[open]`](#open) 中使用的可用打开器，例如：

```toml
[opener]
play = [
	{ run = 'mpv "$@"', orphan = true, for = "unix" },
	{ run = '"C:\Program Files\mpv.exe" %*', orphan = true, for = "windows" }
]
edit = [
	{ run = '$EDITOR "$@"', block = true, for = "unix" },
	{ run = "%EDITOR% %*",  block = true, for = "windows" },
]
open = [
	{ run = 'xdg-open "$@"', desc = "Open" },
]
# ...
```

可用选项如下：

- `run`：打开选定文件的命令，可使用以下变量：
  - `$n`（Unix）/ `%n`（Windows）：第 N 个选定文件，从 `1` 开始。例如 `$2` 表示第二个选定的文件。
  - `$@`（Unix）/ `%*`（Windows）：所有选定的文件，即 `$1`、`$2`、...、`$n`。
  - `$0`（Unix）/ `%0`（Windows）：悬停的文件。
  - 请注意，这些变量遵循平台特定的差异。例如，Unix shell 需要用引号包裹 `$`，而 Windows 批处理脚本中的 `%` 则不需要。请参阅 `sh` 和 `cmd.exe` 的文档以获取详细信息。
- `block`：以阻塞方式打开。设置此选项后，Yazi 将隐藏到辅助屏幕，并在主屏幕上显示程序，直到它退出。在此期间，它可以接收 I/O 信号，这对于交互式程序很有用。
- `orphan`：即使 Yazi 已退出，也保持进程运行，一旦指定，进程将从任务调度系统中分离。
- `desc`：打开器的描述，显示在交互式组件中，例如"打开方式"和帮助菜单。
- `for`：打开器仅在此系统上可用；如果未指定，则在所有系统上可用。可用值：
  - `unix`：Linux 和 macOS
  - `windows`：Windows
  - `linux`：Linux
  - `macos`：macOS
  - `android`：Android（Termux）

## [open] {#open}

为特定文件设置打开规则。你可以通过 `prepend_rules` 和 `append_rules` 将规则前置或追加到默认规则（详见[配置混合](/docs/configuration/overview#mixing)）：

```toml
[open]
prepend_rules = [
	{ name = "*.json", use = "edit" },

	# 单个规则的多个打开器
	{ name = "*.html", use = [ "open", "edit" ] },
]
append_rules = [
	{ name = "*", use = "my-fallback" },
]
```

如果你的 `append_rules` 包含通配符规则，它们将始终优先于默认通配符规则作为后备。

或者，使用 `rules` 重写整个默认规则：

```toml
[open]
rules = [
	{ mime = "text/*", use = "edit" },
	{ mime = "video/*", use = "play" },

	# { mime = "application/json", use = "edit" },
	{ name = "*.json", use = "edit" },

	# 单个规则的多个打开器
	{ name = "*.html", use = [ "open", "edit" ] },
]
```

可用的规则选项如下：

- `name`：用于匹配文件名的 Glob 表达式。默认不区分大小写，在开头添加 `\s` 使其区分大小写。
- `mime`：用于匹配 mime-type 的 Glob 表达式。默认不区分大小写，在开头添加 `\s` 使其区分大小写。
- `use`：打开器名称，对应于 [`[opener]` 部分](#opener)中的名称。

注意：

- 你可以在文件上 [`spot`](/docs/configuration/keymap#mgr.spot) 以使用默认的 <kbd>Tab</kbd> 键检查其 mime-type。
- 如果 `use` 是包含多个打开器的数组，则这些打开器中的所有命令将被合并。[`open`](/docs/configuration/keymap#mgr.open) 将运行这些命令中的第一个；[`open --interactive`](/docs/configuration/keymap#mgr.open) 将在"打开方式"菜单中列出所有这些命令。

## [tasks] {#tasks}

### `micro_workers` {#tasks.micro_workers}

并发微任务的最大数量。

### `macro_workers` {#tasks.macro_workers}

并发宏任务的最大数量。

### `bizarre_retry` {#tasks.bizarre_retry}

发生奇怪故障时的最大重试次数。

### `suppress_preload` {#tasks.suppress_preload}

从任务列表中排除系统创建的预加载任务，不报告其进度，并且在应用退出确认时不考虑它们。

### `image_alloc` {#tasks.image_alloc}

解码单个图像的最大内存分配限制（字节），`0` 表示无限制。

### `image_bound` {#tasks.image_bound}

`[width, height]` 数组，解码单个图像的最大图像大小（像素），`0` 表示无限制。

## [plugin] {#plugin}

### fetchers {#plugin.fetchers}

:::warning
Fetchers 尚未完成，API 可能会在没有事先通知的情况下发生变化！
:::

TODO

你可以通过 `prepend_fetchers` 和 `append_fetchers` 将新的 fetchers 前置或追加到 `[plugin]` 下的默认 `fetchers`，详见[配置混合](/docs/configuration/overview#mixing)。
以下是单个规则的可用选项：

- `id`（字符串）：Fetcher 的 ID。
- `name`（字符串）：用于匹配文件名的 Glob 表达式。默认不区分大小写，在开头添加 `\s` 使其区分大小写。
- `run`（字符串）：要运行的 Lua 插件的名称。
- `if`（字符串）：基于此条件执行 fetcher。
- `prio`（字符串）：任务的优先级。`high`、`normal` 或 `low` 之一。

### previewers {#plugin.previewers}

你可以通过 `prepend_previewers` 和 `append_previewers` 将新的预览规则前置或追加到 `[plugin]` 下的默认 `previewers`，详见[配置混合](/docs/configuration/overview#mixing)。
以下是单个规则的可用选项：

- `name`（字符串）：用于匹配文件名的 Glob 表达式。默认不区分大小写，在开头添加 `\s` 使其区分大小写。
- `mime`（字符串）：用于匹配 mime-type 的 Glob 表达式。默认不区分大小写，在开头添加 `\s` 使其区分大小写。
- `run`（字符串）：要运行的 Lua 插件的名称。

```toml
[plugin]
prepend_previewers = [
	# HEIC 预览器
	{ mime = "image/heic", run = "heic" },
	# RAF 预览器
	{ name = "*.raf", run = "raf" },
]

append_previewers = [
	# 我的后备预览器
	{ name = "*", run = "binary" },
]
```

如果你的 `append_previewers` 包含通配符 `name` 规则（`"*"` 或 `"*/"`），它们将始终优先于默认通配符规则作为后备。

Yazi 附带了这些预览器插件：

- folder：Yazi 文件系统与预览之间的桥梁
- code：内置代码高亮与预览之间的桥梁，提供异步并发渲染
- json：`jq` 与预览之间的桥梁，提供异步并发渲染
- noop：无操作
- image：内置图像预览的呈现层，提供混合预览功能
- video：`ffmpeg` 与预览之间的桥梁，提供混合预览功能
- pdf：`pdftoppm` 与预览之间的桥梁，提供混合预览功能
- archive：7-Zip 与预览之间的桥梁，提供混合预览和并发渲染功能

如果你想创建自己的预览器，请参阅 [Previewer API](/docs/plugins/overview#previewer)。

### preloaders {#plugin.preloaders}

你可以通过 `prepend_preloaders` 和 `append_preloaders` 将新的预览规则前置或追加到 `[plugin]` 下的默认 `preloaders`，详见[配置混合](/docs/configuration/overview#mixing)。
以下是单个规则的可用选项：

- `name`（字符串）：用于匹配文件名的 Glob 表达式。默认不区分大小写，在开头添加 `\s` 使其区分大小写。
- `mime`（字符串）：用于匹配 mime-type 的 Glob 表达式。默认不区分大小写，在开头添加 `\s` 使其区分大小写。
- `cond`（字符串）：条件表达式 - 只有满足此条件并且满足 `name` 或 `mime` 的规则才会被应用。例如，`A & B` 表示 A 和 B，`A | !B` 表示 A 或非 B。以下是可用的因子：
  - `mime`：此文件具有 mime-type。
- `run`（字符串）：要运行的 Lua 插件的名称。
- `prio`（字符串）：预加载优先级，`low`、`normal` 或 `high`。如果未指定，默认为 `normal`。

```toml
[plugin]
prepend_preloaders = [
	# HEIC 预加载器
	{ mime = "image/heic", run = "heic" },
]
```

Yazi 附带了这些预加载器插件：

- mime：分块预加载文件的 mime-type
- noop：无操作
- image：预加载和缓存图像
- video：预加载和缓存视频
- pdf：预加载和缓存 PDF。

如果你想创建自己的预加载器，请参阅 [Preloader API](/docs/plugins/overview#preloader)。

## [input] {#input}

### `cursor_blink` {#input.cursor_blink}

控制光标闪烁。

- `true`：闪烁。
- `false`：不闪烁。

你可以自定义每个输入的标题和位置。有以下输入：`cd`、`create`、`rename`、`filter`、`find`、`search` 和 `shell`。要更改它们的配置，请在名称和选项之间使用下划线，如 `cd_origin`。

至于位置，它由两部分组成：[Origin](#input.origin) 和 [Offset](#input.offset)。
原点是输入框的左上角，偏移是从此原点的增量。它们共同确定输入框在屏幕上的区域。

### Origin {#input.origin}

有关可用值，请参阅 [`Origin`](/docs/plugins/aliases#origin)。

### Offset {#input.offset}

至于偏移，它是一个 4 元素元组：`(x, y, width, height)`。

### Placeholder {#input.placeholder}

某些输入具有特殊的占位符，将在显示时替换为实际内容：

- cd_title：字符串

  [`cd --interactive`](/docs/configuration/keymap/#mgr.cd) 输入的标题，用于输入目标路径。

- create_title：[字符串, 字符串]

  这是一个 2 元素元组：第一个用于 [`create`](/docs/configuration/keymap/#mgr.create) 输入标题，第二个用于 `create --dir` 命令。

- rename_title：字符串

  [`rename`](/docs/configuration/keymap/#mgr.rename) 输入的标题，用于输入新名称。

- filter_title：字符串

  [`filter`](/docs/configuration/keymap/#mgr.filter) 输入的标题，用于输入关键字。

- find_title：[字符串, 字符串]

  这是一个 2 元素元组：第一个用于 [`find`](/docs/configuration/keymap/#mgr.find)，第二个用于 `find --previous`。

- search_title：字符串

  - `{n}`：当前 [`search`](/docs/configuration/keymap/#mgr.search) 引擎的名称。

- shell_title：[字符串, 字符串]

  这是一个 2 元素元组：第一个用于 [`shell --interactive`](/docs/configuration/keymap/#mgr.shell)，第二个用于 `shell --interactive --block`。

## [confirm] {#confirm}

与 [`[input]`](#input) 部分相同。有几个可用的：`trash`、`delete`、`overwrite` 和 `quit`。

## [pick] {#pick}

与 [`[input]`](#input) 部分相同。可用选择器：`open`。

## [which] {#which}

### `sort_by` {#which.sort_by}

候选项排序方法。

- `"none"`：不排序。
- `"key"`：按键排序。
- `"desc"`：按描述排序。

### `sort_sensitive` {#which.sort_sensitive}

区分大小写排序。

- `true`：区分大小写
- `false`：不区分大小写

### `sort_reverse` {#which.sort_reverse}

以相反顺序显示候选项。

- `true`：反向顺序
- `false`：正常顺序

### `sort_translit` {#which.sort_translit}

音译文件名以进行排序，即将 `Â` 替换为 `A`，将 `Æ` 替换为 `AE`，等等。

这对于包含匈牙利语字符的文件很有用。

- `true`：启用
- `false`：禁用
