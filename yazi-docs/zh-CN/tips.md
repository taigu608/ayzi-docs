---
sidebar_position: 9
description: A few helpful tips for using Yazi.
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import useBaseUrl from '@docusaurus/useBaseUrl';

# 技巧

这些技巧需要事先了解 Yazi 配置文件。

如果你是第一次使用 Yazi,请先阅读我们的[配置](/docs/configuration/overview)和[插件](/docs/plugins/overview)文档。

## 完整边框 {#full-border}

<img src={useBaseUrl("/webp/full-border.webp")} width="600" />

已移至:https://github.com/yazi-rs/plugins/tree/main/full-border.yazi

## 进入 shell {#dropping-to-shell}

在你的 `keymap.toml` 中添加此快捷键绑定:

```toml
[[mgr.prepend_keymap]]
on   = "!"
for  = "unix"
run  = 'shell "$SHELL" --block'
desc = "Open $SHELL here"

# If you also using Yazi on Windows:
[[mgr.prepend_keymap]]
on   = "!"
for  = "windows"
run  = 'shell "powershell.exe" --block'
desc = "Open PowerShell here"
```

## 通过一次 <kbd>Esc</kbd> 按键关闭输入 {#close-input-by-esc}

你可以在 `keymap.toml` 中将输入组件的 <kbd>Esc</kbd> 从默认的 `escape` 更改为 `close` 命令:

```toml
[[input.prepend_keymap]]
on   = "<Esc>"
run  = "close"
desc = "Cancel input"
```

直接退出输入,而不进入 Vi 模式,使其行为像常规输入框。

## 智能 enter:`open` 文件或 `enter` 目录,全在一个键中 {#smart-enter}

已移至:https://github.com/yazi-rs/plugins/tree/main/smart-enter.yazi

## 智能粘贴:无需进入目录即可 `paste` 文件 {#smart-paste}

已移至:https://github.com/yazi-rs/plugins/tree/main/smart-paste.yazi

## 智能标签:创建标签并进入悬停的目录 {#smart-tab}

将这些行保存为 `~/.config/yazi/plugins/smart-tab.yazi/main.lua`:

```lua
--- @sync entry
return {
	entry = function()
		local h = cx.active.current.hovered
		ya.emit("tab_create", h and h.cha.is_dir and { h.url } or { current = true })
	end,
}
```

然后在你的 `keymap.toml` 中将其绑定到 <kbd>t</kbd> 键:

```toml
[[mgr.prepend_keymap]]
on   = "t"
run  = "plugin smart-tab"
desc = "Create a tab and enter the hovered directory"
```

## 智能切换:如果正在切换的标签不存在则创建标签 {#smart-switch}

将这些行保存为 `~/.config/yazi/plugins/smart-switch.yazi/main.lua`:

```lua
--- @sync entry
local function entry(_, job)
	local cur = cx.active.current
	for _ = #cx.tabs, job.args[1] do
		ya.emit("tab_create", { cur.cwd })
		if cur.hovered then
			ya.emit("reveal", { cur.hovered.url })
		end
	end
	ya.emit("tab_switch", { job.args[1] })
end

return { entry = entry }
```

然后在你的 `keymap.toml` 中将其绑定到 <kbd>2</kbd> 键:

```toml
[[mgr.prepend_keymap]]
on   = "2"
run  = "plugin smart-switch 1"
desc = "Switch or create tab 2"
```

## 特定文件夹规则 {#folder-rules}

