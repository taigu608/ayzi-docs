---
sidebar_position: 4
description: How to preview images in Yazi.
---

# 图像预览

Yazi 做了大量工作以适应不同的终端和多路复用器,尽力为用户提供开箱即用的体验。

这绝非易事,为了减少维护成本,我们只保证它在终端和多路复用器(tmux、Zellij)的 **_最新版本_** 中可用:

| 平台                                                                          | 协议                                   | 支持                                                 |
| ---------------------------------------------------------------------------- | -------------------------------------- | ---------------------------------------------------- |
| [kitty](https://github.com/kovidgoyal/kitty) (>= 0.28.0)                     | [Kitty unicode placeholders][kgp]      | ✅ 内置                                              |
| [iTerm2](https://iterm2.com)                                                 | [Inline images protocol][iip]          | ✅ 内置                                              |
| [WezTerm](https://github.com/wez/wezterm)                                    | [Inline images protocol][iip]          | ✅ 内置                                              |
| [Konsole](https://invent.kde.org/utilities/konsole)                          | [Kitty old protocol][kgp-old]          | ✅ 内置                                              |
| [foot](https://codeberg.org/dnkl/foot)                                       | [Sixel graphics format][sixel]         | ✅ 内置                                              |
| [Ghostty](https://github.com/ghostty-org/ghostty)                            | [Kitty unicode placeholders][kgp]      | ✅ 内置                                              |
| [Windows Terminal](https://github.com/microsoft/terminal) (>= v1.22.10352.0) | [Sixel graphics format][sixel]         | ✅ 内置                                              |
| [st with Sixel patch](https://github.com/bakkeby/st-flexipatch)              | [Sixel graphics format][sixel]         | ✅ 内置                                              |
| [Warp](https://www.warp.dev) (macOS/Linux only)                              | [Inline images protocol][iip]          | ✅ 内置                                              |
| [Tabby](https://github.com/Eugeny/tabby)                                     | [Inline images protocol][iip]          | ✅ 内置                                              |
| [VSCode](https://github.com/microsoft/vscode)                                | [Inline images protocol][iip]          | ✅ 内置                                              |
| [Rio](https://github.com/raphamorim/rio)                                     | [Inline images protocol][iip]          | ❌ Rio 无法正确清除图像 [#709][rio-bug]               |
| [Black Box](https://gitlab.gnome.org/raggesilver/blackbox)                   | [Sixel graphics format][sixel]         | ✅ 内置                                              |
| [Hyper](https://github.com/vercel/hyper)                                     | [Inline images protocol][iip]          | ✅ 内置                                              |
| [Bobcat](https://github.com/ismail-yilmaz/Bobcat)                            | [Inline images protocol][iip]          | ✅ 内置                                              |
| X11 / Wayland                                                                | Window system protocol                 | ☑️ 需要 [Überzug++][ueberzug]                        |
| Fallback                                                                     | [ASCII art (Unicode block)][ascii-art] | ☑️ 需要 [Chafa][chafa]                               |

Yazi 会根据从上到下的优先级自动为你选择合适的预览方法。
这依赖于 `$TERM`、`$TERM_PROGRAM` 和 `$XDG_SESSION_TYPE` 变量,确保你没有错误地覆盖它们!

例如,如果你的终端是 Alacritty,它本身不支持显示图像,但你在 X11/Wayland 环境中运行,
它会自动使用 "Window system protocol" 来显示图像 - 这需要你安装 Überzug++。

<!-- Protocols -->

[kgp]: https://sw.kovidgoyal.net/kitty/graphics-protocol/#unicode-placeholders
[kgp-old]: https://github.com/sxyazi/yazi/blob/main/yazi-adapter/src/drivers/kgp_old.rs
[iip]: https://iterm2.com/documentation-images.html
[sixel]: https://www.vt100.net/docs/vt3xx-gp/chapter14.html
[ascii-art]: https://en.wikipedia.org/wiki/ASCII_art

<!-- Dependencies -->

[ueberzug]: https://github.com/jstkdng/ueberzugpp
[chafa]: https://hpjansson.org/chafa/

<!-- Rio bug -->

[rio-bug]: https://github.com/raphamorim/rio/issues/709

## tmux 用户 {#tmux}

要使 Yazi 的图像预览在 tmux 中正常工作,请在你的 `tmux.conf` 中添加以下 3 个选项:

```sh
set -g allow-passthrough on
set -ga update-environment TERM
set -ga update-environment TERM_PROGRAM
```

然后重启 tmux(重要):

```sh
tmux kill-server && tmux || tmux
```

现在你应该可以享受图像预览了。

注意,如果[你使用的协议](#protocol)是 Sixel,请确保在构建 tmux 时传递了 `--enable-sixel`,因为它默认是禁用的。
你可以通过 [tmux/tmux#4104](https://github.com/tmux/tmux/issues/4104#issuecomment-2326339395) 验证这一点。

## Zellij 用户 {#zellij}

Zellij 目前只支持 Sixel 图形格式,因此你需要一个也支持 Sixel 的终端。

注意,Zellij 的 Sixel 实现目前存在很多 bug 并且有严重的性能问题,
在快速切换图像时会导致明显的卡顿,有时甚至会[图像撕裂](https://github.com/zellij-org/zellij/issues/2576#issuecomment-1707107473)或[完全无法工作](https://github.com/zellij-org/zellij/issues/2814#issuecomment-2318473921)。

这种情况在 Zellij 改进其 Sixel 实现或[提供 passthrough 模式](https://github.com/zellij-org/zellij/issues/775)之前不会改善。如果图像对你更重要,考虑在 Zellij 外运行 Yazi 或使用 Überzug++:

```sh
# 欺骗 Yazi 认为你在 kitty 中运行,
# 强制它回退到 Überzug++ 或 Chafa
TERM=xterm-kitty yazi
```

## Windows 用户 {#windows}

目前,只有以下 3 个终端支持在 Windows 上显示图像:

- [WezTerm](https://wezterm.org/install/windows.html) (latest nightly)
- [Windows Terminal](https://github.com/microsoft/terminal/releases) (>= v1.22.10352.0)
- [Bobcat v0.9.0](https://github.com/ismail-yilmaz/Bobcat/releases/tag/0.9.0)

## 使用 WSL 的 Windows 用户 {#wsl}

由于 ConPTY 的限制,Windows 版本不得不实现许多变通方法,这些方法并不完美。

但是,如果你在 WSL 中运行 Yazi,你可以通过 [`wezterm ssh`][wezterm-ssh] 体验完美的图像预览。<br/>
[WezTerm][wezterm] 是一个出色的终端,可以通过其 SSH 功能绕过 ConPTY 的限制,它目前是唯一允许这种方法的终端。

你需要在 WSL 中安装 `sshd` 并启动它:

```sh
sudo apt install openssh-server
sudo service ssh restart
```

然后,在 _主机_ 机器上,通过 SSH 连接到 WSL:

```sh
wezterm ssh 127.0.0.1
```

就是这样!你现在可以让 Yazi 的图像预览正常工作了。

[wezterm]: https://wezfurlong.org/wezterm/
[wezterm-ssh]: https://wezfurlong.org/wezterm/cli/ssh.html

## Neovim 用户 {#neovim}

Neovim 中的内置终端模拟器(`:term`)[不支持任何图形协议](https://github.com/neovim/neovim/issues/4349),因此 Yazi 会尝试按顺序回退到 X11/Wayland/Chafa。

注意,Überzug++ 可能会在错误的位置显示图像;在这种情况下,请使用 [`ueberzug_offset`](/docs/configuration/yazi/#preview.ueberzug_scale) 手动调整。

## 为什么我的图像不能适应终端大小? {#size}

图像的大小取决于两个因素:

1. [max_width](/docs/configuration/yazi#preview.max_width) 和 [max_height](/docs/configuration/yazi#preview.max_height) 配置选项,需要用户根据需要调整。
2. 终端的像素大小。

Yazi 将使用这两个因素中较小的一个作为图像预览大小。

但是,某些终端(如 VSCode、Tabby 和所有 Windows 终端)没有实现 `ioctl` 系统调用,在 [Add `CSI 14 t` sequence support](https://github.com/crossterm-rs/crossterm/pull/810) 合并之前,无法获取终端的实际像素宽度和高度。

因此,在这种情况下只会使用 `max_width` 和 `max_height`。

## 如何知道 Yazi 使用什么图像协议? {#protocol}

Yazi 提供了一个 `yazi --debug` 命令,包含所有环境信息,如终端模拟器、图像适配器、是否在 SSH 模式等。

在你想要预览图像的终端中运行它,你会看到如下输出:

```sh
...
Adapter
    Adapter.matches: Kgp
...
```

这表示 Yazi 检测到并使用的图像协议:

| `Adapter.matches` | 协议                                   | 说明                                                                                              |
| ----------------- | -------------------------------------- | ------------------------------------------------------------------------------------------------- |
| `Kgp`             | [Kitty unicode placeholders][kgp]      | 确保你的终端是最新的以支持它                                                                        |
| `KgpOld`          | [Kitty old protocol][kgp-old]          | 由于协议本身的限制,在 `tmux` 下不工作                                                              |
| `Iip`             | [Inline images protocol][iip]          | -                                                                                                 |
| `Sixel`           | [Sixel graphics format][sixel]         | 如果你使用其中任何一个,请参阅 [tmux](#tmux) 和 [Zellij](#zellij) 部分                              |
| `X11`             | Window system protocol                 | 需要 [Überzug++][ueberzug]                                                                        |
| `Wayland`         | Window system protocol                 | 需要 [Überzug++][ueberzug] 并且 [_仅_ 支持 Sway、Hyprland 和 Wayfire][uberzug-wayland]           |
| `Chafa`           | [ASCII art (Unicode block)][ascii-art] | 需要 [Chafa][chafa] 作为最后的回退手段                                                             |

[uberzug-wayland]: https://github.com/jstkdng/ueberzugpp/blob/eea57daece774e152aedba9ac82a8113056fbab4/README.md?plain=1#L12

## 为什么我无法通过 Überzug++ 预览图像? {#debug-ueberzug}

这可能是 Überzug++ 本身的问题。请[在调试模式下运行 Yazi](/docs/plugins/overview#logging),然后悬停在导致问题的图像上。

然后在你的日志文件中按时间排序找到最后一个 Überzug++ 命令,它通常在文件的最末尾,看起来像这样:

```
ueberzugpp command: {"action":"add","identifier":"yazi","x":96,"y":1,"max_width":400,"max_height":150,"path":"/root/test.jpg"}
```

最后,直接在终端中运行 `ueberzugpp layer`,不要在 Yazi 内和外部,并粘贴命令:

```sh
{"action":"add","identifier":"yazi","x":96,"y":1,"max_width":400,"max_height":150,"path":"/root/test.jpg"}
```

输入后,按 `Enter`,看看是否显示任何图像,不要退出 Überzug++。

如果独立使用 Überzug++ 时图像显示正常,但与 Yazi 一起使用时不正常,请创建一个 bug 报告,包含:

- 你的日志文件内容。
- `/tmp/ueberzugpp-$USER.log` 的内容。
- 上述步骤的 GIF 演示。
