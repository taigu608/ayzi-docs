---
sidebar_position: 1
description: 学习如何使用 Yazi 的 Lua API。
---

# 类型

## Url {#url}

创建一个 Url：

```lua
-- 常规文件
local url = Url("/root/Downloads/logo.png")

-- 来自归档 `ost.zip` 的 `bgm.mp3`
local url = Url("archive:///root/ost.zip#bgm.mp3")
```

### `name` {#url.name}

url 的文件名。

|      |           |
| ---- | --------- |
| 类型 | `string?` |

### `stem` {#url.stem}

不带扩展名的文件名。

|      |           |
| ---- | --------- |
| 类型 | `string?` |

### `frag` {#url.frag}

Url 片段。

假设 url 是 `archive:///root/my-archive.zip#1.jpg`，片段是 `1.jpg`。

|      |           |
| ---- | --------- |
| 类型 | `string?` |

### `parent` {#url.parent}

父目录。

|      |         |
| ---- | ------- |
| 类型 | `Self?` |

### `is_regular` {#url.is_regular}

url 表示的文件是否为常规文件。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_search`

url 表示的文件是否来自搜索结果。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_archive` {#url.is_archive}

url 表示的文件是否来自归档。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_absolute`

url 表示的路径是否为绝对路径。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `has_root` {#url.has_root}

url 表示的路径是否有根。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `join(self, another)` {#url.join}

与 `another` 连接以创建新 url。

| 输入/输出 | 类型               |
| --------- | ------------------ |
| `self`    | `Self`             |
| `another` | `Self` \| `string` |
| 返回      | `Self`             |

### `starts_with(self, another)` {#url.starts_with}

url 是否以 `another` 开头。

| 输入/输出 | 类型               |
| --------- | ------------------ |
| `self`    | `Self`             |
| `another` | `Self` \| `string` |
| 返回      | `boolean`          |

### `ends_with(self, another)` {#url.ends_with}

url 是否以 `another` 结尾。

| 输入/输出 | 类型               |
| --------- | ------------------ |
| `self`    | `Self`             |
| `another` | `Self` \| `string` |
| 返回      | `boolean`          |

### `strip_prefix(self, another)` {#url.strip_prefix}

去除 `another` 的前缀。

| 输入/输出 | 类型               |
| --------- | ------------------ |
| `self`    | `Self`             |
| `another` | `Self` \| `string` |
| 返回      | `Self`             |

### `__eq(self, another)` {#url.\_\_eq}

url 是否等于 `another`。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| `another` | `Self`    |
| 返回      | `boolean` |

### `__tostring(self)` {#url.\_\_tostring}

将 url 转换为字符串。

| 输入/输出 | 类型     |
| --------- | -------- |
| `self`    | `Self`   |
| 返回      | `string` |

### `__concat(self, another)` {#url.\_\_concat}

将 url 与 `another` 连接。

| 输入/输出 | 类型     |
| --------- | -------- |
| `self`    | `Self`   |
| `another` | `string` |
| 返回      | `Self`   |

### `__new(value)` {#url.\_\_new}

创建一个新 url。

| 输入/输出 | 类型               |
| --------- | ------------------ |
| `value`   | `string` \| `Self` |
| 返回      | `Self`             |

## Cha {#cha}

一个文件的特征。

### `is_dir` {#cha.is_dir}

文件是否为目录。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_hidden` {#cha.is_hidden}

文件是否为隐藏文件。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_link` {#cha.is_link}

文件是否为符号链接。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_orphan` {#cha.is_orphan}

文件是否为损坏的符号链接，指向不存在的文件。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_dummy` {#cha.is_dummy}

文件是否为虚拟文件，无法加载完整元数据，可能文件系统不支持，如 FUSE。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_block` {#cha.is_block}

文件是否为块设备。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_char` {#cha.is_char}

文件是否为字符设备。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_fifo` {#cha.is_fifo}

文件是否为 FIFO。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_sock` {#cha.is_sock}

文件是否为套接字。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_exec` {#cha.is_exec}

文件是否可执行。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_sticky` {#cha.is_sticky}

文件是否设置了粘滞位。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `len` {#cha.len}

文件的长度（以字节为单位）。

如果你想获取目录的大小，请使用 [`size()`](/docs/plugins/context#fs-file.size)。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `atime` {#cha.atime}

文件的访问时间（Unix 时间戳）。

|      |            |
| ---- | ---------- |
| 类型 | `integer?` |

### `btime` {#cha.btime}

文件的创建时间（Unix 时间戳）。

|      |            |
| ---- | ---------- |
| 类型 | `integer?` |

### `mtime` {#cha.mtime}

文件的修改时间（Unix 时间戳）。

|      |            |
| ---- | ---------- |
| 类型 | `integer?` |

### `uid` {#cha.uid}

文件的用户 id。

|      |                |
| ---- | -------------- |
| 类型 | `integer?`     |
| 可用 | 仅 Unix-like 系统 |

### `gid` {#cha.gid}

文件的组 id。

|      |                |
| ---- | -------------- |
| 类型 | `integer?`     |
| 可用 | 仅 Unix-like 系统 |

### `nlink` {#cha.nlink}

文件的硬链接数。

|      |                |
| ---- | -------------- |
| 类型 | `integer?`     |
| 可用 | 仅 Unix-like 系统 |

### `perm(self)` {#cha.perm}

Unix 权限表示，如 `drwxr-xr-x`。

|      |                |
| ---- | -------------- |
| 类型 | `string?`      |
| 可用 | 仅 Unix-like 系统 |

## File {#file}

一个没有任何上下文信息的裸文件。另见 [`fs::File`](/docs/plugins/context#fs-file)。

### `url` {#file.url}

文件的 url。

|      |       |
| ---- | ----- |
| 类型 | `Url` |

### `cha` {#file.cha}

文件的 cha。

|      |       |
| ---- | ----- |
| 类型 | `Cha` |

### `link_to` {#file.link_to}

文件指向的 url，如果它是符号链接。

|      |        |
| ---- | ------ |
| 类型 | `Url?` |

### `name` {#file.name}

文件的名称。

|      |          |
| ---- | -------- |
| 类型 | `string` |

## Icon {#icon}

一个图标。

### `text` {#icon.text}

图标的文本。

|      |          |
| ---- | -------- |
| 类型 | `string` |

### `style` {#icon.style}

图标的[样式](/docs/plugins/layout#style)。

|      |         |
| ---- | ------- |
| 类型 | `Style` |

## Error {#error}

一个错误。

### `code` {#error.code}

原始错误代码。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `__tostring(self)` {#error.\_\_tostring}

将错误转换为字符串。

| 输入/输出 | 类型     |
| --------- | -------- |
| `self`    | `Self`   |
| 返回      | `string` |

### `__concat(self, another)` {#error.\_\_concat}

将错误与 `another` 连接。

| 输入/输出 | 类型     |
| --------- | -------- |
| `self`    | `Self`   |
| `another` | `string` |
| 返回      | `Error`  |

## Window {#window}

### `rows` {#window.rows}

行数。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `cols` {#window.cols}

列数。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `width` {#window.width}

宽度（以像素为单位）。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `height` {#window.height}

高度（以像素为单位）。

|      |           |
| ---- | --------- |
| 类型 | `integer` |
