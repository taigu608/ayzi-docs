---
sidebar_position: 4
description: 学习如何使用 Yazi 的 Lua API。
---

# 运行时

## rt {#rt}

你可以通过 `rt` 访问 Yazi 的运行时以获取启动参数、终端属性、[用户偏好](/docs/configuration/yazi)等。

### `args` {#rt.args}

用户启动 Yazi 时传递的命令行参数。

|      |                        |
| ---- | ---------------------- |
| 类型 | [`rt::Args`](#rt-args) |

### `term` {#rt.term}

用户的终端模拟器属性。

|      |                        |
| ---- | ---------------------- |
| 类型 | [`rt::Term`](#rt-term) |

### `mgr` {#rt.mgr}

[\[mgr\]](/docs/configuration/yazi#mgr) 下的用户偏好。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `plugin` {#rt.plugin}

[\[plugin\]](/docs/configuration/yazi#plugin) 下的用户偏好。

|      |                            |
| ---- | -------------------------- |
| 类型 | [`rt::Plugin`](#rt-plugin) |

### `preview` {#rt.preview}

[\[preview\]](/docs/configuration/yazi#preview) 下的用户偏好。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `tasks` {#rt.tasks}

[\[tasks\]](/docs/configuration/yazi#tasks) 下的用户偏好。

|      |         |
| ---- | ------- |
| 类型 | `table` |

## th {#th}

你可以通过 `th` 访问用户的主题和风格配置。

### `mgr` {#th.mgr}

详见 [\[mgr\]](/docs/configuration/theme#mgr)。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `tabs` {#th.tabs}

详见 [\[tabs\]](/docs/configuration/theme#tabs)。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `mode` {#th.mode}

详见 [\[mode\]](/docs/configuration/theme#mode)。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `status` {#th.status}

详见 [\[status\]](/docs/configuration/theme#status)。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `which` {#th.which}

详见 [\[which\]](/docs/configuration/theme#which)。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `confirm` {#th.confirm}

详见 [\[confirm\]](/docs/configuration/theme#confirm)。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `spot` {#th.spot}

详见 [\[spot\]](/docs/configuration/theme#spot)。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `notify` {#th.notify}

详见 [\[notify\]](/docs/configuration/theme#notify)。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `pick` {#th.pick}

详见 [\[pick\]](/docs/configuration/theme#pick)。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `input` {#th.input}

详见 [\[input\]](/docs/configuration/theme#input)。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `cmp` {#th.cmp}

详见 [\[cmp\]](/docs/configuration/theme#cmp)。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `tasks` {#th.tasks}

详见 [\[tasks\]](/docs/configuration/theme#tasks)。

|      |         |
| ---- | ------- |
| 类型 | `table` |

### `help` {#th.help}

详见 [\[help\]](/docs/configuration/theme#help)。

|      |         |
| ---- | ------- |
| 类型 | `table` |

## rt::Args {#rt-args}

### `entries` {#rt-args.entries}

TODO

### `cwd_file` {#rt-args.cwd_file}

TODO

### `chooser_file` {#rt-args.chooser_file}

TODO

## rt::Term {#rt-term}

用户的终端模拟器属性。

### `light` {#rt-term.light}

终端是否处于亮色模式。

|      |           |
| ---- | --------- |
| 类型 | `boolean` |

## rt::Plugin {#rt-plugin}

TODO
