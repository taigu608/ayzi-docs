---
sidebar_position: 2
description: Learn how to configure keyboard shortcuts with Yazi.
---

import KeymapArrow from './keymap-arrow.md'

# keymap.toml

:::info
如果你想微调默认设置，第一步是[创建你自己的配置文件](/docs/configuration/overview)。
:::

你可以在 `keymap.toml` 文件中更改 Yazi 的键位绑定，该文件由以下 8 个层组成：

- [\[mgr\]](#mgr) - 文件列表。
- [\[tasks\]](#tasks) - 任务管理器。
- [\[spot\]](#spot) - 文件信息查看器。
- [\[pick\]](#pick) - 选择组件。例如文件的"打开方式"。
- [\[input\]](#input) - 输入组件。例如创建、重命名等。
- [\[confirm\]](#confirm) - 确认对话框。例如删除、覆盖等。
- [\[cmp\]](#cmp) - 补全组件。例如"cd"路径补全。
- [\[help\]](#help) - 帮助菜单。

在每个层中，有两个属性：`prepend_keymap` 和 `append_keymap`。
Prepend 插入到[默认键位绑定](https://github.com/sxyazi/yazi/blob/shipped/yazi-config/preset/keymap-default.toml)之前，而 append 插入到它们之后。

由于 Yazi 选择第一个匹配的键来运行，prepend 始终具有比默认值更高的优先级，而 append 始终具有比默认值更低的优先级：

```toml
[mgr]
prepend_keymap = [
	{ on = "<C-a>", run = "my-cmd1", desc = "Single command with `Ctrl + a`" },
]
append_keymap = [
	{ on = [ "g", "b" ], run = "my-cmd2",          desc = "Single command with `g => b`" },
	{ on = "c",          run = [ "cmd1", "cmd2" ], desc = "Multiple commands with `c`" }
]
```

或者以另一种不同的风格：

```toml
[[mgr.prepend_keymap]]
on   = "<C-a>"
run  = "my-cmd1"
desc = "Single command with `Ctrl + a`"

[[mgr.append_keymap]]
on  = [ "g", "b" ]
run = "my-cmd2"
desc = "Single command with `g => b`"

[[mgr.append_keymap]]
on  = "c"
run = [ "my-cmd1", "my-cmd2" ]
desc = "Multiple commands with `c`"
```

但请记住，你只能选择其中一种，不能两者结合使用，因为 TOML 语言不允许这样做：

```toml
[mgr]
prepend_keymap = [
	{ on = "<C-a>", run = "my-cmd1" },
]

[[mgr.append_keymap]]
on  = [ "g", "b" ]
run = "my-cmd2"
```

当你不需要任何默认设置并希望完全自定义键位绑定时，使用 `keymap`，例如：

```toml
[mgr]
keymap = [
	# 这将覆盖所有默认键位绑定，只保留下面的两个。
	{ on = "<C-a>",      run = "my-cmd1" },
	{ on = [ "g", "b" ], run = "my-cmd2" },
]
```

## 键位标记 {#notation}

你可以在每个键位绑定规则的 `on` 中指定一个或多个键，每个键可以用以下标记表示：

| 标记             | 描述       | 标记          | 描述       |
| ---------------- | ---------- | ------------- | ---------- |
| `a` - `z`        | 小写字母   | `A` - `Z`     | 大写字母   |
| `<Space>`        | 空格键     | `<Backspace>` | 退格键     |
| `<Enter>`        | 回车键     | -             | -          |
| `<Left>`         | 左箭头键   | `<Right>`     | 右箭头键   |
| `<Up>`           | 上箭头键   | `<Down>`      | 下箭头键   |
| `<Home>`         | Home 键    | `<End>`       | End 键     |
| `<PageUp>`       | PageUp 键  | `<PageDown>`  | PageDown 键|
| `<Tab>`          | Tab 键     | `<BackTab>`   | Shift + Tab 键 |
| `<Delete>`       | Delete 键  | `<Insert>`    | Insert 键  |
| `<F1>` - `<F19>` | 功能键     | `<Esc>`       | Escape 键  |

你可以为上述键组合以下修饰符：

| 修饰符   | 描述                     |
| -------- | ------------------------ |
| `<S-…>`  | Shift 键。               |
| `<C-…>`  | Ctrl 键。                |
| `<A-…>`  | Alt/Meta 键。            |
| `<D-…>`  | Command/Windows/Super 键。|

例如：

- `<C-a>` 表示 <kbd>Ctrl</kbd> + <kbd>a</kbd>。
- `<C-S-b>` 或 `<C-B>` 表示 <kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>b</kbd>。

请注意：

1. 并非所有终端都支持 `<D-...>` - 如果你想使用它，请确保你的终端支持并启用了 [CSI u][CSI u]。
2. macOS 没有 <kbd>Alt</kbd> 键，所以 `<A-...>` 不起作用。一些终端提供了将 <kbd>Option</kbd> 映射为 <kbd>Alt</kbd> 键的设置，请确保你已启用它。
3. [传统终端键盘协议][Control character]将 `<Tab>` 和 `<C-i>`、`<Enter>` 和 `<C-m>` 等视为同一个键。如果你想区分它们，请确保你的终端支持并启用了 [CSI u][CSI u]。

[CSI u]: https://sw.kovidgoyal.net/kitty/keyboard-protocol/
[Control character]: https://en.wikipedia.org/wiki/Control_character

## [mgr] {#mgr}

### `escape` {#mgr.escape}

取消查找、退出可视模式、清除选择、取消过滤或取消搜索。

| 参数/选项      | 描述           |
| -------------- | -------------- |
| `--all`        | 执行以下所有操作。|
| `--find`       | 取消查找。     |
| `--visual`     | 退出可视模式。 |
| `--select`     | 清除选择。     |
| `--filter`     | 取消过滤。     |
| `--search`     | 取消搜索。     |

默认自动确定操作，指定选项后只执行选定的操作；可以叠加多个选项。

### `quit` {#mgr.quit}

退出进程。

| 参数/选项       | 描述                                                        |
| --------------- | ----------------------------------------------------------- |
| `--no-cwd-file` | 不要将当前目录输出到 `yazi --cwd-file` 指定的文件。        |

### `close` {#mgr.close}

关闭当前标签页；如果是最后一个标签页，则退出进程。

| 参数/选项       | 描述                                                                   |
| --------------- | ---------------------------------------------------------------------- |
| `--no-cwd-file` | 退出时不要将当前目录输出到 `yazi --cwd-file` 指定的文件。             |

### `suspend` {#mgr.suspend}

暂停 Yazi 并返回到父 shell 以继续执行其他任务。

完成这些任务后，使用 shell 的 `fg` 命令发送恢复信号并返回到 Yazi。

### `arrow` {#mgr.arrow}

<KeymapArrow />

### `leave` {#mgr.leave}

返回到悬停文件的父目录，如果没有悬停文件，则返回到当前工作目录的父目录。

### `enter` {#mgr.enter}

进入子目录。

### `back` {#mgr.back}

返回到上一个目录。

### `forward` {#mgr.forward}

前进到下一个目录。

### `seek` {#mgr.seek}

滚动预览面板中的内容。

| 参数/选项 | 描述                                           |
| --------- | ---------------------------------------------- |
| `[n]`     | 使用负值向上查找，使用正值向下查找。           |

### `spot` {#mgr.spot}

使用预设或用户自定义的查看器显示文件信息。

### `cd` {#mgr.cd}

更改当前目录。

| 参数/选项       | 描述                           |
| --------------- | ------------------------------ |
| `[path]`        | 要更改到的路径。               |
| `--interactive` | 使用交互式 UI 输入路径。       |

你可以添加自己的 `g` 系列键来实现简单的书签功能：

```toml
[[mgr.prepend_keymap]]
on   = [ "g", "d" ]
run  = "cd ~/Downloads"
desc = "Cd to ~/Downloads"

[[mgr.prepend_keymap]]
on   = [ "g", "p" ]
run  = "cd ~/Pictures"
desc = "Cd to ~/Pictures"
```

对于 Windows 用户，你还可以使用 `cd` 命令切换驱动器：

```toml
[[mgr.prepend_keymap]]
on   = [ "g", "d" ]
run  = "cd D:"
desc = "Switch to D drive"

[[mgr.prepend_keymap]]
on   = [ "g", "p" ]
run  = 'cd "E:\\Pictures"'  # We need to escape the backslash
desc = 'Cd to E:\Pictures'
```

查看[资源页面](/docs/resources)以获取更全面的书签插件。

### `reveal` {#mgr.reveal}

悬停在指定的文件上。

如果文件不在当前目录中，它将更改当前目录为文件的父目录。

| 参数/选项 | 描述             |
| --------- | ---------------- |
| `[path]`  | 要显示的路径。   |

### `toggle` {#mgr.toggle}

切换悬停文件的选择状态。

| 参数/选项    | 描述           |
| ------------ | -------------- |
| N/A          | 反转选择。     |
| `--state=on` | 选择文件。     |
| `--state=off`| 取消选择文件。 |

### `toggle_all` {#mgr.toggle_all}

切换当前工作目录中所有文件的选择状态。

| 参数/选项    | 描述           |
| ------------ | -------------- |
| N/A          | 反转选择。     |
| `--state=on` | 选择文件。     |
| `--state=off`| 取消选择文件。 |

请注意，`toggle_all --state=off` 只会取消选择 CWD 中的文件，如果你在多个目录中选择了文件，并且想要取消选择所有文件，请使用 [`escape --select`](#mgr.escape)。

### `visual_mode` {#mgr.visual_mode}

进入可视模式。

| 参数/选项 | 描述         |
| --------- | ------------ |
| N/A       | 选择模式。   |
| `--unset` | 取消设置模式。|

### `open` {#mgr.open}

使用 [`[open]` 中的规则](/docs/configuration/yazi#open)打开选定的文件。

| 参数/选项       | 描述                                                       |
| --------------- | ---------------------------------------------------------- |
| `--interactive` | 使用交互式 UI 打开悬停/选定的文件以选择打开方法。         |
| `--hovered`     | 无论选择状态如何，始终打开悬停的文件。                     |

### `yank` {#mgr.yank}

复制选定的文件。

| 参数/选项 | 描述       |
| --------- | ---------- |
| N/A       | 复制模式。 |
| `--cut`   | 剪切模式。 |

### `unyank` {#mgr.unyank}

取消文件的复制状态。

### `paste` {#mgr.paste}

粘贴复制的文件。

| 参数/选项  | 描述                                                                       |
| ---------- | -------------------------------------------------------------------------- |
| `--force`  | 如果目标文件存在，则覆盖它。                                               |
| `--follow` | 复制符号链接指向的文件，而不是链接本身。只能在复制期间使用。               |

### `link` {#mgr.link}

为复制的文件创建符号链接。（这在 Windows 上是一个特权操作，必须以管理员身份运行。）

| 参数/选项    | 描述                           |
| ------------ | ------------------------------ |
| `--relative` | 使用相对路径创建符号链接。     |
| `--force`    | 如果目标文件存在，则覆盖它。   |

### `hardlink` {#mgr.hardlink}

为复制的文件创建硬链接。

| 参数/选项  | 描述                                         |
| ---------- | -------------------------------------------- |
| `--force`  | 如果目标文件存在，则覆盖它。                 |
| `--follow` | 硬链接符号链接指向的文件，而不是符号链接本身。|

### `remove` {#mgr.remove}

在 macOS/Windows 上将文件移动到废纸篓/回收站。对于 Linux，它将遵循 [FreeDesktop.org 废纸篓规范](https://specifications.freedesktop.org/trash-spec/1.0/)。

在 Android 平台上，你只能使用 `--permanently` 选项，因为缺少废纸篓的概念。

| 参数/选项       | 描述                                             |
| --------------- | ------------------------------------------------ |
| `--force`       | 不显示确认对话框，直接删除/丢弃文件。           |
| `--permanently` | 永久删除文件。                                   |
| `--hovered`     | 无论选择状态如何，始终删除悬停的文件。           |

### `create` {#mgr.create}

创建文件或目录。以 `/`（Unix）或 `\`（Windows）结尾表示目录。

| 参数/选项 | 描述                                                           |
| --------- | -------------------------------------------------------------- |
| `--dir`   | 始终创建目录，无论是否以 `/` 或 `\` 结尾。                     |
| `--force` | 如果目标文件存在，直接覆盖它，而不显示确认对话框。             |

### `rename` {#mgr.rename}

重命名文件或目录，或批量重命名（如果选择了多个文件）（默认使用 `$EDITOR` 编辑文件名，详见[为批量重命名指定不同的编辑器](/docs/tips#bulk-editor)）。

- `--hovered`：无论选择状态如何，始终重命名悬停的文件。
- `--force`：如果目标文件存在，直接覆盖它，而不显示确认对话框。
- `--empty`：清空文件名的一部分。
  - `"stem"`：清空主干。例如 `"foo.jpg"` -> `".jpg"`。
  - `"ext"`：清空扩展名。例如 `"foo.jpg"` -> `"foo."`。
  - `"dot_ext"`：清空点和扩展名。例如 `"foo.jpg"` -> `"foo"`。
  - `"all"`：清空整个文件名。例如 `"foo.jpg"` -> `""`。
- `--cursor`：指定重命名输入框的光标位置。
  - `"end"`：文件名的末尾。
  - `"start"`：文件名的开头。
  - `"before_ext"`：文件名扩展名之前。

你还可以将 `--cursor` 与 `--empty` 一起使用，例如，`rename --empty=stem --cursor=start` 将清空文件的主干，并将光标移动到开头。

这会导致文件名 `foo.jpg` 的输入框内容为 `|.jpg`，其中"|"表示光标位置。

### `copy` {#mgr.copy}

复制选定或悬停的文件或目录的路径。

| 参数/选项     | 描述                                       |
| ------------- | ------------------------------------------ |
| `[what]`      | 要复制的内容，请参见下表。                 |
| `--separator` | 路径分隔符，请参见下表。                   |
| `--hovered`   | 无论选择状态如何，始终复制悬停的文件。     |

`[what]` 可以是以下值之一：

| 值                   | 描述                   |
| -------------------- | ---------------------- |
| `"path"`             | 绝对路径。             |
| `"dirname"`          | 父目录的路径。         |
| `"filename"`         | 文件名。               |
| `"name_without_ext"` | 不带扩展名的文件名。   |

`--separator` 可以是以下值之一：

| 值       | 描述                                                    |
| -------- | ------------------------------------------------------- |
| N/A      | 平台特定的分隔符，例如 Windows 的 `\` 和 Unix 的 `/`。 |
| `"unix"` | 对所有平台使用 `/`。                                    |

### `shell` {#mgr.shell}

运行 shell 命令。

| 参数/选项       | 描述                                                                                                                                                |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `[template]`    | 可选，要运行的命令模板。                                                                                                                            |
| `--block`       | 以阻塞方式打开。设置此选项后，Yazi 将隐藏到辅助屏幕，并在主屏幕上显示程序，直到它退出。在此期间，它可以接收 I/O 信号，这对于交互式程序很有用。    |
| `--orphan`      | 即使 Yazi 已退出，也保持进程运行，一旦指定，进程将从任务调度系统中分离。                                                                            |
| `--interactive` | 请求用户交互式输入要运行的命令                                                                                                                      |
| `--cursor`      | 设置交互式命令输入框中光标的初始位置。例如，`shell 'zip -r .zip "$0"' --cursor=7 --interactive` 将光标放在 `.zip` 之前。                           |

你可以在 `[template]` 中使用以下 shell 变量：

- `$n`（Unix）/ `%n`（Windows）：第 N 个选定文件，从 `1` 开始。例如 `$2` 表示第二个选定的文件。
- `$@`（Unix）/ `%*`（Windows）：所有选定的文件，即 `$1`、`$2`、...、`$n`。
- `$0`（Unix）/ `%0`（Windows）：悬停的文件。
- 请注意，这些变量遵循平台特定的差异。例如，Unix shell 需要用引号包裹 `$`，而 Windows 批处理脚本中的 `%` 则不需要。请参阅 `sh` 和 `cmd.exe` 的文档以获取详细信息。

你可以使用选项结束标记（`--`）来避免任何转义 - `--` 后面的所有内容都将被视为原始字符串：

```diff
[[mgr.prepend_keymap]]
on = "d"
- run = "shell 'trash-put \"$@\"'"
+ run = 'shell -- trash-put "$@"'
desc = "Trash selected files"
```

对于复杂的 shell 脚本，你可以使用 TOML 的基本字符串（`'''` 或 `"""`）在多行中编写它们，如[此技巧](/docs/tips#email-selected-files)所示。

### `hidden` {#mgr.hidden}

设置隐藏文件的可见性。

| 参数/选项  | 描述             |
| ---------- | ---------------- |
| `"show"`   | 显示隐藏文件。   |
| `"hide"`   | 隐藏隐藏文件。   |
| `"toggle"` | 切换隐藏状态。   |

### `linemode` {#mgr.linemode}

设置[行模式](/docs/configuration/yazi#mgr.linemode)。

| 参数/选项       | 描述                                                                                                                                                           |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `"none"`        | 无行模式。                                                                                                                                                     |
| `"size"`        | 显示文件的字节大小。请注意，目前只有在 [`sort_by = "size"`](/docs/configuration/yazi#mgr.sort_by) 时才会评估目录大小，这可能会在未来改变。                  |
| `"btime"`       | 显示文件的创建时间。                                                                                                                                           |
| `"mtime"`       | 显示文件的最后修改时间。                                                                                                                                       |
| `"permissions"` | 显示文件的权限，仅在 Unix 类系统上可用。                                                                                                                       |
| `"owner"`       | 显示文件的所有者，仅在 Unix 类系统上可用。                                                                                                                     |

### `search` {#mgr.search}

| 参数/选项 | 描述                                                                       |
| --------- | -------------------------------------------------------------------------- |
| `--via`   | 搜索引擎，可用值：[`fd`][fd]、[`rg`][rg] 和 [`rga`][rga]                  |
| `--args`  | 传递给指定引擎的附加参数，例如 `search --via=fd --args='-e -H'`           |

你可以通过 `fd` 使用空关键字（`""`）进行搜索以实现平铺视图。

<details>
  <summary>演示平铺视图</summary>
	<p>原始帖子：https://github.com/sxyazi/yazi/issues/676#issuecomment-1943494129</p>
	<video src="https://github.com/sxyazi/yazi/assets/17523360/d2c9df9b-b7ef-41ec-889f-26b2f1117cd0" width="100%" controls muted></video>
</details>

或者将键绑定到 `search_do` 命令以快速切换到平铺视图：

```toml
[[mgr.prepend_keymap]]
on   = [ "g", "f" ]
run  = 'search_do --via=fd --args="-d 3"'
desc = "Switch to the flat view with a max depth of 3"
```

[fd]: https://github.com/sharkdp/fd
[rg]: https://github.com/BurntSushi/ripgrep
[rga]: https://github.com/phiresky/ripgrep-all

### `find` {#mgr.find}

在当前工作目录中以交互和增量方式查找文件。

| 参数/选项       | 描述                                                                   |
| --------------- | ---------------------------------------------------------------------- |
| `--previous`    | 查找上一个匹配项。                                                     |
| `--smart`       | 使用智能大小写查找，即如果查询包含大写字符则区分大小写，否则不区分。   |
| `--insensitive` | 使用不区分大小写的查找。                                               |

### `find_arrow` {#mgr.find_arrow}

将光标移动到下一个或上一个匹配项。

| 参数/选项    | 描述                 |
| ------------ | -------------------- |
| `--previous` | 移动到上一个匹配项。 |

### `filter` {#mgr.filter}

| 参数/选项       | 描述                                                                   |
| --------------- | ---------------------------------------------------------------------- |
| `--smart`       | 使用智能大小写过滤，即如果关键字包含大写字符则区分大小写，否则不区分。 |
| `--insensitive` | 使用不区分大小写的过滤。                                               |

### `sort` {#mgr.sort}

- `[by]`：可选，如果未提供，排序方法将保持不变。
  - `"none"`：不排序。
  - `"mtime"`：按最后修改时间排序。
  - `"btime"`：按创建时间排序。
  - `"extension"`：按文件扩展名排序。
  - `"alphabetical"`：按字母顺序排序，例如 `1.md` < `10.md` < `2.md`
  - `"natural"`：按自然顺序排序，例如 `1.md` < `2.md` < `10.md`
  - `"size"`：按文件大小排序。
  - `"random"`：随机排序。
- `--reverse`：以相反顺序显示文件。`--reverse` 或 `--reverse=yes` 启用，`--reverse=no` 禁用。
- `--dir-first`：优先显示目录。`--dir-first` 或 `--dir-first=yes` 启用，`--dir-first=no` 禁用。
- `--translit`：音译文件名以进行排序，详见 [sort_translit](/docs/configuration/yazi#mgr.sort_translit)。`--translit` 或 `--translit=yes` 启用，`--translit=no` 禁用。

### `tab_create` {#mgr.tab_create}

| 参数/选项   | 描述                                 |
| ----------- | ------------------------------------ |
| `[path]`    | 可选，使用指定的路径创建新标签页。   |
| `--current` | 可选，使用当前路径创建新标签页。     |

如果既未指定 `[path]` 也未指定 `--current`，将使用启动目录创建标签页。

### `tab_close` {#mgr.tab_close}

| 参数/选项 | 描述                                 |
| --------- | ------------------------------------ |
| `[n]`     | 关闭位置 `n` 的标签页，从 0 开始。   |

如果你想关闭当前标签页，请改用 [`close`](/docs/configuration/keymap/#mgr.close) 命令。

### `tab_switch` {#mgr.tab_switch}

| 参数/选项    | 描述                                                                           |
| ------------ | ------------------------------------------------------------------------------ |
| `[n]`        | 切换到位置 `n` 的标签页，从 0 开始。                                           |
| `--relative` | 切换到相对于当前标签页位置的标签页。使用此参数时，`n` 的值可以为负数。        |

### `tab_swap` {#mgr.tab_swap}

| 参数/选项 | 描述                                                                     |
| --------- | ------------------------------------------------------------------------ |
| `[n]`     | 将当前标签页与位置 `n` 的标签页交换，负值向前移动标签页，正值向后移动。 |

### `help` {#mgr.help}

打开帮助菜单。

### `plugin` {#mgr.plugin}

请参阅[功能插件](/docs/plugins/overview#functional-plugin)。

### `noop` {#mgr.noop}

如果你想禁用某些预设键位绑定而不重写整个 `keymap`，可以使用虚拟 `noop` 命令。

例如，要禁用 <kbd>g</kbd> ⇒ <kbd>c</kbd> 的默认键位绑定，请使用：

```toml
[[mgr.prepend_keymap]]
on  = [ "g", "c" ]
run = "noop"
```

或者，如果你更喜欢数组风格：

```toml
[[mgr.prepend_keymap]]
on  = [ "g", "c" ]
run = [ "noop" ]  # The array can only have one element and must be "noop"
```

禁用的键在按下时不会触发任何操作，也不会出现在 `which` 组件中。

## [tasks] {#tasks}

### `show` {#tasks.show}

显示任务管理器。

### `close` {#tasks.close}

隐藏任务管理器。

### `arrow` {#tasks.arrow}

<KeymapArrow />

### `inspect` {#tasks.inspect}

检查任务日志：

- 失败的文件操作的 I/O 错误
- 失败的异步插件任务的 Lua 错误
- 后台运行或失败的 shell 任务的实时 stdout/stderr

按 `q` 退出检查视图。

### `cancel` {#tasks.cancel}

取消任务。

### `help` {#tasks.help}

打开帮助菜单。

### `plugin` {#tasks.plugin}

请参阅[功能插件](/docs/plugins/overview#functional-plugin)。

### `noop` {#tasks.noop}

请参阅 [`noop` 命令](#mgr.noop)。

## [spot] {#spot}

### `close` {#spot.close}

隐藏查看器。

### `arrow` {#spot.arrow}

<KeymapArrow />

### `swipe` {#spot.swipe}

| 参数/选项 | 描述                                                                     |
| --------- | ------------------------------------------------------------------------ |
| `[n]`     | 在文件列表中向上或向下滑动 `n` 个文件。负值向上，正值向下。             |

### `copy` {#spot.copy}

从查看器复制内容。

| 参数/选项 | 描述                 |
| --------- | -------------------- |
| `"cell"`  | 复制选定的表格单元格 |

### `plugin` {#spot.plugin}

请参阅[功能插件](/docs/plugins/overview#functional-plugin)。

### `noop` {#spot.noop}

请参阅 [`noop` 命令](#mgr.noop)。

### `help` {#spot.help}

打开帮助菜单。

## [pick] {#pick}

### `close` {#pick.close}

取消选择器。

| 参数/选项  | 描述         |
| ---------- | ------------ |
| `--submit` | 提交选择器。 |

### `arrow` {#pick.arrow}

<KeymapArrow />

### `help` {#pick.help}

打开帮助菜单。

### `plugin` {#pick.plugin}

请参阅[功能插件](/docs/plugins/overview#functional-plugin)。

### `noop` {#pick.noop}

请参阅 [`noop` 命令](#mgr.noop)。

## [input] {#input}

### `close` {#input.close}

取消输入。

| 参数/选项  | 描述       |
| ---------- | ---------- |
| `--submit` | 提交输入。 |

### `escape` {#input.escape}

返回正常模式，或取消输入。

### `move` {#input.move}

向左或向右移动光标。

| 参数/选项        | 描述                                                           |
| ---------------- | -------------------------------------------------------------- |
| `[n]`            | 向左或向右移动光标 `n` 个字符。负值向左，正值向右。           |
| `--in-operating` | 仅在当前正在等待操作时移动光标。                               |

### `backward` {#input.backward}

向后移动到当前或上一个单词的开头。

### `forward` {#input.forward}

向前移动到下一个单词的开头。

| 参数/选项       | 描述                                 |
| --------------- | ------------------------------------ |
| `--end-of-word` | 向前移动到当前或下一个单词的末尾。   |

### `insert` {#input.insert}

进入插入模式。此命令仅在正常模式下可用。

| 参数/选项  | 描述             |
| ---------- | ---------------- |
| `--append` | 在光标后插入。   |

### `visual` {#input.visual}

进入可视模式。此命令仅在正常模式下可用。

### `delete` {#input.delete}

删除选定的字符。此命令仅在正常模式下可用。

| 参数/选项  | 描述                                       |
| ---------- | ------------------------------------------ |
| `--cut`    | 将选定的字符剪切到剪贴板，而不仅仅是删除。 |
| `--insert` | 删除并进入插入模式。                       |

### `yank` {#input.yank}

复制选定的字符。此命令仅在正常模式下可用。

### `paste` {#input.paste}

在光标后粘贴复制的字符。此命令仅在正常模式下可用。

| 参数/选项  | 描述                     |
| ---------- | ------------------------ |
| `--before` | 在光标前粘贴复制的字符。 |

### `undo` {#input.undo}

撤消上一个操作。此命令仅在正常模式下可用。

### `redo` {#input.redo}

重做上一个操作。此命令仅在正常模式下可用。

### `help` {#input.help}

打开帮助菜单。此命令仅在正常模式下可用。

### `backspace` {#input.backspace}

删除光标前的字符。此命令仅在插入模式下可用。

| 参数/选项 | 描述                 |
| --------- | -------------------- |
| `--under` | 删除光标下的字符。   |

### `kill` {#input.kill}

删除指定范围的字符。此命令仅在插入模式下可用。

| 参数/选项    | 描述                       |
| ------------ | -------------------------- |
| `"bol"`      | 向后删除到行首。           |
| `"eol"`      | 向前删除到行尾。           |
| `"backward"` | 向后删除到当前单词的开头。 |
| `"forward"`  | 向前删除到当前单词的末尾。 |

### `plugin` {#input.plugin}

请参阅[功能插件](/docs/plugins/overview#functional-plugin)。此命令仅在正常模式下可用。

### `noop` {#input.noop}

请参阅 [`noop` 命令](#mgr.noop)。

## [confirm] {#confirm}

### `close` {#confirm.close}

取消并关闭确认对话框。

| 参数/选项  | 描述         |
| ---------- | ------------ |
| `--submit` | 提交确认。   |

### `arrow` {#confirm.arrow}

<KeymapArrow />

### `help` {#confirm.help}

打开帮助菜单。

## [cmp] {#cmp}

### `close` {#cmp.close}

隐藏补全菜单。

| 参数/选项  | 描述         |
| ---------- | ------------ |
| `--submit` | 提交补全。   |

### `arrow` {#cmp.arrow}

<KeymapArrow />

### `help` {#cmp.help}

打开帮助菜单。

### `plugin` {#cmp.plugin}

请参阅[功能插件](/docs/plugins/overview#functional-plugin)。

### `noop` {#cmp.noop}

请参阅 [`noop` 命令](#mgr.noop)。

## [help] {#help}

### `close` {#help.close}

隐藏帮助菜单。

### `escape` {#help.escape}

清除过滤器，或隐藏帮助菜单。

### `arrow` {#help.arrow}

<KeymapArrow />

### `filter` {#help.filter}

为帮助项应用过滤器。

### `plugin` {#help.plugin}

请参阅[功能插件](/docs/plugins/overview#functional-plugin)。

### `noop` {#help.noop}

请参阅 [`noop` 命令](#mgr.noop)。
