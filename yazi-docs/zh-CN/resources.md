---
sidebar_position: 10
description: Some awesome plugins for Yazi.
---

# 资源

:::warning
插件系统仍处于早期阶段,以下大多数插件只保证与 Yazi 的最新代码兼容!

请确保你的 Yazi 和插件都在 `HEAD` 上以确保正常功能!
:::

## 🖼️ Previewers {#previewers}

通用:

- [piper.yazi](https://github.com/yazi-rs/plugins/tree/main/piper.yazi) - 将任何 shell 命令作为 previewer 管道传输。
- [mux.yazi](https://github.com/peterfication/mux.yazi) - Plugin multiplexer。为同一文件定义并循环使用 previewers。

媒体:

- [exifaudio.yazi](https://github.com/Sonico98/exifaudio.yazi) - 使用 [exiftool](https://exiftool.org/) 预览音频元数据和封面。
- [mediainfo.yazi](https://github.com/boydaihungst/mediainfo.yazi) - 使用 `ffmpeg` 和 `mediainfo` 预览图像、音频、视频、字幕和许多媒体文件。

归档:

- [ouch.yazi](https://github.com/ndtoan96/ouch.yazi) - Yazi 的 [ouch](https://github.com/ouch-org/ouch) 插件,支持预览和压缩。
- [zless-preview.yazi](https://github.com/vmikk/zless-preview.yazi) - 使用 `zless` 预览压缩的文本文件。
- [comicthumb.yazi](https://github.com/navysky12/comicthumb.yazi) - 在 Linux 上使用 p7zip 预览漫画归档文件。

文档:

- [djvu-view.yazi](https://github.com/Shallow-Seek/djvu-view.yazi) - 使用 [djvulibre](https://github.com/DjvuNet/DjVuLibre) 的 `ddjvu` 预览 Djvu

数据文件:

- [duckdb.yazi](https://github.com/wylie102/duckdb.yazi) - 使用 [duckdb](https://github.com/duckdb/duckdb) 预览 CSV/TSV、JSON 和 Parquet 文件。查看原始数据,或带有所有列的数据类型、最小值、最大值、平均值等的汇总视图。

BitTorrent:

- [torrent-preview.yazi](https://github.com/kirasok/torrent-preview.yazi) - 使用 [transmission-cli](https://github.com/transmission/transmission) 预览 "\*.torrent" 文件。

Jupyter notebooks:

- [nbpreview.yazi](https://github.com/AnirudhG07/nbpreview.yazi) - 使用 [nbpreview](https://github.com/paw-lu/nbpreview) 预览 jupyter notebooks(\*.ipynb) 文件。

杂项:

- [rich-preview.yazi](https://github.com/AnirudhG07/rich-preview.yazi) - 使用 [rich-cli](https://github.com/textualize/rich-cli) 预览 Markdown、JSON、CSV 等

## 🧩 功能插件 {#functional}

跳转:

- [relative-motions.yazi](https://github.com/dedukun/relative-motions.yazi) - 基于 vim 动作的 Yazi 插件。
- [jump-to-char.yazi](https://github.com/yazi-rs/plugins/tree/main/jump-to-char.yazi) - 类似 Vim 的 `f<char>`,跳转到名称以 `<char>` 开头的下一个文件。
- [time-travel.yazi](https://github.com/iynaix/time-travel.yazi) - 通过 BTRFS / ZFS 快照向前和向后浏览时间。
- [cdhist.yazi](https://github.com/bulletmark/cdhist.yazi) - 使用 cdhist 从你的目录历史记录中模糊选择和导航 Yazi。
- [cd-git-root.yazi](https://github.com/ciarandg/cd-git-root.yazi) - 切换到你当前所在的 git 仓库的根目录。
- [fazif.yazi](https://github.com/Shallow-Seek/fazif.yazi) - 使用 `fd`、`rg` `rga` 搜索选定的项目,并在 Yazi 中生成任何 FZF 配置。

书签:

- [bookmarks.yazi](https://github.com/dedukun/bookmarks.yazi) - 添加类似 Vi 的标记的基本功能的 Yazi 插件。
- [mactag.yazi](https://github.com/yazi-rs/plugins/tree/main/mactag.yazi) - 将 macOS 出色的标记功能带到 Yazi!该插件仅适用于 macOS,就像名字所说的那样。
- [simple-tag.yazi](https://github.com/boydaihungst/simple-tag.yazi) - 适用于 Linux、macOS 和 Windows 的标记功能!
- [yamb.yazi](https://github.com/h-hg/yamb.yazi) - 又一个书签插件。它支持持久化、通过键跳转、通过 [fzf](https://github.com/junegunn/fzf) 跳转。
- [bunny.yazi](https://github.com/stelcodes/bunny.yazi) - 带有持久和临时书签、模糊搜索、返回上一个目录以及切换到其他标签中打开的目录的书签菜单。
- [whoosh.yazi](https://github.com/WhoSowSee/whoosh.yazi) - 高级书签管理器,具有持久/临时书签、目录历史、fzf 集成、路径截断和跨平台支持。使用键或模糊搜索即时在位置之间跳转。

标签:

- [projects.yazi](https://github.com/MasouShizuka/projects.yazi) - 将所有标签及其状态保存为项目,并随时恢复它们。
- [close-and-restore-tab.yazi](https://github.com/MasouShizuka/close-and-restore-tab.yazi) - 恢复已关闭的标签。

文件操作:

- [chmod.yazi](https://github.com/yazi-rs/plugins/tree/main/chmod.yazi) - 在选定的文件上执行 `chmod` 以更改其模式。
- [diff.yazi](https://github.com/yazi-rs/plugins/tree/main/diff.yazi) - 将选定的文件与悬停的文件进行 diff,创建一个活动补丁,并将其复制到剪贴板。
- [compress.yazi](https://github.com/KKV9/compress.yazi) - 将选定的文件压缩到归档的 Yazi 插件。
- [ouch.yazi](https://github.com/ndtoan96/ouch.yazi) - Yazi 的 [ouch](https://github.com/ouch-org/ouch) 插件,支持预览和压缩。
- [archivemount.yazi](https://github.com/AnirudhG07/archivemount.yazi) - 使用 [archivemount](https://github.com/cybernoid/archivemount) 在 yazi 中挂载和卸载归档。
- [reflink.yazi](https://github.com/Ape/reflink.yazi) - 创建文件的 reflinks。
- [rsync.yazi](https://github.com/GianniBYoung/rsync.yazi) - 简单的 rsync 本地和远程服务器复制。
- [sshfs.yazi](https://github.com/uhs-robert/sshfs.yazi) - 通过 SSH 使用 SSHFS 挂载和管理远程目录。支持来自 `~/.ssh/config` 的主机或自定义定义的连接。包括密钥/密码认证。
- [what-size.yazi](https://github.com/pirafrank/what-size.yazi) - 计算当前选择或当前工作目录的总大小。
- [lazygit.yazi](https://github.com/Lil-Dank/lazygit.yazi) - 使用快捷方式通过 [lazygit](https://github.com/jesseduffield/lazygit) 管理 Git 目录。
- [sudo.yazi](https://github.com/TD-Sky/sudo.yazi) - 使用 `sudo` 权限执行特定文件操作。
- [restore.yazi](https://github.com/boydaihungst/restore.yazi) - 使用 `trash-cli` 恢复/恢复最新删除的文件/文件夹。
- [recycle-bin.yazi](https://github.com/uhs-robert/recycle-bin.yazi) - 从 Yazi 管理你的回收站:浏览内容、恢复或删除选定的项目、按年龄清空或完全清空,使用 `trash-cli`。
- [gvfs.yazi](https://github.com/boydaihungst/gvfs.yazi) - 挂载和管理 MTP、GPhoto2 (PTP) 设备(Android、相机等)、SMB、SFTP、NFS、FTP、Google Drive、DNS-SD、DAV (WebDAV)、AFP、AFC(仅限 Linux)。[支持的协议](<https://wiki.gnome.org/Projects(2f)gvfs(2f)schemes.html>)列表。
- [kdeconnect-send.yazi](https://github.com/Deepak22903/kdeconnect-send.yazi) - 使用 KDE Connect 将选定的文件发送到你的智能手机或其他设备。
- [zoom.yazi](https://github.com/yazi-rs/plugins/tree/main/zoom.yazi) - 放大或缩小预览图像。

剪贴板:

- [copy-file-contents.yazi](https://github.com/AnirudhG07/plugins-yazi/tree/main/copy-file-contents.yazi) - 一个简单的插件,只需从 Yazi 复制文件内容,无需进入编辑器。
- [system-clipboard.yazi](https://github.com/orhnk/system-clipboard.yazi) - 简单系统剪贴板的跨平台实现。
- [wl-clipboard.yazi](https://github.com/grappas/wl-clipboard.yazi) - 简单系统剪贴板的 Wayland 实现。
- [path-from-root.yazi](https://github.com/aresler/path-from-root.yazi) - 复制相对于 git 根的文件路径
- [clippy.yazi](https://github.com/gallardo994/clippy.yazi) - 在 macOS 上使用 Clippy 将文件复制到剪贴板

`filter` 增强:

- [smart-filter.yazi](https://github.com/yazi-rs/plugins/tree/main/smart-filter.yazi) - 使过滤器更智能:连续过滤、自动进入唯一目录、提交时打开文件。

`enter` 增强:

- [smart-enter.yazi](https://github.com/yazi-rs/plugins/tree/main/smart-enter.yazi) - 在一个键中 `打开` 文件或 `进入` 目录!
- [bypass.yazi](https://github.com/Rolv-Apneseth/bypass.yazi) - Yazi 插件,用于跳过只有一个子目录的目录。
- [fast-enter.yazi](https://github.com/ourongxing/fast-enter.yazi) - 自动解压归档并进入,或进入最深的目录,直到它不是唯一的子目录。

`shell` 增强:

- [open-with-cmd.yazi](https://github.com/Ape/open-with-cmd.yazi) - 使用提示的命令打开文件。

`search` 增强:

- [vcs-files.yazi](https://github.com/yazi-rs/plugins/tree/main/vcs-files.yazi) - 显示 Git 文件更改。
- [git-files.yazi](https://github.com/ktunprasert/git-files.yazi) - 显示 Git 文件更改(包括未跟踪的,通过 `git status --porcelain`)
- [modif.yazi](https://github.com/Shallow-Seek/modif.yazi) - 显示最近修改的。

`paste` 增强:

- [smart-paste.yazi](https://github.com/yazi-rs/plugins/tree/main/smart-paste.yazi) - 将文件粘贴到悬停的目录中,如果悬停在文件上则粘贴到 CWD。

通用命令增强:

- [augment-command.yazi](https://github.com/hankertrix/augment-command.yazi) - 通过更好地处理选定项目和悬停项目之间的选择来增强一些 Yazi 命令。

UI 增强:

- [full-border.yazi](https://github.com/yazi-rs/plugins/tree/main/full-border.yazi) - 为 Yazi 添加完整边框,使其看起来更华丽。
- [toggle-pane.yazi](https://github.com/yazi-rs/plugins/tree/main/toggle-pane.yazi) - 切换不同窗格的显示、隐藏和最大化状态:父窗格、当前窗格和预览窗格。
- [git.yazi](https://github.com/yazi-rs/plugins/tree/main/git.yazi) - 在文件列表中以 linemode 显示 Git 文件更改的状态。
- [mount.yazi](https://github.com/yazi-rs/plugins/tree/main/mount.yazi) - Yazi 的挂载管理器,提供磁盘挂载、卸载和弹出功能。
- [starship.yazi](https://github.com/Rolv-Apneseth/starship.yazi) - Yazi 的 Starship 提示符插件。
- [omp.yazi](https://github.com/saumyajyoti/omp.yazi) - Yazi 的 oh-my-posh 提示符插件。
- [yatline.yazi](https://github.com/imsi32/yatline.yazi) - 通过简单的配置自定义 header-line 和 status-line。
- [simple-status.yazi](https://github.com/Ape/simple-status.yazi) - 带有有用文件属性信息的简约状态行。
- [no-status.yazi](https://github.com/yazi-rs/plugins/tree/main/no-status.yazi) - 移除状态栏。
- [pref-by-location.yazi](https://github.com/boydaihungst/pref-by-location.yazi) - 根据目录位置保存和恢复 linemode/排序/隐藏首选项。

## 🚀 Preloaders {#preloaders}

图像:

- [allmytoes.yazi](https://github.com/Sonico98/allmytoes.yazi) - 使用 [allmytoes](https://gitlab.com/allmytoes/allmytoes) 预览 freedesktop 兼容的缩略图。

## 🔍Fetchers {#fetchers}

Mime-type:

- [`mime-ext.yazi`](https://github.com/yazi-rs/plugins/tree/main/mime-ext.yazi) - 基于文件扩展名数据库的 mime-type 提供程序,替换内置的 `file(1)` 以加快 mime-type 检索速度,但会牺牲准确性。

## 🧑‍💻 Devtools {#devtools}

[types.yazi](https://github.com/yazi-rs/plugins/tree/main/types.yazi) - Yazi 的 Lua API 的类型定义,为高效的插件开发体验提供支持。

## 📝 (Neo)vim 插件 {#vim}

Neovim:

- [yazi.nvim](https://github.com/mikavilpas/yazi.nvim) - yazi 终端文件管理器的 Neovim 插件。
- [tfm.nvim](https://github.com/Rolv-Apneseth/tfm.nvim) - 终端文件管理器集成的 Neovim 插件。
- [fm-nvim](https://github.com/Eric-Song-Nop/fm-nvim) - Neovim 插件,让你使用你喜欢的终端文件管理器。

Vim:

- [vim-yazi](https://github.com/yukimura1227/vim-yazi) - Vim 插件,集成 Yazi 以实现无缝的编辑器内文件浏览和导航。
- [yazi.vim](https://github.com/chriszarate/yazi.vim) - Yazi 的 Vim 插件。

## 📝 Helix {#helix}

- [Yazelix](https://github.com/luccahuguet/yazelix) - 为 Helix 添加文件树和 helix 友好的 Zellij 快捷键绑定

## 🐚 Shell 插件 {#shell}

- [yazi-prompt.sh](https://github.com/Sonico98/yazi-prompt.sh) - 在 yazi 子 shell 中运行时在提示符中显示指示符。
- [custom-shell.yazi](https://github.com/AnirudhG07/custom-shell.yazi) - 通过你的默认系统 shell 运行任何命令。
- [command.yazi](https://github.com/KKV9/command.yazi) - 显示执行 yazi 命令的提示符。

## 🛠️ 实用工具 {#utilities}

- [icons-brew.yazi](https://github.com/lpnh/icons-brew.yazi) - 使用你喜欢的调色板为你的 Yazi 图标制作一个热门的 `theme.toml`。
- [lsColorsToToml](https://github.com/Mellbourn/lsColorsToToml) - 根据你的 `$LS_COLORS` 为 `theme.toml` 中的 `[filetype]` 部分生成颜色规则。

## 💖 添加你的 {#add-yours}

我们很高兴将你的插件添加到此页面!

如果你的插件满足以下要求,请点击下面的 "Edit this page" 来添加它:

- **功能性** - 我们会安装并测试它,因为我们希望此页面上包含的所有链接都是有效的。如果它仅在特定平台上可用,应在 README 中添加说明。
- **遵循约定** - 它应该是一个以 `.yazi` 结尾的目录/仓库,并包含[插件文档](/docs/plugins/overview)中列出的文件。

如果它是 Neovim 或 shell 插件,在名称后附加 `.nvim` 或 `.sh` 以使其可区分是最佳实践,但不是必需的。
