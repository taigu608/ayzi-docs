---
sidebar_position: 7
description: 学习如何使用 Yazi 的 Lua API。
---

# 别名

## Origin {#origin}

表示位置原点的常量集。

|      |                                                                                                                                          |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| 别名 | `"top-left"` \| `"top-center"` \| `"top-right"` \| `"bottom-left"` \| `"bottom-center"` \| `"bottom-right"` \| `"center"` \| `"hovered"` |

## Sendable {#sendable}

可以跨线程发送的值。详见[可发送值](/docs/plugins/overview#sendable)。

|      |                                                                                   |
| ---- | --------------------------------------------------------------------------------- |
| 别名 | `nil` \| `boolean` \| `number` \| `string` \| `Url` \| `{ [Sendable]: Sendable }` |

## Renderable {#renderable}

可以渲染的元素。

|      |                                                                       |
| ---- | --------------------------------------------------------------------- |
| 别名 | `Bar` \| `Border` \| `Clear` \| `Gauge` \| `Line` \| `List` \| `Text` |

## AsPos {#as-pos}

可以协变处理为 [`Pos`](/docs/plugins/layout#pos) 的值。

|      |                                                                                |
| ---- | ------------------------------------------------------------------------------ |
| 别名 | `Pos` \| `{ [1]: Origin, x: integer?, y: integer?, w: integer?, h: integer? }` |

## AsSpan {#as-span}

可以协变处理为 [`Span`](/docs/plugins/layout#span) 的值。

|      |                    |
| ---- | ------------------ |
| 别名 | `string` \| `Span` |

## AsLine {#as-line}

可以协变处理为 [`Line`](/docs/plugins/layout#line) 的值。

|      |                                                          |
| ---- | -------------------------------------------------------- |
| 别名 | `string` \| `Span` \| `Line` \| `(string\|Span\|Line)[]` |

## AsText {#as-text}

可以协变处理为 [`Text`](/docs/plugins/layout#text) 的值。

|      |                                                          |
| ---- | -------------------------------------------------------- |
| 别名 | `string` \| `Span` \| `Line` \| `(string\|Span\|Line)[]` |

## AsColor {#as-color}

表示颜色的常量集。

|      |                                                                                                                                                                                                                                                                     |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 别名 | `"black"` \| `"white"` \| `"red"` \| `"lightred"` \| `"green"` \| `"lightgreen"` \| `"yellow"` \| `"lightyellow"` \| `"blue"` \| `"lightblue"` \| `"magenta"` \| `"lightmagenta"` \| `"cyan"` \| `"lightcyan"` \| `"gray"` \| `"darkgray"` \| `"reset"` \| `string` |
