## 语法和数据类型

#### 基础

JavaScript区分**大小写**，并使用Unicode字符集。

在JavaScript中，指令被称为  [语句](https://developer.mozilla.org/en-US/docs/Glossary/Statement)，并用分号 (;)分隔。空格、制表符和换行符被称为空白字符。

#### 注释

```javascript
// 单行注释

/* 这是一个更长的,
   多行注释
*/

/* 然而, 你不能, /* 嵌套注释 */ 语法错误 */
```

#### 声明 

JavaScript有三种声明方式。

`var` ：声明一个变量，可赋一个初始化之。（若未赋初始值，则其值为`undefined`）

`let`  ：声明一个**块作用域的局部变量**，可赋一个初始化值。（若未赋初始值，则其值为`undefined`）

`const` ：声明一个**块作用域**的**只读**的命名**常量**。

##### 变量 

又叫标识符，需要遵守一定规则。

标识符必须以字母、下划线（_）或者美元符号（$）开头；后续的字符也可以是数字。	

若试图访问一个未声明的变量会导致一个`ReferenceError`异常被抛出。

`undefined`值在布尔类型环境中会被当做`false`。

数值类环境中`undefined`值会被转换为`NaN`。

当对一个`null`变量求值时，空值`null`在数值类型环境中会被当做0来对待，而布尔类型环境中会被当做`false`。

##### 变量的作用域

在所有函数之外声明的变量，叫做*全局*变量，因为它可被当前文档中的任何其他代码所访问。在函数内部声明的变量，叫做*局部*变量，因为它只能在该函数内部访问。

ECMAScript 6 之前的JavaScript没有 [语句块](https://developer.mozilla.org/zh-CN/docs/JavaScript/Guide/Statements#Block_Statement) 作用域；相反，语句块中声明的变量将成为语句块所在代码段的局部变量。例如，如下的代码将在控制台输出 5，因为 `x` 的作用域是声明了 `x` 的那个函数（或全局范围），而不是 `if` 语句块。而使用ECMAScript6中的`let`声明，则结果不一样。

```javascript
if (true) {
  var x = 5;
}
console.log(x); // 5
```

```javascript
if (true) {
  let y = 5;
}
console.log(y); // ReferenceError: y is not defined
```

##### 变量声明提升（Variable hoisting）

JavaScript 变量的另一特别之处是，你可以引用稍后声明的变量而不会引发异常。这一概念称为变量声明提升(**hoisting**)；JavaScript 变量感觉上是被“提升”或移到了所有函数和语句之前。<u>然而提升后的变量将返回 undefined 值</u>。所以在使用或引用某个变量之后进行声明和初始化操作，这个被提升的引用仍将得到 undefined 值。

```javascript
/**
 * Example 1
 */
console.log(x === undefined); // true
var x = 3;


/**
 * Example 2
 */
// will return a value of undefined
var myvar = "my value";

(function() {
  console.log(myvar); // undefined
  var myvar = "local value";
})();
```

上面列子，可写成：

```javascript
/**
 * Example 1
 */
var x;
console.log(x === undefined); // true
x = 3;
 
/**
 * Example 2
 */
var myvar = "my value";
 
(function() {
  var myvar;
  console.log(myvar); // undefined
  myvar = "local value";
})();
```

由于存在变量声明提升，一个函数中所有的`var`语句应尽可能地放在接近函数顶部的地方。这将大大提升程序代码的清晰度。

在 ECMAScript 2015 中，let（const）将**不会提升**变量到代码块的顶部。因此，在变量声明之前引用这个变量，将抛出错误[`ReferenceError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/ReferenceError)。这个变量将从代码块一开始的时候就处在一个“暂时性死区”，直到这个变量被声明为止。

```javascript
console.log(x); // ReferenceError
let x = 3;
```

##### 函数提升（Function hoisting)

对于函数，只有函数声明会被提升到顶部，而不包括函数表达式。

```javascript
/* 函数声明 */

foo(); // "bar"

function foo() {
  console.log("bar");
}


/* 函数表达式 */

baz(); // TypeError: baz is not a function

var baz = function() {
  console.log("bar2");
};
```

##### 全局变量（Global variables）

全局变量实际上是全局对象的属性。在网页中，（译注：缺省的）全局对象是 [`window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)，所以你可以用形如 window.*variable*的语法来设置和访问全局变量。

##### 常量（Constants）

可以用关键字 [`const`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/const) 创建一个只读的常量。常量标识符的命名规则和变量相同：必须以字母、下划线或美元符号开头并可以包含有字母、数字或下划线。

常量不可以通过赋值改变其值，也不可以在脚本运行时重新声明。**它必须被初始化为某个值。**

常量的作用域规则与 `let` 块级作用域变量相同。若省略`const`关键字，则该标识符将被视为变量。

在同一作用域中，不能使用与变量名或函数名相同的名字来命名常量。例如：

```javascript
// THIS WILL CAUSE AN ERROR
function f() {};
const f = 5;

// THIS WILL CAUSE AN ERROR ALSO
function f() {
  const g = 5;
  var g;

  //statements
}
```

然而,对象属性是不受保护的,所以可以使用如下语句来执行。

```javascript
const MY_OBJECT = {"key": "value"};
MY_OBJECT.key = "otherValue";
```
#### 数据结构和类型

JavaScript可以识别7种类型的值：

* 六种原型数据类型
  * Boolean.布尔值，true和false.
  * null.一个表明null值得特殊关键字。JavaScript是大小写敏感的，因此null和Null、NULL或其他变量完全不同。
  * undefined.变量未定义时的属性。
  * Number.表示数字。
  * String.表示字符串。
  * Symbol(在ECMAScript中新添加的类型)。一种数据类型，它的实例是唯一不可改变的。
* 以及Object对象。

[Objects](https://developer.mozilla.org/zh-CN/docs/JavaScript/Reference/Global_Objects/Object) 和 [functions](https://developer.mozilla.org/zh-CN/docs/JavaScript/Reference/Global_Objects/Function) 是本语言的其他两个基本要素。你可以将对象视为存放值的命名容器，而将函数视为你的应用程序能够执行的过程(procedures)。

##### 数据类型的转换（Data type conversion）

JavaScript是一种动态类型语言(dynamically typed language)。这意味着你声明变量时可以不必指定数据类型，而数据类型会在脚本执行时根据需要自动转换。

在涉及加法运算符(+)的数字和字符串表达式中，JavaScript 会把**数字值转换为字符串**。例如，假设有如下的语句：

```javascript
x = "The answer is " + 42 // "The answer is 42"
y = 42 + " is the answer" // "42 is the answer"
```

在涉及其它运算符（译注：如下面的减号'-'）时，JavaScript语言不会把数字变为字符串。例如（译注：第一例是数学运算，第二例是字符串运算）：

```javascript
"37" - 7 // 30
"37" + 7 // "377"
```

##### 字符串转换为数字（converting strings to numbers）

有一些方法可以将内存中表示一个数字的字符串转换为对应的数字

`parseInt()`和`parseFloat()`

`parseInt `仅能够返回整数，所以使用它会丢失小数部分。另外，调用 parseInt 时最好总是带上进制(radix) 参数，这个参数用于指定使用哪一种进制。

**单目加法运算符**

将字符串转换为数字的另一种方法是使用**单目加法运算符**。

```javascript
"1.1" + "1.1" = "1.11.1"
(+"1.1") + (+"1.1") = 2.2   // 注：加入括号为清楚起见，不是必需的。
```

#### 字面量（Literals）

（译注：字面量是由语法表达式定义的**常量**；或，通过由一定字词组成的语词表达式定义的常量）

在JavaScript中，你可以使用各种字面量。这些字面量是脚本中按字面意思给出的固定的值，而不是变量。（译注：字面量是常量，其值是固定的，而且在程序脚本运行中不可更改，比如*false*，3.1415，thisIsStringOfHelloworld ，invokedFunction: myFunction("myArgument")。本节将介绍以下类型的字面量：

- [数组字面量(Array literals)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Grammar_and_Types#%E6%95%B0%E7%BB%84%E5%AD%97%E9%9D%A2%E9%87%8F(Array_literals))
- [布尔字面量(Boolean literals)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Grammar_and_Types#%E5%B8%83%E5%B0%94%E5%AD%97%E9%9D%A2%E9%87%8F(Boolean_literals))
- [浮点数字面量(Floating-point literals)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Grammar_and_Types#%E6%B5%AE%E7%82%B9%E6%95%B0%E5%AD%97%E9%9D%A2%E9%87%8F(Floating-point_literals))
- [整数(Intergers)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Grammar_and_Types#%E6%95%B4%E6%95%B0(Intergers))
- [对象字面量(Object literals)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Grammar_and_Types#%E5%AF%B9%E8%B1%A1%E5%AD%97%E9%9D%A2%E9%87%8F(Object_literals))
- [RegExp literals](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Grammar_and_Types#RegExp_literals)
- [字符串字面量(String literals)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Grammar_and_Types#%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%AD%97%E9%9D%A2%E9%87%8F(String_literals))

##### 数组字面量（Array literals）

数组字面值是一个封闭在方括号对([])中的包含有零个或多个表达式的列表，其中每个表达式代表数组的一个元素。当你使用数组字面值创建一个数组时，该数组将会以指定的值作为其元素进行初始化，而其长度被设定为元素的个数。

下面的示例用3个元素生成数组`coffees`，它的长度是3。

```javascript
var coffees = ["French Roast", "Colombian", "Kona"];

var a=[3];

console.log(a.length); // 1

console.log(a[0]); // 3
```

若在顶层（全局）脚本里用字面值创建数组，JavaScript语言将会在每次对包含该数组字面值的表达式求值时解释该数组。另一方面，在函数中使用的数组，将在每次调用函数时都会被创建一次。

数组字面值同时也是数组对象。有关数组对象的详情请参见[数组对象](https://developer.mozilla.org/zh-CN/docs/JavaScript/Guide/Predefined_Core_Objects#Array_Object)一文。

###### 数组字面值中的多余逗号

（译注：声明时）你不必列举数组字面值中的所有元素。若你在同一行中连写两个逗号（,），数组中就会产生一个没有被指定的元素，其初始值是`undefined`。以下示例创建了一个名为`fish`的数组：

```javascript
var fish = ["Lion", , "Angel"];
```

在这个数组中，有两个已被赋值的元素，和一个空元素（fish[0]是"Lion"，fish[1]是undefined，而fish[2]是"Angel"；译注：此时数组的长度属性fish.length是3)。

##### 布尔字面量 (Boolean literals)

（译注：即逻辑字面量）

布尔类型有两种字面量：`true`和`false`。

不要混淆作为布尔对象的真和假与布尔类型的原始值true和false。布尔对象是原始布尔数据类型的一个包装器。使用一下语法创建布尔对象。

```javascript
var b = new Boolean(value);
if (b) 	//这个条件评估为真
if (b == true)	//这个条件评估为假
```

不要把原始数据类型的true和false与布尔对象的true和false混淆在一起。任何对象的值都不是 `undefined` , `null`, `0`, `NaN` 或者是空字符串，即使布尔对象的值是假，当执行条件语句是，也会被评定为真。

##### 整数 (Integers)

（译注：原文如此，没写成“整数字面量”，这里指的是整数字面量。）

整数可以用十进制（基数为10）、十六进制（基数为16）、八进制（基数为8）以及二进制（基数为2）表示。

- 十进制整数字面量由一串数字序列组成，且没有前缀0。
- 八进制的整数以 0（或0O、0o）开头，只能包括数字0-7。
- 十六进制整数以0x（或0X）开头，可以包含数字（0-9）和字母 a~f 或 A~F。
- 二进制整数以0b（或0B）开头，只能包含数字0和1。

**严格模式下，八进制整数字面量必须以0o或0O开头，而不能以0开头。**

整数字面量举例：

```javascript
0, 117 and -345 (十进制, 基数为10)
015, 0001 and -0o77 (八进制, 基数为8) 
0x1123, 0x00111 and -0xF1A7 (十六进制, 基数为16或"hex")
0b11, 0b0011 and -0b11 (二进制, 基数为2)
```

##### 浮点数字面量 (Floating-point literals)

浮点数字面值可以有以下的组成部分：

- 一个十进制整数，可以带正负号（即前缀“+”或“ - ”），
- 小数点（“.”），
- 小数部分（由一串十进制数表示），
- 指数部分。

指数部分以“e”或“E”开头，后面跟着一个整数，可以有正负号（即前缀“+”或“-”）。浮点数字面量至少有一位数字，而且必须带小数点或者“e”（大写“E”也可）。

简言之，其语法是：

```javascript
[(+|-)][digits][.digits][(E|e)[(+|-)]digits]
```

例如：

```javascript
3.14      
-.2345789 // -0.23456789
-3.12e+12  // -3.12*1012
.1e-23    // 0.1*10-23=10-24=1e-24
```

##### 对象字面量 (Object literals)

对象字面值是封闭在花括号对({})中的一个对象的零个或多个"属性名-值"对的（元素）列表。你不能在一条语句的开头就使用对象字面值，这将导致错误或产生超出预料的行为， 因为此时左花括号（{）会被认为是一个语句块的起始符号。

更进一步的，你可以使用数字或字符串字面值作为属性的名字，或者在另一个字面值内嵌套上一个字面值 。

对象属性名字可以是任意字符串，包括空串。如果对象属性名字不是合法的javascript标识符，它必须用""包裹。属性的名字不合法，那么便不能用.访问属性值，而是通过类数组标记("[]")访问和赋值。

```javascript
var unusualPropertyNames = {
  "": "An empty string",
  "!": "Bang!"
}
console.log(unusualPropertyNames."");   // 语法错误: Unexpected string
console.log(unusualPropertyNames[""]);  // An empty string
console.log(unusualPropertyNames.!);    // 语法错误: Unexpected token !
console.log(unusualPropertyNames["!"]); // Bang!
```

```javascript
var foo = {a: "alpha", 2: "two"};
console.log(foo.a);    // alpha
console.log(foo[2]);   // two
//console.log(foo.2);  // Error: missing ) after argument list
//console.log(foo[a]); // Error: a is not defined
console.log(foo["a"]); // alpha
console.log(foo["2"]); // two	
```

##### RegExp 字面值

一个正则表达式是字符被斜线（译注：正斜杠“/”）围成的表达式。下面是一个正则表达式文字的一个例子。

```javascript
var re = /ab+c/;
```

##### 字符串字面量 (String literals)

字符串字面量是由双引号（"）对或单引号（'）括起来的零个或多个字符。字符串被限定在同种引号之间；也即，必须是成对单引号或成对双引号。

###### 在字符串中使用的特殊字符

作为一般字符的扩展，你可以在字符串中使用特殊字符，如下例所示。

```javascript
"one line \n another line"
```

以下表格列举了你能在JavaScript的字符串中使用的特殊字符。

| 字符        | 意思                                                         |
| ----------- | ------------------------------------------------------------ |
| \0          | Null字节                                                     |
| \b          | 退格符                                                       |
| \f          | 换页符                                                       |
| \n          | 换行符                                                       |
| \r          | 回车符                                                       |
| \t          | Tab (制表符)                                                 |
| \v          | 垂直制表符                                                   |
| \'          | 单引号                                                       |
| \"          | 双引号                                                       |
| \\          | 反斜杠字符（\）                                              |
| \*XXX*      | 由从0到377最多三位八进制数*XXX*表示的 Latin-1 字符。例如，\251是版权符号的八进制序列。 |
| \x*XX*      | 由从00和FF的两位十六进制数字XX表示的Latin-1字符。例如，\ xA9是版权符号的十六进制序列。 |
| *\uXXXX*    | 由四位十六进制数字XXXX表示的Unicode字符。例如，\ u00A9是版权符号的Unicode序列。见[Unicode escape sequences](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Lexical_grammar#String_literals) (Unicode 转义字符). |
| \u*{XXXXX}* | Unicode代码点 (code point) 转义字符。例如，\u{2F804} 相当于Unicode转义字符 \uD87E\uDC04的简写。 |

译注：严格模式下，不能使用八进制转义字符。

###### 转义字符

对于那些未出现在上表中的字符，其所带的前导反斜线'\'将被忽略。但是，这一用法已被废弃，应当避免使用。

通过在引号前加上反斜线'\'，可以在字符串中插入引号，这就是*引号转义*。例如:

```javascript
var quote = "He read \"The Cremation of Sam McGee\" by R.W. Service.";
console.log(quote);
```

代码的运行结果为:

```javascript
He read "The Cremation of Sam McGee" by R.W. Service.
```

要在字符串中插入'\'字面值，必须转义反斜线。例如，要把文件路径 c:\temp 赋值给一个字符串，可以采用如下方式:

```javascript
var home = "c:\\temp";
```

也可以在换行之前加上反斜线以转义换行（译注：实际上就是一条语句拆成多行书写），这样反斜线和换行都不会出现在字符串的值中。

```javascript
var str = "this string \
is broken \
across multiple\
lines."
console.log(str);   // this string is broken across multiplelines.
```

Javascript没有“heredoc”语法，但可以用行末的换行符转义和转义的换行来近似实现 

```javascript
var poem = 
"Roses are red,\n\
Violets are blue.\n\
Sugar is sweet,\n\
and so is foo."
```

ECMAScript 2015 增加了一种新的字面量，叫做模板字面量 **template literals。**它包含一些新特征，包括了多行字符串！

```javascript
var poem =
`Roses are red,
Violets are blue.
Sugar is sweet,
and so is foo.`
```

## 流程控制与错误处理

#### 语句块

最基本的语句是用于组合语句的块语句。该块由一对大括号分隔：

```javascript
{
   statement_1;   statement_2;
   statement_3;
   ...
   statement_n;
}
```

重要：在ECMAScript 6标准之前，Javascript没有块作用域。如果你在块的外部声明了一个变量，然后在块中声明了一个相同变量名的变量，并赋予不同的值。那么在程序执行中将会使用块中的值，这样做虽然是合法的，但是这不同于JAVA与C。示例：

```javascript
var x = 1;
{
  var x = 2;
}
alert(x); // 输出的结果为 2
```

这段代码的输出是**2**，这是因为块级作用域中的 var x变量声明与之前的声明在同一个作用域内。在C语言或是Java语言中，同样的代码输出的结果是1。

从 ECMAScript 2015 开始，使用 `let` 定义的变量是块作用域的。 

##### let

`let` 语句声明一个块级作用域的本地变量，并且可选的将其初始化为一个值。 

**let**允许你声明一个作用域被限制在块级中的变量、语句或者表达式。与[var](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/var)关键字不同的是，它声明的变量只能是全局或者整个函数块的。 

###### 作用域规则

**let**声明的变量只在其声明的块或子块中可用，这一点，与**var**相似。二者之间最主要的区别在于**var**声明的变量的作用域是整个封闭函数。

```javascript
function varTest() {
  var x = 1;
  if (true) {
    var x = 2;  // 同样的变量!
    console.log(x);  // 2
  }
  console.log(x);  // 2
}

function letTest() {
  let x = 1;
  if (true) {
    let x = 2;  // 不同的变量
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```

###### 简化内部函数代码

当用到内部函数的时候，**let**会让你的代码更加简洁。

```javascript
var list = document.getElementById('list');

for (let i = 1; i <= 5; i++) {
  let item = document.createElement('li');
  item.appendChild(document.createTextNode('Item ' + i));

  item.onclick = function(ev) {
    console.log('Item ' + i + ' is clicked.');
  };
  list.appendChild(item);
}

// to achieve the same effect with 'var'
// you have to create a different context
// using a closure to preserve the value
for (var i = 1; i <= 5; i++) {
  var item = document.createElement('li');
  item.appendChild(document.createTextNode('Item ' + i));

  (function(i){
    item.onclick = function(ev) {
      console.log('Item ' + i + ' is clicked.');
    };
  })(i);
  list.appendChild(item);
}
```

以上示例的工作原理是因为（匿名）内部函数的五个实例引用了变量`i`的五个不同实例。注意，如果你将**let**替换为**var**，则它将无法正常工作，因为所有内部函数都将返回相同的`i`：6的最终值。此外，我们可以通过将创建新元素的代码移动到每个循环的作用域来保持循环更清晰。

在程序或者函数的顶层，**let**并不会像`**var**`一样在全局对象上创造一个属性，比如：

```javascript
var x = 'global';
let y = 'global';
console.log(this.x); // "global"
console.log(this.y); // undefined
```

###### 模仿私有接口

在处理[构造函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Classes/constructor)的时候，可以通过**let**绑定来共享一个或多个私有成员，而不使用[闭包](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)：

```javascript
var Thing;

{
  let privateScope = new WeakMap();
  let counter = 0;

  Thing = function() {
    this.someProperty = 'foo';
    
    privateScope.set(this, {
      hidden: ++counter,
    });
  };

  Thing.prototype.showPublic = function() {
    return this.someProperty;
  };

  Thing.prototype.showPrivate = function() {
    return privateScope.get(this).hidden;
  };
}

console.log(typeof privateScope);
// "undefined"

var thing = new Thing();

console.log(thing);
// Thing {someProperty: "foo"}

thing.showPublic();
// "foo"

thing.showPrivate();
// 1
```

###### `let`暂存死区的错误

在相同的函数或块作用域内重新声明同一个变量会引发[`SyntaxError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/SyntaxError)。

```javascript
if (x) {
  let foo;
  let foo; // TypeError thrown.
}
```

`**let**`在包含声明的作用域顶部被创建，通常这种被叫做“变量提升”。但和var不同的是，var的创建会设置一个初试的undefined值，let变量在没有运行到声明代码时是不会被初始化的。引用它将会导致 `ReferenceError`（而使用 var 声明变量则恰恰相反，该变量的值是 undefined ）。直到初始化执行的时候，该变量都处于从块开始到初始化处理的“暂存死区”。

```javascript
function do_something() {
  console.log(bar); // undefined
  console.log(foo); // ReferenceError: foo is not defined
  var bar = 1;
  let foo = 2;
}
```

在 [`switch`](https://developer.mozilla.org/zh-CN/docs/JavaScript/Reference/Statements/switch) 声明中你可能会遇到这样的错误，因为它只有一个块.

```javascript
switch (x) {
  case 0:
    let foo;
    break;
    
  case 1:
    let foo; // TypeError for redeclaration.
    break;
}
```

但是，重要的是要指出嵌套在case子句内的块将创建一个新的块作用域的词法环境，这不会产生上面显示的重新声明错误。

```javascript
let x = 1;

switch(x) {
  case 0: {
    let foo;
    break;
  }  
  case 1: {
    let foo;
    break;
  }
}
```

###### 与词法作用域结合的暂存死区

由于词法作用域，表达式`(foo + 55)`内的标识符“**foo**”会解析为if块的foo，而**不是**覆盖值为33的foo。在这一行中，if块的“foo”已经在词法环境中创建，但尚未达到（并终止）其初始化（这是语句本身的一部分）：它仍处于暂存死区。

```javascript
function test(){
   var foo = 33;
   if (true) {
      let foo = (foo + 55); // ReferenceError
   }
}
test();
```

这种现象可能会使您陷入以下情况。指令`let n of n.a`已经在for循环块的私有范围内，因此标识符“**n.a**”被解析为位于指令本身的第一部分（“let n”）中的'n'对象的属性'a' ，由于尚未达成和终止其声明，因此仍处于暂存死区。

```javascript
function go(n) {
  // n here is defined!
  console.log(n); // Object {a: [1,2,3]}

  for (let n of n.a) { // ReferenceError
    console.log(n);
  }
}

go({a: [1, 2, 3]});
```

#### 条件判断语句

##### `if...else` 语句

当一个逻辑条件为真，用if语句执行一个语句。当这个条件为假，使用可选择的 else 从句来执行这个语句。if 语句如下所示：

```javascript
if (condition) {
  statement_1;
}
[else {
  statement_2;
}] //推荐使用严格的语句块模式，语句else可选
```

条件可以是任何返回结果是 true 或 false的表达式。如果条件表达式返回的是 true，statement_1 会被执行；否则，statement_2 被执行。statement_1 和 statement_2 可以是任何语句，甚至你可以将另一个if语句嵌套其中。 

你也可以组合语句通过使用 else if 来测试连续的多种条件判断，就像下面这样:

```javascript
if (condition_1) {
  statement_1;
}
[else if (condition_2) {
  statement_2;
}]
...
[else if (condition_n_1) {
  statement_n_1;
}]
[else {
  statement_n;
}]
```

要执行多个语句，可以使用语句块({ ... }) 来分组这些语句。一般情况下，总是使用语句块是一个好的习惯，特别是在代码涉及比较多的 if 语句时:

```javascript
if (条件) {
  当条件为真的时候，执行语句1;
  当条件为真的时候，执行语句2;
} else {
  当条件为假的时候，执行语句3;
  当条件为假的时候，执行语句4;
}
```

不建议在条件表达式中使用赋值操作，因为在快速查阅代码时容易看成等值比较。 

如果你需要在条件表达式中使用赋值，通常在赋值语句前后额外添加一对括号。例如：

```javascript
if ((x = y)) {
  /* do the right thing */
}
```

**False 等效值**

下面这些值将被计算出 false (also known as [Falsy](https://developer.mozilla.org/en-US/docs/Glossary/Falsy) values):

- `false`
- `undefined`
- `null`
- `0`
- `NaN`
- 空字符串（`""`）

当传递给条件语句时，所有其他值，包括所有对象会被计算为真 。

请不要混淆原始的布尔值`true`和`false` 与 [`Boolean`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Boolean)对象的真和假。例如：

```javascript
var b = new Boolean(false);
if (b) // this condition evaluates to true
if (b == true) // this condition evaluates to false
```

##### `switch` 语句

`switch` 语句允许一个程序求一个表达式的值并且尝试去匹配表达式的值到一个 `case` 标签。如果匹配成功，这个程序执行相关的语句。`switch` 语句如下所示：

```
switch (expression) {
   case label_1:
      statements_1
      [break;]
   case label_2:
      statements_2
      [break;]
   ...
   default:
      statements_def
      [break;]
}
```

程序首先查找一个与 `expression `匹配的 `case `语句，然后将控制权转移到该子句，执行相关的语句。如果没有匹配值， 程序会去找 `default `语句，如果找到了，控制权转移到该子句，执行相关的语句。如果没有找到 `default`，程序会继续执行 `switch `语句后面的语句。`default` 语句通常出现在switch语句里的最后面，当然这不是必须的。

`可选的 break` 语句与每个 `case` 语句相关联， 保证在匹配的语句被执行后程序可以跳出 `switch `并且继续执行 `switch` 后面的语句。如果break被忽略，则程序将继续执行switch语句中的下一条语句。 

#### 异常处理语句

你可以用 `throw` 语句抛出一个异常并且用 `try...catch` 语句捕获处理它。

- [`throw` ](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Control_flow_and_error_handling$edit#throw_statement)语句
- [`try...catch` ](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Control_flow_and_error_handling$edit#try...catch_statement)语句

##### 异常类型

JavaScript 可以抛出任意对象。然而，不是所有对象能产生相同的结果。尽管抛出数值或者字母串作为错误信息十分常见，但是通常用下列其中一种异常类型来创建目标更为高效：

- [ECMAScript exceptions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error#Error_types)
- [`DOMException`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMException) and [`DOMError`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMError)

##### `throw` 语句

使用`throw`语句抛出一个异常。当你抛出异常，你规定一个含有值的表达式要被抛出。

```javascript
throw expression;
```

你可以抛出任意表达式而不是特定一种类型的表达式。下面的代码抛出了几个不同类型的表达式：

```javascript
throw "Error2";   // String type
throw 42;         // Number type
throw true;       // Boolean type
throw {toString: function() { return "I'm an object!"; } };
```

注意：你可以在抛出异常时声明一个对象。那你就可以在catch块中查询到对象的属性。 

```javascript
// Create an object type UserException
function UserException (message){
  this.message=message;
  this.name="UserException";
}

// Make the exception convert to a pretty string when used as
// a string (e.g. by the error console)
UserException.prototype.toString = function (){
  return this.name + ': "' + this.message + '"';
}

// Create an instance of the object type and throw it
throw new UserException("Value too high");
```

##### `try...catch` 语句

`try...catch` 语句标记一块待尝试的语句，并规定一个以上的响应应该有一个异常被抛出。如果我们抛出一个异常，`try...catch`语句就捕获它。

`try...catch` 语句有一个包含一条或者多条语句的try代码块，0个或多个的`catch`代码块，catch代码块中的语句会在try代码块中抛出异常时执行。 换句话说，如果你在try代码块中的代码如果没有执行成功，那么你希望将执行流程转入catch代码块。如果try代码块中的语句（或者`try` 代码块中调用的方法）一旦抛出了异常，那么执行流程会立即进入`catch` 代码块。如果try代码块没有抛出异常，catch代码块就会被跳过。`finally` 代码块总会紧跟在try和catch代码块之后执行，但会在try和catch代码块之后的其他代码之前执行。

下面的例子使用了`try...catch`语句。示例调用了一个函数用于从一个数组中根据传递值来获取一个月份名称。如果该值与月份数值不相符，会抛出一个带有`"InvalidMonthNo"`值的异常，然后在捕捉块语句中设`monthName`变量为`unknown`。

```javascript
function getMonthName(mo) {
  mo = mo - 1; // Adjust month number for array index (1 = Jan, 12 = Dec)
  var months = ["Jan","Feb","Mar","Apr","May","Jun","Jul",
                "Aug","Sep","Oct","Nov","Dec"];
  if (months[mo]) {
    return months[mo];
  } else {
    throw "InvalidMonthNo"; //throw keyword is used here
  }
}

try { // statements to try
  monthName = getMonthName(myMonth); // function could throw exception
}
catch (e) {
  monthName = "unknown";
  logMyErrors(e); // pass exception object to error handler -> your own function
}
```

###### `catch` 块

你可以使用`catch`块来处理所有可能在`try`块中产生的异常。

```javascript
catch (catchID) {
  statements
}
```

捕捉块指定了一个标识符（上述语句中的`catchID`）来存放抛出语句指定的值；你可以用这个标识符来获取抛出的异常信息。在插入`throw`块时JavaScript创建这个标识符；标识符只存在于`catch`块的存续期间里；当`catch`块执行完成时，标识符不再可用。

举个例子，下面代码抛出了一个异常。当异常出现时跳到`catch`块。

```javascript
try {
   throw "myException" // generates an exception
}
catch (e) {
// statements to handle any exceptions
   logMyErrors(e) // pass exception object to error handler
}
```

###### `finally`块

`finally`块包含了在try和catch块完成后、下面接着try...catch的语句之前执行的语句。`finally`块无论是否抛出异常都会执行。如果抛出了一个异常，就算没有异常处理，`finally`块里的语句也会执行。

你可以用`finally`块来令你的脚本在异常发生时优雅地退出；举个例子，你可能需要在绑定的脚本中释放资源。接下来的例子用文件处理语句打开了一个文件（服务端的JavaScript允许你进入文件）。如果在文件打开时一个异常抛出，`finally`块会在脚本错误之前关闭文件。

```javascript
openMyFile();
try {
    writeMyFile(theData); //This may throw a error
}catch(e){
    handleError(e); // If we got a error we handle it
}finally {
    closeMyFile(); // always close the resource
}
```

如果`finally`块返回一个值，该值会是整个`try-catch-finally`流程的返回值，不管在`try`和`catch`块中语句返回了什么：

```javascript
function f() {
  try {
    console.log(0);
    throw "bogus";
  } catch(e) {
    console.log(1);
    return true; // this return statement is suspended
                 // until finally block has completed
    console.log(2); // not reachable
  } finally {
    console.log(3);
    return false; // overwrites the previous "return"
    console.log(4); // not reachable
  }
  // "return false" is executed now  
  console.log(5); // not reachable
}
f(); // console 0, 1, 3; returns false
```

用`finally`块覆盖返回值也适用于在`catch`块内抛出或重新抛出的异常：

```javascript
function f() {
  try {
    throw 'bogus';
  } catch(e) {
    console.log('caught inner "bogus"');
    throw e; // this throw statement is suspended until 
             // finally block has completed
  } finally {
    return false; // overwrites the previous "throw"
  }
  // "return false" is executed now
}

try {
  f();
} catch(e) {
  // this is never reached because the throw inside
  // the catch is overwritten
  // by the return in finally
  console.log('caught outer "bogus"');
}

// OUTPUT
// caught inner "bogus"
```

###### 嵌套 try...catch 语句

你可以嵌套一个或多个`try ... catch`语句。如果一个内部`try ... catch`语句没有`catch`块，它需要有一个`finally`块，并且封闭的`try ... catch`语句的`catch`块被检查匹配。有关更多信息，请参阅[try... catch](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/try...catch)参考页上的[嵌套try-blocks](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch#Nested_try-blocks)。

##### 使用`Error`对象

根据错误类型，你也许可以用'name'和'message'获取更精炼的信息。'name'提供了常规的错误类（如 'DOMException' 或 'Error'），而'message'通常提供了一条从错误对象转换成字符串的简明信息。

在抛出你个人所为的异常时，为了充分利用那些属性（比如你的`catch`块不能分辨是你个人所为的异常还是系统的异常时），你可以使用 Error 构造函数。比如：

```javascript
function doSomethingErrorProne () {
  if (ourCodeMakesAMistake()) {
    throw (new Error('The message'));
  } else {
    doSomethingToGetAJavascriptError();
  }
}
....
try {
  doSomethingErrorProne();
}
catch (e) {
  console.log(e.name); // logs 'Error'
  console.log(e.message); // logs 'The message' or a JavaScript error message)
}
```

#### Promises

##### 语法

```javascript
new Promise( function(resolve, reject) {...} /* executor */  );
```

**executor**

executor是带有 `resolve` 和 `reject` 两个参数的函数 。Promise构造函数执行时立即调用`executor` 函数， `resolve` 和 `reject` 两个函数作为参数传递给`executor`（executor 函数在Promise构造函数返回新建对象前被调用）。`resolve` 和 `reject` 函数被调用时，分别将promise的状态改为*fulfilled（*完成）或rejected（失败）。executor 内部通常会执行一些异步操作，一旦完成，可以调用resolve函数来将promise状态改成*fulfilled*，或者在发生错误时将它的状态改为rejected。

如果在executor函数中抛出一个错误，那么该promise 状态为rejected。executor函数的返回值被忽略。

##### 描述

从 ECMAScript 6 开始，JavaScript 增加了对 [`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise) 对象的支持，它允许你对延时和异步操作流进行控制。

`Promise` 对象有以下几种状态：

- *pending：*初始的状态，即正在执行，不处于 fulfilled 或 rejected 状态。
- *fulfilled：*成功的完成了操作。
- *rejected：*失败，没有完成操作。
- *settled：*Promise 处于 fulfilled 或 rejected 二者中的任意一个状态, 不会是 pending。

![img](https://mdn.mozillademos.org/files/8633/promises.png)

##### 创建Promise

`Promise` 对象是由关键字 `new` 及其构造函数来创建的。该构造函数会?把一个叫做“处理器函数”（executor function）的函数作为它的参数。这个“处理器函数”接受两个函数——`resolve` 和 `reject` ——作为其参数。当异步任务顺利完成且返回结果值时，会调用 `resolve` 函数；而当异步任务失败且返回失败原因（通常是一个错误对象）时，会调用`reject` 函数。

```javascript
const myFirstPromise = new Promise((resolve, reject) => {
  // ?做一些异步操作，最终会调用下面两者之一:
  //
  //   resolve(someValue); // fulfilled
  // ?或
  //   reject("failure reason"); // rejected
});
```

想要某个函数?拥有promise功能，只需让其返回一个promise即可。

```javascript
function myAsyncFunction(url) {
  return new Promise((resolve, reject) => {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.onload = () => resolve(xhr.responseText);
    xhr.onerror = () => reject(xhr.statusText);
    xhr.send();
  });
};
```

##### 示例

###### 非常简单的例子

```javascript
let myFirstPromise = new Promise(function(resolve, reject){
    //当异步代码执行成功时，我们才会调用resolve(...), 当异步代码失败时就会调用reject(...)
    //在本例中，我们使用setTimeout(...)来模拟异步代码，实际编码时可能是XHR请求或是HTML5的一些API方法.
    setTimeout(function(){
        resolve("成功!"); //代码正常执行！
    }, 250);
});

myFirstPromise.then(function(successMessage){
    //successMessage的值是上面调用resolve(...)方法传入的值.
    //successMessage参数不一定非要是字符串类型，这里只是举个例子
    console.log("Yay! " + successMessage);
});
```

###### 高级一点的例子

本例展示了 `Promise` 的一些机制。 `testPromise()` 方法在每次点击 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/button) 按钮时被调用，该方法会创建一个promise 对象，使用 [`window.setTimeout()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/setTimeout) 让Promise等待 1-3 秒不等的时间来填充数据（通过Math.random()方法）。

Promise 的值的填充过程都被日志记录（logged）下来，这些日志信息展示了方法中的同步代码和异步代码是如何通过Promise完成解耦的。

```javascript
'use strict';
var promiseCount = 0;

function testPromise() {
    let thisPromiseCount = ++promiseCount;

    let log = document.getElementById('log');
    log.insertAdjacentHTML('beforeend', thisPromiseCount +
        ') 开始 (<small>同步代码开始</small>)<br/>');

    // 新构建一个 Promise 实例：使用Promise实现每过一段时间给计数器加一的过程，每段时间间隔为1~3秒不等
    let p1 = new Promise(
        // resolver 函数在 Promise 成功或失败时都可能被调用
       (resolve, reject) => {
            log.insertAdjacentHTML('beforeend', thisPromiseCount +
                ') Promise 开始 (<small>异步代码开始</small>)<br/>');
            // 创建一个异步调用
            window.setTimeout(
                function() {
                    // 填充 Promise
                    resolve(thisPromiseCount);
                }, Math.random() * 2000 + 1000);
        }
    );

    // Promise 不论成功或失败都会调用 then
    // catch() 只有当 promise 失败时才会调用
    p1.then(
        // 记录填充值
        function(val) {
            log.insertAdjacentHTML('beforeend', val +
                ') Promise 已填充完毕 (<small>异步代码结束</small>)<br/>');
        })
    .catch(
        // 记录失败原因
       (reason) => {
            console.log('处理失败的 promise ('+reason+')');
        });
        
        log.insertAdjacentHTML('beforeend', thisPromiseCount +
        ') Promise made (<small>同步代码结束</small>)<br/>');
}
```

 

##### 通过 XHR 加载图片

你可以在 MDN GitHub [promise-test](https://github.com/mdn/js-examples/tree/master/promises-test) 中找到这个简单的例子，它使用了 Promise 和 `XMLHttpRequest` 来加载一张图片，你也可以直接在[这个页面](https://mdn.github.io/js-examples/promises-test/)查看他的效果。同时为了让你更清楚的了解 Promise 和 XHR 的结构，代码中每一个步骤后都附带了注释。

这里有一个未注释的版本，展现了 `Promise` 的工作流，希望可以对你的理解有所帮助。

```javascript
function imgLoad(url) {
  return new Promise(function(resolve, reject) {
    var request = new XMLHttpRequest();
    request.open('GET', url);
    request.responseType = 'blob';
    request.onload = function() {
      if (request.status === 200) {
        resolve(request.response);
      } else {
        reject(Error('Image didn\'t load successfully; error code:' 
                     + request.statusText));
      }
    };
    request.onerror = function() {
      reject(Error('There was a network error.'));
    };
    request.send();
  });
}
```
## 循环和迭代

循环有很多种类，但本质上它们都做的是同一件事：它们把一个动作重复了很多次（实际上重复的次数有可能为0）。各种循环机制提供了不同的方法去确定循环的开始和结束。不同情况下，某一种类型循环会比其它的循环用起来更简单。 

#### `for` 语句

一个[`for循环`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for)会一直重复执行，直到指定的循环条件为fasle。 JavaScript的for循环和Java与C的for循环是很相似的。一个for语句是这个样子的：

```
for ([initialExpression]; [condition]; [incrementExpression])
  statement
```

当一个for循环执行的时候，会发生以下事件：

1. 如果有初始化表达式`initialExpression`，它将被执行。这个表达式通常会初始化一个或多个循环计数器，但语法上是允许一个任意复杂度的表达式的。这个表达式也可以声明变量。
2. 计算`condition`表达式的值。如果`condition的值是`true，循环中的statement会被执行。如果`condition`的值是false，for循环终止。如果`condition表达式整个都被省略掉了，`condition的值会被认为是true。
3. `循环中的statement被执行。如果需要执行多条语句，可以使用块` (`{ ... }`)`来包裹这些语句。`
4. 如果有更新表达式`incrementExpression`，执行它.
5. 然后流程回到步骤2。

#### `do...while` 语句

`do...while` 语句一直重复直到指定的条件求值得到假（false）。 一个 do...while 语句看起来像这样：

```
do
  statement
while (condition);
```

`statement` 在检查条件之间会执行一次。要执行多条语句（语句块），要使用块语句 ({ ... }) 包括起来。 如果 `condition` 为真（true），`statement` 将再次执行。 在每个执行的结尾会进行条件的检查。当 `condition` 为假（false），执行会停止并且把控制权交回给 do...while 后面的语句。

#### `while` 语句

一个 `while` 语句只要指定的条件求值为真（true）就会一直执行它的语句块。一个 while 语句看起来像这样：

```
while (condition)
  statement
```

如果这个条件变为假，循环里的 `statement` 将会停止执行并把控制权交回给 while 语句后面的代码。

条件检测会在每次 `statement` 执行之前发生。如果条件返回为真， `statement` 会被执行并紧接着再次测试条件。如果条件返回为假，执行将停止并把控制权交回给 while 后面的语句。

要执行多条语句（语句块），要使用块语句 ({ ... }) 包括起来。

#### `labeled` 语句

一个 [label](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/label) 提供了一个可以让你引用到您程序别的位置的标识符。例如，你可以用 label 标识一个循环， 然后使用 break 或者 continue 来指出程序是否该停止循环还是继续循环。

label 语句的语法看起来像这样：

```javascript
label :
   statement
```

`label` 的值可以是任何的非保留字的 JavaScript 标识符， `statement 可以是任意你想要标识的语句（块）。`

在这个例子里，标记 `markLoop` 标识了一个 while 循环。

```javascript
markLoop:
while (theMark == true) {
   doSomething();
}
```

#### `break` 语句

使用 `break` 语句来终止循环，`switch`， 或者是链接到 label 语句。

- 当你使用不带 label 的 `break` 时， 它会立即终止当前所在的 `while`，`do-while`，`for`，或者 `switch` 并把控制权交回这些结构后面的语句。
- 当你使用带 label 的 `break` 时，它会终止指定的标记（label）了的语句。

break 语句的语法看起来像这样：

1. `break;`
2. `break *label*;`

第一种形式的语法终止当前所在的循环或 switch； 第二种形式的语法终止指定的 label 语句。

#### `continue` 语句

这个 `continue` 语句可以用来继续执行（跳过代码块的剩余部分并进入下一循环）一个 while， do-while， for， 或者 label 语句。

- 当你使用不带 label 的 `continue` 时， 它终止当前 while，do-while，或者 for 语句到结尾的这次的循环并且继续执行下一次循环。
- 当你使用带 label 的 `continue` 时， 它会应用被 label 标识的循环语句。

`continue` 的语法看起来像这样：

1. `continue;`
2. `continue `*label;*

#### `for...in` 语句

这个 [`for...in`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/for...in) 语句循环一个指定的变量来循环一个对象所有可枚举的属性。JavaScript 会为每一个不同的属性执行指定的语句。

```javascript
for (variable in object) {
  statements
}
```

#### `for...of` 语句

`for...of语句在可迭代的对象上创建了一个循环`(包括[`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Array), [`Map`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Map), [`Set`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set), 参数对象（ [arguments](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/arguments)） 等等)，对值的每一个独特的属性调用一个将被执行的自定义的和语句挂钩的迭代。

```
for (variable of object) {
  statement
}
```

下面的这个例子展示了 `for...of 和` `for...in 两种循环语句之间的区别。与` `for...in` 循环遍历的结果是数组元素的下标不同的是， `for...of 遍历的结果是元素的值：`

```javascript
let arr = [3, 5, 7];
arr.foo = "hello";

for (let i in arr) {
   console.log(i); // logs "0", "1", "2", "foo"
}

for (let i of arr) {
   console.log(i); // logs "3", "5", "7" // 注意这里没有 hello
}
```