你可以通过 [DDS 提供的 `cd` 事件](/docs/dds#cd)订阅目录更改事件,然后执行任何你想要的操作,例如为特定目录设置不同的排序方法。

以下代码演示了使 `Downloads` 目录按修改时间排序,而其他目录按字母顺序排序。将这些行保存为 `~/.config/yazi/plugins/folder-rules.yazi/main.lua`:

```lua
local function setup()
	ps.sub("cd", function()
		local cwd = cx.active.current.cwd
		if cwd:ends_with("Downloads") then
			ya.emit("sort", { "mtime", reverse = true, dir_first = false })
		else
			ya.emit("sort", { "alphabetical", reverse = false, dir_first = true })
		end
	end)
end

return { setup = setup }
```

然后在你的 `~/.config/yazi/init.lua` 中启用它:

```lua
require("folder-rules"):setup()
```

感谢 [@tianze0926 分享它](https://github.com/sxyazi/yazi/issues/623#issuecomment-2096270843)。

## 特定文件夹的 previewer 和 preloader {#folder-previewer}

除了 `mime` 规则外,Yazi 还有 pre\{viewer,loader} 的 `name` 规则,它接受一个 glob 表达式。
这允许为不同的目录灵活创建不同的 pre\{viewer,loader} 规则。

例如,你可以为像 `/remote` 这样的远程挂载点使用 `noop` 内置 preloader,在该目录中禁用预加载:

```toml
# yazi.toml
[[plugin.prepend_preloaders]]
name = "/remote/**"
run  = "noop"
```

如果你想禁用所有预设的 previewers、preloaders:

```toml
# yazi.toml
[plugin]
preloaders = []
previewers = []
```

## 通过 [`dragon`](https://github.com/mwh/dragon) 拖放 {#drag-and-drop}

原始帖子:https://github.com/sxyazi/yazi/discussions/327

```toml
[[mgr.prepend_keymap]]
on  = "<C-n>"
run = 'shell -- dragon -x -i -T "$1"'
```

## 设置壁纸

要使用 "Open with" 菜单(<kbd>O</kbd> 键默认)设置壁纸,通过为你的桌面环境选择适当的命令在 `yazi.toml` 中添加 `set-wallpaper` opener:

```toml
# Linux: Hyprland + Hyprpaper
[[opener.set-wallpaper]]
run  = 'hyprctl hyprpaper reload ,"$0"'
for  = "linux"
desc = "Set as wallpaper"

# Linux: Swaybg
[[opener.set-wallpaper]]
run  = 'killall swaybg; swaybg -m fill -i "$0"'
for  = "linux"
desc = "Set as wallpaper"
orphan = true

# macOS
[[opener.set-wallpaper]]
run = '''
	osascript -e 'on run {img}' -e 'tell application "System Events" to set picture of every desktop to img' -e 'end run' "$0"
'''
for  = "macos"
desc = "Set as wallpaper"
```

然后将 `set-wallpaper` opener 应用于图像文件:

```toml
# yazi.toml
[[open.prepend_rules]]
mime = "image/*"
use  = [ "set-wallpaper", "open" ]
```

或者,你也可以使用快捷键更改壁纸,例如 <kbd>Ctrl</kbd> + <kbd>w</kbd>:

```toml
# keymap.toml
[[mgr.prepend_keymap]]
on   = "<C-w>"
for  = "linux"
run  = 'shell -- hyprctl hyprpaper reload ,"$0"'
desc = "Set hovered file as wallpaper"
```

上面的示例适用于 Hyprland + Hyprpaper,根据需要适配你的相应 DE 的命令。

## Linux:复制选定的文件到系统剪贴板同时进行 yank {#selected-files-to-clipboard}

Yazi 允许将多个命令绑定到一个键,因此你可以将 <kbd>y</kbd> 设置为不仅执行 `yank` 还运行 shell 脚本:

```toml
[[mgr.prepend_keymap]]
on  = "y"
run = [ 'shell -- echo "$@" | xclip -i -selection clipboard -t text/uri-list', "yank" ]
```

上面适用于 X11,还有一个 Wayland 版本(感谢 [@hurutparittya 在 Yazi 的 discord 服务器上分享这个](https://discord.com/channels/1136203602898194542/1136203604076802092/1188498323867455619)):

```toml
[[mgr.prepend_keymap]]
on  = "y"
run = [ 'shell -- for path in "$@"; do echo "file://$path"; done | wl-copy -t text/uri-list', "yank" ]
```

## `cd` 回到当前 Git 仓库的根目录 {#cd-to-git-root}

```toml
[[mgr.prepend_keymap]]
on = [ "g", "r" ]
run = 'shell -- ya emit cd "$(git rev-parse --show-toplevel)"'
```

感谢 [@aidanzhai 在 Yazi 的 telegram 群组中分享它](https://t.me/yazi_rs/3325/15373)。

## Unix:添加字幕到正在运行的 MPV {#mpv-subtitle}

在你的 `~/.config/yazi/yazi.toml` 中添加这些行:

```toml
[[opener.add-sub]]
run  = ''' echo sub-add "'$0'" | socat - /tmp/mpv.sock '''
desc = "Add sub to MPV"

[[open.prepend_rules]]
name = "*.{ass,srt,ssa,sty,sup,vtt}"
use  = [ "add-sub", "edit" ]
```

为使其工作,请确保你已:

1. 安装了 `socat` 并且可以在你的 `$PATH` 中找到
2. 启用并配置 ipc socket 到 `/tmp/mpv.sock`,即在你的 `~/.config/mpv/mpv.conf` 中包含:
   ```
   input-ipc-server=/tmp/mpv.sock
   ```
   更多信息请参阅 [`--input-ipc-server` 的文档](https://mpv.io/manual/stable/#options-input-ipc-server)。

## 最大化预览窗格 {#max-preview}

已移至:https://github.com/yazi-rs/plugins/tree/main/toggle-pane.yazi

## 隐藏预览窗格 {#hide-preview}

已移至:https://github.com/yazi-rs/plugins/tree/main/toggle-pane.yazi

## 在父目录中导航而不离开 CWD {#parent-arrow}

将这些行保存为 `~/.config/yazi/plugins/parent-arrow.yazi/main.lua`:

<Tabs>
  <TabItem value="classic" label="Classic" default>

```lua
--- @sync entry
local function entry(_, job)
	local parent = cx.active.parent
	if not parent then return end

	local target = parent.files[parent.cursor + 1 + job.args[1]]
	if target and target.cha.is_dir then
		ya.emit("cd", { target.url })
	end
end

return { entry = entry }
```

  </TabItem>
  <TabItem value="skip-files" label="Skip files">

```lua
--- @sync entry
local function entry(_, job)
	local parent = cx.active.parent
	if not parent then return end

	local offset = tonumber(job.args[1])
	if not offset then return ya.err(job.args[1], 'is not a number') end

	local start = parent.cursor + 1 + offset
	local end_ = offset < 0 and 1 or #parent.files
	local step = offset < 0 and -1 or 1
	for i = start, end_, step do
		local target = parent.files[i]
		if target and target.cha.is_dir then
			return ya.emit("cd", { target.url })
		end
	end
end

return { entry = entry }
```

  </TabItem>
</Tabs>

然后在你的 `keymap.toml` 中为 <kbd>K</kbd> 和 <kbd>J</kbd> 键绑定它:

```toml
[[mgr.prepend_keymap]]
on  = "K"
run = "plugin parent-arrow -1"

[[mgr.prepend_keymap]]
on  = "J"
run = "plugin parent-arrow 1"
```

## 如果打开多个标签则在退出前确认 {#confirm-quit}

将这些行保存为 `~/.config/yazi/plugins/confirm-quit.yazi/main.lua`:

```lua
local count = ya.sync(function() return #cx.tabs end)

local function entry()
	if count() < 2 then
		return ya.emit("quit", {})
	end

	local yes = ya.confirm {
		pos = { "center", w = 60, h = 10 },
		title = "Quit?",
		content = ui.Text("There are multiple tabs open. Are you sure you want to quit?"):wrap(ui.Wrap.YES),
	}
	if yes then
		ya.emit("quit", {})
	end
end

return { entry = entry }
```

接下来,在你的 `keymap.toml` 中将其绑定到 <kbd>q</kbd> 键:

```toml
[[mgr.prepend_keymap]]
on  = "q"
run = "plugin confirm-quit"
```

感谢 [@lpnh 分享它](https://github.com/sxyazi/yazi/issues/2267#issuecomment-2624805134)。

## 无状态栏 {#no-status-bar}

<img src={useBaseUrl("/webp/no-status-bar.webp")} width="600" />

已移至:https://github.com/yazi-rs/plugins/tree/main/no-status.yazi

## 在状态栏中显示符号链接 {#symlink-in-status}

<img src={useBaseUrl("/webp/symlink-in-status.webp")} width="600" />

在你的 `~/.config/yazi/init.lua` 中添加以下代码:

```lua
Status:children_add(function(self)
	local h = self._current.hovered
	if h and h.link_to then
		return " -> " .. tostring(h.link_to)
	else
		return ""
	end
end, 3300, Status.LEFT)
```

## 在状态栏中显示文件的用户/组 {#user-group-in-status}

<img src={useBaseUrl("/webp/owner.webp")} width="600" />

在你的 `~/.config/yazi/init.lua` 中添加以下代码:

```lua
Status:children_add(function()
	local h = cx.active.current.hovered
	if not h or ya.target_family() ~= "unix" then
		return ""
	end

	return ui.Line {
		ui.Span(ya.user_name(h.cha.uid) or tostring(h.cha.uid)):fg("magenta"),
		":",
		ui.Span(ya.group_name(h.cha.gid) or tostring(h.cha.gid)):fg("magenta"),
		" ",
	}
end, 500, Status.RIGHT)
```

## 在标题中显示用户名和主机名 {#username-hostname-in-header}

<img src={useBaseUrl("/webp/hostname-in-header.webp")} width="600" />

在你的 `~/.config/yazi/init.lua` 中添加以下代码:

```lua
Header:children_add(function()
	if ya.target_family() ~= "unix" then
		return ""
	end
	return ui.Span(ya.user_name() .. "@" .. ya.host_name() .. ":"):fg("blue")
end, 500, Header.LEFT)
```

## macOS:使用系统 Quick Look 预览文件 {#macos-quick-look}

```toml
[[mgr.prepend_keymap]]
on = "<C-p>"
run = 'shell -- qlmanage -p "$@"'
```

感谢 [@UncleGravity 在 Yazi 的 discord 服务器上分享它](https://discord.com/channels/1136203602898194542/1146658361740369960/1293471643959558156)。

## 为批量重命名指定不同的编辑器 {#bulk-editor}

对于批量重命名,Yazi 在你的 [`[open]`](/docs/configuration/yazi#open) 规则中找到第一个匹配的 opener:

|         | 值                  |
| ------- | ------------------- |
| `block` | `true`              |
| `name`  | `"bulk-rename.txt"` |
| `mime`  | `"text/plain"`      |

用作编辑文件列表的编辑器。

默认情况下,这与你用于打开普通文本文件的编辑器匹配,如果你想使用不同的编辑器:

```toml
# ~/.config/yazi/yazi.toml
[[opener.bulk-rename]]
run   = 'hx "$@"'
block = true

[[open.prepend_rules]]
name = "bulk-rename.txt"
use  = "bulk-rename"
```

## Linux:Yazi 作为你的系统文件选择器 {#system-chooser}

`xdg-desktop-portal-termfilechooser` 后端允许你用 Yazi 替换默认的文件选择器,提供与应用程序(如 Firefox)的无缝集成。

有关安装步骤,请参阅[安装指南](https://github.com/hunkyburrito/xdg-desktop-portal-termfilechooser?tab=readme-ov-file#installation)和那里提供的其他说明。

## Helix 中的文件树选择器与 Zellij {#helix-with-zellij}

Yazi 可以用作文件选择器,在你当前的 Helix 实例(在 Zellij 会话中运行)中浏览和打开文件。

在你的 Helix 配置中添加快捷键,例如 <kbd>Ctrl</kbd> + <kbd>y</kbd>:

<Tabs>
  <TabItem value="helix-stable" label="Helix <= 25.01.1" default>

```toml
# ~/.config/helix/config.toml
[keys.normal]
C-y = ":sh zellij run -n Yazi -c -f -x 10% -y 10% --width 80% --height 80% -- bash ~/.config/helix/yazi-picker.sh open"
```

  </TabItem>
  <TabItem value="helix-nightly" label="Helix Nightly (> 25.01.1)">

```toml
# ~/.config/helix/config.toml
[keys.normal]
C-y = ":sh zellij run -n Yazi -c -f -x 10%% -y 10%% --width 80%% --height 80%% -- bash ~/.config/helix/yazi-picker.sh open %{buffer_name}"
```

  </TabItem>
</Tabs>

如果你还想在分屏窗格中打开文件,可以定义额外的快捷键绑定:

<Tabs>
  <TabItem value="helix-stable" label="Helix <= 25.01.1" default>

```toml
# ~/.config/helix/config.toml
[keys.normal.C-y]
# Open the file(s) in the current window
y = ":sh zellij run -n Yazi -c -f -x 10% -y 10% --width 80% --height 80% -- bash ~/.config/helix/yazi-picker.sh open"
# Open the file(s) in a vertical split
v = ":sh zellij run -n Yazi -c -f -x 10% -y 10% --width 80% --height 80% -- bash ~/.config/helix/yazi-picker.sh vsplit"
# Open the file(s) in a horizontal split
h = ":sh zellij run -n Yazi -c -f -x 10% -y 10% --width 80% --height 80% -- bash ~/.config/helix/yazi-picker.sh hsplit"
```

  </TabItem>
  <TabItem value="helix-nightly" label="Helix Nightly (> 25.01.1)">

```toml
# ~/.config/helix/config.toml
[keys.normal.C-y]
# Open the file(s) in the current window
y = ":sh zellij run -n Yazi -c -f -x 10%% -y 10%% --width 80%% --height 80%% -- bash ~/.config/helix/yazi-picker.sh open %{buffer_name}"
# Open the file(s) in a vertical split
v = ":sh zellij run -n Yazi -c -f -x 10%% -y 10%% --width 80%% --height 80%% -- bash ~/.config/helix/yazi-picker.sh vsplit %{buffer_name}"
# Open the file(s) in a horizontal split
h = ":sh zellij run -n Yazi -c -f -x 10%% -y 10%% --width 80%% --height 80%% -- bash ~/.config/helix/yazi-picker.sh hsplit %{buffer_name}"
```

  </TabItem>
</Tabs>

然后将以下脚本保存为 `~/.config/helix/yazi-picker.sh`:

```sh
#!/usr/bin/env bash

paths=$(yazi "$2" --chooser-file=/dev/stdout | while read -r; do printf "%q " "$REPLY"; done)

if [[ -n "$paths" ]]; then
	zellij action toggle-floating-panes
	zellij action write 27 # send <Escape> key
	zellij action write-chars ":$1 $paths"
	zellij action write 13 # send <Enter> key
else
	zellij action toggle-floating-panes
fi
```

注意:这使用浮动窗口,但你也应该能够在侧面或原地打开新窗格。查看 Zellij 文档以获取更多信息。

原始帖子:https://github.com/zellij-org/zellij/issues/3018#issuecomment-2086166900,感谢 [@rockboynton](https://github.com/rockboynton)、[@postsolar](https://github.com/postsolar)、[@TheAwiteb](https://github.com/TheAwiteb) 和 [@Dreaming-Codes](https://github.com/Dreaming-Codes) 分享和完善它!

<details>
  <summary>演示 Helix+Zellij+Yazi 工作流</summary>
	<video src="https://github.com/helix-editor/helix/assets/17523360/a4dde9e0-96bf-42a4-b946-40cbee984e69" width="100%" controls muted></video>
</details>

## Helix 中的文件树选择器与 tmux {#helix-with-tmux}

Yazi 可以用作文件选择器,在你当前的 Helix 实例(在 tmux 会话中运行)中浏览和打开文件。

在你的 Helix 配置中添加快捷键,例如 <kbd>Ctrl</kbd> + <kbd>y</kbd>:

```toml
# ~/.config/helix/config.toml
[keys.normal]
C-y = ":sh tmux new-window -n fx '~/.config/helix/yazi-picker.sh open'"
```

如果你还想在分屏窗格中打开文件,可以定义额外的快捷键绑定:

```toml
# ~/.config/helix/config.toml
[keys.normal.C-y]
# Open file(s) in the current window
y = ":sh tmux new-window -n fx '~/.config/helix/yazi-picker.sh open'"
# Open file(s) in a vertical split
v = ":sh tmux new-window -n fx '~/.config/helix/yazi-picker.sh vsplit'"
# Open file(s) in a horizontal split
h = ":sh tmux new-window -n fx '~/.config/helix/yazi-picker.sh hsplit'"
```

然后将以下脚本保存为 `~/.config/helix/yazi-picker.sh`:

```sh
#!/usr/bin/env bash

paths=$(yazi --chooser-file=/dev/stdout)

if [[ -n "$paths" ]]; then
	tmux last-window
	tmux send-keys Escape
	tmux send-keys ":$1 $paths"
	tmux send-keys Enter
else
	tmux kill-window -t fx
fi
```

## 通过电子邮件发送选定的文件

要使用 [Thunderbird](https://www.thunderbird.net) 发送选定的文件,使用快捷键 <kbd>Ctrl</kbd> + <kbd>m</kbd>:

```toml
# ~/.config/yazi/keymap.toml
[[mgr.prepend_keymap]]
on  = "<C-m>"
run = '''shell --
	paths=$(for p in "$@"; do echo "$p"; done | paste -s -d,)
	thunderbird -compose "attachment='$paths'"
'''
```

或者,使用 [NeoMutt](https://neomutt.org) 命令行邮件客户端:

```toml
# ~/.config/yazi/keymap.toml
[[mgr.prepend_keymap]]
on  = "<C-m>"
run = 'shell --block -- neomutt -a "$@"'
```

## 让 Yazi 比快更快 {#make-yazi-even-faster}

虽然 Yazi 已经很快了,但在特定用户或特定条件下仍有大量优化空间:

- 对于完全不需要图像预览的用户,禁用默认的 `image` previewer 和 preloader 将通过减少 I/O 读取文件和 CPU 解码图像的消耗使 Yazi 更快。
- 对于管理网络文件的用户,建议[禁用所有 previewers 和 preloaders](/docs/tips#folder-previewer),因为预览和预加载这些文件意味着它们需要下载到本地。
- 对于像树莓派这样的低规格设备,[降低并发度](/docs/configuration/yazi#tasks)将使 Yazi 更快,因为默认配置是为 PC 优化的,在这些低规格设备上的高并发可能产生相反的效果。
- 对于不需要准确 mime-type 的用户,[`mime-ext.yazi`](https://github.com/yazi-rs/plugins/tree/main/mime-ext.yazi) 可能有用,因为它只是根据文件扩展名返回 mime-type,而 Yazi 默认基于文件内容获取 mime-type 以确保准确性。Mime-type 用于匹配打开、预览、渲染规则。鼓励用户根据需要选择适当的 `mime` 插件,这就是我们决定向插件开发者开放它的原因。
- 对于高性能终端模拟器,降低 [`image_delay` 选项](/docs/configuration/yazi/#preview.image_delay)或将其设置为 0 可以减少图像预览延迟。
