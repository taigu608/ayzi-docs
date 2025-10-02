---
sidebar_position: 12
description: Answers to some frequently asked questions about Yazi.
---

# 常见问题

## 为什么 Yazi 这么快? {#why-yazi-fast}

参见 [Why is Yazi fast?](/blog/why-is-yazi-fast)。

## 为什么我无法编辑文本文件? {#why-cant-edit}

在 Linux/macOS 上,Yazi 默认使用 `$EDITOR` 作为文本编辑器。
如果你无法编辑文件,请检查你的 Bash/Zsh/Fish 配置文件中是否有 `export EDITOR=vim` 之类的设置。你也可以[更改 Yazi 的文本 opener](/docs/configuration/yazi#opener) 从 `$EDITOR` 改为 vim/nvim/nano 等。

对于 Windows,没有 `$EDITOR` 的概念,因此用户需要根据需要修改文本 opener。

## 为什么我无法打开/编辑/预览文件? {#why-cant-preview}

Yazi 依赖 `file(1)` 来获取文件的 mimetype 以运行相应的 opener 和 previewer 规则,请检查你的系统是否已预装它。

对于 Windows,请确保你已按照 [Windows 要求](/docs/installation#windows)设置了 `YAZI_FILE_ONE` 环境变量。

## 为什么我的文本颜色不明显? {#why-text-indistinct}

Yazi 的默认主题使用 base16 颜色,以尽可能匹配用户的终端主题。

不幸的是,这无法满足所有用户的需求,甚至同一用户在亮/暗模式下所需的颜色也可能不同,更不用说某些终端的默认配色方案很差,比如这个 [#149 (comment)](https://github.com/sxyazi/yazi/issues/149#issuecomment-1798349727)。

因此,请[使用与你的终端主题匹配的 Yazi flavor](https://github.com/yazi-rs/flavors)。当然,如果你找到了更好的颜色能覆盖大多数终端,欢迎创建 PR!

## 为什么 "Open" 和 "Enter" 不能是同一个命令? {#why-separate-open-enter}

将 `enter` 和 `open` 命令分开是有意为之的。

Yazi 将来会添加将归档视为目录的功能,允许直接操作里面的文件。

归档是一个文件,所以它是"可打开的",但它也作为目录是"可进入的";因此用户可以选择他们想要执行的操作。

对于实际的目录也是如此 - 目录可以进入(在 Yazi 中),或打开(在 VSCode 或桌面文件管理器等程序中)。

如果你确实不需要区分它们,可以使用这个 [smart-enter 技巧](/docs/tips#smart-enter)。

## 为什么我的图标在 [kitty](https://sw.kovidgoyal.net/kitty/) 中缩小,滚动时变大? {#why-icons-shrink}

简而言之:为 Yazi 使用 flavor,https://github.com/yazi-rs/flavors

这可能是 kitty 中的一个 bug(或者功能?我不知道)。在 kitty 中,你必须为文件列表项添加样式(如前景色)才能使图标与文本大小匹配。然而,Yazi 的默认主题无法添加该颜色,因为它无法预测用户的终端是白底黑字还是黑底白字。

所以它继承了默认的终端字体颜色。这导致了图标大小问题,而且我只在 kitty 中发现了这个问题 - 其他终端没有这个问题。因此,请为 kitty 终端使用 Yazi flavor。

## 如何排查终端响应超时错误? {#trt}

这个错误发生在 tmux 中运行 Yazi 时 - tmux 以各种方式干扰了 Yazi 和实际终端之间的通信。

为了避免 tmux 的任何干扰,Yazi 必须实现许多 hack,大多数情况下都能正常工作。如果对你不起作用,请检查:

1. 你的 tmux 是最新的吗?
2. 你是否已[为 tmux 启用了 passthrough](/docs/image-preview#tmux)?
3. 你是否将 `Alt+Shift+p` 绑定到了 tmux?[tmux 目前不支持 `DECRQSS`](https://github.com/tmux/tmux/issues/4034),并且[它的响应](https://vt100.net/docs/vt510-rm/DECRQSS.html)与 <kbd>Alt</kbd> 序列部分重叠,导致 tmux 错误地将其解释为按键事件。
4. 如果你使用的是 `st` 终端,它是最新的吗?旧版本的 `st` 直到 [f17abd25](https://git.suckless.org/st/commit/f17abd25b376c292f783062ecf821453eaa9cc4c.html) 修复之前都不能正确支持 [Device Status Report (DSR)](https://vt100.net/docs/vt510-rm/DSR.html)。确保你的 `st` 或其分支已包含该补丁。
5. 注释掉所有自定义配置 _除了_ [passthrough](/docs/image-preview#tmux),以检查问题是否由你的设置引起。如果是,逐个添加回来以找出原因。

## 为什么 "orphan" 默认设置为 false? {#why-orphan-false}

`orphan=true` 是一个紧急出口;一旦指定,你的任务将不会被 Yazi 管理。

例如,如果你意识到对错误的文件使用了 `unzip`,需要取消它,使用 `orphan=false`,你可以在 Yazi 的任务管理器中按 `x` 轻松完成。
但是,使用 `orphan=true`,你只能返回 shell 来终止它。

另一方面,`orphan=false` 的任务通过 Yazi 任务系统调度。它可以限制并发任务数量(用户可配置),以防止系统资源耗尽,例如当你提取 100 个文件时。

## 我不喜欢 nerd‐fonts! {#dont-like-nerd-fonts}

Yazi 默认启用 `nerd-fonts` 图标,看起来很酷!

如果你不想使用它并希望保持简洁,当然可以,你可以在 [`theme.toml`](/docs/configuration/theme) 中随意修改这些图标:

```toml
[status]
sep_left = { open = "", close = "" }
sep_right = { open = "", close = "" }

[icon]
globs = []
dirs  = []
files = []
exts  = []
conds = []
```

上面的代码隐藏了所有图标,整个世界都安静了。不错!

## 为什么 XXX 功能不存在? {#why-no-xxx}

Yazi 是一个新项目,从第一次提交到现在(截至 2023 年 9 月 20 日)不到 3 个月,一些功能仍在大力开发中。

如果你发现缺少 XXX 功能,请检查它是否已在 [Feature Requests](https://github.com/sxyazi/yazi/issues/51) 中提到,并搜索相关问题。如果没有,你可以创建一个新请求。

我们的时间片是有限的,它们被分配给我们认为关键的问题,例如 bug 修复和使 Yazi 稳定,这包括解决可能在未来导致破坏性更改的提案。

大多数维护者都有全职工作,所以如果你的请求没有及时实现,请理解。当然,我们欢迎任何建设性的 PR 来帮助加快你想要的功能的实现!
