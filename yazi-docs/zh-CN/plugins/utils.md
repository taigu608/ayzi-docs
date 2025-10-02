---
sidebar_position: 5
description: 学习如何使用 Yazi 的 Lua API。
---

# 工具函数

## ya {#ya}

### `hide()` {#ya.hide}

通过返回到终端将 Yazi 隐藏到辅助屏幕，完全由请求的插件控制。

```lua
local permit = ya.hide()
```

此方法返回此资源的 `permit`。当需要恢复 TUI 显示时，调用其 `drop()` 方法：

```lua
permit:drop()
```

注意，由于始终只有一个可用的终端控制资源，在前一个 `permit` 被 drop 之前，`ya.hide()` 不能再次被调用，否则会抛出错误，有效地避免死锁。

| 输入/输出 | 类型           |
| --------- | -------------- |
| 返回      | `Permit`       |
| 可用      | 仅异步上下文   |

### `file_cache(opts)` {#ya.file_cache}

计算给定文件对应的缓存 [Url](/docs/plugins/types#url)。

```lua
ya.file_cache {
	-- 要缓存的文件。
	file = file,
	-- 要跳过的单位数。单位很大程度上取决于你的预览器，
	-- 如代码的行数和视频的百分比。
	skip = 1,
}
```

如果文件不允许被缓存，例如在用户配置中被忽略，或者文件本身就是缓存，则返回 `nil`。

| 输入/输出 | 类型                            |
| --------- | ------------------------------- |
| `opts`    | `{ file: File, skip: integer }` |
| 返回      | `Url?`                          |

### `render()` {#ya.render}

重新渲染 UI：

```lua
local update_state = ya.sync(function(self, new_state)
	self.state = new_state
	ya.render()
end)
```

| 输入/输出 | 类型         |
| --------- | ------------ |
| 返回      | `unknown`    |
| 可用      | 仅同步上下文 |

### `emit(cmd, args)` {#ya.emit}

向 [`[mgr]`](/docs/configuration/keymap#mgr) 发送命令，而无需等待执行器执行：

```lua
ya.emit("my-cmd", { "hello", 123, foo = true, bar_baz = "world" })

-- 等同于：
-- my-cmd "hello" "123" --foo --bar-baz="world"
```

| 输入/输出 | 类型                              | 注意                                                     |
| --------- | --------------------------------- | -------------------------------------------------------- |
| `cmd`     | `string`                          | -                                                        |
| `args`    | `{ [integer\|string]: Sendable }` | 表值是遵循[所有权转移规则][ownership]的[可发送值][sendable] |
| 返回      | `unknown`                         | -                                                        |

### `image_show(url, rect)` {#ya.image_show}

在 `rect` 内显示 `url` 的图像，图像会自动缩放以适应区域：

| 输入/输出 | 类型         |
| --------- | ------------ |
| `url`     | `Url`        |
| `rect`    | `Rect`       |
| 返回      | `unknown`    |
| 可用      | 仅异步上下文 |

### `image_precache(src, dist)` {#ya.image_precache}

根据用户配置的 [`max_width` 和 `max_height`](/docs/configuration/yazi#preview)，将 `src` 的图像预缓存为 `dist`。

| 输入/输出 | 类型         |
| --------- | ------------ |
| `src`     | `Url`        |
| `dist`    | `Url`        |
| 返回      | `unknown`    |
| 可用      | 仅异步上下文 |

### `which(opts)` {#ya.which}

向用户提示一组可用的按键：

```lua
local cand = ya.which {
	-- 按键候选，包含以下字段：
	--   `on`：要提示的按键，如果是多个则是字符串表。
	--   `desc`：按键的描述。
	cands = {
		{ on = "a" },
		{ on = "b", desc = "optional description" },
		{ on = "<C-c>", desc = "key combination" },
		{ on = { "d", "e" }, desc = "multiple keys" },
	},
	-- 是否显示按键指示器的 UI
	silent = false,
}
```

当用户点击有效候选时，`ya.which` 返回该 `cand` 的 1-based 索引；
否则返回 `nil`，表示用户已取消按键操作。

| 输入/输出 | 类型                                                                     |
| --------- | ------------------------------------------------------------------------ |
| `opts`    | `{ cands: { on: string\|string[], desc: string? }[], silent: boolean? }` |
| 返回      | `number?`                                                                |
| 可用      | 仅异步上下文                                                             |

### `input(opts)` {#ya.input}

请求用户输入：

```lua
local value, event = ya.input {
	-- 标题
	title = "Archive name:",
	-- 默认值
	value = "",
	-- 是否隐藏输入。
	obscure = false,
	-- 位置
	position = { "top-center", y = 3, w = 40 },
	-- 是否实时报告用户输入。
	realtime = false,
	-- 等待用户停止输入的秒数，在 `realtime = true` 时可用。
	debounce = 0.3,
}
```

返回 `(value, event)`：

- `value`：此事件携带的用户输入值，如果 `event` 非零则为字符串；否则为 `nil`。
- `event`：事件类型，是一个整数：
  - 0：未知错误。
  - 1：用户已确认输入。
  - 2：用户已取消输入。
  - 3：用户已更改输入（仅在 `realtime` 为 true 时）。

当指定 `realtime = true` 时，`ya.input()` 返回一个接收器，它有一个可以多次调用以接收事件的 `recv()` 方法：

```lua
local input = ya.input {
	title = "Input in realtime:",
	position = { "center", w = 50 },
	realtime = true,
}

while true do
	local value, event = input:recv()
	if not value then
		break
	end

	ya.dbg(value)
end
```

| 输入/输出 | 类型                                                                                                           |
| --------- | -------------------------------------------------------------------------------------------------------------- |
| `opts`    | `{ title: string, value: string?, obscure: boolean?, position: AsPos, realtime: boolean?, debounce: number? }` |
| 返回      | `(string?, integer)` \| `Recv`                                                                                 |
| 可用      | 仅异步上下文                                                                                                   |

### `notify(opts)` {#ya.notify}

向用户发送前台通知：

```lua
ya.notify {
	-- 标题。
	title = "Hello, World!",
	-- 内容。
	content = "This is a notification from Lua!",
	-- 超时时间。
	timeout = 6.5,
	-- 级别，可用值："info"、"warn" 和 "error"，默认为 "info"。
	level = "info",
}
```

| 输入/输出 | 类型                                                                                       |
| --------- | ------------------------------------------------------------------------------------------ |
| `opts`    | `{ title: string, content: string, timeout: number, level: "info"\|"warn"\|"error"\|nil }` |
| 返回      | `unknown`                                                                                  |

### `confirm(opts)` {#ya.confirm}

请求用户确认：

```lua
local answer = ya.confirm {
	-- 位置
  pos = { "center", w = 40, h = 10 },
	-- 标题
  title = "Test",
	-- 正文
  body = "Hello, World!",
}
```

| 输入/输出 | 类型                                          |
| --------- | --------------------------------------------- |
| `opts`    | `{ pos: AsPos, title: AsLine, body: AsText }` |
| 返回      | `boolean`                                     |
| 可用      | 仅异步上下文                                  |

### `dbg(msg, ...)` {#ya.dbg}

在 debug 级别向[日志文件](/docs/plugins/overview#logging)追加消息：

```lua
ya.dbg("Hello", "World!")                       -- 支持多个参数
ya.dbg({ foo = "bar", baz = 123, qux = true })  -- 支持任何类型的数据
```

| 输入/输出 | 类型      |
| --------- | --------- |
| `msg`     | `any`     |
| `...`     | `any`     |
| 返回      | `unknown` |

### `err(msg, ...)` {#ya.err}

在 error 级别向[日志文件](/docs/plugins/overview#logging)追加消息：

```lua
ya.err("Hello", "World!")                       -- 支持多个参数
ya.err({ foo = "bar", baz = 123, qux = true })  -- 支持任何类型的数据
```

| 输入/输出 | 类型      |
| --------- | --------- |
| `msg`     | `any`     |
| `...`     | `any`     |
| 返回      | `unknown` |

### `preview_code(opts)` {#ya.preview_code}

将文件作为代码预览到指定区域：

```lua
ya.preview_code {
	-- 可用预览区域
  area = area,
	-- 要预览的文件。
  file = file,
	-- 文件的 MIME 类型。
  mime = "text/plain",
	-- 要跳过的单位数。单位取决于你的预览器，
	-- 如代码的行数和视频的百分比。
  skip = 1,
}
```

返回 `(err, upper_bound)`：

- `err`：如果预览失败则为错误字符串；否则为 `nil`。
- `upper_bound`：如果预览失败且是因为超过最大上限，则返回此上限；否则为 `nil`。

| 输入/输出 | 类型                                                      |
| --------- | --------------------------------------------------------- |
| `opts`    | `{ area: Rect, file: File, mime: string, skip: integer }` |
| 返回      | `Error?, integer?`                                        |
| 可用      | 仅异步上下文                                              |

### `preview_widget(opts, widget)` {#ya.preview_widget}

```lua
local opts = {
	-- 可用预览区域。
	area = area,
	-- 要预览的文件。
	file = file,
	-- 文件的 MIME 类型。
	mime = "text/plain",
	-- 要跳过的单位数。单位取决于你的预览器，
	-- 如代码的行数和视频的百分比。
	skip = 1,
}

-- 在指定区域预览一个小部件。
ya.preview_widget(opts, ui.Line("Hello world"):area(area))

-- 在指定区域预览多个小部件。
ya.preview_widget(opts, {
	ui.Line("Hello"):area(area1),
	ui.Line("world"):area(area2),
})
```

| 输入/输出 | 类型                                                      |
| --------- | --------------------------------------------------------- |
| `opts`    | `{ area: Rect, file: File, mime: string, skip: integer }` |
| `widget`  | `Renderable` \| `Renderable[]`                            |
| 返回      | `unknown`                                                 |
| 可用      | 仅异步上下文                                              |

### `sync(fn)` {#ya.sync}

详见[异步上下文](/docs/plugins/overview#async-context)。

| 输入/输出 | 类型                 |
| --------- | -------------------- |
| `fn`      | `fun(...: any): any` |
| 返回      | `fun(...: any): any` |

### `target_os()` {#ya.target_os}

返回描述正在使用的特定操作系统的字符串。

| 输入/输出 | 类型                                                                                                                                                    |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 返回      | `string` \| `"linux"` \| `"macos"` \| `"ios"` \| `"freebsd"` \| `"dragonfly"` \| `"netbsd"` \| `"openbsd"` \| `"solaris"` \| `"android"` \| `"windows"` |

### `target_family()` {#ya.target_family}

返回操作系统族。

| 输入/输出 | 类型                                            |
| --------- | ----------------------------------------------- |
| 返回      | `string` \| `"unix"` \| `"windows"` \| `"wasm"` |

### `hash(str)` {#ya.hash}

返回 `str` 的哈希值：

```lua
ya.hash("Hello, World!")
```

它设计用于与算法无关的任务，例如生成文件缓存名称。

当前实现使用 MD5，但将来会被更快的哈希算法（如 [xxHash](https://github.com/Cyan4973/xxHash)）取代。因此，不要依赖此实现细节。

| 输入/输出 | 类型         |
| --------- | ------------ |
| `str`     | `string`     |
| 返回      | `string`     |
| 可用      | 仅异步上下文 |

### `quote(str)` {#ya.quote}

引用 `str` 中在 shell 中可能具有特殊含义的字符：

```lua
local handle = io.popen("ls " .. ya.quote(filename))
```

| 输入/输出 | 类型     |
| --------- | -------- |
| `str`     | `string` |
| 返回      | `string` |

### `truncate(text, opts)` {#ya.truncate}

将 `text` 截断到指定宽度并返回截断结果：

```lua
ya.truncate("Hello, World!", {
	-- 文本的最大宽度。
	max = 5,
	-- 是否从右到左截断文本。
	rtl = false
})
```

| 输入/输出 | 类型                              |
| --------- | --------------------------------- |
| `text`    | `string`                          |
| `opts`    | `{ max: integer, rtl: boolean? }` |
| 返回      | `string`                          |

### `clipboard(text)` {#ya.clipboard}

获取或设置系统剪贴板的内容：

```lua
-- 如果未提供参数，则从剪贴板获取内容
local content = ya.clipboard()

-- 将内容设置到剪贴板
ya.clipboard("new content")
```

| 输入/输出 | 类型         |
| --------- | ------------ |
| `text`    | `string?`    |
| 返回      | `string?`    |
| 可用      | 仅异步上下文 |

### `time()` {#ya.time}

返回当前时间戳，这是一个浮点数，整数部分表示秒，小数部分表示毫秒。

| 输入/输出 | 类型     |
| --------- | -------- |
| 返回      | `number` |

### `sleep(secs)` {#ya.sleep}

等待直到 `secs` 过去：

```lua
ya.sleep(0.5)  -- 睡眠 500 毫秒
```

| 输入/输出 | 类型         |
| --------- | ------------ |
| `secs`    | `number`     |
| 返回      | `unknown`    |
| 可用      | 仅异步上下文 |

### `uid()` {#ya.uid}

返回当前用户的 id。

| 输入/输出 | 类型             |
| --------- | ---------------- |
| 返回      | `integer`        |
| 可用      | 仅 Unix-like 系统 |

### `gid()` {#ya.gid}

返回当前用户的组 id。

| 输入/输出 | 类型             |
| --------- | ---------------- |
| 返回      | `integer`        |
| 可用      | 仅 Unix-like 系统 |

### `user_name(uid)` {#ya.user_name}

通过 `uid` 获取用户名：

```lua
-- 如果未提供参数，则获取当前用户的名称
ya.user_name()

-- 获取 id 为 1000 的用户名称
ya.user_name(1000)
```

| 输入/输出 | 类型             |
| --------- | ---------------- |
| `uid`     | `integer?`       |
| 返回      | `string?`        |
| 可用      | 仅 Unix-like 系统 |

### `group_name(gid)` {#ya.group_name}

通过 `gid` 获取组名：

```lua
-- 如果未提供参数，则获取当前用户的组名
ya.group_name()

-- 获取 id 为 1000 的组名称
ya.group_name(1000)
```

| 输入/输出 | 类型             |
| --------- | ---------------- |
| `gid`     | `integer?`       |
| 返回      | `string?`        |
| 可用      | 仅 Unix-like 系统 |

### `host_name()` {#ya.host_name}

返回当前机器的主机名。

| 输入/输出 | 类型             |
| --------- | ---------------- |
| 返回      | `string?`        |
| 可用      | 仅 Unix-like 系统 |

## ps {#ps}

Yazi 的 DDS（数据分发服务）使用基于 Lua 的发布-订阅模型作为其载体。也就是说，你可以通过 `ps` API 实现跨实例通信和状态持久化。详见 [DDS](/docs/dds)。

以下函数只能在同步上下文中使用。

### `pub(kind, value)` {#ps.pub}

向当前实例发布消息，所有通过 `sub()` 订阅此 `kind` 的插件都会收到它，实现实例内部通信：

```lua
ps.pub("greeting", "Hello, World!")
```

由于 `kind` 是全局使用的，添加插件名称作为前缀是最佳实践。
例如，插件 `my-plugin` 和 kind `event1` 的组合将是 `my-plugin-event1`。

| 输入/输出 | 类型       | 注意                                                         |
| --------- | ---------- | ------------------------------------------------------------ |
| `kind`    | `string`   | 带破折号的字母数字，不能是[内置 kinds](/docs/dds#kinds)      |
| `value`   | `Sendable` | 遵循[所有权转移规则][ownership]的[可发送值][sendable]        |
| 返回      | `unknown`  | -                                                            |

### `pub_to(receiver, kind, value)` {#ps.pub_to}

向 ID 为 `receiver` 的特定实例发布消息：

```lua
ps.pub_to(1711957283332834, "greeting", "Hello, World!")
```

其中：

- 本地 - `receiver` 是当前实例，并通过 `sub()` 订阅此 `kind`，它将收到消息。
- 远程 - `receiver` 不是当前实例，并通过 `sub_remote()` 订阅此 `kind`，它将收到消息。
- 广播 - `receiver` 是 `0`，所有通过 `sub_remote()` 订阅此 `kind` 的远程实例都将收到消息。

| 输入/输出  | 类型       | 注意                                                         |
| ---------- | ---------- | ------------------------------------------------------------ |
| `receiver` | `integer`  | -                                                            |
| `kind`     | `string`   | 带破折号的字母数字，不能是[内置 kinds](/docs/dds#kinds)      |
| `value`    | `Sendable` | 遵循[所有权转移规则][ownership]的[可发送值][sendable]        |
| 返回       | `unknown`  | -                                                            |

### `sub(kind, callback)` {#ps.sub}

订阅 `kind` 的本地消息并为其调用 `callback` 处理程序：

```lua
-- 来自同一插件的相同 `kind` 只能订阅一次，
-- 在取消订阅（`unsub()`）之前重新订阅（`sub()`）会抛出错误。
ps.sub("cd", function(body)
	ya.dbg("New cwd", cx.active.current.cwd)
end)
```

它在同步上下文中运行，因此你可以通过 `cx` 访问所有状态以获取感兴趣的数据。

| 输入/输出  | 类型                  | 注意                                                     |
| ---------- | --------------------- | -------------------------------------------------------- |
| `kind`     | `string`              | 带破折号的字母数字，不能是[内置 kinds](/docs/dds#kinds)  |
| `callback` | `fun(body: Sendable)` | 回调中不应进行耗时的工作                                 |
| 返回       | `unknown`             | -                                                        |

### `sub_remote(kind, callback)` {#ps.sub_remote}

与 `sub()` 相同，只是它订阅此 `kind` 的远程消息而不是本地消息。

| 输入/输出  | 类型                  | 注意            |
| ---------- | --------------------- | --------------- |
| `kind`     | `string`              | 与 `sub()` 相同 |
| `callback` | `fun(body: Sendable)` | 与 `sub()` 相同 |
| 返回       | `unknown`             | -               |

### `unsub(kind)` {#ps.unsub}

取消订阅此 `kind` 的本地消息：

```lua
ps.unsub("my-message")
```

| 输入/输出 | 类型      | 注意                                                     |
| --------- | --------- | -------------------------------------------------------- |
| `kind`    | `string`  | 带破折号的字母数字，不能是[内置 kinds](/docs/dds#kinds)  |
| 返回      | `unknown` | -                                                        |

### `unsub_remote(kind)` {#ps.unsub_remote}

取消订阅此 `kind` 的远程消息：

```lua
ps.unsub_remote("my-message")
```

| 输入/输出 | 类型      | 注意              |
| --------- | --------- | ----------------- |
| `kind`    | `string`  | 与 `unsub()` 相同 |
| 返回      | `unknown` | -                 |

## fs {#fs}

以下函数只能在异步上下文中使用。

### `cwd()` {#fs.cwd}

获取进程的当前工作目录（CWD）。

此 API 是为了弥补 Lua 中缺少 [`getcwd`][getcwd] 而添加的；它用于检索最后一次 [`chdir`][chdir] 调用的目录：

```lua
local url, err = fs.cwd()
```

你可能永远不需要它，更可能需要 [`cx.active.current.cwd`][folder-cwd]，这是用户工作的当前目录。

具体来说，当用户更改目录时，`cx.active.current.cwd` 会立即更新，而通过 `chdir` 将此更新与文件系统同步涉及 I/O 操作，例如检查目录是否有效。

因此可能会有一些延迟，这在慢速设备上特别明显。例如，当 HDD 从休眠中唤醒时，通常需要 3~4 秒。

如果你只需要一个有效目录作为进程的 CWD 来开始一些不依赖于 CWD 的工作，它是有用的。

| 输入/输出 | 类型         |
| --------- | ------------ |
| 返回      | `Url?, Error?` |
| 可用      | 仅异步上下文 |

[getcwd]: https://man7.org/linux/man-pages/man3/getcwd.3.html
[chdir]: https://man7.org/linux/man-pages/man2/chdir.2.html
[folder-cwd]: /docs/plugins/context#tab-folder.cwd

### `cha(url, follow)` {#fs.cha}

获取指定 `url` 的 [Cha](/docs/plugins/types#cha)：

```lua
-- 不跟随符号链接
local cha, err = fs.cha(url)

-- 跟随符号链接
local cha, err = fs.cha(url, true)
```

| 输入/输出 | 类型         |
| --------- | ------------ |
| `url`     | `Url`        |
| `follow`  | `boolean?`   |
| 返回      | `Cha?, Error?` |
| 可用      | 仅异步上下文 |

### `write(url, data)` {#fs.write}

将 `data` 写入指定的 `url`：

```lua
local ok, err = fs.write(url, "hello world")
```

| 输入/输出 | 类型         |
| --------- | ------------ |
| `url`     | `Url`        |
| `data`    | `string`     |
| 返回      | `boolean, Error?` |
| 可用      | 仅异步上下文 |

### `create(type, url)` {#fs.create}

在文件系统的 `url` 处创建文件：

```lua
local ok, err = fs.create("dir_all", Url("/tmp/test/nest/nested"))
```

其中 `type` 可以是以下之一：

- `"dir"`：创建一个新的空目录。
- `"dir_all"`：递归创建目录及其所有缺失的父目录。

| 输入/输出 | 类型                               |
| --------- | ---------------------------------- |
| `type`    | `string` \| `"dir"` \| `"dir_all"` |
| `url`     | `Url`                              |
| 返回      | `boolean, Error?`                  |
| 可用      | 仅异步上下文                       |

### `remove(type, url)` {#fs.remove}

删除文件系统的 `url` 处的文件：

```lua
local ok, err = fs.remove("file", Url("/tmp/test.txt"))
```

其中 `type` 可以是以下之一：

- `"file"`：从文件系统中删除文件。
- `"dir"`：删除现有的空目录。
- `"dir_all"`：删除此 url 处的目录，删除其所有内容后。小心使用！
- `"dir_clean"`：删除其下所有空目录，如果目录本身之后也为空，则也删除它。

| 输入/输出 | 类型                                                            |
| --------- | --------------------------------------------------------------- |
| `type`    | `string` \| `"file"` \| `"dir"` \| `"dir_all"` \| `"dir_clean"` |
| `url`     | `Url`                                                           |
| 返回      | `boolean, Error?`                                               |
| 可用      | 仅异步上下文                                                    |

### `read_dir(url, options)` {#fs.read_dir}

读取 `url` 的目录内容：

```lua
local files, err = fs.read_dir(url, {
	-- 如果提供，用于过滤文件的 Glob 模式。
	glob = nil,
	-- 要读取的最大文件数，默认为无限制。
	limit = 10,
	-- 是否解析符号链接，默认为 `false`。
	resolve = false,
})
```

| 输入/输出 | 类型                                                    |
| --------- | ------------------------------------------------------- |
| `url`     | `Url`                                                   |
| `options` | `{ glob: string?, limit: integer?, resolve: boolean? }` |
| 返回      | `File[]?, Error?`                                       |
| 可用      | 仅异步上下文                                            |

### `unique_name(url)` {#fs.unique_name}

从给定的 `url` 获取唯一名称，以确保它在文件系统中是唯一的：

```lua
local url, err = fs.unique_name(Url("/tmp/test.txt"))
```

如果文件已经存在，它会在文件名后附加 `_n`，其中 `n` 是一个数字，并不断递增直到找到第一个可用名称。

| 输入/输出 | 类型         |
| --------- | ------------ |
| `url`     | `Url`        |
| 返回      | `Url?, Error?` |
| 可用      | 仅异步上下文 |

## Command {#command}

你可以通过以下方式调用外部程序：

```lua
local child, err = Command("ls")
	:args({ "-a", "-l" })
	:stdout(Command.PIPED)
	:spawn()
```

与 Lua 的 `os.execute` 相比，它提供了许多全面而方便的方法，整个过程是异步的。

它更好地利用了并发调度的好处。但是，它只能在异步上下文中使用，例如预加载器、预览器和异步功能插件。

### `arg(self, arg)` {#Command.arg}

向命令追加一个或多个参数：

```lua
local cmd = Command("ls"):arg("-a"):arg("-l")
-- 等同于：
local cmd = Command("ls"):arg { "-a", "-l" }
```

| 输入/输出 | 类型                   |
| --------- | ---------------------- |
| `self`    | `Self`                 |
| `arg`     | `string` \| `string[]` |
| 返回      | `self`                 |

### `cwd(self, dir)` {#Command.cwd}

设置命令的当前工作目录：

```lua
local cmd = Command("ls"):cwd("/root")
```

| 输入/输出 | 类型     |
| --------- | -------- |
| `self`    | `Self`   |
| `dir`     | `string` |
| 返回      | `self`   |

### `env(self, key, value)` {#Command.env}

向命令追加环境变量：

```lua
local cmd = Command("ls"):env("PATH", "/bin"):env("HOME", "/home")
```

| 输入/输出 | 类型     |
| --------- | -------- |
| `self`    | `Self`   |
| `key`     | `string` |
| `value`   | `string` |
| 返回      | `self`   |

### `stdin(self, stdio)` {#Command.stdin}

设置命令的 stdin：

```lua
local cmd = Command("ls"):stdin(Command.PIPED)
```

其中 `stdio` 可以是以下之一：

- `Command.PIPED`：管道 stdin。
- `Command.NULL`：丢弃 stdin（默认）。
- `Command.INHERIT`：继承 stdin。

| 输入/输出 | 类型    |
| --------- | ------- |
| `self`    | `Self`  |
| `stdio`   | `Stdio` |
| 返回      | `self`  |

### `stdout(self, stdio)` {#Command.stdout}

设置命令的 stdout：

```lua
local cmd = Command("ls"):stdout(Command.PIPED)
```

其中 `stdio` 可以是以下之一：

- `Command.PIPED`：管道 stdout。
- `Command.NULL`：丢弃 stdout（默认）。
- `Command.INHERIT`：继承 stdout。

| 输入/输出 | 类型    |
| --------- | ------- |
| `self`    | `Self`  |
| `stdio`   | `Stdio` |
| 返回      | `self`  |

### `stderr(self, stdio)` {#Command.stderr}

设置命令的 stderr：

```lua
local cmd = Command("ls"):stderr(Command.PIPED)
```

其中 `stdio` 可以是以下之一：

- `Command.PIPED`：管道 stderr。
- `Command.NULL`：丢弃 stderr（默认）。
- `Command.INHERIT`：继承 stderr。

| 输入/输出 | 类型    |
| --------- | ------- |
| `self`    | `Self`  |
| `stdio`   | `Stdio` |
| 返回      | `self`  |

### `spawn(self)` {#Command.spawn}

生成命令：

```lua
local child, err = Command("ls"):spawn()
```

| 输入/输出 | 类型             |
| --------- | ---------------- |
| `self`    | `Self`           |
| 返回      | `Child?, Error?` |

### `output(self)` {#Command.output}

生成命令并等待其完成：

```lua
local output, err = Command("ls"):output()
```

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| 返回      | `Output?, Error?` |

### `status(self)` {#Command.status}

将命令作为子进程执行，等待其完成并收集其退出状态：

```lua
local status, err = Command("ls"):status()
```

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| 返回      | `Status?, Error?` |

### `__new(value)` {#command.\_\_new}

创建一个新命令。

| 输入/输出 | 类型     |
| --------- | -------- |
| `value`   | `string` |
| 返回      | `Self`   |

## Child {#child}

此对象由 [`Command:spawn()`](#Command.spawn) 创建，表示正在运行的子进程。

你可以通过其专有方法访问此进程的运行时数据。

### `read(self, len)` {#Child.read}

从可用数据源交替读取数据：

```lua
local data, event = child:read(1024)
```

"可用数据源"是指设置了 `Command.PIPED` 的 `stdout` 或 `stderr`，或两者都有，`event` 指示数据来自何处：

- 数据来自 stdout，如果 event 为 0。
- 数据来自 stderr，如果 event 为 1。
- 没有数据可从 stdout 和 stderr 读取，如果 event 为 2。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `len`     | `integer`         |
| 返回      | `string, integer` |

### `read_line(self)` {#Child.read_line}

与 [`read()`](#Child.read) 相同，只是它逐行读取数据：

```lua
local line, event = child:read_line()
```

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| 返回      | `string, integer` |

### `read_line_with(self, opts)` {#Child.read_line_with}

与 [`read_line()`](#Child.read_line) 相同，只是它接受一个选项表：

```lua
local line, event = child:read_line_with {
	-- 读取超时
	timeout = 500,
}
```

它有一个额外的事件：

- 超时，如果 event 为 3。

| 输入/输出 | 类型                   |
| --------- | ---------------------- |
| `self`    | `Self`                 |
| `opts`    | `{ timeout: integer }` |
| 返回      | `string, integer`      |

### `write_all(self, src)` {#Child.write_all}

将所有 `src` 写入子进程的 stdin：

```lua
local ok, err = child:write_all(src)
```

调用此方法时，请确保子进程的 stdin 可用，具体来说：

1. 设置了 [`stdin(Command.PIPED)`](/docs/plugins/utils#Command.stdin)。
2. 从未调用过 [`take_stdin()`](/docs/plugins/utils#Child.take_stdin)。

否则会抛出错误。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `src`     | `string`          |
| 返回      | `boolean, Error?` |

### `flush(self)` {#Child.flush}

将任何缓冲数据刷新到子进程的 stdin：

```lua
local ok, err = child:flush()
```

调用此方法时，请确保子进程的 stdin 可用，具体来说：

1. 设置了 [`stdin(Command.PIPED)`](/docs/plugins/utils#Command.stdin)。
2. 从未调用过 [`take_stdin()`](/docs/plugins/utils#Child.take_stdin)。

否则会抛出错误。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `返回`    | `boolean, Error?` |

### `wait(self)` {#Child.wait}

等待子进程完成：

```lua
local status, err = child:wait()
```

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| 返回      | `Status?, Error?` |

### `wait_with_output(self)` {#Child.wait_with_output}

等待子进程完成并获取输出：

```lua
local output, err = child:wait_with_output()
```

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| 返回      | `Output?, Error?` |

### `start_kill(self)` {#Child.start_kill}

向子进程发送 SIGTERM 信号：

```lua
local ok, err = child:start_kill()
```

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| 返回      | `boolean, Error?` |

### `take_stdin(self)` {#Child.take_stdin}

获取并返回子进程的 stdin 流：

```lua
local stdin = child:take_stdin()
```

此方法只能调用一次，仅适用于设置了 [`stdin(Command.PIPED)`](/docs/plugins/utils#Command.stdin) 的进程；
否则返回 `nil`。

| 输入/输出 | 类型     |
| --------- | -------- |
| `self`    | `Self`   |
| 返回      | `Stdio?` |

### `take_stdout(self)` {#Child.take_stdout}

获取并返回子进程的 stdout 流：

```lua
local stderr = child:take_stdout()
```

这在将 stdout 重定向到另一个进程的 stdin 时很有用：

```lua
local echo = Command("echo"):arg("Hello"):stdout(Command.PIPED):spawn()

local rev = Command("rev"):stdin(echo:take_stdout()):stdout(Command.PIPED):output()

ya.dbg(rev.stdout) -- "olleH\n"
```

此方法只能调用一次，仅适用于设置了 [`stdout(Command.PIPED)`](/docs/plugins/utils#Command.stdin) 的进程；
否则返回 `nil`。

| 输入/输出 | 类型     |
| --------- | -------- |
| `self`    | `Self`   |
| 返回      | `Stdio?` |

### `take_stderr(self)` {#Child.take_stderr}

获取并返回子进程的 stderr 流：

```lua
local stderr = child:take_stderr()
```

参见 [`take_stdout()`](/docs/plugins/utils#Child.take_stdout) 的示例。

此方法只能调用一次，仅适用于设置了 [`stderr(Command.PIPED)`](/docs/plugins/utils#Command.stdin) 的进程；
否则返回 `nil`。

| 输入/输出 | 类型     |
| --------- | -------- |
| `self`    | `Self`   |
| 返回      | `Stdio?` |

## Output {#output}

### `status` {#output.status}

子进程的状态。

|      |                     |
| ---- | ------------------- |
| 类型 | [`Status`](#status) |

### `stdout` {#output.stdout}

子进程的 stdout。

|      |          |
| ---- | -------- |
| 类型 | `string` |

### `stderr` {#output.stderr}

子进程的 stderr。

|      |          |
| ---- | -------- |
| 类型 | `string` |

## Status {#status}

此对象表示子进程的退出状态，由 [`wait()`](#Child.wait) 或 [`output()`](#Command.output) 创建。

### `success` {#status.success}

子进程是否成功退出。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `code` {#status.code}

子进程的退出代码。

|      |            |
| ---- | ---------- |
| 类型 | `integer?` |

<!-- Links -->

[sendable]: /docs/plugins/overview#sendable
[ownership]: /docs/plugins/overview#ownership
