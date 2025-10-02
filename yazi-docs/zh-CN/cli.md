---
sidebar_position: 7
description: Command-line interface for Yazi
---

# CLI

Yazi 提供了一个名为 `ya` 的命令行工具,用于协助插件管理、flavor 管理、DDS 消息发布和订阅等功能。

它是 Yazi 的重要组成部分。大多数发行版在安装 Yazi 时默认包含它,但如果你的发行版没有包含,你需要[从源码构建](/docs/installation#source)。请确保 `ya` 和 `yazi` 的版本完全相同。

## 包管理器 {#pm}

你可以使用 `ya pkg` 子命令来管理你的插件和 flavors。例如,要从 https://github.com/owner/my-plugin.yazi 安装插件,运行:

```sh
ya pkg add owner/my-plugin
```

`ya pkg` 还支持从 monorepo 安装子目录作为包。例如,要从 https://github.com/yazi-rs/plugins/tree/main/git.yazi 安装包,运行:

```sh
ya pkg add yazi-rs/plugins:git
```

它会自动从 GitHub 克隆它们,复制到你的插件目录,并更新 `package.toml` 以锁定它们的版本:

```toml
# ~/.config/yazi/package.toml
[[plugin.deps]]
use  = "owner/my-plugin"
rev  = "0573024"
hash = "d81b64a39432fcd6224cd75d296e7510"

[[plugin.deps]]
use  = "yazi-rs/plugins:git"
rev  = "9a1129c"
hash = "a8e15d3c21c02a5af41d46ed04778a02"
```

要删除一个插件:

```sh
ya pkg delete yazi-rs/plugins:git
```

要列出所有由 `ya pkg` 管理的插件:

```sh
ya pkg list
```

要在新系统上从 `package.toml` 安装所有锁定版本的插件:

```sh
ya pkg install
```

要将所有插件升级到最新版本:

```sh
ya pkg upgrade
```

如果你想将插件固定到特定版本,以便在运行 `ya pkg upgrade` 时不会升级,可以在 `rev` 中的哈希值前添加 `=` 限定符:

```diff
[[plugin.deps]]
use = "owner/my-plugin"
- rev = "9a1129c"
+ rev = "=9a1129c"
```

对于 `add` 和 `delete`,它们可以接受多个参数,这意味着你可以一次操作多个包:

```sh
ya pkg add owner/my-plugin yazi-rs/plugins:git
ya pkg delete owner/my-plugin yazi-rs/plugins:git
```

## 数据分发服务 {#dds}

你可以使用 `ya` 作为用户界面与数据分发服务交互。

更多信息请参阅 [DDS 部分](/docs/dds)。
