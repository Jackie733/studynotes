# 盒模型（Box Model)

在一个文档中，每个**元素**都被表示为一个**矩形的盒子**。确定这些盒子的尺寸, 属性 --- 像它的颜色，背景，边框方面 --- 和位置是渲染引擎的目标。

在CSS中，使用标准**盒模型**描述这些矩形盒子中的每一个。这个模型描述了元素所占空间的内容。每个盒子有四个边：**外边距边**, **边框边**, **内填充边** 与 **内容边**。 

![](https://developer.mozilla.org/files/72/boxmodel%20(1).png)

**内容区域content area** 是包含元素真实内容的区域。它通常包含**背景、颜色或者图片**等，位于内容边界的内部，它的大小为内容宽度 或 *content-box*宽及内容高度或content-box高。 

如果 [`box-sizing`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/box-sizing) 为默认值， [`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width), [`min-width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/min-width), [`max-width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/max-width), [`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height), [`min-height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/min-height) 与 [`max-height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/max-height) 控制内容大小。 

> **box-sizing** 属性用于更改用于计算元素宽度和高度的默认的 [CSS 盒子模型](https://developer.mozilla.org/en-US/docs/CSS/Box_model)。<u>可以使用此属性来模拟不正确支持CSS盒子模型规范的浏览器的行为。</u> 
>
> 在CSS中，你设置一个元素的 [`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width) 与 [`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height) 只会应用到这个元素的**内容区**。如果这个元素有任何的 [`border`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border) 或 [`padding`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding) ，绘制到屏幕上时的盒子宽度和高度会加上设置的边框和内边距值。这意味着当你调整一个元素的宽度和高度时需要时刻注意到这个元素的边框和内边距。当我们实现响应式布局时，这个特点尤其烦人。
>
> box-sizing 属性可以被用来调整这些表现:
>
> - `content-box`  是**默认值**,**width和height只包括内容的宽和高**。如果你设置一个元素的宽为100px，那么这个元素的内容区会有100px宽，并且任何边框和内边距的宽度都会被增加到最后绘制出来的元素宽度中。比如. 如果 .box {width: 350px}; 而且 {border: 10px solid black;} 那么在浏览器中的渲染的实际宽度将是370px; 
>
>   width = 内容的width，height = 内容的height。宽度和高度都不包含内容的边框（border）和内边距（padding） 
>
> - `border-box` width和height属性包括内容，内边距和边框，**但不包括外边距**。也就是说，如果你将一个元素的width设为100px,那么这100px会包含其它的border和padding，内容区的实际宽度会是width减去border + padding的计算值。大多数情况下这使得我们更容易的去设定一个元素的宽高。
>
>   *width = border + padding + 内容的  width*， 
>
>   *height = border + padding + 内容的 height*。 

**内边距区域padding area** 延伸到包围padding的边框。如果**内容区域content area**设置了背景、颜色或者图片，这些样式将会延伸到padding上(**<u>!！而不仅仅是作用于内容区域</u>**)**。**它位于内边距边界内部, <u>它的大小为 *padding-box*  宽与 *padding-box* 高</u>。

内边距与内容边界之间的空间可以由 [`padding-top`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding-top), [`padding-right`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding-right), [`padding-bottom`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding-bottom), [`padding-left`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding-left) 和简写属性 [`padding`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding) 控制。

**边框区域border area** 是包含边框的区域，扩展了内边距区域。它位于边框边界内部，大小为 *border-box*  宽和 *border-box* 高。由 [`border-width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-width) 及简写属性 [`border`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border)控制。

**外边距区域** **margin area**用空白区域扩展边框区域，以分开相邻的元素。它的大小为  *margin-box* 的高宽。

外边距区域大小由 [`margin-top`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-top), [`margin-right`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-right), [`margin-bottom`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-bottom), [`margin-left`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-left) 及简写属性 [`margin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin) 控制。

在 [外边距合并](https://developer.mozilla.org/en/CSS/margin_collapsing) 的情况下，由于盒之间共享外边距，外边距不容易弄清楚。

> **外边距合并** 
>
> 块级元素的[上外边距](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-top)和[下外边距](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-bottom)有时会合并（或折叠）为一个外边距，其大小取其中的最大者，这种行为称为**外边距折叠**（margin collapsing），有时也翻译为**外边距合并**。注意[浮动元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/float)和[绝对定位元素](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position#absolute)的外边距不会折叠（因为这里触发了 [块格式化上下文 Block Formatting Context， BFC](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)）。 
>
> 会发生外边距折叠的三种情况：
>
> - 相邻元素之间
>
>   毗邻的两个元素之间的外边距会折叠（除非后一个元素需要[清除之前的浮动](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clear)）。 
>
> - 父元素与其第一个或最后一个子元素之间
>
>   如果在父元素与其第一个子元素之间不存在边框、内边距、行内内容，也没有创建[块格式化上下文](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)、或者[清除浮动](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clear)将两者的 [`margin-top`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-top) 分开；或者在父元素与其最后一个子元素之间不存在边框、内边距、行内内容、[`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height)、[`min-height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/min-height)、[`max-height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/max-height)将两者的 [`margin-bottom`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-bottom) 分开，那么这两对外边距之间会产生折叠。此时子元素的外边距会“溢出”到父元素的外面。 
>
> - 空的块级元素
>
>   如果一个块级元素中不包含任何内容，并且在其 [`margin-top`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-top) 与 [`margin-bottom`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin-bottom) 之间没有边框、内边距、行内内容、[`height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/height)、[`min-height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/min-height) 将两者分开，则该元素的上下外边距会折叠。
>
> **！！一些需要注意的地方**
>
> - 上述情况的组合会产生更复杂的外边距折叠。
> - 即使某一外边距为0，这些规则仍然适用。因此就算父元素的外边距是0，第一个或最后一个子元素的外边距仍然会“溢出”到父元素的外面。
> - 如果参与折叠的外边距中包含负值，折叠后的外边距的值为<u>最大的正边距与最小的负边距（即绝对值最大的负边距）的和</u>。
> - 如果所有参与折叠的外边距都为负，折叠后的外边距的值为<u>最小的负边距的值</u>。这一规则适用于相邻元素和嵌套元素。

最后，请注意，对于非替换的行内元素来说，尽管内容周围存在内边距与边框，但其占用空间（行高）由 [`line-height`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/line-height) 属性决定。