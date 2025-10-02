---
sidebar_position: 2
description: A quick guide on the basic usage of Yazi.
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# 快速开始

一旦你[安装了 Yazi](/docs/installation),使用以下命令启动程序:

```sh
yazi
```

按 <kbd>q</kbd> 退出,<kbd>F1</kbd> 或 <kbd>~</kbd> 打开帮助菜单。

## Shell wrapper

我们建议使用这个 `y` shell wrapper,它提供了退出 Yazi 时更改当前工作目录的功能。

<Tabs>
  <TabItem value="bash-zsh" label="Bash / Zsh" default>

```bash
function y() {
	local tmp="$(mktemp -t "yazi-cwd.XXXXXX")" cwd
	yazi "$@" --cwd-file="$tmp"
	IFS= read -r -d '' cwd < "$tmp"
	[ -n "$cwd" ] && [ "$cwd" != "$PWD" ] && builtin cd -- "$cwd"
	rm -f -- "$tmp"
}
```

  </TabItem>
  <TabItem value="fish" label="Fish">

```sh
function y
	set tmp (mktemp -t "yazi-cwd.XXXXXX")
	yazi $argv --cwd-file="$tmp"
	if read -z cwd < "$tmp"; and [ -n "$cwd" ]; and [ "$cwd" != "$PWD" ]
		builtin cd -- "$cwd"
	end
	rm -f -- "$tmp"
end
```

  </TabItem>
  <TabItem value="nushell" label="Nushell">

```sh
def --env y [...args] {
	let tmp = (mktemp -t "yazi-cwd.XXXXXX")
	yazi ...$args --cwd-file $tmp
	let cwd = (open $tmp)
	if $cwd != "" and $cwd != $env.PWD {
		cd $cwd
	}
	rm -fp $tmp
}
```

  </TabItem>
  <TabItem value="elvish" label="Elvish">

```elvish
edit:add-var y~ {|@argv|
	use str
	use os
	use file
	var tmp = (os:temp-file)
	yazi $@argv --cwd-file=$tmp[name]
	var cwd = (slurp < $tmp)
	file:close $tmp
	os:remove $tmp[name]
	if (and (not-eq $cwd '') (not-eq $cwd $pwd)) {
		cd $cwd
	}
}
```

  </TabItem>
  <TabItem value="powershell" label="PowerShell">

```powershell
function y {
    $tmp = (New-TemporaryFile).FullName
    yazi $args --cwd-file="$tmp"
    $cwd = Get-Content -Path $tmp -Encoding UTF8
    if (-not [String]::IsNullOrEmpty($cwd) -and $cwd -ne $PWD.Path) {
        Set-Location -LiteralPath (Resolve-Path -LiteralPath $cwd).Path
    }
    Remove-Item -Path $tmp
}
```

  </TabItem>
  <TabItem value="command-prompt" label="Command Prompt">

创建文件 `y.cmd` 并将其放在你的 `%PATH%` 中。

```cmd
@echo off

set tmpfile=%TEMP%\yazi-cwd.%random%

yazi %* --cwd-file="%tmpfile%"

:: If the file does not exist, then exit
if not exist "%tmpfile%" exit /b 0

:: If the file exist, then read the content and change the directory
set /p cwd=<"%tmpfile%"
if not "%cwd%"=="" (
    cd /d "%cwd%"
)
del "%tmpfile%"
```

  </TabItem>
  <TabItem value="xonsh" label="Xonsh">

```xonsh
def _y(args):
    tmp = $(mktemp -t "yazi-cwd.XXXXXX")
    args.append(f"--cwd-file={tmp}")
    $[yazi @(args)]
    with open(tmp) as f:
        cwd = f.read()
    if cwd != $PWD:
        cd @(cwd)
    rm -f -- @(tmp)

aliases["y"] = _y
```

  </TabItem>
</Tabs>

要使用它,将函数复制到你相应的 shell 配置文件中。

然后使用 `y` 而不是 `yazi` 来启动,按 <kbd>q</kbd> 退出,你会看到 CWD 已更改。有时,你不想更改,按 <kbd>Q</kbd> 退出。

## 快捷键

