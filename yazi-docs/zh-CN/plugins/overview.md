---
id: overview
sidebar_position: 0
sidebar_label: Plugins (BETA)
description: 学习如何使用 Lua 插件扩展 Yazi。
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# 插件 (BETA)

你可以通过 Lua 插件扩展 Yazi 的功能，插件需要放置在 Yazi 配置目录的 `plugins` 子目录中，即：

- Unix-like 系统：`~/.config/yazi/plugins/`
- Windows 系统：`%AppData%\yazi\config\plugins\`

```
~/.config/yazi/
├── init.lua
├── plugins/
│   ├── foo.yazi/
│   └── bar.yazi/
└── yazi.toml
```

每个插件都是一个以 `.yazi` 结尾的 [kebab-case](https://developer.mozilla.org/en-US/docs/Glossary/Kebab_case) 命名的目录，至少包含以下文件：

```
~/.config/yazi/plugins/bar.yazi/
├── main.lua
├── README.md
└── LICENSE
```

其中：

- `main.lua` 是该插件的入口点。
- `README.md` 是该插件的文档。
- `LICENSE` 是该插件的许可证文件。

## 用法 {#usage}

插件有两种用法：

- [功能插件](#functional-plugin)：在 `keymap.toml` 中将 `plugin` 命令绑定到按键，然后按键激活它。
- [自定义预览器、预加载器](/docs/configuration/yazi#plugin)：在 `yazi.toml` 的 `[plugin]` 下将它们配置为预览器或预加载器。

### 功能插件 {#functional-plugin}

你可以在 `keymap.toml` 中将 `plugin` 命令绑定到特定按键：

| 参数/选项 | 描述                          |
| --------- | ----------------------------- |
| `[name]`  | 必需，要运行的插件名称。      |
| `[args]`  | 可选，传递给插件的 shell 风格参数。 |

例如，`plugin test -- foo --bar --baz=qux` 将在异步上下文中使用参数 `foo --bar --baz=qux` 运行 `test` 插件。

要在插件中访问参数，使用 `job.args`：

```lua
-- ~/.config/yazi/plugins/test.yazi/main.lua
return {
	entry = function(self, job)
		ya.dbg(job.args[1])  -- "foo"
		ya.dbg(job.args.bar) -- true
		ya.dbg(job.args.baz) -- "qux"
	end,
}
```

注意，目前 Yazi 仅支持位置参数（`foo`）和命名参数（`--bar`），不支持像 `-a` 这样的简写参数。

简写参数目前会被视为位置参数，但随着 Yazi 未来添加对它的支持，它们的行为会改变。因此请避免使用它们以防止潜在冲突。

## 同步与异步 {#sync-vs-async}

插件系统采用异步优先的设计理念。因此，除非特别指定，例如 [`@sync` 注解](/docs/plugins/overview#@sync)，否则所有插件都在异步上下文中运行。

有一个例外：用户的 `init.lua` 是同步的，因为 `init.lua` 通常用于初始化插件配置：

```lua
-- ~/.config/yazi/init.lua
require("my-plugin"):setup {
	key1 = "value1",
	key2 = "value2",
	-- ...
}
```

```lua
-- ~/.config/yazi/plugins/my-plugin.yazi/main.lua
return {
	setup = function(state, opts)
		-- 将用户配置保存到插件的状态中
		state.key1 = opts.key1
		state.key2 = opts.key2
	end,
}
```

### 同步上下文 {#sync-context}

同步上下文伴随整个应用生命周期，在 UI 渲染期间（UI 插件）和执行[同步功能插件](/docs/plugins/overview#@sync)时处于活动状态。

为了更好的性能，同步上下文仅在应用启动时创建一次，并在整个过程中保持单例。因此，在此上下文中运行的插件共享状态，这促使插件开发者使用插件特定的状态持久化来防止全局空间污染：

```lua
--- @sync entry
-- ~/.config/yazi/test.yazi/main.lua
return {
  entry = function(state)
    state.i = state.i or 0
    ya.dbg("i = " .. state.i)

    state.i = state.i + 1
  end,
}
```

Yazi 在运行之前会为每个 _同步_ 插件初始化 `state`，并且它在整个生命周期中独立存在。
执行三次 `plugin test`，你会看到日志输出：

```sh
i = 0
i = 1
i = 2
```

### 异步上下文 {#async-context}

当插件异步执行时，会自动为其创建一个隔离的异步上下文。

在此上下文中，你可以使用 Yazi 支持的所有异步函数，它与主线程并发运行，确保主线程不被阻塞。

你还可以通过调用"同步块"从同步上下文中获取[少量数据](#sendable)：

```lua
-- ~/.config/yazi/plugins/my-async-plugin.yazi/main.lua
local set_state = ya.sync(function(state, a)
	-- 你可以在 `sync()` 块中通过 `state` 参数
	-- 获取/设置插件的状态
	state.a = a
end)

