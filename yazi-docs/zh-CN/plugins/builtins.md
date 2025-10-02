---
sidebar_position: 6
description: 学习如何配置 Yazi 的内置插件。
---

# 内置插件

Yazi 附带了有用的内置插件，无需额外设置即可帮助增强你的工作流程。本页介绍这些内置插件及其可用的配置选项。

## `fzf.lua` {#fzf}

将 [`fzf`](https://github.com/junegunn/fzf) 的强大功能集成到 Yazi 中，让你能够通过模糊匹配快速搜索和导航文件和目录。

源代码：https://github.com/sxyazi/yazi/blob/main/yazi-plugin/preset/plugins/fzf.lua

### 用法

如何调用 fzf：

- 按 <kbd>z</kbd> 在 CWD 内快速导航文件子树。
- 或者，如果你处于选择模式，按 <kbd>z</kbd> 在已选择的项目中快速导航。

如果你退出 fzf 时选择了单个文件：

- [`reveal`](/docs/configuration/keymap#mgr.reveal) 该文件。
- 或者，如果它是目录，则 [`cd`](/docs/configuration/keymap#mgr.cd) 到它。

如果你退出 fzf 时选择了多个文件：

- 在 Yazi 中选择 fzf 选择的文件。
- 或者，如果你处于选择模式，在 Yazi 中取消选择 fzf 选择的文件。

## `zoxide.lua` {#zoxide}

通过 [`zoxide`](https://github.com/ajeetdsouza/zoxide)（一个更智能的 `cd`）增强你在外部 shell 中历史目录导航的体验。

源代码：https://github.com/sxyazi/yazi/blob/main/yazi-plugin/preset/plugins/zoxide.lua

### 用法

点击 <kbd>Z</kbd> 启动交互式 zoxide UI。请确保：

1. 你已安装最新版本的 zoxide。
2. 你已安装最新版本的 [fzf](https://github.com/junegunn/fzf)，这是 zoxide 的依赖项。
3. 你已根据[其文档](https://github.com/ajeetdsouza/zoxide?tab=readme-ov-file#installation)为你的 shell 正确配置 zoxide。

### 选项

| 选项             | 描述                                    |
| ---------------- | --------------------------------------- |
| `update_db` (bool) | 每当你切换 CWD 时，将路径添加到 zoxide 数据库。 |

你可以在 `init.lua` 中 _可选地_ 更改某些选项，如下所示：

```lua
-- ~/.config/yazi/init.lua
require("zoxide"):setup {
	update_db = true,
}
```
