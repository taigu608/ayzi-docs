---
sidebar_position: 1
description: How to install Yazi on various operating systems.
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# 安装

要使用 Yazi,你必须安装以下前置依赖:

- [`file`](https://github.com/file/file) (用于文件类型检测)

Yazi 可以通过其他命令行工具 **可选** 扩展以启用额外功能。

- [nerd-fonts](https://www.nerdfonts.com/) ([_推荐_](/docs/faq#dont-like-nerd-fonts))
- [`ffmpeg`](https://www.ffmpeg.org/) (用于视频缩略图)
- [7-Zip](https://www.7-zip.org/) (用于归档提取和预览,需要非独立版本)
- [`jq`](https://jqlang.github.io/jq/) (用于 JSON 预览)
- [`poppler`](https://poppler.freedesktop.org/) (用于 PDF 预览)
- [`fd`](https://github.com/sharkdp/fd) (用于文件搜索)
- [`rg`](https://github.com/BurntSushi/ripgrep) (用于文件内容搜索)
- [`fzf`](https://github.com/junegunn/fzf) (用于快速文件子树导航,>= 0.53.0)
- [`zoxide`](https://github.com/ajeetdsouza/zoxide) (用于历史目录导航,需要 `fzf`)
- [`resvg`](https://github.com/linebender/resvg) (用于 SVG 预览)
- [ImageMagick](https://imagemagick.org/) (用于字体、HEIC 和 JPEG XL 预览,>= 7.1.1)
- [`xclip`](https://github.com/astrand/xclip) / [`wl-clipboard`](https://github.com/bugaevc/wl-clipboard) / [`xsel`](https://github.com/kfish/xsel) (用于 Linux 剪贴板支持)

如果某些功能未按预期工作,请将这些依赖升级到最新版本。

## 打包状态 {#packaging}

此页面上的大多数软件包由社区维护,它们 **_可能并不总是最新的_**。请在安装前检查它们的版本:

<a alt="Yazi packaging status" href="https://repology.org/project/yazi/versions">
	<img alt="Yazi packaging status" height="785" src="https://repology.org/badge/vertical-allrepos/yazi.svg" />
</a>

## Arch Linux {#arch}

```sh
sudo pacman -S yazi ffmpeg 7zip jq poppler fd ripgrep fzf zoxide resvg imagemagick
```

如果你想使用最新的 Git 版本,可以从 [AUR](https://aur.archlinux.org/packages/yazi-git/) 或 [Arch Linux CN](https://github.com/archlinuxcn/repo/) 安装:

```sh
paru -S yazi-git ffmpeg 7zip jq poppler fd ripgrep fzf zoxide resvg imagemagick
```

你也可以从 [AUR](https://aur.archlinux.org/packages/yazi-nightly-bin) 安装[官方 nightly 发布二进制文件](https://github.com/sxyazi/yazi/releases/tag/nightly),
它是在过去 6 小时内从最新代码构建的:

```sh
paru -S yazi-nightly-bin ffmpeg 7zip jq poppler fd ripgrep fzf zoxide resvg imagemagick
```

## Nix {#nix}

有一个 Yazi 的 [Nix 包](https://search.nixos.org/packages?channel=unstable&show=yazi)。

```sh
# NixOS:
nix-env -iA nixos.yazi

# Not NixOS:
nix-env -iA nixpkgs.yazi
```

或将以下内容添加到你的配置中:

```nix
# configuration.nix
environment.systemPackages = with pkgs; [
	(yazi.override {
		_7zz = _7zz-rar;  # Support for RAR extraction
	})
];
```

你也可以使用 [home-manager](https://nix-community.github.io/home-manager/options.xhtml#opt-programs.yazi.enable) 管理 Yazi 的配置,这是一个配置模板示例:

<details>
  <summary>演示使用 home-manager 配置 Yazi</summary>

```nix
{pkgs, ...}: let
	yazi-plugins = pkgs.fetchFromGitHub {
		owner = "yazi-rs";
		repo = "plugins";
		rev = "...";
		hash = "sha256-...";
	};
in {
	programs.yazi = {
		enable = true;
		enableZshIntegration = true;
		shellWrapperName = "y";

		settings = {
			mgr = {
				show_hidden = true;
			};
			preview = {
				max_width = 1000;
				max_height = 1000;
			};
		};

		plugins = {
			chmod = "${yazi-plugins}/chmod.yazi";
			full-border = "${yazi-plugins}/full-border.yazi";
			toggle-pane = "${yazi-plugins}/toggle-pane.yazi";
			starship = pkgs.fetchFromGitHub {
				owner = "Rolv-Apneseth";
				repo = "starship.yazi";
				rev = "...";
				sha256 = "sha256-...";
			};
		};

		initLua = ''
			require("full-border"):setup()
			require("starship"):setup()
		'';

		keymap = {
			mgr.prepend_keymap = [
				{
					on = "T";
					run = "plugin toggle-pane max-preview";
					desc = "Maximize or restore the preview pane";
				}
				{
					on = ["c" "m"];
					run = "plugin chmod";
					desc = "Chmod on selected files";
				}
			];
		};
	};
}
```

</details>

## Nix flakes {#flakes}

上游仓库提供了一个 flake,以便 Nix 用户可以轻松跟上最新进展。一个基本的 `flake.nix` 设置来帮助你入门:

```nix
{
	inputs = {
		nixpkgs.url = "github:NixOS/nixpkgs/nixos-23.11";

		home-manager = {
			url = "github:nix-community/home-manager/release-23.11";
			inputs.nixpkgs.follows = "nixpkgs";
		};

		yazi.url = "github:sxyazi/yazi";
	};

	outputs = { nixpkgs, home-manager, yazi, ... }: {
		# To install Yazi system-wide:
		nixosConfigurations = {
			nixos = nixpkgs.lib.nixosSystem {
				modules = [
					({ pkgs, ... }: {
						environment.systemPackages = [
							(yazi.packages.${pkgs.system}.default.override {
								_7zz = pkgs._7zz-rar;  # Support for RAR extraction
							})
						];
					})
				];
			};
		};

		# To install it for a specific user:
		homeConfigurations = {
			"alice@nixos" = home-manager.lib.homeManagerConfiguration {
				pkgs = nixpkgs.legacyPackages.x86_64-linux;
				modules = [
					({ pkgs, ... }: {
						home.packages = [
							(yazi.packages.${pkgs.system}.default.override {
								_7zz = pkgs._7zz-rar;  # Support for RAR extraction
							})
						];
					})
				];
			};
		};
	};
}
```

如果你想用 flake 中的包覆盖 nixpkgs 中的包,可以使用 overlays:

```nix
nixpkgs.overlays = [ yazi.overlays.default ];
```

NixOS 和 home-manager 都有可用的模块:

```nix
programs.yazi = {
	enable = true;
	# You can omit this if you use overlays
	package = yazi.packages.${pkgs.system}.default.override {
		_7zz = pkgs._7zz-rar;  # Support for RAR extraction
	};
};
```

### Cache

预构建的构件在 https://yazi.cachix.org 提供,这样 Nix 用户就不必在他们的机器上构建 Yazi。
你可以通过在 NixOS 或 home-manager 配置中向 `nix.settings` 添加以下选项来使用它:

```nix
extra-substituters = [ "https://yazi.cachix.org" ];
extra-trusted-public-keys = [ "yazi.cachix.org-1:Dcdz63NZKfvUCbDGngQDAZq6kOroIrFoyO064uvLh8k=" ];
```

注意,缓存只会在你重建 Nix 配置 _之后_ 应用。如果你想确保缓存立即应用,请将上述选项添加到你的 flake 的 `nixConfig` 属性中。

如果你遇到任何问题,请参考 Cachix FAQ 中的这个[条目](https://docs.cachix.org/faq#why-is-nix-not-picking-up-on-any-of-the-pre-built-artifacts)。

## Homebrew {#homebrew}

首先,确保 Homebrew 是完全最新的,使用 `brew update`。

然后你可以安装 Yazi(和可选依赖):

```sh
brew install yazi ffmpeg sevenzip jq poppler fd ripgrep fzf zoxide resvg imagemagick font-symbols-only-nerd-font
```

如果你更喜欢使用最新代码,在安装 Yazi 时使用 `--HEAD` 标志。

```sh
brew install yazi --HEAD
```

## MacPorts {#macports}

```bash
sudo port install yazi ffmpeg 7zip jq poppler fd ripgrep fzf zoxide ImageMagick
```

## Void Linux

```sh
sudo xbps-install -S yazi ffmpeg 7zip jq poppler fd ripgrep fzf zoxide resvg ImageMagick
```

## NetBSD {#netbsd}

```sh
pkgin install yazi ffmpeg7 p7zip jq poppler fd ripgrep fzf zoxide ImageMagick
```

## Windows {#windows}

Yazi 依赖 `file(1)` 来检测文件的 mime-type,在 Windows 上获取它最简单和最可靠的方法是安装 Git for Windows 并使用它附带的 `file.exe`。

1. 通过运行[官方安装程序](https://git-scm.com/download/win)或通过你选择的包管理器安装 Git for Windows。
2. 为了让 Yazi 使用 `file(1)`,将 `<Git_Installed_Directory>\usr\bin\file.exe` 添加到你的 `YAZI_FILE_ONE` 环境变量中,这取决于你如何安装 Git:
   - 如果你使用安装程序安装 Git,它将是 `C:\Program Files\Git\usr\bin\file.exe`。
   - 如果你使用 Scoop 安装 Git,它将是 `C:\Users\<Username>\scoop\apps\git\current\usr\bin\file.exe`。
3. 重启你的终端。

这是 **我们推荐的唯一方式**。我们不推荐通过 Scoop 或 Chocolatey 安装 `file`,因为它们无法正确处理 Unicode 文件名(如 `oliver-sjöström.jpg`)并且缺少一些必需的参数。

大多数用户已经安装了 Git,并且 Yazi 也通过 Git 托管,所以这通常不是问题。但如果你真的没有/不想安装它,[`mime-ext.yazi`](https://github.com/yazi-rs/plugins/tree/main/mime-ext.yazi) 插件可以提供帮助,它使用扩展名数据库而不是依赖 `file(1)` 二进制文件。

### 使用 Scoop 安装

```sh
scoop install yazi
# Install the optional dependencies (recommended):
scoop install ffmpeg 7zip jq poppler fd ripgrep fzf zoxide resvg imagemagick
```

### 使用 WinGet 安装

```sh
winget install sxyazi.yazi
# Install the optional dependencies (recommended):
winget install Gyan.FFmpeg 7zip.7zip jqlang.jq oschwartz10612.Poppler sharkdp.fd BurntSushi.ripgrep.MSVC junegunn.fzf ajeetdsouza.zoxide ImageMagick.ImageMagick
```

resvg 尚未在 WinGet 上,请使用 Scoop 安装或从 [resvg](https://github.com/linebender/resvg/releases) 手动下载。

## 基于 Debian 的 Linux {#debian}

在 Debian/Ubuntu 中,你目前需要[手动构建 Yazi](/docs/installation#source) 或[下载我们的官方二进制版本](/docs/installation#binaries)。

你可以使用以下命令安装可选依赖:

```sh
apt install ffmpeg 7zip jq poppler-utils fd-find ripgrep fzf zoxide imagemagick
```

注意,这些依赖在某些 Debian/Ubuntu 版本上相当旧,可能会导致 Yazi 故障。在这种情况下,你需要从最新源代码手动构建它们。

如果你知道如何为 Debian/Ubuntu 打包 Yazi 并愿意帮助我们提交,请[提交一个 issue](https://github.com/sxyazi/yazi/issues/new/choose)。

## Fedora/Centos Stream 9+/RHEL 9+ {#copr}

:::info
这使用由 [Peter Li](https://github.com/lihaohong6) 维护的[非官方 COPR 仓库](https://copr.fedorainfracloud.org/coprs/lihaohong/yazi)。
:::

```sh
dnf copr enable lihaohong/yazi
dnf install yazi
```

`dnf` 会自动安装推荐的依赖。如果只想安装 Yazi:

```sh
dnf copr enable lihaohong/yazi
dnf install yazi --setopt=install_weak_deps=False
```

如果 `dnf` 提示 "No such command: copr",运行 `dnf install dnf-plugins-core` 然后重新运行上述命令。

## Snapcraft

<a href="https://snapcraft.io/yazi">
	<img height="40" alt="Install Yazi from Snapcraft" src="https://snapcraft.io/en/dark/install.svg" />
</a>

你可以从 [Snap Store](https://snapcraft.io/yazi) 安装 Yazi:

```sh
sudo snap install yazi --classic
```

如果你想跟上最新进展,获取 nightly 版本:

```sh
sudo snap install yazi --classic --edge
```

## Flatpak

:::warning
由于沙箱限制,Flatpak 版本有很多限制 - 详见其 [README](https://github.com/flathub/io.github.sxyazi.yazi)。

建议高级用户转向其他安装方式以避免意外的故障。
:::

<a href="https://flathub.org/apps/io.github.sxyazi.yazi">
	<img height="40" alt="Install Yazi from Flathub" src="https://flathub.org/api/badge?locale=en" />
</a>

[安装](https://flathub.org/apps/io.github.sxyazi.yazi)后,你可以在终端中运行 Yazi:

```sh
flatpak run io.github.sxyazi.yazi
```

你可能想创建一个 shell 别名:

```sh
alias yazi="flatpak run io.github.sxyazi.yazi"
```

更多信息请参阅 Flatpak 版本的 [README](https://github.com/flathub/io.github.sxyazi.yazi)。

## AOSC OS {#aosc}

```sh
sudo oma install yazi ffmpeg p7zip jq poppler fd ripgrep fzf zoxide imagemagick
```

## x-cmd {#x-cmd}

```sh
x env use yazi ffmpeg 7zz jq fd rg fzf zoxide magick
```

## 官方二进制文件 {#binaries}

你可以从 GitHub Releases 下载最新的官方二进制文件:https://github.com/sxyazi/yazi/releases

在此页面上,我们提供 GNU/Musl 构建以满足不同用户的需求。

此页面还包括一个 [nightly 版本](https://github.com/sxyazi/yazi/releases/tag/nightly),它是在过去 6 小时内从最新代码构建的。

## crates.io {#crates}

Yazi 在 [crates.io](https://crates.io/) 上以 [`yazi-fm`](https://crates.io/crates/yazi-fm) 和 [`yazi-cli`](https://crates.io/crates/yazi-cli) 的形式提供。由于 [Cargo 的限制](https://github.com/rust-lang/cargo/issues/11599),它们必须通过 [`yazi-build`](https://crates.io/crates/yazi-build) 安装。

要安装 Yazi,通过 [rustup](https://rustup.rs/) 设置最新的稳定 Rust 工具链:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup update
```

现在你可以通过 `cargo` 安装 `yazi-build`,它会依次安装 `yazi-fm` 和 `yazi-cli`:

```sh
cargo install --force yazi-build
```

或安装最新的 Git 版本 Yazi:

```sh
cargo install --force --git https://github.com/sxyazi/yazi.git yazi-build
```

如果构建失败,请检查你的系统上是否安装了 `make` 和 `gcc`。

## 从源码构建 {#source}

通过 [rustup](https://rustup.rs/) 设置最新的稳定 Rust 工具链:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup update
```

克隆仓库并构建 Yazi:

<Tabs>
  <TabItem value="non-windows" label="non-Windows" default>

```sh
git clone https://github.com/sxyazi/yazi.git
cd yazi
cargo build --release --locked
```

  </TabItem>
  <TabItem value="windows" label="Windows">

```sh
git clone https://github.com/sxyazi/yazi.git
cd yazi
cargo build --profile release-windows --locked
```

  </TabItem>
</Tabs>

然后,将 `yazi` 和 `ya` 添加到你的 `$PATH`:

<Tabs>
  <TabItem value="non-windows" label="non-Windows" default>

```sh
mv target/release/yazi target/release/ya /usr/local/bin/
```

  </TabItem>
  <TabItem value="windows" label="Windows">

```sh
move target\release-windows\yazi.exe "%ProgramFiles%\yazi.exe"
move target\release-windows\ya.exe "%ProgramFiles%\ya.exe"
```

  </TabItem>
</Tabs>

如果构建失败,请检查你的系统上是否安装了 `make` 和 `gcc`。

## 在调试模式下从源码构建 {#debug}

:::warning
在调试模式下构建 Yazi 仅用于调试目的,因为它可以加快构建速度并提供更多的堆栈跟踪信息。

它不应该用于日常使用,因为调试模式会显著降低性能!
:::

通过 [rustup](https://rustup.rs/) 设置最新的稳定 Rust 工具链:

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup update
```

克隆仓库并构建 Yazi:

```sh
git clone https://github.com/sxyazi/yazi.git
cd yazi
cargo build --locked
```

然后,在调试模式下运行 `yazi`:

<Tabs>
  <TabItem value="unix" label="Unix-like" default>

```sh
YAZI_LOG=debug RUST_BACKTRACE=1 ./target/debug/yazi
```

  </TabItem>

  <TabItem value="powershell" label="PowerShell">

```powershell
$env:YAZI_LOG = "debug"; $env:RUST_BACKTRACE = 1; .\target\debug\yazi.exe
```

  </TabItem>
</Tabs>

如果构建失败,请检查你的系统上是否安装了 `make` 和 `gcc`。