local get_state = ya.sync(function(state, b)
	-- 在异步插件中，你可以在 `sync()` 块内通过 `cx`
	-- 访问所有状态
	local h = cx.active.current.hovered
	return h and state.a .. tostring(h.url) or b
end)

return {
	entry = function()
		set_state("hello from a")
		local h = get_state("hello from b")
		-- 执行一些耗时的工作，如读取文件、网络请求等
		-- 它将与主线程并发执行
	end,
}
```

注意 `ya.sync()` 调用必须在顶层：

```lua
-- 错误！！！
local get_state
if some_condition then
	get_state = ya.sync(function(state)
		-- ...
	end)
end
```

向 `ya.sync()` 块传递数据和从中返回数据涉及跨线程数据交换。如果数据包含 userdata，会导致[所有权转移](/docs/plugins/overview#ownership)。

## 注解 {#annotations}

每个插件可以包含零个或多个注解，指定插件在运行时的行为。

每个注解以 `---` 开头，后跟 `@` 和注解名称，最后是注解的值。

这些注解 _必须_ 位于文件的最顶部，前面不能有任何内容，注解之间也不应出现非注解内容。

### `@sync` {#@sync}

指定插件中的方法在同步上下文而非默认的异步上下文中运行。可用值：

- `entry`：在同步上下文中运行 `entry` 方法。
- `peek`：在同步上下文中运行 `peek` 方法。

例如：

```lua
--- @sync entry
return {
	entry = function() end
}
```

### `@since` {#@since}

指定插件支持的最低 Yazi 版本。

如果指定了此注解，且用户的 Yazi 版本低于指定版本，将触发错误以提示用户升级 Yazi 版本，防止插件意外执行：

```lua
--- @since 25.2.13
return {
	--- ...
}
```

## 接口 {#interface}

### 预览器 {#previewer}

预览器需要返回一个实现 `peek` 和 `seek` 方法的表。这两个方法都接受一个表参数 `job`，不返回任何值：

```lua
local M = {}

function M:peek(job)
	-- ...
end

function M:seek(job)
	-- ...
end

return M
```

当用户按 <kbd>j</kbd> 或 <kbd>k</kbd> 在悬停文件之间切换时，`peek` 被调用，参数包括：

| 键     | 描述                                                                                           |
| ------ | ---------------------------------------------------------------------------------------------- |
| `area` | 可用预览区域的 [Rect](/docs/plugins/layout#rect)。                                             |
| `args` | 传递给预览器的参数。                                                                           |
| `file` | 要预览的 [File](/docs/plugins/types#file)。                                                    |
| `skip` | 要跳过的单位数。单位取决于你的预览器，如代码的行数和视频的百分比。                             |

当用户按 <kbd>J</kbd> 或 <kbd>K</kbd> 滚动文件预览时，`seek` 被调用，参数包括：

| 键      | 描述                                                    |
| ------- | ------------------------------------------------------- |
| `file`  | 正在滚动的 [File](/docs/plugins/types#file)。           |
| `area`  | 可用预览区域的 [Rect](/docs/plugins/layout#rect)。      |
| `units` | 要滚动的单位数。                                        |

`peek` 的任务是根据 `file` 和 `skip` 的值在预览区域绘制。这个过程是异步的。

`seek` 的任务是根据用户行为改变 `skip` 的值并再次触发 `peek`。它是同步的，这意味着你可以访问[上下文](/docs/plugins/context)。

有一些预设的预览器和预加载器可供参考：[Yazi 预设插件](https://github.com/sxyazi/yazi/tree/shipped/yazi-plugin/preset/plugins)

### 预加载器 {#preloader}

你需要返回一个实现 `preload` 方法的表：

```lua
local M = {}

function M:preload(job)
	-- ...
	return false, Err("some error")
end