:::tip
要查看所有快捷键,请参阅[默认的 `keymap.toml` 文件](https://github.com/sxyazi/yazi/blob/shipped/yazi-config/preset/keymap-default.toml)。
:::

### 导航

要在文件和目录之间导航,你可以使用箭头键 <kbd>←</kbd>、<kbd>↓</kbd>、<kbd>↑</kbd> 和 <kbd>→</kbd>
或类似 Vim 的键,如 <kbd>h</kbd>、<kbd>j</kbd>、<kbd>k</kbd>、<kbd>l</kbd>:

| 快捷键       | 备用键        | 操作                     |
| ------------ | ------------- | ------------------------ |
| <kbd>k</kbd> | <kbd>↑</kbd>  | 向上移动光标             |
| <kbd>j</kbd> | <kbd>↓</kbd>  | 向下移动光标             |
| <kbd>l</kbd> | <kbd>→</kbd>  | 进入悬停的目录           |
| <kbd>h</kbd> | <kbd>←</kbd>  | 离开当前目录并进入其父目录 |

更多导航命令可以在下表中找到。

| 快捷键                  | 操作                                                         |
| ----------------------- | ------------------------------------------------------------ |
| <kbd>K</kbd>            | 在预览中向上查找 5 个单位                                      |
| <kbd>J</kbd>            | 在预览中向下查找 5 个单位                                      |
| <kbd>g</kbd> ⇒ <kbd>g</kbd> | 将光标移到顶部                                           |
| <kbd>G</kbd>            | 将光标移到底部                                                |
| <kbd>z</kbd>            | 通过 fzf [Cd][mgr.cd] 到目录或 [reveal][mgr.reveal] 文件      |
| <kbd>Z</kbd>            | 通过 zoxide [Cd][mgr.cd] 到目录                               |

[mgr.cd]: /docs/configuration/keymap#mgr.cd
[mgr.reveal]: /docs/configuration/keymap#mgr.reveal

### 选择

要选择文件和目录,可以使用以下命令。

| 快捷键                     | 操作                         |
| -------------------------- | ---------------------------- |
| <kbd>Space</kbd>           | 切换悬停文件/目录的选择状态   |
| <kbd>v</kbd>               | 进入可视模式(选择模式)        |
| <kbd>V</kbd>               | 进入可视模式(取消选择模式)     |
| <kbd>Ctrl</kbd> + <kbd>a</kbd> | 选择所有文件               |
| <kbd>Ctrl</kbd> + <kbd>r</kbd> | 反向选择所有文件           |
| <kbd>Esc</kbd>             | 取消选择                      |

### 文件操作

要与选定的文件/目录交互,使用以下任何命令。

| 快捷键                          | 操作                                                              |
| ------------------------------- | ----------------------------------------------------------------- |
| <kbd>o</kbd>                    | 打开选定的文件                                                     |
| <kbd>O</kbd>                    | 交互式打开选定的文件                                               |
| <kbd>Enter</kbd>                | 打开选定的文件                                                     |
| <kbd>Shift</kbd> + <kbd>Enter</kbd> | 交互式打开选定的文件(某些终端尚不支持)                         |
| <kbd>Tab</kbd>                  | 显示文件信息                                                       |
| <kbd>y</kbd>                    | 复制选定的文件(复制)                                               |
| <kbd>x</kbd>                    | 复制选定的文件(剪切)                                               |
| <kbd>p</kbd>                    | 粘贴复制的文件                                                     |
| <kbd>P</kbd>                    | 粘贴复制的文件(如果目标存在则覆盖)                                   |
| <kbd>Y</kbd> 或 <kbd>X</kbd>    | 取消复制状态                                                       |
| <kbd>d</kbd>                    | 将选定的文件移到回收站                                              |
| <kbd>D</kbd>                    | 永久删除选定的文件                                                  |
| <kbd>a</kbd>                    | 创建文件(以 / 结尾表示目录)                                         |
| <kbd>r</kbd>                    | 重命名选定的文件                                                   |
| <kbd>.</kbd>                    | 切换隐藏文件的可见性                                                |

更多文件操作命令可以在下表中找到。

| 快捷键                     | 操作                           |
| -------------------------- | ------------------------------ |
| <kbd>;</kbd>               | 运行 shell 命令                |
| <kbd>:</kbd>               | 运行 shell 命令(阻塞直到完成)   |
| <kbd>-</kbd>               | 对复制文件的绝对路径创建符号链接 |
| <kbd>\_</kbd>              | 对复制文件的相对路径创建符号链接 |
| <kbd>Ctrl</kbd> + <kbd>-</kbd> | 对复制的文件创建硬链接      |

### 复制路径

要复制路径,使用以下任何命令。

_注意:<kbd>c</kbd> ⇒ <kbd>d</kbd> 表示按 <kbd>c</kbd> 键,然后按 <kbd>d</kbd> 键。_

| 快捷键                  | 操作                     |
| ----------------------- | ------------------------ |
| <kbd>c</kbd> ⇒ <kbd>c</kbd> | 复制文件路径         |
| <kbd>c</kbd> ⇒ <kbd>d</kbd> | 复制目录路径         |
| <kbd>c</kbd> ⇒ <kbd>f</kbd> | 复制文件名           |
| <kbd>c</kbd> ⇒ <kbd>n</kbd> | 复制不带扩展名的文件名 |

### 过滤文件

| 快捷键   | 操作     |
| -------- | -------- |
| <kbd>f</kbd> | 过滤文件 |

### 查找文件

| 快捷键   | 操作         |
| -------- | ------------ |
| <kbd>/</kbd> | 查找下一个文件 |
| <kbd>?</kbd> | 查找上一个文件 |
| <kbd>n</kbd> | 转到下一个找到的文件 |
| <kbd>N</kbd> | 转到上一个找到的文件 |

### 搜索文件

| 快捷键                     | 操作                                                                       |
| -------------------------- | -------------------------------------------------------------------------- |
| <kbd>s</kbd>               | 使用 [fd](https://github.com/sharkdp/fd) 按名称搜索文件                     |
| <kbd>S</kbd>               | 使用 [ripgrep](https://github.com/BurntSushi/ripgrep) 按内容搜索文件        |
| <kbd>Ctrl</kbd> + <kbd>s</kbd> | 取消正在进行的搜索                                                     |

### 排序

要对文件/目录进行排序,使用以下命令。

_注意:<kbd>,</kbd> ⇒ <kbd>a</kbd> 表示按 <kbd>,</kbd> 键,然后按 <kbd>a</kbd> 键。_

| 快捷键                  | 操作                 |
| ----------------------- | -------------------- |
| <kbd>,</kbd> ⇒ <kbd>m</kbd> | 按修改时间排序   |
| <kbd>,</kbd> ⇒ <kbd>M</kbd> | 按修改时间排序(反向) |
| <kbd>,</kbd> ⇒ <kbd>b</kbd> | 按创建时间排序   |
| <kbd>,</kbd> ⇒ <kbd>B</kbd> | 按创建时间排序(反向) |
| <kbd>,</kbd> ⇒ <kbd>e</kbd> | 按文件扩展名排序 |
| <kbd>,</kbd> ⇒ <kbd>E</kbd> | 按文件扩展名排序(反向) |
| <kbd>,</kbd> ⇒ <kbd>a</kbd> | 按字母顺序排序   |
| <kbd>,</kbd> ⇒ <kbd>A</kbd> | 按字母顺序排序(反向) |
| <kbd>,</kbd> ⇒ <kbd>n</kbd> | 按自然顺序排序   |
| <kbd>,</kbd> ⇒ <kbd>N</kbd> | 按自然顺序排序(反向) |
| <kbd>,</kbd> ⇒ <kbd>s</kbd> | 按大小排序       |
| <kbd>,</kbd> ⇒ <kbd>S</kbd> | 按大小排序(反向) |
| <kbd>,</kbd> ⇒ <kbd>r</kbd> | 随机排序         |

### 多标签

| 快捷键                                    | 操作                   |
| ----------------------------------------- | ---------------------- |
| <kbd>t</kbd>                              | 使用 CWD 创建新标签    |
| <kbd>1</kbd>, <kbd>2</kbd>, ..., <kbd>9</kbd> | 切换到第 N 个标签  |
| <kbd>[</kbd>                              | 切换到上一个标签       |
| <kbd>]</kbd>                              | 切换到下一个标签       |
| <kbd>\{</kbd>                             | 将当前标签与上一个标签交换 |
| <kbd>}</kbd>                              | 将当前标签与下一个标签交换 |
| <kbd>Ctrl</kbd> + <kbd>c</kbd>            | 关闭当前标签           |

## Flavors

从我们的 [flavors 仓库](https://github.com/yazi-rs/flavors)中选择你喜欢的配色方案,或者[制作一个 flavor](/docs/flavors/overview#cooking)!
