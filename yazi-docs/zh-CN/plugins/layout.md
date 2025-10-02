---
sidebar_position: 2
description: 学习如何使用 Yazi 的 Lua API。
---

# 布局

Line、Text、List、Bar、Border 和 Gauge 是可渲染的元素；其他元素需要放置在它们中的任何一个中。

## Rect {#rect}

Rect 通过四个属性表示终端内的一个区域：

```lua
ui.Rect {
	x = 10, -- x 位置
	y = 10, -- y 位置
	w = 20, -- 宽度
	h = 30, -- 高度
}

ui.Rect.default  -- 等同于 `ui.Rect { x = 0, y = 0, w = 0, h = 0 }`
```

你可以通过 [`ui.Layout()`](#layout) 获取预先计算的 `Rect`。
注意，如果你打算自己创建 `Rect`，请确保这些值计算准确；否则可能导致 Yazi 崩溃！

### `x` {#rect.x}

Rect 的 X 位置。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `y` {#rect.y}

Rect 的 Y 位置。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `w` {#rect.w}

Rect 的宽度。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `h` {#rect.h}

Rect 的高度。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `left` {#rect.left}

Rect 的左侧位置。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `right` {#rect.right}

Rect 的右侧位置。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `top` {#rect.top}

Rect 的顶部位置。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `bottom` {#rect.bottom}

Rect 的底部位置。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `pad(self, padding)` {#rect.pad}

对 rect 应用 `padding`。

| 输入/输出 | 类型          |
| --------- | ------------- |
| `self`    | `Self`        |
| `padding` | [`Pad`](#pad) |
| 返回      | `self`        |

### `__new(value)` {#rect.\_\_new}

创建一个新的 rect。

| 输入/输出 | 类型                                                     |
| --------- | -------------------------------------------------------- |
| `value`   | `{ x: integer?, y: integer?, w: integer?, h: integer? }` |
| 返回      | `Self`                                                   |

## Pad {#pad}

`Pad` 表示内边距，其所有参数都是整数：

```lua
ui.Pad(top, right, bottom, left)
```

### `top` {#pad.top}

顶部内边距。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `right` {#pad.right}

右侧内边距。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `bottom` {#pad.bottom}

底部内边距。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `left` {#pad.left}

左侧内边距。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `top(top)` {#pad.Top}

创建仅包含顶部值的内边距，等同于 `ui.Pad(top, 0, 0, 0)`。

| 输入/输出 | 类型      |
| --------- | --------- |
| `top`     | `integer` |
| 返回      | `Self`    |

### `right(right)` {#pad.Right}

创建仅包含右侧值的内边距，等同于 `ui.Pad(0, right, 0, 0)`。

| 输入/输出 | 类型      |
| --------- | --------- |
| `right`   | `integer` |
| 返回      | `Self`    |

### `bottom(bottom)` {#pad.Bottom}

创建仅包含底部值的内边距，等同于 `ui.Pad(0, 0, bottom, 0)`。

| 输入/输出 | 类型      |
| --------- | --------- |
| `bottom`  | `integer` |
| 返回      | `Self`    |

### `left(left)` {#pad.Left}

创建仅包含左侧值的内边距，等同于 `ui.Pad(0, 0, 0, left)`。

| 输入/输出 | 类型      |
| --------- | --------- |
| `left`    | `integer` |
| 返回      | `Self`    |

### `x(x)` {#pad.X}

在 x 轴两侧创建内边距，等同于 `ui.Pad(0, x, 0, x)`。

| 输入/输出 | 类型      |
| --------- | --------- |
| `x`       | `integer` |
| 返回      | `Self`    |

### `y(y)` {#pad.Y}

在 y 轴两侧创建内边距，等同于 `ui.Pad(y, 0, y, 0)`。

| 输入/输出 | 类型      |
| --------- | --------- |
| `y`       | `integer` |
| 返回      | `Self`    |

### `xy(x, y)` {#pad.XY}

在 x 和 y 轴两侧创建内边距，等同于 `ui.Pad(y, x, y, x)`。

| 输入/输出 | 类型      |
| --------- | --------- |
| `x`       | `integer` |
| `y`       | `integer` |
| 返回      | `Self`    |

### `__new(top, right, bottom, left)` {#pad.\_\_new}

创建一个新的内边距。

| 输入/输出 | 类型      |
| --------- | --------- |
| `top`     | `integer` |
| `right`   | `integer` |
| `bottom`  | `integer` |
| `left`    | `integer` |
| 返回      | `Self`    |

## Pos {#pos}

`Pos` 表示一个位置，由一个原点和相对于该原点的偏移组成：

```lua
ui.Pos { "center", x = 5, y = 3, w = 20, h = 10 }
```

它的唯一参数是一个包含以下键的表：

- `[1]`：位置的[原点](/docs/plugins/aliases#origin)。
- `x`：相对于原点的 X 偏移，默认为 0。
- `y`：相对于原点的 Y 偏移，默认为 0。
- `w`：宽度，默认为 0。
- `h`：高度，默认为 0。

### `[1]` {#pos.[1]}

位置的原点。

|      |                                          |
| ---- | ---------------------------------------- |
| 类型 | [`Origin`](/docs/plugins/aliases#origin) |

### `x` {#pos.x}

相对于原点的 X 偏移。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `y` {#pos.y}

相对于原点的 Y 偏移。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `w` {#pos.w}

位置的宽度。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `h` {#pos.h}

位置的高度。

|      |           |
| ---- | --------- |
| 类型 | `integer` |

### `__new(value)` {#pos.\_\_new}

创建一个新的位置。

| 输入/输出 | 类型                                                                  |
| --------- | --------------------------------------------------------------------- |
| `value`   | `{ [1]: Origin, x?: integer, y?: integer, w?: integer, h?: integer }` |
| 返回      | `Self`                                                                |

## Style {#style}

创建一个样式：

```lua
ui.Style()
```

### `fg(self, color)` {#style.fg}

应用前景色。

| 输入/输出 | 类型                                        |
| --------- | ------------------------------------------- |
| `self`    | `Self`                                      |
| `color`   | [`AsColor`](/docs/plugins/aliases#as-color) |
| 返回      | `self`                                      |

### `bg(self, color)` {#style.bg}

应用背景色。

| 输入/输出 | 类型                                        |
| --------- | ------------------------------------------- |
| `self`    | `Self`                                      |
| `color`   | [`AsColor`](/docs/plugins/aliases#as-color) |
| 返回      | `self`                                      |

### `bold(self)` {#style.bold}

应用粗体样式。

| 输入/输出 | 类型   |
| --------- | ------ |
| `self`    | `Self` |
| 返回      | `self` |

### `dim(self)` {#style.dim}

应用暗淡样式。

| 输入/输出 | 类型   |
| --------- | ------ |
| `self`    | `Self` |
| 返回      | `self` |

### `italic(self)` {#style.italic}

应用斜体样式。

| 输入/输出 | 类型   |
| --------- | ------ |
| `self`    | `Self` |
| 返回      | `self` |

### `underline(self)` {#style.underline}

应用下划线样式。

| 输入/输出 | 类型   |
| --------- | ------ |
| `self`    | `Self` |
| 返回      | `self` |

### `blink(self)` {#style.blink}

应用闪烁样式。

注意，此样式可能不被所有终端支持。

| 输入/输出 | 类型   |
| --------- | ------ |
| `self`    | `Self` |
| 返回      | `self` |

### `blink_rapid(self)` {#style.blink_rapid}

应用快速闪烁样式。

注意，此样式可能不被所有终端支持。

| 输入/输出 | 类型   |
| --------- | ------ |
| `self`    | `Self` |
| 返回      | `self` |

### `reverse(self)` {#style.reverse}

应用反转样式。

| 输入/输出 | 类型   |
| --------- | ------ |
| `self`    | `Self` |
| 返回      | `self` |

### `hidden(self)` {#style.hidden}

应用隐藏样式。

| 输入/输出 | 类型   |
| --------- | ------ |
| `self`    | `Self` |
| 返回      | `self` |

### `crossed(self)` {#style.crossed}

应用删除线样式。

| 输入/输出 | 类型   |
| --------- | ------ |
| `self`    | `Self` |
| 返回      | `self` |

### `reset(self)` {#style.reset}

应用重置样式。

| 输入/输出 | 类型   |
| --------- | ------ |
| `self`    | `Self` |
| 返回      | `self` |

### `patch(self, another)` {#style.patch}

用 `another` 修补样式。

| 输入/输出 | 类型                |
| --------- | ------------------- |
| `self`    | `Self`              |
| `another` | `Self`              |
| 返回      | `self`              |
| 私有      | 此方法不能被继承。  |

### `__new()` {#style.\_\_new}

创建一个新的样式。

| 输入/输出 | 类型   |
| --------- | ------ |
| 返回      | `Self` |

## Span {#span}

`ui.Span` 是文本的最小单位，也是 `ui.Line` 的组成部分。创建一个 span：

```lua
ui.Span("foo")
```

为方便起见，`ui.Span` 也可以接受自身作为参数：

```lua
ui.Span(ui.Span("bar"))
```

|      |                   |                                            |
| ---- | ----------------- | ------------------------------------------ |
| 继承 | [`Style`](#style) | 可直接调用 [`Style`](#style) 方法          |

### `visible(self)` {#span.visible}

span 是否可见，即包含任何可打印字符。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| 返回      | `boolean` |

### `style(self, style)` {#span.style}

设置 span 的样式。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `style`   | [`Style`](#style) |
| 返回      | `self`            |

Span 继承自 `Style`，除了应用整个 `Style`，你也可以直接在其上调用 `Style` 的那些方法，这意味着：

```lua
local style = ui.Style():fg("white"):bg("black"):bold()
ui.Span("Hello world"):style(style)
```

也可以写成：

```lua
ui.Span("Hello world"):fg("white"):bg("black"):bold()
```

### `__new(value)` {#span.\_\_new}

创建一个新的 span。

| 输入/输出 | 类型                                      |
| --------- | ----------------------------------------- |
| `value`   | [`AsSpan`](/docs/plugins/aliases#as-span) |
| 返回      | `Self`                                    |

## Line {#line}

`ui.Line` 表示一行，由多个 `ui.Span` 组成，它接受它们的表：

```lua
ui.Line { ui.Span("foo"), ui.Span("bar") }
```

为方便起见，还支持以下类型：

```lua
-- 字符串
ui.Line("foo")

-- ui.Span
ui.Line(ui.Span("bar"))

-- ui.Line 本身
ui.Line(ui.Line("baz"))

-- 字符串、ui.Span、ui.Line 的混合表
ui.Line { "foo", ui.Span("bar"), ui.Line("baz") }
```

|      |                   |                                       |
| ---- | ----------------- | ------------------------------------- |
| 继承 | [`Style`](#style) | 可直接调用 [`Style`](#style) 方法     |

### `area(self, rect)` {#line.area}

设置行的区域。

| 输入/输出 | 类型                      |
| --------- | ------------------------- |
| `self`    | `Self`                    |
| `rect`    | [`Rect?`](#rect)          |
| 返回      | `self` \| [`Rect`](#rect) |

如果未指定 `rect`，则返回当前区域。

### `width(self)` {#line.width}

计算行的宽度。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| 返回      | `integer` |

### `align(self, align)` {#line.align}

设置行的对齐方式。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `align`   | [`Align`](#align) |
| 返回      | `self`            |

### `visible(self)` {#line.visible}

行是否可见，即包含任何可打印字符。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| 返回      | `boolean` |

### `style(self, style)` {#line.style}

设置行的样式。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `style`   | [`Style`](#style) |
| 返回      | `self`            |

与 [`Span`](#span) 类似，你也可以直接在其上调用 [`Style`](#style) 方法：

```lua
ui.Line("Hello world"):fg("white"):bg("black"):bold()
```

### `__new(value)` {#line.\_\_new}

创建一个新的行。

| 输入/输出 | 类型                                      |
| --------- | ----------------------------------------- |
| `value`   | [`AsLine`](/docs/plugins/aliases#as-line) |
| 返回      | `Self`                                    |

## Text {#text}

`ui.Text` 用于表示多行文本，它接受一个 `ui.Line` 的表：

```lua
ui.Text { ui.Line("foo"), ui.Line("bar") }
```

为方便起见，还支持以下类型：

```lua
-- 字符串
ui.Text("foo\nbar")

-- ui.Line
ui.Text(ui.Line("foo"))

-- ui.Span
ui.Text(ui.Span("bar"))

-- 字符串、ui.Line、ui.Span 的混合表
ui.Text { "foo", ui.Line("bar"), ui.Span("baz") }
```

你也可以使用 `ui.Text.parse(code)` 将 [ANSI 转义序列](https://en.wikipedia.org/wiki/ANSI_escape_code)字符串解析为文本。

|      |                   |                                       |
| ---- | ----------------- | ------------------------------------- |
| 继承 | [`Style`](#style) | 可直接调用 [`Style`](#style) 方法     |

### `area(self, rect)` {#text.area}

设置文本的区域。

| 输入/输出 | 类型                      |
| --------- | ------------------------- |
| `self`    | `Self`                    |
| `rect`    | [`Rect?`](#rect)          |
| 返回      | `self` \| [`Rect`](#rect) |

如果未指定 `rect`，则返回当前区域。

### `align(self, align)` {#text.align}

设置文本的对齐方式。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `align`   | [`Align`](#align) |
| 返回      | `self`            |

### `wrap(self, wrap)` {#text.wrap}

设置文本的换行方式。

| 输入/输出 | 类型            |
| --------- | --------------- |
| `self`    | `Self`          |
| `wrap`    | [`Wrap`](#wrap) |
| 返回      | `self`          |

### `max_width(self)` {#text.max_width}

计算所有行中文本的最大宽度。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| 返回      | `integer` |

### `style(self, style)` {#text.style}

设置文本的样式。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `style`   | [`Style`](#style) |
| 返回      | `self`            |

与 [`Span`](#span) 类似，你也可以直接在其上调用 [`Style`](#style) 方法：

```lua
ui.Text("Hello world"):fg("white"):bg("black"):bold()
```

### `__new(value)` {#text.\_\_new}

创建一个新的文本。

| 输入/输出 | 类型                                      |
| --------- | ----------------------------------------- |
| `value`   | [`AsText`](/docs/plugins/aliases#as-text) |
| 返回      | `Self`                                    |

## Layout {#layout}

创建一个布局：

```lua
local areas = ui.Layout()
	:direction(ui.Layout.HORIZONTAL)
	:constraints({ ui.Constraint.Percentage(50), ui.Constraint.Percentage(50) })
	:split(area)

local left = areas[1] -- 第一个 rect
local right = areas[2] -- 第二个 rect
```

### `direction(self, direction)` {#layout.direction}

设置布局的方向。

| 输入/输出   | 类型        |
| ----------- | ----------- |
| `self`      | `Self`      |
| `direction` | `Direction` |
| 返回        | `self`      |

`direction` 接受以下常量：

- `ui.Layout.HORIZONTAL`
- `ui.Layout.VERTICAL`

### `margin(self, margin)` {#layout.margin}

设置布局的边距。

| 输入/输出 | 类型      | 注意     |
| --------- | --------- | -------- |
| `self`    | `Self`    | -        |
| `margin`  | `integer` | 正整数   |
| 返回      | `self`    | -        |

### `margin_h(self, margin)` {#layout.margin_h}

设置布局的水平边距。

| 输入/输出 | 类型      | 注意     |
| --------- | --------- | -------- |
| `self`    | `Self`    | -        |
| `margin`  | `integer` | 正整数   |
| 返回      | `self`    | -        |

### `margin_v(self, margin)` {#layout.margin_v}

设置布局的垂直边距。

| 输入/输出 | 类型      | 注意     |
| --------- | --------- | -------- |
| `self`    | `Self`    | -        |
| `margin`  | `integer` | 正整数   |
| 返回      | `self`    | -        |

### `constraints(self, constraints)` {#layout.constraints}

设置布局的约束。

| 输入/输出     | 类型                          |
| ------------- | ----------------------------- |
| `self`        | `Self`                        |
| `constraints` | [`Constraint[]`](#constraint) |
| 返回          | `self`                        |

### `split(self, rect)` {#layout.split}

根据约束将布局分割为多个 [Rect](#rect)。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `rect`    | [`Rect`](#rect)   |
| 返回      | [`Rect[]`](#rect) |

### `__new()` {#layout.\_\_new}

创建一个新的布局。

| 输入/输出 | 类型   |
| --------- | ------ |
| 返回      | `Self` |

## Constraint {#constraint}

定义布局元素大小的约束。

约束可用于指定固定大小、可用空间的百分比、可用空间的比率、最小或最大大小，或布局元素的按比例填充值。

相对约束（百分比、比率）是相对于被划分的整个空间计算的，而不是应用更固定约束（最小值、最大值、长度）后的可用空间。

约束按以下顺序优先级排序：

1. `ui.Constraint.Min(min)`
2. `ui.Constraint.Max(max)`
3. `ui.Constraint.Length(len)`
4. `ui.Constraint.Percentage(p)`
5. `ui.Constraint.Ratio(num, den)`
6. `ui.Constraint.Fill(scale)`

### `Min(min)` {#constraint.Min}

对元素应用最小尺寸约束。

| 输入/输出 | 类型      |
| --------- | --------- |
| `min`     | `integer` |
| 返回      | `Self`    |

元素大小至少设置为指定的数量。

```lua
-- { Percentage(100), Min(20) }
-- ┌────────────────────────────┐┌──────────────────┐
-- │            30 px           ││       20 px      │
-- └────────────────────────────┘└──────────────────┘

-- { Percentage(100), Min(10) }
-- ┌──────────────────────────────────────┐┌────────┐
-- │                 40 px                ││  10 px │
-- └──────────────────────────────────────┘└────────┘
```

### `Max(max)` {#constraint.Max}

对元素应用最大尺寸约束。

| 输入/输出 | 类型      |
| --------- | --------- |
| `max`     | `integer` |
| 返回      | `Self`    |

元素大小最多设置为指定的数量。

```lua
-- { Percentage(0), Max(20) }
-- ┌────────────────────────────┐┌──────────────────┐
-- │            30 px           ││       20 px      │
-- └────────────────────────────┘└──────────────────┘

-- { Percentage(0), Max(10) }
-- ┌──────────────────────────────────────┐┌────────┐
-- │                 40 px                ││  10 px │
-- └──────────────────────────────────────┘└────────┘

```

### `Length(len)` {#constraint.Length}

对元素应用长度约束。

| 输入/输出 | 类型      |
| --------- | --------- |
| `len`     | `integer` |
| 返回      | `Self`    |

元素大小设置为指定的数量：

```lua
-- { Length(20), Length(20) }
-- ┌──────────────────┐┌──────────────────┐
-- │       20 px      ││       20 px      │
-- └──────────────────┘└──────────────────┘

-- { Length(20), Length(30) }
-- ┌──────────────────┐┌────────────────────────────┐
-- │       20 px      ││            30 px           │
-- └──────────────────┘└────────────────────────────┘
```

### `Percentage(p)` {#constraint.Percentage}

对元素应用可用空间的百分比。

| 输入/输出 | 类型      |
| --------- | --------- |
| `p`       | `integer` |
| 返回      | `Self`    |

将给定的百分比转换为浮点值并乘以区域。
此值作为布局分割计算的一部分舍入回整数。

```lua
-- { Percentage(75), Fill(1) }
-- ┌────────────────────────────────────┐┌──────────┐
-- │                38 px               ││   12 px  │
-- └────────────────────────────────────┘└──────────┘

-- { Percentage(50), Fill(1) }
-- ┌───────────────────────┐┌───────────────────────┐
-- │         25 px         ││         25 px         │
-- └───────────────────────┘└───────────────────────┘
```

### `Ratio(num, den)` {#constraint.Ratio}

对元素应用可用空间的比率。

| 输入/输出 | 类型      |
| --------- | --------- |
| `num`     | `integer` |
| `den`     | `integer` |
| 返回      | `Self`    |

将给定的比率转换为浮点值并乘以区域。
此值作为布局分割计算的一部分舍入回整数。

```lua
-- { Ratio(1, 2), Ratio(1, 2) }
-- ┌───────────────────────┐┌───────────────────────┐
-- │         25 px         ││         25 px         │
-- └───────────────────────┘└───────────────────────┘

-- { Ratio(1, 4), Ratio(1, 4), Ratio(1, 4), Ratio(1, 4) }
-- ┌───────────┐┌──────────┐┌───────────┐┌──────────┐
-- │   13 px   ││   12 px  ││   13 px   ││   12 px  │
-- └───────────┘└──────────┘└───────────┘└──────────┘
```

### `Fill(scale)` {#constraint.Fill}

应用与所有其他 `Fill` 元素成比例的缩放因子来填充多余空间。

| 输入/输出 | 类型      |
| --------- | --------- |
| `scale`   | `integer` |
| 返回      | `Self`    |

元素只会扩展或填充到多余的可用空间，按比例匹配其他 `Fill` 元素，同时满足所有其他约束。

```lua
-- { Fill(1), Fill(2), Fill(3) }
-- ┌──────┐┌───────────────┐┌───────────────────────┐
-- │ 8 px ││     17 px     ││         25 px         │
-- └──────┘└───────────────┘└───────────────────────┘

-- { Fill(1), Percentage(50), Fill(1) }
-- ┌───────────┐┌───────────────────────┐┌──────────┐
-- │   13 px   ││         25 px         ││   12 px  │
-- └───────────┘└───────────────────────┘└──────────┘
```

更多信息请参阅 https://docs.rs/ratatui/latest/ratatui/layout/enum.Constraint.html。

## List {#list}

创建一个接受 `ui.Text` 表的 `List`：

```lua
ui.List { ui.Text("foo"), ui.Text("bar") }
```

为方便起见，还支持以下类型：

```lua
-- 字符串表
ui.List { "foo", "bar" }

-- ui.Line 表
ui.List { ui.Line("foo"), ui.Line("bar") }

-- ui.Span 表
ui.List { ui.Span("foo"), ui.Span("bar") }

-- 字符串、ui.Line、ui.Span 的混合表
ui.List { "foo", ui.Line("bar"), ui.Span("baz") }
```

### `area(self, rect)` {#list.area}

设置列表的区域。

| 输入/输出 | 类型                      |
| --------- | ------------------------- |
| `self`    | `Self`                    |
| `rect`    | [`Rect?`](#rect)          |
| 返回      | `self` \| [`Rect`](#rect) |

如果未指定 `rect`，则返回当前区域。

### `style(self, style)` {#list.style}

设置列表的样式。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `style`   | [`Style`](#style) |
| 返回      | `self`            |

### `__new(value)` {#list.\_\_new}

创建一个新的列表。

| 输入/输出 | 类型                                                                     |
| --------- | ------------------------------------------------------------------------ |
| `value`   | `string` \| `Span` \| `Line` \| `Text` \| `(string\|Span\|Line\|Text)[]` |
| 返回      | `Self`                                                                   |

## Bar {#bar}

创建一个 bar：

```lua
ui.Bar(edge)
```

第一个属性表示 bar 的方向，接受一个 [`Edge`](#edge) 常量。

### `area(self, rect)` {#bar.area}

设置 bar 的区域。

| 输入/输出 | 类型                      |
| --------- | ------------------------- |
| `self`    | `Self`                    |
| `rect`    | [`Rect?`](#rect)          |
| 返回      | `self` \| [`Rect`](#rect) |

如果未指定 `rect`，则返回当前区域。

### `symbol(self, symbol)` {#bar.symbol}

设置 bar 的符号。

| 输入/输出 | 类型     |
| --------- | -------- |
| `self`    | `Self`   |
| `symbol`  | `string` |
| 返回      | `self`   |

### `style(self, style)` {#bar.style}

设置 bar 的样式。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `style`   | [`Style`](#style) |
| 返回      | `self`            |

### `__new(edge)` {#bar.\_\_new}

创建一个新的 bar。

| 输入/输出 | 类型            |
| --------- | --------------- |
| `edge`    | [`Edge`](#edge) |
| 返回      | `Self`          |

## Border {#border}

创建一个 border：

```lua
ui.Border(edge)
```

第一个属性表示 border 的边缘，接受一个 [`Edge`](#edge) 常量。

### `area(self, rect)` {#border.area}

设置 border 的区域。

| 输入/输出 | 类型                      |
| --------- | ------------------------- |
| `self`    | `Self`                    |
| `rect`    | [`Rect?`](#rect)          |
| 返回      | `self` \| [`Rect`](#rect) |

如果未指定 `rect`，则返回当前区域。

### `type(self, type)` {#border.type}

设置 border 的类型。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| `type`    | `integer` |
| 返回      | `self`    |

`type` 接受以下常量：

- `ui.Border.PLAIN`
- `ui.Border.ROUNDED`
- `ui.Border.DOUBLE`
- `ui.Border.THICK`
- `ui.Border.QUADRANT_INSIDE`
- `ui.Border.QUADRANT_OUTSIDE`

### `style(self, style)` {#border.style}

设置 border 的样式。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `style`   | [`Style`](#style) |
| 返回      | `self`            |

### `__new(edge)` {#border.\_\_new}

创建一个新的 border。

| 输入/输出 | 类型            |
| --------- | --------------- |
| `edge`    | [`Edge`](#edge) |
| 返回      | `Self`          |

## Gauge {#gauge}

创建一个 gauge：

```lua
ui.Gauge()
```

### `area(self, rect)` {#gauge.area}

设置 gauge 的区域。

| 输入/输出 | 类型                      |
| --------- | ------------------------- |
| `self`    | `Self`                    |
| `rect`    | [`Rect?`](#rect)          |
| 返回      | `self` \| [`Rect`](#rect) |

如果未指定 `rect`，则返回当前区域。

### `percent(self, percent)` {#gauge.percent}

设置 gauge 的百分比。

| 输入/输出 | 类型      |
| --------- | --------- |
| `self`    | `Self`    |
| `percent` | `integer` |
| 返回      | `self`    |

### `ratio(self, ratio)` {#gauge.ratio}

设置 gauge 的比率。

| 输入/输出 | 类型     | 注意     |
| --------- | -------- | -------- |
| `self`    | `Self`   | -        |
| `ratio`   | `number` | 0 到 1 之间 |
| 返回      | `self`   | -        |

### `label(self, label)` {#gauge.label}

设置 gauge 的标签。

| 输入/输出 | 类型     |
| --------- | -------- |
| `self`    | `Self`   |
| `label`   | `string` |
| 返回      | `self`   |

### `style(self, style)` {#gauge.style}

设置除 gauge 本身之外的所有内容的样式。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `style`   | [`Style`](#style) |
| 返回      | `self`            |

### `gauge_style(self, style)` {#gauge.gauge_style}

设置 gauge 本身的样式。

| 输入/输出 | 类型              |
| --------- | ----------------- |
| `self`    | `Self`            |
| `style`   | [`Style`](#style) |
| 返回      | `self`            |

### `__new()` {#gauge.\_\_new}

创建一个新的 gauge。

| 输入/输出 | 类型   |
| --------- | ------ |
| 返回      | `Self` |

## Clear {#clear}

清除特定区域的内容，该区域是一个 [Rect](#rect)。将其放在你想清除的组件后面：

```lua
local components = {
	ui.Text("..."):area(rect),
	-- ...

	ui.Clear(rect),
}
```

### `area(self, rect)` {#clear.area}

设置 clear 的区域。

| 输入/输出 | 类型                      |
| --------- | ------------------------- |
| `self`    | `Self`                    |
| `rect`    | [`Rect?`](#rect)          |
| 返回      | `self` \| [`Rect`](#rect) |

如果未指定 `rect`，则返回当前区域。

### `__new(rect)` {#clear.\_\_new}

创建一个新的 clear。

| 输入/输出 | 类型            |
| --------- | --------------- |
| `rect`    | [`Rect`](#rect) |
| 返回      | `Self`          |

## Align {#align}

元素（如 [`Text`](#text) 或 [`Line`](#line)）的对齐方式。

### `LEFT` {#align.LEFT}

左对齐。

|      |        |
| ---- | ------ |
| 类型 | `Self` |

### `CENTER` {#align.CENTER}

居中对齐。

|      |        |
| ---- | ------ |
| 类型 | `Self` |

### `RIGHT` {#align.RIGHT}

右对齐。

|      |        |
| ---- | ------ |
| 类型 | `Self` |

## Wrap {#wrap}

[`Text`](#text) 的换行行为。

### `NO` {#wrap.NO}

禁用换行。

|      |        |
| ---- | ------ |
| 类型 | `Self` |

### `YES` {#wrap.YES}

启用换行。

|      |        |
| ---- | ------ |
| 类型 | `Self` |

### `TRIM` {#wrap.TRIM}

启用换行并修剪前导空格。

|      |        |
| ---- | ------ |
| 类型 | `Self` |

## Edge {#edge}

元素（如 [`Bar`](#bar) 或 [`Border`](#border)）的哪些边缘应该被应用。

### `NONE` {#edge.NONE}

不应用任何边缘。

|      |        |
| ---- | ------ |
| 类型 | `Self` |

### `TOP` {#edge.TOP}

应用顶部边缘。

|      |        |
| ---- | ------ |
| 类型 | `Self` |

### `RIGHT` {#edge.RIGHT}

应用右侧边缘。

|      |        |
| ---- | ------ |
| 类型 | `Self` |

### `BOTTOM` {#edge.BOTTOM}

应用底部边缘。

|      |        |
| ---- | ------ |
| 类型 | `Self` |

### `LEFT` {#edge.LEFT}

应用左侧边缘。

|      |        |
| ---- | ------ |
| 类型 | `Self` |

### `ALL` {#edge.ALL}

应用所有边缘。

|      |        |
| ---- | ------ |
| 类型 | `Self` |