return M
```

它接收一个 `job` 参数，这是一个表：

| 键     | 描述                                                |
| ------ | --------------------------------------------------- |
| `area` | 可用预览区域的 [Rect](/docs/plugins/layout#rect)。  |
| `args` | 传递给预加载器的参数。                              |
| `file` | 要预加载的 [File](/docs/plugins/types#file)。       |
| `skip` | 始终为 `0`                                          |

并返回 `(complete, err)`：

- `complete`：必需，预加载是否完成，这是一个布尔值。
  - `true`：将任务标记为完成，任务不会再次被调用。
  - `false`：将任务标记为未完成，任务将重试直到完成（返回 `true`）。
- `err`：可选，要记录的错误。

当 `complete = false` 时，预加载器将在下一个时间点重新触发，例如用户滚动导致页面切换时。这通常用于以下两种情况：

- 文件加载失败时重试
- 加载成功时刷新文件状态

Yazi 会自动为页面上匹配预加载规则的每个文件并发调用 `preload`。

## 可发送值 {#sendable}

Yazi 的插件可以在多个线程上并发运行。为了更好的性能，只有以下类型的组合可用于线程间数据交换：

- Nil
- Boolean
- Number
- String
- [Url](/docs/plugins/types#url)
- 表和嵌套表，其值为上述类型

## 所有权转移 {#ownership}

Yazi 的插件系统继承了 [Rust 的所有权和生命周期](https://doc.rust-lang.org/nomicon/ownership.html)概念。

所有 [userdata](https://www.lua.org/pil/28.1.html) 都是原生 Rust 类型，拥有自己的所有权，以确保跨不同线程的安全高效传输，避免任何内存重新分配开销。具体来说：

- [Url](/docs/plugins/types#url)

将这些 userdata 传递给跨线程函数如 [`ya.emit()`](/docs/plugins/utils#ya.emit) 会转移所有权。转移后，原始 userdata 不再可用，例如：

```lua
local target = Url("/tmp")
ya.emit("cd", { target })  -- 所有权已转移

ya.dbg(tostring(url)) -- 错误：userdata 已被销毁
```

要保留原始数据，克隆一个新的 userdata 并传递它，但这会分配额外的内存 - `Url()` 构造函数可以接受一个 `Url` userdata 并返回该 `Url` 的新克隆：

```diff
- ya.emit("cd", { target })
+ ya.emit("cd", { Url(target) })
```

更聪明的方法是颠倒执行顺序，在转移之前使用 `target`，以避免需要克隆：

```lua
local target = Url("/tmp")
local target_str = tostring(target)

ya.emit("cd", { target })  -- 所有权已转移
ya.dbg(target_str) -- 没有错误
```

## 调试 {#debugging}

请确保你的 `~/.config/yazi/init.lua` 包含有效的 Lua 代码和正确的语法，否则会导致 Yazi 无法解析和执行你的 `init.lua` 来初始化。

我们建议在编辑器中安装 Lua 插件进行语法检查以避免任何语法错误。
例如，为 VSCode 安装 [Lua 插件](https://marketplace.visualstudio.com/items?itemName=sumneko.lua)，对于 Neovim，使用 [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig) 配置你的 Lua LSP。

如果你没有 Lua 经验，可以通过 https://learnxinyminutes.com/docs/lua/ 快速入门

### 日志记录 {#logging}

如果你想调试一些运行时数据，使用 [`ya.dbg()`](/docs/plugins/utils#ya.dbg) 和 [`ya.err()`](/docs/plugins/utils#ya.err) 打印你想调试的内容到：

- Unix-like 系统：`~/.local/state/yazi/yazi.log`
- Windows 系统：`%AppData%\yazi\state\yazi.log`

在启动 Yazi 之前，请确保设置 `YAZI_LOG` 环境变量：

<Tabs>
  <TabItem value="unix" label="Unix-like" default>

```sh
YAZI_LOG=debug yazi
```

  </TabItem>

  <TabItem value="powershell" label="PowerShell">

```powershell
$env:YAZI_LOG = "debug"; yazi
```

  </TabItem>
</Tabs>

否则不会记录任何日志。它的值可以是（按详细程度降序）：

- `debug`
- `info`
- `warn`
- `error`

### 调试预设插件

1. 克隆最新的源代码。
2. 进入 `yazi-plugin/preset` 文件夹，找到你想调试的插件，进行修改，例如[记录某些运行时数据](/docs/plugins/overview#logging)。
3. [以调试模式构建](/docs/installation#debug)并使用适当的 `YAZI_LOG` 运行 `yazi` 二进制文件。
