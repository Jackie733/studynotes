# CSS媒体查询

一个**媒体查询**由一个可选的媒体类型和零个或多个使用媒体功能限制样式表范围的表达式组成, 例如 宽度，高度和颜色。在[CSS3](https://developer.mozilla.org/zh-CN/docs/CSS/CSS3)中添加的**媒体查询**，允许内容的呈现针对一个特定范围的输出设备而定制，而不必改变内容本身。

##  语法

媒体查询包含一个可选的[媒体类型](https://developer.mozilla.org/en-US/docs/CSS/@media)和零个或多个满足CSS3规范的表达式. 这些表达式描述了媒体特征, 最终会被解析为true或false. 如果媒体查询中指定的媒体类型匹配展示文档所使用的设备类型, 并且所有的表达式的值都是true, 那么该媒体查询的结果为true.

```html
<!-- link元素中的CSS媒体查询 -->
<link rel="stylesheet" media="(max-width: 800px)" href="example.css" />

<!-- 样式表中的CSS媒体查询 -->
<style>
@media (max-width: 600px) {
  .facet_sidebar {
    display: none;
  }
}
</style>
```

当媒体查询为true时, 其对应的样式表或样式规则就会遵循正常的级联规则进行应用. 即使媒体查询返回false, `<link>` 标签指向的样式表[也将会被下载](http://scottjehl.github.com/CSS-Download-Tests/)(但是它们不会被应用)

### 逻辑操作符

可以使用not，and和 only 等逻辑操作符构建复杂的媒体查询。`and` 操作符用来把多个 [媒体属性](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries#Media_features) 组合成一条媒体查询。只有当每个属性都为真时，结果才为真。`not` 操作符用来对一条媒体查询的结果进行取反。`only` 操作符表示仅在媒体查询匹配成功的情况下应用指定样式。可以通过它让选中的样式在老式浏览器中不被应用。若使用了 `not `或 `only` 操作符，必须明确指定一个媒体类型。

你也可以将多个媒体查询以逗号分隔放在一起；只要其中任何一个为真，整个媒体语句就返回真。相当于 `or` 操作符。

#### and

`and` 关键字用于合并多个媒体属性或合并媒体属性与媒体类型。一个基本的媒体查询，即一个媒体属性与默认指定的 `all`媒体类型，可能像这样子：

```css
@media (min-width: 700px) { ... }

/*如果你只想在横屏时应用这个，你可以使用 and 操作符合并媒体属性：*/
@media (min-width: 700px) and (orientation: landscape) { ... }

/*现在上面的媒体查询仅在可视区域宽度不小于700像素并在在横屏时有效。如果，你仅想再电视媒体上应用，你可以使用 and 操作符合并媒体属性：*/
@media tv and (min-width: 700px) and (orientation: landscape) { ... }
```

#### 逗号分隔列表

媒体查询中使用逗号分隔效果等同于 `or `逻辑操作符。当使用逗号分隔的媒体查询时，如果任何一个媒体查询返回真，样式就是有效的。逗号分隔的列表中每个查询都是独立的，一个查询中的操作符并不影响其它的媒体查询。这意味着逗号媒体查询列表能够作用于不同的媒体属性、类型和状态。

例如，如果你想在最小宽度为700像素或是横屏的手持设备上应用一组样式，你可以这样写：

```css
@media (min-width: 700px), handheld and (orientation: landscape) { ... }
```

#### not

`not` 关键字应用于整个媒体查询，在媒体查询为假时返回真 (比如 `monochrome` 应用于彩色显示设备上或一个600像素的屏幕应用于 `min-width: 700px` 属性查询上 )。在逗号媒体查询列表中 `not` 仅会否定它应用到的媒体查询上而不影响其它的媒体查询。 `not` 关键字仅能应用于整个查询，而不能单独应用于一个独立的查询。例如，`not` 在下面的查询中最后被计算：

```css
@media not all and (monochrome) { ... }

/*等价于*/
@media not (all and (monochrome)) { ... }

/*而不是*/
@media (not all) and (monochrome) { ... }

/*另一个例子*/
@media not screen and (color), print and (color)
  
/*等价于*/
@media (not (screen and (color))), print and (color)
```

#### only

`only` 关键字防止老旧的浏览器不支持带媒体属性的查询而应用到给定的样式：

```css
<link rel="stylesheet" media="only screen and (color)" href="example.css" />
```

### 伪范式

媒体查询是大小写不敏感的。包含未知媒体类型的查询通常返回假。

```html
media_query_list: <media_query> [, <media_query> ]*
media_query: [[only | not]? <media_type> [ and <expression> ]*]
  | <expression> [ and <expression> ]*
expression: ( <media_feature> [: <value>]? )
media_type: all | aural | braille | handheld | print |
  projection | screen | tty | tv | embossed
media_feature: width | min-width | max-width
  | height | min-height | max-height
  | device-width | min-device-width | max-device-width
  | device-height | min-device-height | max-device-height
  | aspect-ratio | min-aspect-ratio | max-aspect-ratio
  | device-aspect-ratio | min-device-aspect-ratio | max-device-aspect-ratio
  | color | min-color | max-color
  | color-index | min-color-index | max-color-index
  | monochrome | min-monochrome | max-monochrome
  | resolution | min-resolution | max-resolution
  | scan | grid
```

## 媒体属性

大多数媒体属性可以带有“min-”或“max-”前缀，用于表达“最低...”或者“最高...”。例如，max-width:12450px表示应用其所包含样式的条件最高是宽度为12450px，大于12450px则不满足条件，不会应用此样式。这避免了使用与HTML和XML冲突的“<”和“>”字符。如果你未向媒体属性指定一个值，并且该特性的实际值不为零，则该表达式被解析为真。

### 颜色（color）

**值：** <color>
**媒体：** [`media/visual`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Visual)
**是否接受 min/max 前缀：是**

指定输出设备每个像素单元的比特值。如果设备不支持输出颜色，则该值为0。

**注意：**如果每个颜色单元具有不同数量的比特值，则使用最小的。例如，如果显示器为蓝色和红色提供5比特，而为绿色提供6比特，则认为每个颜色单元有5比特**。**如果设备使用索引颜色，则使用颜色表中颜色单元的最小比特数。

#### 示例

向所有能显示颜色的设备应用样式表：

```css
@media all and (color) { ... }
```

向每个颜色单元至少有4个比特的设备应用样式表：

```css
@media all and (min-color: 4) { ... }
```

### 颜色索引（color-index）

**值**：<integer>
**媒体：** [`media/visual`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Visual)
**是否接受 min/max 前缀：**是

指定了输出设备中颜色查询表中的条目数量。

#### 示例

向所有使用索引颜色的设备应用样式表，你可以这么做：

```css
@media all and (color-index) { ... }
```

向所有使用至少256个索引颜色的设备应用样式表：

```html
<link rel="stylesheet" media="all and (min-color-index: 256)" href="http://foo.bar.com/stylesheet.css" />
```

### 宽高比（aspect-ratio）

**值：**<ratio>
**媒体：** [`media/visual`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Visual), [`media/tactile`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Tactile)
**是否接受 min/max 前缀**：是

描述了输出设备目标显示区域的宽高比。该值包含两个以“/”分隔的正整数。代表了水平像素数（第一个值）与垂直像素数（第二个值）的比例。

#### 示例

下面为显示区域宽高至少为一比一的设备选择了一个特殊的样式表。

```css
@media screen and (min-aspect-ratio: 1/1) { ... }
```

这指定了宽高比或者1：1或者更大。换句话说，可视区域或者是正方形或者是宽屏。

### 设备宽高比（device-aspect-ratio）

**值：**<ratio>
**媒体：**[`media/visual`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Visual), [`media/tactile`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Tactile)
**是否接受 min/max 前缀：是**

描述了输出设备的宽高比。该值包含两个以“/”分隔的正整数。代表了水平像素数（第一个值）与垂直像素数（第二个值）的比例。

#### 示例

下面为宽屏设备选择了一个特殊的样式表。

```css
@media screen and (device-aspect-ratio: 16/9), screen and (device-aspect-ratio: 16/10) { ... }
```

### 设备高度（device-height）

**值：**<length>
**媒体：**[`media/visual`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Visual), [`media/tactile`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Tactile)
**是否接受 min/max 前缀：是**

描述了输出设备的高度（整个屏幕或页的高度，而不是仅仅像文档窗口一样的渲染区域）。

#### 示例

向显示在最大宽度800px的屏幕上的文档应用样式表，你可以这样做：

```css
<link rel="stylesheet" media="screen and (max-device-width: 799px)" />
```

### 设备宽度（device-width）

**值：**<length>
**媒体：** [`media/visual`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Visual), [`media/tactile`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Tactile)
**是否接受 min/max 前缀**：是

描述了输出设备的宽度（整个屏幕或页的高度，而不是仅仅像文档窗口一样的渲染区域）。

### 网格（grid）

**值：**<integar>
**媒体：**all
**是否接受 min/max 前缀：** 否

判断输出设备是网格设备还是位图设备。如果设备是基于网格的（例如电传打字机终端或只能显示一种字形的电话），该值为1，否则为0。

#### 示例

向一个15字符宽度或更窄的手持设备应用样式：

```css
@media handheld and (grid) and (max-width: 15em) { ... }
```

**注意：**“em” 在网格设备中有不同的意义；一个“em”的实际宽度不得而知，假设1em相当于一个网格单元的宽高。

### 高度（height）

**值：**<length>
**媒体：**[`media/visual`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Visual), [`media/tactile`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Tactile)
**是否接受 min/max 前缀：**是

`height` 媒体属性描述了输出设备渲染区域（如可视区域的高度或打印机纸盒的高度）的高度。

**注意：**用户调整窗口大小后，火狐浏览器会根据使用了`width`和`height`属性的媒体查询来切换合适的样式表。

### 黑白（monochrome）

**值：**<integer>
**媒体：** [`media/visual`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Visual)
**是否接受 min/max 前缀：是**

指定了一个黑白（灰度）设备每个像素的比特数。如果不是黑白设备，值为0。

#### 示例

向所有黑白设备应用样式表：

```css
@media all and (monochrome) { ... }
```

向每个像素至少8比特的黑白设备应用样式表：

```css
@media all and (min-monochrome: 8) { ... }
```

### 方向（orientation）

**值：**`landscape` | `portrait`
**媒体：**[`media/visual`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Visual)
**是否接受 min/max 前缀：否**

指定了设备处于横屏（宽度大于高度）模式还是竖屏（高度大于宽度）模式。

#### 示例

向竖屏设备应用样式表：

```css
@media all and (orientation: portrait) { ... }
```

### 分辨率（resolution）

**值：**<resolution>
**媒体：** [`bitmap`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Bitmap)
**是否接受 min/max 前缀：是**

指定输出设备的分辨率（像素密度）。分辨率可以用每英寸（dpi）或每厘米（dpcm）的点数来表示。

#### 示例

为每英寸至多300点的打印机应用样式：

```css
@media print and (min-resolution: 300dpi) { ... }
```

替换老旧的 (min-device-pixel-ratio: 2) 语法：

```css
@media screen and (min-resolution: 2dppx) { ... }
```

### 扫描（scan）

**值：** `progressive` | `interlace`
**媒体：**[`media/tv`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/TV)
**是否接受 min/max 前缀：否**

描述了电视输出设备的扫描过程。

#### 示例

向以顺序方式扫描的电视机上应用样式表：

```css
@media tv and (scan: progressive) { ... }
```

### 宽度（width）

**值：**<length>
**媒体：** [`media/visual`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Visual), [`media/tactile`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Media/Tactile)
**是否接受 min/max 前缀：是**

`width` 媒体属性描述了输出设备渲染区域（如可视区域的宽度或打印机纸盒的宽度）的宽度。

**注意：**用户调整窗口大小后，火狐浏览器会根据使用了`width`和`height`属性的媒体查询来切换合适的样式表。

#### 示例

如果你想向最小宽度20em的手持设备或屏幕应用样式表，你可以使用这样的查询：

```css
@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }
```

这个媒体查询将向最小宽度8.5英寸的打印机应用样式表：

```css
<link rel="stylesheet" media="print and (min-width: 8.5in)"
    href="http://foo.com/mystyle.css" />
```

这个查询适用于宽度在500px和800px之间的屏幕：

```css
@media screen and (min-width: 500px) and (max-width: 800px) { ... }
```

