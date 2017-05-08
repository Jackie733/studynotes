# Flexbox

## flex 模型说明

当元素表现为 flex 框时，它们沿着两个轴来布局：

![](https://developer.mozilla.org/files/3739/flex_terms.png)

- **主轴（main axis）**是沿着 flex 元素放置的方向延伸的轴（比如页面上的横向的行、纵向的列）。该轴的开始和结束被称为** main start** 和** main end**。
- **横轴（cross axis）**是垂直于 flex 元素放置方向的轴。该轴的开始和结束被称为 **cross start** 和** cross end**。
- 设置了 `display: flex` 的父元素被称之为 **flex 容器（flex container）。**
- 在 flex 容器中表现为 flexible 框的元素被称之为 **flex 项**（**flex item**）。

## 列还是行?

Flexbox 提供了 [`flex-direction`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-direction) 这样一个属性，它可以指定主轴的方向（flexbox 子类放置的地方）— 它默认值是 `row，`这使得它们在按你浏览器的默认语言方向排成一排（在英语/中文浏览器中是从左到右）。

尝试将以下声明添加到 section 元素的 css 规则里：

```css
flex-direction: column
```

**注意：**你还可以使用 `row-reverse `和 `column-reverse `值反向排列 flex 项目。用这些值试试看吧！

## 换行

当你在布局中使用定宽或者定高的时候，可能会有一个问题出来即处于容器中的 flexbox 子元素会溢出，破坏了布局。

![](https://mdn.mozillademos.org/files/13410/flexbox-example3.png)在这里我们看到，子代确实超出了它们的容器。 解决此问题的一种方法是将以下声明添加到 section css 规则中：

```css
flex-wrap: wrap
```

现在尝试一下吧；你会看到布局比原来好多了：

## ![](https://mdn.mozillademos.org/files/13412/flexbox-example4.png)flex-flow 缩写

到这里，应当注意到存在着 [`flex-direction`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-direction) 和 [`flex-wrap`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-wrap) — 的缩写 [`flex-flow`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-flow)。比如，你可以将

```css
flex-direction: row;
flex-wrap: wrap;
```

替换为

```css
flex-flow: row wrap;
```

## flex 项的动态尺寸

现在让我们回到第一个例子，看看是如何控制 flex 项占用空间的比例的。打开你本地的 [flexbox0.html](https://github.com/mdn/learning-area/blob/master/css/css-layout/flexbox/flexbox0.html)，或者拷贝 [flexbox1.html](https://github.com/mdn/learning-area/blob/master/css/css-layout/flexbox/flexbox1.html) 作为新的开始（[查看线上](http://mdn.github.io/learning-area/css/css-layout/flexbox/flexbox1.html)）。

先，将以下规则添加到 CSS 的底部：

```css
article {
  flex: 1;
}
```

这是一个无单位的比例值，表示每个 flex 项沿主轴的可用空间大小。本例中，我们设置<article>元素的 flex 值为 1，这表示每个元素占用空间都是相等的，占用的空间是在设置 padding 和 margin 之后剩余的空间。因为它是一个比例，这意味着将每个 flex 项的设置为 400000 的效果和 1 的时候是完全一样的。

现在在上一个规则下添加：

```css
article:nth-of-type(3) {
  flex: 2;
}
```

现在当你刷新，你会看到第三个<article>元素占用了两倍的可用宽度和剩下的一样 — 现在总共有四个比例单位可用。 前两个 flex 项各有一个，因此它们占用每个可用空间的1/4。 第三个有两个单位，所以它占用2/4或这说是1/2的可用空间。

您还可以指定 flex 的最小值。 尝试修改现有的 article 规则：

```css
article {
  flex: 1 200px;
}

article:nth-of-type(3) {
  flex: 2 200px;
}
```

这表示“每个flex 项将首先给出200px的可用空间，然后，剩余的可用空间将根据分配的比例共享“。 尝试刷新，你会看到分配空间的差别。

![](https://mdn.mozillademos.org/files/13406/flexbox-example1.png)Flexbox 的真正价值可以体现在它的灵活性/响应性，如果你调整浏览器窗口的大小，或者增加一个<article>元素，这时的布局仍旧是好的。

## flex: 缩写与全写

[`flex`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex) 是一个可以指定最多三个不同值的缩写属性：

- 第一个就是上面所讨论过的无单位比例。可以单独指定全写 [`flex-grow`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-grow) 属性的值。
- 第二个无单位比例 — [`flex-shrink`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-shrink) — 一般用于溢出容器的 flex 项。这指定了从每个 flex 项中取出多少溢出量，以阻止它们溢出它们的容器。 这是一个相当高级的flexbox功能，我们不会在本文中进一步说明。
- 第三个是上面讨论的最小值。可以单独指定全写 [`flex-basis`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex-basis) 属性的值。

我们建议不要使用全写属性，除非你真的需要（比如要去覆盖之前写的）。使用全写会多些很多的代码，它们也可能有点让人困惑。

## 水平和垂直对齐

还可以使用 flexbox 的功能让 flex 项沿主轴或横轴对齐。

[`align-items`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/align-items) 控制 flex 项在横轴上的位置。

- 默认的值是 `stretch，其会使`所有 flex 项沿着横轴的方向拉伸以填充父容器。如果父容器在横轴方向上没有固定宽度（即高度），则所有 flex 项将变得与最长的 flex 项一样长（即高度保持一致）。我们的第一个例子在默认情况下得到相等的高度的列的原因。
- `在上面规则中我们使用的 center` 值会使这些项保持其原有的高度，但是会在横纵居中。这就是那些按钮垂直居中的原因。
- 你也可以设置诸如 `flex-start` 或 `flex-end 这样使 flex 项在横轴的开始或结束处对齐所有的值。`查看 [`align-items`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/align-items) 了解更多。

[`justify-content`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/justify-content) 控制 flex 项在主轴上的位置。

- `默认值是 flex-start，`这会使所有 flex 项都位于主轴的开始处。
- 你也可以用 `flex-end 来让 flex 项到结尾处。`
- `center` 在 `justify-content 里也是可用的，可以让 flex 项在主轴居中。`
- 而我们上面用到的值 `space-around 是很有用的`—它会使所有 flex 项沿着主轴均匀地分布，在任意一端都会留有一点空间。
- `还有一个值是 space-between，它和` `space-around 非常相似，只是它不会在两端留下任何空间。`

## flex 项排序

Flexbox 也有可以改变 flex 项的布局位置的功能，而不会影响到源顺序（即 dom 树里元素的顺序）。这也是传统布局方式很难做到的一点。

代码也很简单，将下面的 CSS 添加到示例代码下面。

```css
button:first-child {
  order: 1;
}
```

下面我们谈下它实现的一些细节：

- 所有 flex 项默认的 [`order`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/order) 值是 0。
- order 值大的 flex 项比 order 值小的在显示顺序中更靠后。
- 相同 order 值的 flex 项按源顺序显示。所以假如你有四个元素，其 order 值分别是2，1，1和0，那么它们的显示顺序就分别是第四，第二，第三，和第一。
- 第三个元素显示在第二个后面是因为它们的 order 值一样，且第三个元素在源顺序中排在第二个后面。

你也可以给 order 设置负值使它们比值为 0 的元素排得更前面。比如，你可以设置 "Blush" 按钮排在主轴的最前面：

```css
button:last-child {
  order: -1;
}
```

## flex 嵌套

flexbox 也能创建一些颇为复杂的布局。设置 flex 项为 flex 容器也是没有什么问题的，它的孩子也就表现为 flexible box 了。

![](https://mdn.mozillademos.org/files/13418/flexbox-example7.png)这个例子的 HTML 是相当简单的。我们用用一个<section>元素包含了三个<article>。第三个<article>元素包含了三个<div>：

```
section - article
          article
          article - div - button   
                    div   button
                    div   button
                          button
                          button
```

现在让我们看一下布局用到的代码。

首先，我们设置<section>的子代布局为 flexible box。

```css
section {
  display: flex;
}
```

下面我们给<article>元素设置一些 flex 值。特别注意这里的第二条规则—我们设置第三个<article>元素里的子元素同样表现为 flex 项，但是这次我们使它们放置为列。

```css
article {
  flex: 1 200px;
}

article:nth-of-type(3) {
  flex: 3 200px;
  display: flex;
  flex-flow: column;
}
```

接下来，我们选择了第一个<div>。首先使用 `flex: 1 100px;` 简单的给它一个最小的高度 100px，然后设置它的子代（<button>元素）为 flex 项。在这里我们将它们放在一个包装行中，使它们居中对齐，就像我们在前面看到的单个按钮示例中所做的那样。 

```css
article:nth-of-type(3) div:first-child {
  flex: 1 100px;
  display: flex;
  flex-flow: row wrap;
  align-items: center;
  justify-content: space-around;
}
```

最后，我们给按钮设置大小，有意思的是我们给它一个值为1的 flex 属性。如果你调整浏览器窗口宽度，你会看到这是一个非常有趣的效果。按钮将占用尽可能多的空间，尽可能多的坐在同一条线上，但是当它们不再适合在同一条线上，他们会到下一行去。

```css
button {
  flex: 1;
  margin: 5px;
  font-size: 18px;
  line-height: 1.5;
}
```