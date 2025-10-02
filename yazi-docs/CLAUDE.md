# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains the documentation for **Yazi**, a blazing-fast terminal file manager written in Rust. The documentation is written in Markdown format and appears to be structured for a documentation site (likely Docusaurus or similar static site generator).

The main Yazi repository is at: https://github.com/sxyazi/yazi

## Repository Structure

The documentation is organized into several main sections:

### Core Documentation Files
- `installation.md` - Installation instructions for various platforms and package managers
- `quick-start.md` - Basic usage guide and keybindings reference
- `cli.md` - Documentation for the `ya` CLI tool (package manager, DDS interface)
- `dds.md` - Data Distribution Service for inter-instance communication
- `faq.md` - Frequently asked questions
- `tips.md` - Advanced usage tips
- `term.md` - Terminal compatibility information
- `image-preview.md` - Image preview configuration
- `resources.md` - Community plugins and resources

### Configuration Documentation (`configuration/`)
- `overview.md` - Configuration system overview
- `yazi.md` - Main configuration file (`yazi.toml`)
- `keymap.md` - Keybinding configuration (`keymap.toml`)
- `theme.md` - Color scheme configuration (`theme.toml`)

### Plugin Documentation (`plugins/`)
Plugin system documentation for Lua-based plugins:
- `overview.md` - Plugin system architecture, sync/async contexts
- `layout.md` - UI layout system for plugins
- `context.md` - Accessing application state
- `utils.md` - Utility functions available to plugins
- `types.md` - Type definitions for plugin development
- `runtime.md` - Plugin runtime environment
- `aliases.md` - Common function aliases
- `builtins.md` - Built-in plugin functions

### Flavor Documentation (`flavors/`)
- `overview.md` - Color scheme/theme system

## Key Concepts

### Yazi Architecture
Yazi is a terminal file manager with:
- **Multi-pane UI**: Parent directory, current directory, and preview pane
- **Plugin system**: Lua-based, with sync and async contexts
- **DDS (Data Distribution Service)**: Pub/sub message system for inter-instance communication
- **Package manager**: `ya pkg` for managing plugins and flavors
- **CLI tool**: `ya` for automation and scripting

### Configuration Files
Users configure Yazi through three TOML files in `~/.config/yazi/`:
1. `yazi.toml` - General configuration
2. `keymap.toml` - Keybinding configuration
3. `theme.toml` - Color scheme

Default configurations are on the `shipped` branch: https://github.com/sxyazi/yazi/tree/shipped/yazi-config/preset

### Plugin System
- Plugins are Lua modules in directories ending with `.yazi`
- Support both **sync** and **async** execution contexts
- Can be used as functional plugins, previewers, or preloaders
- Managed via `ya pkg add/delete/upgrade` commands

### DDS (Data Distribution Service)
Communication system supporting:
- `ya pub/pub-to` - Publish messages to instances
- `ya emit/emit-to` - Execute commands in instances
- Real-time stdout event reporting with `--local-events` and `--remote-events`
- Built-in event kinds: `cd`, `hover`, `rename`, `bulk`, `yank`, `move`, `trash`, `delete`

## Documentation Conventions

### Markdown Format
- Uses Docusaurus-style frontmatter with `sidebar_position`, `description`, etc.
- Includes JSX components: `<Tabs>`, `<TabItem>` for multi-platform examples
- Uses `:::warning`, `:::tip`, `:::info` admonitions
- Links to other docs use `/docs/section/page` format

### Code Examples
Documentation includes many shell examples for different platforms:
- Bash/Zsh
- Fish
- Nushell
- PowerShell
- Command Prompt
- Elvish
- Xonsh

Always wrap platform-specific examples in appropriate `<TabItem>` components.

### Cross-references
- Configuration options reference specific sections: `/docs/configuration/yazi#section`
- Plugin API references: `/docs/plugins/utils#function`
- Keybindings reference the default keymap: https://github.com/sxyazi/yazi/blob/shipped/yazi-config/preset/keymap-default.toml

## Important Notes

### Version Compatibility
- Documentation should match the latest stable release
- Plugin system is marked as **BETA** - plugins may require updates
- Default configs are on the `shipped` branch, NOT `main`

### Platform Support
Yazi supports:
- Linux (various distributions)
- macOS (Homebrew, MacPorts)
- Windows (Scoop, WinGet)
- BSD systems (NetBSD)
- Nix/NixOS
- Universal: cargo build from source

### External Dependencies
Optional but commonly used:
- `ffmpeg` - video thumbnails
- `fd` - file searching
- `rg` (ripgrep) - content searching
- `fzf` - fuzzy finding
- `zoxide` - directory jumping
- `7zip` - archive preview
- `jq` - JSON preview
- `poppler` - PDF preview

## Common Documentation Tasks

### Adding New Features
When documenting new features:
1. Update relevant configuration file docs if config options are added
2. Add examples for multiple shells/platforms where applicable
3. Update plugin API docs if new Lua functions are added
4. Consider adding to FAQ if it's a commonly asked question
5. Link to related sections using absolute `/docs/` paths

### Updating Plugin Documentation
Plugin system changes frequently:
- Update `overview.md` for API changes
- Update `utils.md` for new utility functions
- Update `types.md` for new type definitions
- Check that examples still work with current version

### Writing Examples
- Provide cross-platform examples when possible (Unix vs Windows)
- Use realistic file paths (`/root/Downloads`, `%AppData%`, etc.)
- Show both command-line and config file approaches
- Include error cases and troubleshooting where relevant
