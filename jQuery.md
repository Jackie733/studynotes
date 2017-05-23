### jQuery选择器

#### 内容筛选选择器

![](http://img.mukewang.com/57cd20bf0001a97f05290214.jpg)

**注意事项：**

1. :contains与:has都有查找的意思，但是contains查找包含“指定文本”的元素，has查找包含“指定元素”的元素
2. 如果:contains匹配的文本包含在元素的子元素中，同样认为是符合条件的。
3. :parent与:empty是相反的，两者所涉及的子元素，包括文本节点

#### 可见性筛选选择器

![](http://img.mukewang.com/5590f6de0001e2b204460106.jpg)

```
:hidden选择器，不仅仅包含样式是display="none"的元素，还包括隐藏表单、visibility等等
```

我们有几种方式可以隐藏一个元素：

1. CSS display的值是none。
2. type="hidden"的表单元素。
3. 宽度和高度都显式设置为0。
4. 一个祖先元素是隐藏的，该元素是不会在页面上显示
5. CSS visibility的值是hidden
6. CSS opacity的指是0

```
如果元素中占据文档中一定的空间,元素被认为是可见的。
可见元素的宽度或高度，是大于零。
元素的visibility: hidden 或 opacity: 0被认为是可见的，因为他们仍然占用空间布局。
```

#### 属性筛选选择器

![](http://img.mukewang.com/57d654200001c46507360560.jpg)

在这么多属性选择器中[attr="value"]和[attr*="value"]是最实用的

```
[attr="value"]能帮我们定位不同类型的元素，特别是表单form元素的操作，比如说input[type="text"],input[type="checkbox"]等
[attr*="value"]能在网站中帮助我们匹配不同类型的文件
```

#### 子元素筛选选择器

![](http://img.mukewang.com/559105da0001301105960331.jpg)

注意事项：

1. :first只匹配一个单独的元素，但是:first-child选择器可以匹配多个：即为每个父级元素匹配第一个子元素。这相当于:nth-child(1)
2. :last 只匹配一个单独的元素， :last-child 选择器可以匹配多个元素：即，为每个父级元素匹配最后一个子元素
3. 如果子元素只有一个的话，:first-child与:last-child是同一个
4. :only-child匹配某个元素是父元素中唯一的子元素，就是说当前子元素是父元素中唯一的元素，则匹配
5. jQuery实现:nth-child(n)是严格来自CSS规范，所以n值是“索引”，也就是说，从1开始计数，:nth-child(index)从1开始的，而eq(index)是从0开始的
6. nth-child(n) 与 :nth-last-child(n) 的区别前者是从前往后计算，后者从后往前计算

#### 表单元素选择器

![](http://img.mukewang.com/5592040d0001f8eb04940441.jpg)

注意事项：

除了input筛选选择器，几乎每个表单类别筛选器都对应一个input元素的type值。大部分表单类别筛选器可以使用属性筛选器替换。比如 

```
 $(':password') == $('[type=password]')
```

#### 表单对象属性筛选选择器

![](http://img.mukewang.com/55920c2f0001198b04940201.jpg)

注意事项：

1. 选择器适用于复选框和单选框，对于下拉框元素, 使用 :selected 选择器
2. 在某些浏览器中，选择器:checked可能会错误选取到<option>元素，所以保险起见换用选择器input:checked，确保只会选取<input>元素


#### 特殊选择器this

this是JavaScript中的关键字，指的是当前的上下文对象，简单的说就是方法/属性的所有者

下面例子中，imooc是一个对象，拥有name属性与getName方法,在getName中this指向了所属的对象imooc

```javascript
var imooc = {
    name:"慕课网",
    getName:function(){
        //this,就是imooc对象
        return this.name;
    }
}
imooc.getName(); //慕课网
```

当然在JavaScript中this是动态的，也就是说这个上下文对象都是可以被动态改变的(可以通过call,apply等方法)，具体的大家可以查阅相关资料

同样的在DOM中this就是指向了这个html元素对象，因为this就是DOM元素本身的一个引用

假如给页面一个P元素绑定一个事件:

```javascript
p.addEventListener('click',function(){
    //this === p
    //以下两者的修改都是等价的
    this.style.color = "red";
    p.style.color = "red";
},false);
```

通过addEventListener绑定的事件回调中，this指向的是当前的dom对象，所以再次修改这样对象的样式，只需要通过this获取到引用即可

```
 this.style.color = "red"
```

但是这样的操作其实还是很不方便的，这里面就要涉及一大堆的样式兼容，如果通过jQuery处理就会简单多了，我们只需要把this加工成jQuery对象

换成jQuery的做法：

```javascript
$('p').click(function(){
    //把p元素转化成jQuery的对象
    var $this= $(this) 
    $this.css('color','red')
})
```

通过把$()方法传入当前的元素对象的引用this，把这个this加工成jQuery对象，我们就可以用jQuery提供的快捷方法直接处理样式了

**总体：**

```
this，表示当前的上下文对象是一个html对象，可以调用html对象所拥有的属性和方法。
$(this),代表的上下文对象是一个jquery的上下文对象，可以调用jQuery的方法和属性值。
```

### 属性与样式

#### .attr()与.removeAttr()

操作特性的DOM方法主要有3个，getAttribute方法、setAttribute方法和removeAttribute方法，就算如此在实际操作中还是会存在很多问题，这里先不说。而在jQuery中用一个attr()与removeAttr()就可以全部搞定了，包括兼容问题

jQuery中用attr()方法来获取和设置元素属性,attr是attribute（属性）的缩写，在jQuery DOM操作中会经常用到attr()

**attr()有4个表达式**

1. attr(传入属性名)：获取属性的值
2. attr(属性名, 属性值)：设置属性的值
3. attr(属性名,函数值)：设置属性的函数值
4. attr(attributes)：给指定元素设置多个属性值，即：{属性名一: “属性值一” , 属性名二: “属性值二” , … … }

**removeAttr()删除方法**

.removeAttr( attributeName ) : 为匹配的元素集合中的每个元素中移除一个属性（attribute）

**注意的问题：**

dom中有个概念的区分：Attribute和Property翻译出来都是“属性”，《js高级程序设计》书中翻译为“特性”和“属性”。简单理解，Attribute就是dom节点自带的属性

例如：html中常用的id、class、title、align等：

```html
<div id="immooc" title="慕课网"></div>
```

而Property是这个DOM元素作为对象，其附加的内容，例如,tagName, nodeName, nodeType,, defaultChecked, 和 defaultSelected 使用.prop()方法进行取值或赋值等

#### html()及.text()

**.html()方法** 

获取集合中第一个匹配元素的HTML内容 或 设置每一个匹配元素的html内容，具体有3种用法：

1. .html() 不传入值，就是获取集合中第一个匹配元素的HTML内容
2. .html( htmlString )  设置每一个匹配元素的html内容
3. .html( function(index, oldhtml) ) 用来返回设置HTML内容的一个函数

**注意事项：**

```
.html()方法内部使用的是DOM的innerHTML属性来处理的，所以在设置与获取上需要注意的一个最重要的问题，这个操作是针对整个HTML内容（不仅仅只是文本内容）
```

**.text()方法**

得到匹配元素集合中每个元素的文本内容结合，包括他们的后代，或设置匹配元素集合中每个元素的文本内容为指定的文本内容。，具体有3种用法：

1. .text() 得到匹配元素集合中每个元素的合并文本，包括他们的后代
2. .text( textString ) 用于设置匹配元素内容的文本
3. .text( function(index, text) ) 用来返回设置文本内容的一个函数

**注意事项：**

```
.text()结果返回一个字符串，包含所有匹配元素的合并文本
```

**.html与.text的异同:**

1. .html与.text的方法操作是一样，只是在具体针对处理对象不同
2. .html处理的是元素内容，.text处理的是文本内容
3. .html只能使用在HTML文档中，.text 在XML 和 HTML 文档中都能使用
4. 如果处理的对象只有一个子文本节点，那么html处理的结果与text是一样的
5. 火狐不支持innerText属性，用了类似的textContent属性，.text()方法综合了2个属性的支持，所以可以兼容所有浏览器

#### .val()

jQuery中有一个.val()方法主要是用于处理表单元素的值，比如 input, select 和 textarea。

**.val()方法**

1. .val()无参数，获取匹配的元素集合中第一个元素的当前值
2. .val( value )，设置匹配的元素集合中每个元素的值
3. .val( function ) ，一个用来返回设置值的函数

**注意事项**

1. 通过.val()处理select元素， 当没有选择项被选中，它返回null
2. .val()方法多用来设置表单的字段的值
3. 如果select元素有multiple（多选）属性，并且至少一个选择项被选中， .val()方法返回一个数组，这个数组包含每个选中选择项的值

**.html(),.text()和.val()的差异总结： ** 

1. .html(),.text(),.val()三种方法都是用来读取选定元素的内容；只不过.html()是用来读取元素的html内容（包括html标签），.text()用来读取元素的纯文本内容，包括其后代元素，.val()是用来读取表单元素的"value"值。其中.html()和.text()方法不能使用在表单元素上,而.val()只能使用在表单元素上；另外.html()方法使用在多个元素上时，只读取第一个元素；.val()方法和.html()相同，如果其应用在多个元素上时，只能读取第一个表单元素的"value"值，但是.text()和他们不一样，如果.text()应用在多个元素上时，将会读取所有选中元素的文本内容。

2. .html(htmlString),.text(textString)和.val(value)三种方法都是用来替换选中元素的内容，如果三个方法同时运用在多个元素上时，那么将会替换所有选中元素的内容。

3. .html(),.text(),.val()都可以使用回调函数的返回值来动态的改变多个元素的内容。

   ​

#### 增加样式.addClass()

通过动态改变类名（class），可以让其修改元素呈现出不同的效果。在HTML结构中里，多个class以空格分隔，当一个节点（或称为一个标签）含有多个class时，DOM元素响应的className属性获取的不是class名称的数组，而是一个含有空格的字符串，这就使得多class操作变得很麻烦。同样的jQuery开发者也考虑到这种情况，增加了一个.addClass()方法，用于动态增加class类名

**.addClass( className )方法**

1. .addClass( className ) : 为每个匹配元素所要增加的一个或多个样式名
2. .addClass( function(index, currentClass) ) : 这个函数返回一个或更多用空格隔开的要增加的样式名

**注意事项：**

```
.addClass()方法不会替换一个样式类名。它只是简单的添加一个样式类名到元素上
```

#### 删除样式.removeClass()

jQuery通过.addClass()方法可以很便捷的增加样式。如果需要样式之间的切换，同样jQuery提供了一个很方便的.removeClass()，它的作用是从匹配的元素中删除全部或者指定的class

**.removeClass( )方法**

1. .removeClass( [className ] )：每个匹配元素移除的一个或多个用空格隔开的样式名
2. .removeClass( function(index, class) ) ： 一个函数，返回一个或多个将要被移除的样式名

**注意事项**

如果一个样式类名作为一个参数,只有这样式类会被从匹配的元素集合中删除 。 如果没有样式名作为参数，那么所有的样式类将被移除

#### 切换样式.toggleClass()

在做某些效果的时候，可能会针对同一节点的某一个样式不断的切换，也就是addClass与removeClass的互斥切换，比如隔行换色效果

jQuery提供一个toggleClass方法用于简化这种互斥的逻辑，通过toggleClass方法动态添加删除Class，一次执行相当于addClass，再次执行相当于removeClass

**.toggleClass( )方法：**在匹配的元素集合中的每个元素上添加或删除一个或多个样式类,取决于这个样式类是否存在或值切换属性。即：如果存在（不存在）就删除（添加）一个类

1. .toggleClass( className )：在匹配的元素集合中的每个元素上用来切换的一个或多个（用空格隔开）样式类名
2. .toggleClass( className, switch )：一个布尔值，用于判断样式是否应该被添加或移除
3. .toggleClass( [switch ] )：一个用来判断样式类添加还是移除的 布尔值
4. .toggleClass( function(index, class, switch) [, switch ] )：用来返回在匹配的元素集合中的每个元素上用来切换的样式类名的一个函数。接收元素的索引位置和元素旧的样式类作为参数

**注意事项：**

1. toggleClass是一个互斥的逻辑，也就是通过判断对应的元素上是否存在指定的Class名，如果有就删除，如果没有就增加

2. toggleClass会保留原有的Class名后新增，通过空格隔开

   ​

#### 样式操作.css()

通过JavaScript获取dom元素上的style属性，我们可以动态的给元素赋予样式属性。在jQuery中我们要动态的修改style属性我们只要使用css()方法就可以实现了

**.css() 方法：获取元素样式属性的计算值或者设置元素的CSS属性**

**获取：**

1. .css( propertyName ) ：获取匹配元素集合中的第一个元素的样式属性的计算值
2. .css( propertyNames )：传递一组数组，返回一个对象结果

**设置：**

1. .css(propertyName, value )：设置CSS
2. .css( propertyName, function )：可以传入一个回调函数，返回取到对应的值进行处理
3. .css( properties )：可以传一个对象，同时设置多个样式

**注意事项：**

1. 浏览器属性获取方式不同，在获取某些值的时候都jQuery采用统一的处理，比如颜色采用RBG，尺寸采用px

2. .css()方法支持驼峰写法与大小写混搭的写法，内部做了容错的处理

3. 当一个数只被作为值（value）的时候， jQuery会将其转换为一个字符串，并添在字符串的结尾处添加px，例如 .css("width",50}) 与 .css("width","50px"})一样

   ​

#### .css()与.addClass()设置样式的区别

**可维护性：**

.addClass()的本质是通过定义个class类的样式规则，给元素添加一个或多个类。css方法是通过JavaScript大量代码进行改变元素的样式

通过.addClass()我们可以批量的给相同的元素设置统一规则，变动起来比较方便，可以统一修改删除。如果通过.css()方法就需要指定每一个元素是一一的修改，日后维护也要一一的修改，比较麻烦

**灵活性：**

通过.css()方式可以很容易动态的去改变一个样式的属性，不需要在去繁琐的定义个class类的规则。一般来说在不确定开始布局规则，通过动态生成的HTML代码结构中，都是通过.css()方法处理的

**样式值：**

.addClass()本质只是针对class的类的增加删除，不能获取到指定样式的属性的值，.css()可以获取到指定的样式值。

**样式的优先级：**

css的样式是有优先级的，当外部样式、内部样式和内联样式同一样式规则同时应用于同一个元素的时候，优先级如下

```
外部样式 < 内部样式 < 内联样式
```

1. **.addClass()方法是通过增加class名的方式，那么这个样式是在外部文件或者内部样式中先定义好的，等到需要的时候在附加到元素上**
2. **通过.css()方法处理的是内联样式，直接通过元素的style属性附加到元素上的**

```
通过.css方法设置的样式属性优先级要高于.addClass方法
```

**总结：**

```
.addClass与.css方法各有利弊，一般是静态的结构，都确定了布局的规则，可以用addClass的方法，增加统一的类规则
如果是动态的HTML结构，在不确定规则，或者经常变化的情况下，一般多考虑.css()方式
```

#### 元素的数据存储

html5 dataset是新的HTML5标准，允许你在普通的元素标签里嵌入类似data-*的属性，来实现一些简单数据的存取。它的数量不受限制，并且也能由JavaScript动态修改，也支持CSS选择器进行样式设置。这使得data属性特别灵活，也非常强大。有了这样的属性我们能够更加有序直观的进行数据预设或存储。那么在不支持HTML5标准的浏览器中，我们如何实现数据存取?  jQuery就提供了一个.data()的方法来处理这个问题

使用jQuery初学者一般不是很关心data方式，这个方法是jquery内部预用的，可以用来做性能优化，比如sizzle选择中可以用来缓存部分结果集等等。当然这个也是非常重要的一个API了，常常用于我们存放临时的一些数据，因为它是直接跟DOM元素对象绑定在一起的

jQuery提供的存储接口

```
jQuery.data( element, key, value )   //静态接口,存数据
jQuery.data( element, key )  //静态接口,取数据   
.data( key, value ) //实例接口,存数据
.data( key ) //实例接口,存数据
```

2个方法在使用上存取都是通一个接口，传递元素，键值数据。在jQuery的官方文档中，建议用.data()方法来代替。

我们把DOM可以看作一个对象，那么我们往对象上是可以存在基本类型，引用类型的数据的，但是这里会引发一个问题，可能会存在**循环引用的内存泄漏风险**

通过jQuery提供的数据接口，就很好的处理了这个问题了，我们不需要关心它底层是如何实现，只需要按照对应的data方法使用就行了

同样的也提供2个对应的删除接口，使用上与data方法其实是一致的，只不过是一个是增加一个是删除罢了

```
jQuery.removeData( element [, name ] )
.removeData( [name ] )
```


### DOM

#### DOM创建节点及节点属性

先介绍下需要用到的浏览器提供的一些原生的方法（这里不处理低版本的IE兼容问题）

创建流程，大体如下：

1. 创建节点(常见的：元素、属性和文本)
2. 添加节点的一些属性
3. 加入到文档中

流程中涉及的一点方法：

- 创建元素：document.createElement

- 设置属性：setAttribute

- 添加文本：innerHTML

- 加入文档：appendChild

  ​

写一个最简单的元素创建代码，我们会发现几个问题：

1. 每一个元素节点都必须单独创建

2. 节点属性需要单独设置，而且设置的接口不是很统一

3. 添加到指定的元素位置不灵活

4. 最后还有一个最重要的：浏览器兼容问题处理

   ​

#### jQuery节点创建与属性的处理

**创建元素节点**：

可以有几种方式，后面会慢慢接触。常见的就是直接把这个节点的结构给通过HTML标记字符串描述出来，通过$()函数处理，$("html结构")

```
$("<div></div>")
```

**创建文本节点**：

与创建元素节点类似，可以直接把文本内容一并描述

```
$("<div>我是文本节点</div>")
```

**创建为属性节点**：

与创建元素节点同样的方式

```
$("<div id='test' class='aaron'>我是文本节点</div>")
```

我们通过jQuery把上一届的代码改造一下，如右边代码所示

一条一句就搞定了，跟写HTML结构方式是一样的

```
$("<div class='right'><div class='aaron'>动态创建DIV元素节点</div></div>")
```

#### DOM内部插入append()与appendTo()

动态创建的元素是不够的，它只是临时存放在内存中，最终我们需要放到页面文档并呈现出来。那么问题来了，怎么放到文档上？

这里就涉及到一个位置关系，常见的就是把这个新创建的元素，当作页面某一个元素的子元素放到其内部。针对这样的处理，jQuery就定义2个操作的方法

![](http://img.mukewang.com/56cc12f800017b4104480146.jpg)

append：这个操作与对指定的元素执行原生的appendChild方法，将它们添加到文档中的情况类似。

appendTo：实际上，使用这个方法是颠倒了常规的$(A).append(B)的操作，即不是把B追加到A中，而是把A追加到B中。

简单的总结就是：

.append()和.appendTo()两种方法功能相同，主要的不同是语法——内容和目标的位置不同

```
append()前面是被插入的对象，后面是要在对象内插入的元素内容
appendTo()前面是要插入的元素内容，而后面是被插入的对象
```

#### DOM外部插入after()与before()

节点与节点之前有各种关系，除了父子，祖辈关系，还可以是兄弟关系。之前我们在处理节点插入的时候，接触到了内部插入的几个方法，这节我们开始讲外部插入的处理，也就是兄弟之间的关系处理，这里jQuery引入了2个方法，可以用来在匹配I的元素前后插入内容

![](http://img.mukewang.com/57481b6b00018e3405210197.jpg)

- before与after都是用来对相对选中元素外部增加相邻的兄弟节点
- 2个方法都是都可以接收HTML字符串，DOM 元素，元素数组，或者jQuery对象，用来插入到集合中每个匹配元素的前面或者后面
- 2个方法都支持多个参数传递after(div1,div2,....) 可以参考右边案例代码

#### DOM内部插入prepend()与prependTo()

在元素内部进行操作的方法，除了在被选元素的结尾（仍然在内部）通过append与appendTo插入指定内容外，相应的还可以在被选元素之前插入，jQuery提供的方法是prepend与prependTo

![](http://img.mukewang.com/57481c3900013c6e05000193.jpg)

- prepend()方法将指定元素插入到匹配元素里面作为它的第一个子元素 (如果要作为最后一个子元素插入用.append()).
- .prepend()和.prependTo()实现同样的功能，主要的不同是语法，插入的内容和目标的位置不同
- 对于.prepend() 而言，选择器表达式写在方法的前面，作为待插入内容的容器，将要被插入的内容作为方法的参数
- 而.prependTo() 正好相反，将要被插入的内容写在方法的前面，可以是选择器表达式或动态创建的标记，待插入内容的容器作为参数。

这里总结下内部操作四个方法的区别：

- append()向每个匹配的元素内部追加内容

- prepend()向每个匹配的元素内部前置内容

- appendTo()把所有匹配的元素追加到另一个指定元素的集合中

- prependTo()把所有匹配的元素前置到另一个指定的元素集合中

  ​

#### DOM外部插入insertAfter()与insertBefore()

与内部插入处理一样，jQuery由于内容目标的位置不同，然增加了2个新的方法insertAfter与insertBefore

![](http://img.mukewang.com/57481d230001b0f305170241.jpg)

- .before()和.insertBefore()实现同样的功能。主要的区别是语法——内容和目标的位置。 对于before()选择表达式在函数前面，内容作为参数，而.insertBefore()刚好相反，内容在方法前面，它将被放在参数里元素的前面
- .after()和.insertAfter() 实现同样的功能。主要的不同是语法——特别是（插入）内容和目标的位置。 对于after()选择表达式在函数的前面，参数是将要插入的内容。对于 .insertAfter(), 刚好相反，内容在方法前面，它将被放在参数里元素的后面
- before、after与insertBefore。insertAfter的除了目标与位置的不同外，后面的不支持多参数处理

注意事项：

- insertAfter将JQuery封装好的元素插入到指定元素的后面，如果元素后面有元素了，那将后面的元素后移，然后将JQuery对象插入；
- insertBefore将JQuery封装好的元素插入到指定元素的前面，如果元素前面有元素了，那将前面的元素前移，然后将JQuery对象插入；

#### DOM节点删除之empty()的基本用法

要移除页面上节点是开发者常见的操作，jQuery提供了几种不同的方法用来处理这个问题，这里我们开仔细了解下empty方法

empty 顾名思义，清空方法，但是与删除又有点不一样，因为它只移除了 指定元素中的所有子节点。

这个方法不仅移除子元素（和其他后代元素），同样移除元素里的文本。因为，根据说明，元素里任何文本字符串都被看做是该元素的子节点。请看下面的HTML：

```
<div class="hello"><p>慕课网</p></div>
```

如果我们通过empty方法移除里面div的所有元素，它只是清空内部的html代码，但是标记仍然留在DOM中

```
//通过empty处理
$('.hello').empty()

//结果：<p>慕课网</p>被移除
<div class="hello"></div>
```

#### DOM节点删除之remove()的有参用法和无参用法

remove与empty一样，都是移除元素的方法，但是remove会将元素自身移除，同时也会移除元素内部的一切，包括绑定的事件及与该元素相关的jQuery数据。

例如一段节点，绑定点击事件

```
<div class="hello"><p>慕课网</p></div>
$('.hello').on("click",fn)
```

如果不通过remove方法删除这个节点其实也很简单，但是同时需要把事件给销毁掉，这里是为了防止"内存泄漏"，所以前端开发者一定要注意，绑了多少事件，不用的时候一定要记得销毁

通过remove方法移除div及其内部所有元素，remove内部会自动操作事件销毁方法，所以使用使用起来非常简单

```
//通过remove处理
$('.hello').remove()
//结果：<div class="hello"><p>慕课网</p></div> 全部被移除
//节点不存在了,同事事件也会被销毁
```

**remove表达式参数：**

remove比empty好用的地方就是可以传递一个选择器表达式用来过滤将被移除的匹配元素集合，可以选择性的删除指定的节点

我们可以通过$()选择一组相同的元素，然后通过remove（）传递筛选的规则，从而这样处理

对比右边的代码区域，我们可以通过类似于这样处理

```
$("p").filter(":contains('3')").remove()
```

**empty方法和remove方法的区别**

**empty** **方法**

- 严格地讲，empty()方法并不是删除节点，而是清空节点，它能清空元素中的所有后代节点
- empty不能删除自己本身这个节点

**remove** **方法**

- 该节点与该节点所包含的所有后代节点将同时被删除
- 提供传递一个筛选的表达式，删除指定合集中的元素

#### DOM节点删除之保留数据的删除操作detach()

如果我们希望临时删除页面上的节点，但是又不希望节点上的数据与事件丢失，并且能在下一个时间段让这个删除的节点显示到页面，这时候就可以使用detach方法来处理

detach从字面上就很容易理解。让一个web元素托管。即从当前页面中移除该元素，但保留这个元素的内存模型对象。

来看看jquery官方文档的解释：

```
这个方法不会把匹配的元素从jQuery对象中删除，因而可以在将来再使用这些匹配的元素。与remove()不同的是，所有绑定的事件、附加的数据等都会保留下来。
$("div").detach()这一句会移除对象，仅仅是显示效果没有了。但是内存中还是存在的。当你append之后，又重新回到了文档流中。就又显示出来了。
```

当然这里要特别注意，detach方法是JQuery特有的，所以它只能处理通过JQuery的方法绑定的事件或者数据

参考右边的代码区域，通过 $("p").detach()把所有的P元素删除后，再通过append把删除的p元素放到页面上，通过点击文字，可以证明事件没有丢失



**remove()与detach()区别**

**remove**：移除节点

- 无参数，移除自身整个节点以及该节点的内部的所有节点，包括节点上事件与数据
- 有参数，移除筛选出的节点以及该节点的内部的所有节点，包括节点上事件与数据

**detach**：移除节点

- 移除的处理与remove一致
- 与remove()不同的是，所有绑定的事件、附加的数据等都会保留下来
- 例如：$("p").detach()这一句会移除对象，仅仅是显示效果没有了。但是内存中还是存在的。当你append之后，又重新回到了文档流中。就又显示出来了。

#### DOM拷贝clone()

克隆节点是DOM的常见操作，jQuery提供一个clone方法，专门用于处理dom的克隆

```
.clone()方法深度 复制所有匹配的元素集合，包括所有匹配元素、匹配元素的下级元素、文字节点。
```

clone方法比较简单就是克隆节点，但是需要注意，如果节点有事件或者数据之类的其他处理，我们需要通过clone(ture)传递一个布尔值ture用来指定，这样不仅仅只是克隆单纯的节点结构，还要把附带的事件与数据给一并克隆了

例如：

```
HTML部分
<div></div>

JavaScript部分
$("div").on('click', function() {//执行操作})

//clone处理一
$("div").clone()   //只克隆了结构，事件丢失

//clone处理二
$("div").clone(true) //结构、事件与数据都克隆
```

使用上就是这样简单，使用克隆的我们需要额外知道的细节：

- clone()方法时，在将它插入到文档之前，我们可以修改克隆后的元素或者元素内容，如右边代码我 $(this).clone().css('color','red') 增加了一个颜色
- 通过传递true，将所有绑定在原始元素上的事件处理函数复制到克隆元素上
- clone()方法是jQuery扩展的，只能处理通过jQuery绑定的事件与数据
- 元素数据（data）内对象和数组不会被复制，将继续被克隆元素和原始元素共享。深复制的所有数据，需要手动复制每一个

#### DOM替换replaceWith()和replaceAll()

**.replaceWith( newContent )**：用提供的内容替换集合中所有匹配的元素并且返回被删除元素的集合

简单来说：用$()选择节点A，调用replaceWith方法，传入一个新的内容B（HTML字符串，DOM元素，或者jQuery对象）用来替换选中的节点A

看个简单的例子：一段HTML代码

```
<div>
    <p>第一段</p>
    <p>第二段</p>
    <p>第三段</p>
</div>
```

替换第二段的节点与内容

```
$("p:eq(1)").replaceWith('<a style="color:red">替换第二段的内容</a>')
```

通过jQuery筛选出第二个p元素，调用replaceWith进行替换，结果如下

```
<div>
    <p>第一段</p>
    <a style="color:red">替换第二段的内容</a>'
    <p>第三段</p>
</div>
```

**.replaceAll( target ) **：用集合的匹配元素替换每个目标元素

.replaceAll()和.replaceWith()功能类似，但是目标和源相反，用上述的HTML结构，我们用replaceAll处理

```
$('<a style="color:red">替换第二段的内容</a>').replaceAll('p:eq(1)')
```

总结：

- .replaceAll()和.replaceWith()功能类似，主要是目标和源的位置区别
- .replaceWith()与.replaceAll() 方法会删除与节点相关联的所有数据和事件处理程序
- .replaceWith()方法，和大部分其他jQuery方法一样，返回jQuery对象，所以可以和其他方法链接使用
- .replaceWith()方法返回的jQuery对象引用的是替换前的节点，而不是通过replaceWith/replaceAll方法替换后的节点

#### DOM包裹wrap()方法

如果要将元素用其他元素包裹起来，也就是给它增加一个父元素，针对这样的处理，JQuery提供了一个wrap方法

**.wrap( wrappingElement )****：**在集合中匹配的每个元素周围包裹一个HTML结构

简单的看一段代码：

```
<p>p元素</p>
```

给p元素增加一个div包裹

```
$('p').wrap('<div></div>')
```

最后的结构，p元素增加了一个父div的结构

```
<div>
    <p>p元素</p>
</div>
```

**.wrap( function ) ****：**一个回调函数，返回用于包裹匹配元素的 HTML 内容或 jQuery 对象

使用后的效果与直接传递参数是一样，只不过可以把代码写在函数体内部，写法不同而已

以第一个案例为例：

```
$('p').wrap(function() {
    return '<div></div>';   //与第一种类似，只是写法不一样
})
```

**注意：**

.wrap()函数可以接受任何字符串或对象，可以传递给$()工厂函数来指定一个DOM结构。这种结构可以嵌套了好几层深，但应该只包含一个核心的元素。每个匹配的元素都会被这种结构包裹。该方法返回原始的元素集，以便之后使用链式方法。

#### DOM包裹unwrap()方法

我们可以通过wrap方法给选中元素增加一个包裹的父元素。相反，如果删除选中元素的父元素要如何处理 ?

jQuery提供了一个unwarp()方法 ，作用与wrap方法是相反的。将匹配元素集合的父级元素删除，保留自身（和兄弟元素，如果存在）在原来的位置。

看一段简单案例：

```
<div>
    <p>p元素</p>
</div>
```

我要删除这段代码中的div，一般常规的方法会直接通过remove或者empty方法

```
$('div').remove();
```

但是如果我还要保留内部元素p，这样就意味着需要多做很多处理，步骤相对要麻烦很多，为了更便捷，jQuery提供了unwarp方法很方便的处理了这个问题

```
$('p').unwarp();
```

找到p元素，然后调用unwarp方法，这样只会删除父辈div元素了

结果：

```
<p>p元素</p>
```

#### DOM包裹wrapInner()方法

如果要将合集中的元素内部所有的子元素用其他元素包裹起来，并当作指定元素的子元素，针对这样的处理，JQuery提供了一个wrapInner方法

**.wrapInner( wrappingElement )****：**给集合中匹配的元素的内部，增加包裹的HTML结构

听起来有点绕，可以用个简单的例子描述下，简单的看一段代码：

```
<div>p元素</div>
<div>p元素</div>
```

给所有元素增加一个p包裹

```
$('div').wrapInner('<p></p>')
```

最后的结构，匹配的di元素的内部元素被p给包裹了

```
<div>
    <p>p元素</p>
</div>
<div>
    <p>p元素</p>
</div>
```

**.wrapInner( function ) ****：**允许我们用一个callback函数做参数，每次遇到匹配元素时，该函数被执行，返回一个DOM元素，jQuery对象，或者HTML片段，用来包住匹配元素的内容

以上面案例为例，

```
$('div').wrapInner(function() {
    return '<p></p>'; 
})
```

以上的写法的结果如下，等同于第一种处理了

```
<div>
    <p>p元素</p>
</div>
<div>
    <p>p元素</p>
</div>
```

注意：

```
 当通过一个选择器字符串传递给.wrapInner() 函数，其参数应该是格式正确的 HTML，并且 HTML 标签应该是被正确关闭的。
```