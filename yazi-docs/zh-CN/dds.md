---
sidebar_position: 8
description: Data Distribution Service
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# DDS

DDS(数据分发服务)旨在实现多个 Yazi 实例之间的通信和状态同步,以及状态持久化。它建立在客户端-服务器架构之上,但不需要运行额外的服务器进程。

它深度集成了基于 Lua API 的发布-订阅模型。

## 概念 {#concept}

- Local:当前实例,即当前的 Yazi 进程。
- Remote:当前实例以外的其他实例。
- Static message:kind 以 `@` 开头的消息将被持久化存储,并在新实例启动时自动恢复。要取消持久化,向该 kind 发送 `nil`。

## 用法 {#usage}

DDS 有三种用法:

- [Plugin API](/docs/plugins/utils#ps):使用基于 Lua 的发布-订阅模型作为消息载体。
- [`ya pub` 和 `ya pub-to`](#ya-pub):使用 [`ya` CLI 工具](/docs/cli)作为消息载体。
- [`ya emit` 和 `ya emit-to`](#ya-emit):使用 [`ya` CLI 工具](/docs/cli)作为命令载体。
- [实时 `stdout` 报告](#stdout-reporting):使用 `stdout` 作为载体。

### `ya pub` 和 `ya pub-to` {#ya-pub}

如果你在设置了 `$YAZI_ID` 环境变量的 Yazi 子 shell 中,你可以使用 `ya pub` 向当前实例发送消息。
它需要一个 `kind` 参数,与 [`ps.pub()`](/docs/plugins/utils#ps.pub) 一致:

```sh
ya pub <kind> --str "string body"
ya pub <kind> --list "a" "b" "c"
ya pub <kind> --json '{"key":"json body"}'

# 例如,请求当前实例解压 `a.zip` 和 `b.7z`
ya pub extract --list "/root/a.zip" "/root/b.7z"
```

你也可以使用 `ya pub-to` 向指定的远程实例发送消息,需要 `receiver` 和 `kind` 参数,与 [`ps.pub_to()`](/docs/plugins/utils#ps.pub_to) 一致:

```sh
ya pub-to <receiver> <kind> --str "string body"
ya pub-to <receiver> <kind> --list "a" "b" "c"
ya pub-to <receiver> <kind> --json '{"key":"json body"}'

# 如果你在 Yazi 子 shell 中,
# 你可以通过 `$YAZI_ID` 获取当前实例的 ID。
ya pub-to "$YAZI_ID" my-event --str "Hello world!"

# 如果你不在 Yazi 子 shell 中,即从外部与 Yazi 通信,
# 你可以使用 `yazi --client-id <globally-unique-id>` 启动,并将该 ID 传递给 `ya pub-to`。
MY_UNIQUE_ID="$(date +%s)$RANDOM"
yazi --client-id "$MY_UNIQUE_ID"
ya pub-to "$MY_UNIQUE_ID" my-event --str "Hello world!"
```

为了更方便地在命令行环境中集成,它们支持两种 body 格式:

- String:一种直接的格式,适用于大多数场景,无需额外的工具进行编码
- List:字符串数组,适用于通过 shell 中的 `$@` 向消息传递文件列表
- JSON:用于高级需求,可以通过 JSON 格式表示类型和更复杂的数据

### `ya emit` 和 `ya emit-to` {#ya-emit}

如果你在设置了 `$YAZI_ID` 环境变量的 Yazi 子 shell 中,你可以使用 `ya emit` 向当前实例发送命令以执行。

命令格式与你在 [`keymap.toml`](/docs/configuration/keymap) 中编写的相同:

```sh
ya emit <command> <args>
```

例如:

```sh
ya emit cd /tmp
ya emit reveal /tmp/foo
```

你也可以使用 `ya emit-to` 向特定的远程实例发送命令:

```sh
ya emit-to <receiver> <command> <args>
```

例如:

```sh
ya emit-to "$YAZI_ID" cd /tmp
```

### 实时 `stdout` 报告 {#stdout-reporting}

你可以在启动 Yazi 时指定 `--local-events` 和 `--remote-events` 选项:

```sh
# Local events
yazi --local-events=kind1,kind2
# Remote events
yazi --remote-events=kind1,kind2
# Both local and remote events
yazi --local-events=kind1,kind2 --remote-events=kind1,kind2
```

当接收到指定 kind 的事件时,它将输出到 `stdout`:

```sh
hover,0,200,{"tab":0,"url":"/root/Downloads"}
cd,0,100,{"tab":0,"url":"/root/Downloads"}
```

每行一个 payload,每个 payload 包含以下用逗号分隔的字段:

| 字段     | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| kind     | 消息的 kind                                                  |
| receiver | 接收消息的远程实例 ID;如果为 `0`,则广播到所有远程实例         |
| sender   | 消息的发送者                                                  |
| body     | 消息的 body,是一个 JSON 字符串                                |

这提供了实时报告 Yazi 内部事件的能力,对于外部工具集成(如 Neovim)很有用,因为它们可以订阅用户行为触发的事件。

## 内置 kinds {#kinds}

### `cd` - 更改目录 {#cd}

`sub()` 回调 body:

```lua
{
	tab = 0
}
```

`sub_remote()` 回调 body:

```lua
{
	tab = 0,
	url = Url("/root/Downloads")
}
```

`--local-events` stdout payload:

```sh
cd,1711957542289249,1711957542289249,{"tab":0,"url":"/root/Downloads"}
```

`--remote-events` stdout payload:

```sh
cd,0,100,{"tab":0,"url":"/root/Downloads"}
```

### `hover` - 悬停在文件上 {#hover}

`sub()` 回调 body:

```lua
{
	tab = 0
}
```

`sub_remote()` 回调 body:

```lua
{
	tab = 0,
	url = Url("/root/foo.txt")
}
```

`--local-events` stdout payload:

```sh
hover,1711957283332834,1711957283332834,{"tab":0,"url":"/root/foo.txt"}
```

`--remote-events` stdout payload:

```sh
hover,0,200,{"tab":0,"url":"/root/foo.txt"}
```

### `rename` - 重命名文件 {#rename}

`sub()` / `sub_remote()` 回调 body:

```lua
{
  tab = 0,
  from = Url("/root/foo.txt"),
  to = Url("/root/bar.txt"),
}
```

`--local-events` stdout payload:

```sh
rename,1711957878076791,1711957878076791,{"tab":0,"from":"/root/foo.txt","to":"/root/bar.txt"}
```

`--remote-events` stdout payload:

```sh
rename,0,1711957878076791,{"tab":0,"from":"/root/foo.txt","to":"/root/bar.txt"}
```

### `bulk` - 批量重命名文件 {#bulk}

`sub()` / `sub_remote()` 回调 body:

```lua
-- 由于 `Iterator` 实现了 `__pairs()`,
-- 你可以使用 `pairs(body)` 遍历所有 URL 对
Iterator {
	__len = function(self)
		-- 返回更改的文件数量
	end,
	__pairs = function(self)
		-- 返回 (Url("/path/from.txt"), Url("/path/to.txt"))
	end
}
```

`--local-events` stdout payload:

```sh
bulk,1711957542289249,1711957542289249,{"changes":{"/path/from.txt":"/path/to.txt"}}
```

`--remote-events` stdout payload:

```sh
bulk,0,1711957542289249,{"changes":{"/path/from.txt":"/path/to.txt"}}
```

### `yank` - 复制文件 {#yank}

`sub()` 回调 body:

```lua
{}
```

`sub_remote()` 回调 body:

```lua
-- 由于 `Iterator` 实现了 `__index()`,你可以通过索引访问复制的 URL,
-- 如 `body[1]`,或使用 `ipairs(body)` 遍历所有 URL
Iterator {
	cut = false,
	__len = function(self)
		-- 返回复制的 URL 数量
	end,
	__index = function(self, idx)
		-- 返回给定索引处的 URL
	end
}
```

`--local-events` stdout payload:

```sh
yank,1711960311454247,1711960311454247,{"cut":false,"urls":["/root/foo.txt","/root/bar.txt"]}
```

`--remote-events` stdout payload:

```sh
yank,0,300,{"cut":false,"urls":["/root/foo.txt","/root/bar.txt"]}
```

### `move` - 移动文件 {#move}

`sub()` 回调 body:

```lua
{
	items = {
		{ from = Url("/root/foo.txt"), to = Url("/root/bar.txt") },
		-- ...
	}
}
```

`sub_remote()` 回调 body:

```lua
{
	items = {
		{ from = Url("/root/foo.txt"), to = Url("/root/bar.txt") },
		-- ...
	}
}
```

`--local-events` stdout payload:

```sh
move,1711957542289249,1711957542289249,{"items":[{"from":"/root/foo.txt","to":"/root/bar.txt"}]}
```

`--remote-events` stdout payload:

```sh
move,0,1711957542289249,{"items":[{"from":"/root/foo.txt","to":"/root/bar.txt"}]}
```

### `trash` - 将文件移到回收站 {#trash}

`sub()` 回调 body:

```lua
{
	urls = {
		Url("/root/foo.txt"),
		-- ...
	}
}
```

`sub_remote()` 回调 body:

```lua
{
	urls = {
		Url("/root/foo.txt"),
		-- ...
	}
}
```

`--local-events` stdout payload:

```sh
trash,1711957542289249,1711957542289249,{"urls":["/root/foo.txt"]}
```

`--remote-events` stdout payload:

```sh
trash,0,1711957542289249,{"urls":["/root/foo.txt"]}
```

### `delete` - 删除文件 {#delete}

`sub()` 回调 body:

```lua
{
	urls = {
		Url("/root/foo.txt"),
		-- ...
	}
}
```

`sub_remote()` 回调 body:

```lua
{
	urls = {
		Url("/root/foo.txt"),
		-- ...
	}
}
```

`--local-events` stdout payload:

```sh
delete,1711957542289249,1711957542289249,{"urls":["/root/foo.txt"]}
```

`--remote-events` stdout payload:

```sh
delete,0,1711957542289249,{"urls":["/root/foo.txt"]}
```

### `hi` - 客户端握手 {#hi}

系统保留 kind。

### `hey` - 服务器握手 {#hey}

系统保留 kind。

### `bye`

系统保留 kind。

## 内置插件 {#plugins}

### `dds.lua` {#dds.lua}

此插件提供了一个 `dds-emit` 事件 kind,用于实现 `ya emit` 子命令 — `ya emit` 是 `ya pub` 的简写,发出的命令将被转换为等效的 `ya pub` 事件消息。

使用 `ya emit`,你可以实现许多有趣的功能,例如在退出子 shell 时同步当前 Yazi 实例的 CWD:

<Tabs>
  <TabItem value="Zsh" label="Zsh" default>

```sh
# Change Yazi's CWD to PWD on subshell exit
if [[ -n "$YAZI_ID" ]]; then
	function _yazi_cd() {
		ya emit cd "$PWD"
	}
	add-zsh-hook zshexit _yazi_cd
fi
```

  </TabItem>
  <TabItem value="fish" label="Fish">

```sh
# Change Yazi's CWD to PWD on subshell exit
if [ -n "$YAZI_ID" ]
	trap 'ya emit cd "$PWD"' EXIT
end
```

  </TabItem>
  <TabItem value="nushell" label="Nushell">

```sh
# Please raise a PR if you have a nushell version
```

  </TabItem>
</Tabs>

源代码:https://github.com/sxyazi/yazi/blob/main/yazi-plugin/preset/plugins/dds.lua

### `session.lua` {#session.lua}

此插件提供跨实例的复制能力,这意味着你可以在一个实例中复制文件,然后在另一个实例中粘贴它们。

要启用它,在你的 `init.lua` 中添加这些行,然后重启 _*所有*_ Yazi 实例以应用更改:

```lua
require("session"):setup {
	sync_yanked = true,
}
```

源代码:https://github.com/sxyazi/yazi/blob/main/yazi-plugin/preset/plugins/session.lua

### `extract.lua` {#extract.lua}

此插件为归档提取提供了一个 `extract` 事件 kind,它接受文件 URL 数组。你可以将其绑定为归档的 [opener](/docs/configuration/yazi#opener):

```toml
# ~/.config/yazi/yazi.toml
[opener]
extract = [
	{ run = 'ya pub extract --list "$@"', desc = "Extract here", for = "unix" },
	{ run = 'ya pub extract --list %*',   desc = "Extract here", for = "windows" },
]
```

源代码:https://github.com/sxyazi/yazi/blob/main/yazi-plugin/preset/plugins/extract.lua
