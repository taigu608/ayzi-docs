---
sidebar_position: 3
description: 学习如何访问 Yazi 的同步上下文。
---

# 上下文

## cx {#cx}

你可以在[同步上下文](/docs/plugins/overview#sync-context)中通过 `cx` 访问所有状态。

### `active` {#cx.active}

活动标签。

|      |                        |
| ---- | ---------------------- |
| 类型 | [`tab::Tab`](#tab-tab) |

### `tabs` {#cx.tabs}

所有标签。

|      |                          |
| ---- | ------------------------ |
| 类型 | [`mgr::Tabs`](#mgr-tabs) |

### `tasks` {#cx.tasks}

所有任务。

|      |                                |
| ---- | ------------------------------ |
| 类型 | [`tasks::Tasks`](#tasks-tasks) |

### `yanked` {#cx.yanked}

已复制的文件。

|      |                              |
| ---- | ---------------------------- |
| 类型 | [`mgr::Yanked`](#mgr-yanked) |

## tab::Mode {#tab-mode}

可视模式状态。

### `is_select` {#tab-mode.is_select}

是否处于选择模式。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_unset` {#tab-mode.is_unset}

是否处于取消设置模式。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `is_visual` {#tab-mode.is_visual}

是否处于选择模式或取消设置模式。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `__tostring(self)` {#tab-mode.\_\_tostring}

将模式转换为字符串。

| 输入/输出 | 类型     |
| --------- | -------- |
| `self`    | `Self`   |
| 返回      | `string` |

## tab::Pref {#tab-pref}

特定于标签的用户偏好。

### `sort_by` {#tab-pref.sort_by}

文件排序方法。详见 [`sort_by`](/docs/configuration/yazi#mgr.sort_by)。

|      |                                                                                                                  |
| ---- | ---------------------------------------------------------------------------------------------------------------- |
| 类型 | `"none"` \| `"mtime"` \| `"btime"` \| `"extension"` \| `"alphabetical"` \| `"natural"` \| `"size"` \| `"random"` |

### `sort_sensitive` {#tab-pref.sort_sensitive}

区分大小写排序。详见 [`sort_sensitive`](/docs/configuration/yazi#mgr.sort_sensitive)。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `sort_reverse` {#tab-pref.sort_reverse}

以相反顺序显示文件。详见 [`sort_reverse`](/docs/configuration/yazi#mgr.sort_reverse)。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `sort_dir_first` {#tab-pref.sort_dir_first}

优先显示目录。详见 [`sort_dir_first`](/docs/configuration/yazi#mgr.sort_dir_first)。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `sort_translit` {#tab-pref.sort_translit}

对文件名进行音译排序。详见 [`sort_translit`](/docs/configuration/yazi#mgr.sort_translit)。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `linemode` {#tab-pref.linemode}

行模式。详见 [`linemode`](/docs/configuration/yazi#mgr.linemode)。

|      |                                                                                            |
| ---- | ------------------------------------------------------------------------------------------ |
| 类型 | `string` \| `"none"` \| `"size"` \| `"btime"` \| `"mtime"` \| `"permissions"` \| `"owner"` |

### `show_hidden` {#tab-pref.show_hidden}

显示隐藏文件。详见 [`show_hidden`](/docs/configuration/yazi#mgr.show_hidden)。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

## tab::Selected {#tab-selected}

已选择文件的 [Url](#url)。

### `__len(self)` {#tab-selected.\_\_len}

返回已选择 [Url](#url) 的数量。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| 返回      | `integer` |

### `__pairs(self)` {#tab-selected.\_\_pairs}

遍历已选择的 [Url](#url)。

| 输入/输出 | 类型                                 |
| --------- | ------------------------------------ |
| `self`    | `Self`                               |
| 返回      | `fun(t: self, k: any): integer, Url` |

## tab::Preview {#tab-preview}

预览窗格的状态。

### `skip` {#tab-preview.skip}

要跳过的单位数。单位很大程度上取决于你的预览器，如代码的行数和视频的百分比。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `folder` {#tab-preview.folder}

正在预览的文件夹，如果此预览不是针对文件夹的，则为 `nil`。

|      |                               |
| ---- | ----------------------------- |
| 类型 | [`tab::Folder?`](#tab-folder) |

## tab::Folder {#tab-folder}

一个文件夹。

### `cwd` {#tab-folder.cwd}

当前工作目录。

|      |               |
| ---- | ------------- |
| 类型 | [`Url`](#url) |

### `offset` {#tab-folder.offset}

文件夹的偏移量。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `cursor` {#tab-folder.cursor}

光标位置。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `window` {#tab-folder.window}

可见区域内的文件。

|      |                          |
| ---- | ------------------------ |
| 类型 | [`fs::Files`](#fs-files) |

### `files` {#tab-folder.files}

文件夹中的所有文件。

|      |                          |
| ---- | ------------------------ |
| 类型 | [`fs::Files`](#fs-files) |

### `hovered` {#tab-folder.hovered}

悬停的文件，如果没有文件悬停则为 `nil`。

|      |                         |
| ---- | ----------------------- |
| 类型 | [`fs::File?`](#fs-file) |

## fs::Files {#fs-files}

[`tab::Folder`](#tab-folder) 中的文件。

### `__len(self)` {#fs-files.\_\_len}

返回此文件夹中的文件数量。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| 返回      | `integer` |

### `__index(self, idx)` {#fs-files.\_\_index}

通过索引访问每个文件。

| 输入/输出 | 类型                    |
| --------- | ----------------------- |
| `self`    | `Self`                  |
| `idx`     | `integer`               |
| 返回      | [`fs::File?`](#fs-file) |

## fs::File {#fs-file}

存在于当前上下文中的文件，它继承自 [`File`](/docs/plugins/types#file)，但具有更多特定于上下文的属性和方法。

|      |                                    |                          |
| ---- | ---------------------------------- | ------------------------ |
| 继承 | [`File`](/docs/plugins/types#file) | 访问基本文件属性。       |

### `is_hovered` {#fs-file.is_hovered}

文件是否被悬停。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `size(self)` {#fs-file.size}

文件的大小（以字节为单位），如果是目录且尚未计算，则为 `nil`。

| 输入/输出 | 类型       |
| --------- | ---------- |
| `self`    | `Self`     |
| 返回      | `integer?` |

### `mime(self)` {#fs-file.mime}

文件的 MIME 类型，如果是目录或尚未延迟计算，则为 `nil`。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| 返回      | `string?` |

### `prefix(self)` {#fs-file.prefix}

文件相对于 `CWD` 的前缀，在搜索期间的扁平视图中使用。

例如，如果 `CWD` 是 `/foo`，文件是 `/foo/bar/baz`，则前缀是 `bar/`。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| 返回      | `string?` |

### `icon(self)` {#fs-file.icon}

文件的图标，如果没有 [`[icon]`](/docs/configuration/theme#icon) 规则匹配，则为 `nil`。

| 输入/输出 | 类型    |
| --------- | ------- |
| `self`    | `Self`  |
| 返回      | `Icon?` |

### `style(self)` {#fs-file.style}

文件的样式，如果没有 [`[filetype]`](/docs/configuration/theme#filetype) 规则匹配，则为 `nil`。

| 输入/输出 | 类型     |
| --------- | -------- |
| `self`    | `Self`   |
| 返回      | `Style?` |

### `is_yanked(self)` {#fs-file.is_yanked}

文件是否被复制。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| 返回      | `boolean` |

### `is_selected(self)` {#fs-file.is_selected}

文件是否被选择。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| 返回      | `boolean` |

### `found(self)` {#fs-file.found}

文件查找状态：

- 如果用户不在 [`find`](/docs/configuration/keymap#mgr.find) 模式，则为 `nil`。
- 如果当前文件与用户输入的关键字无关，则为 `nil`。
- 如果当前文件是找到的文件之一，则为 `integer, integer`，其中第一个是它在结果中的索引，第二个是找到的文件总数。

| 输入/输出 | 类型                 |
| --------- | -------------------- |
| `self`    | `Self`               |
| 返回      | `integer?, integer?` |

## mgr::Tabs {#mgr-tabs}

所有标签。

### `idx` {#mgr-tabs.idx}

活动标签的索引。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `__len(self)` {#mgr-tabs.\_\_len}

返回标签的数量。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| 返回      | `integer` |

### `__index(self, idx)` {#mgr-tabs.\_\_index}

通过索引访问每个标签。

| 输入/输出 | 类型                    |
| --------- | ----------------------- |
| `self`    | `Self`                  |
| `idx`     | `integer`               |
| 返回      | [`tab::Tab?`](#tab-tab) |

## tab::Tab {#tab-tab}

一个标签。

### `name` {#tab-tab.name}

标签的名称。

|      |          |
| ---- | -------- |
| 类型 | `string` |

### `mode` {#tab-tab.mode}

标签的模式。

|      |                          |
| ---- | ------------------------ |
| 类型 | [`tab::Mode`](#tab-mode) |

### `pref` {#tab-tab.pref}

标签的偏好设置。

|      |                          |
| ---- | ------------------------ |
| 类型 | [`tab::Pref`](#tab-pref) |

### `current` {#tab-tab.current}

当前工作文件夹。

|      |                              |
| ---- | ---------------------------- |
| 类型 | [`tab::Folder`](#tab-folder) |

### `parent` {#tab-tab.parent}

`CWD` 的父文件夹，如果不存在父文件夹则为 `nil`。

|      |                               |
| ---- | ----------------------------- |
| 类型 | [`tab::Folder?`](#tab-folder) |

### `selected` {#tab-tab.selected}

标签内的已选择文件。

|      |                                  |
| ---- | -------------------------------- |
| 类型 | [`tab::Selected`](#tab-selected) |

### `preview` {#tab-tab.preview}

标签的预览。

|      |                                |
| ---- | ------------------------------ |
| 类型 | [`tab::Preview`](#tab-preview) |

## tasks::Tasks {#tasks-tasks}

### `progress` {#tasks-tasks.progress}

所有任务的进度：

```lua
{
	-- 任务数量
	total = 0,
	succ  = 0,
	fail  = 0,

	-- 任务工作量
	found     = 0,
	processed = 0,
}
```

|      |                                                                                        |
| ---- | -------------------------------------------------------------------------------------- |
| 类型 | `{ total: integer, succ: integer, fail: integer, found: integer, processed: integer }` |

## mgr::Yanked {#mgr-yanked}

已复制的文件。

### `is_cut` {#mgr-yanked.is_cut}

是否处于剪切模式。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

### `__len(self)` {#mgr-yanked.\_\_len}

返回已复制文件的数量。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| 返回      | `integer` |

### `__pairs(self)` {#mgr-yanked.\_\_pairs}

遍历已复制文件的 url。

| 输入/输出 | 类型                                 |
| --------- | ------------------------------------ |
| `self`    | `Self`                               |
| 返回      | `fun(t: self, k: any): integer, Url` |
