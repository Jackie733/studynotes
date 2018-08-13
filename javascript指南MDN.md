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
## 函数

函数是 JavaScript 中的基本组件之一。 一个函数是 JavaScript 过程 — 一组执行任务或计算值的语句。要使用一个函数，你必须将其定义在你希望调用它的作用域内。 

#### 定义函数

##### 函数声明

一个**函数定义**（也称为**函数声明**，或**函数语句**）由一系列的[`function`](https://developer.mozilla.org/zh-CN/docs/JavaScript/Reference/Statements/function)关键字组成，依次为：

- 函数的名称。
- 函数参数列表，包围在括号中并由逗号分隔。
- 定义函数的 JavaScript 语句，用大括号`{}`括起来。

例如，以下的代码定义了一个简单的`square`函数：

```javascript
function square(number) {
  return number * number;
}
```

函数`square`使用了一个参数，叫作`number`。这个函数只有一个语句，它说明该函数将函数的参数（即`number`）自乘后返回。函数的[`return`](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Statements/return)语句确定了函数的返回值。

原始参数（比如一个具体的数字）被作为**值**传递给函数；<u>值被传递给函数，如果被调用函数改变了这个参数的值，这样的改变不会影响到全局或调用函数。</u>

如果你传递一个对象（即一个非原始值，例如[`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Array)或用户自定义的对象）作为参数，而函数改变了这个对象的属性，这样的改变对函数外部是可见的，如下面的例子所示：

```javascript
function myFunc(theObject) {
  theObject.make = "Toyota";
}

var mycar = {make: "Honda", model: "Accord", year: 1998};
var x, y;

x = mycar.make;     // x获取的值为 "Honda"

myFunc(mycar);
y = mycar.make;     // y获取的值为 "Toyota"
                    // (make属性被函数改变了)
```

##### 函数表达式

虽然上面的函数声明在语法上是一个语句，但函数也可以由函数表达式创建。这样的函数可以是**匿名**的；它不必有一个名称。例如，函数`square`也可这样来定义：

```javascript
var square = function(number) { return number * number; };
var x = square(4); // x gets the value 16
```

然而，函数表达式也可以提供函数名，并且可以用于在函数内部代指其本身，或者在调试器堆栈跟踪中识别该函数：

```javascript
var factorial = function fac(n) {return n<2 ? 1 : n*fac(n-1)};

console.log(factorial(3));
```

当将函数作为参数传递给另一个函数时，函数表达式很方便。下面的例子演示了一个叫`map`的函数如何被定义，而后使用一个表达式函数作为其第一个参数进行调用：

```javascript
function map(f,a) {
  var result = [], // 创建一个新的数组
      i;

  for (i = 0; i != a.length; i++)
    result[i] = f(a[i]);
  return result;
}
```

下面的代码：

```javascript
function map(f, a) {
  var result = []; // 创建一个数组
  var i; // 声明一个值，用来循环
  for (i = 0; i != a.length; i++)
    result[i] = f(a[i]);
      return result;
}
var f = function(x) {
   return x * x * x; 
}
var numbers = [0,1, 2, 5,10];
var cube = map(f,numbers);
console.log(cube);
```

返回 [0, 1, 8, 125, 1000]。

在 JavaScript 中，可以根据条件来定义一个函数。比如下面的代码，当`num` 等于 0 的时候才会定义 `myFunc` ：

```javascript
var myFunc;
if (num == 0){
  myFunc = function(theObject) {
    theObject.make = "Toyota"
  }
}
```

除了上述的定义函数方法外，你也可以在运行时用 [`Function`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Function) 构造器由一个字符串来创建一个函数 ，很像 [`eval()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/eval()) 函数。

当一个函数是一个对象的属性时，称之为**方法**。

#### 调用函数

定义一个函数并不会自动的执行它。定义了函数仅仅是赋予函数以名称并明确函数被调用时该做些什么。**调用**函数才会以给定的参数真正执行这些动作。 

函数一定要处于调用它们的域中，但是函数的声明可以被提升(出现在调用语句之后)（函数提升仅适用于函数声明，而不适用于函数表达式），如下例：

```javascript
console.log(square(5));
/* ... */
function square(n) { return n*n }
```

函数域是指函数声明时的所在的地方，或者函数在顶层被声明时指整个程序。

函数的参数并不局限于字符串或数字。你也可以将整个对象传递给函数。 

函数可以被递归，就是说函数可以调用其本身。例如，下面这个函数就是用递归计算阶乘：

```javascript
function factorial(n){
  if ((n == 0) || (n == 1))
    return 1;
  else
    return (n * factorial(n - 1));
}
```
#### 函数作用域

在函数内定义的变量不能在函数之外的任何地方访问，因为变量仅仅在该函数的域的内部有定义。相对应的，一个函数可以访问定义在其范围内的任何变量和函数。换言之，定义在全局域中的函数可以访问所有定义在全局域中的变量。在另一个函数中定义的函数也可以访问在其父函数中定义的所有变量和父函数有权访问的任何其他变量。

```javascript
// 下面的变量定义在全局作用域(global scope)中
var num1 = 20,
    num2 = 3,
    name = "Chamahk";

// 本函数定义在全局作用域
function multiply() {
  return num1 * num2;
}

multiply(); // 返回 60

// 嵌套函数的例子
function getScore() {
  var num1 = 2,
      num2 = 3;
  
  function add() {
    return name + " scored " + (num1 + num2);
  }
  
  return add();
}

getScore(); // 返回 "Chamahk scored 5"
```

#### 作用域和函数堆栈

##### 递归

一个函数可以指向并调用自身。有三种方法可以达到这个目的：

1. 函数名
2. `arguments.callee`
3. 作用域下的一个指向该函数的变量名

例如，思考一下如下的函数定义：

```javascript
var foo = function bar() {
   // statements go here
};
```

在这个函数体内，以下的语句是等价的：

1. `bar()`
2. `arguments.callee()`
3. `foo()`

调用自身的函数我们称之为*递归函数*。在某种意义上说，递归近似于循环。两者都重复执行相同的代码，并且两者都需要一个终止条件（避免无限循环或者无限递归）。例如以下的循环：

```javascript
var x = 0;
while (x < 10) { // "x < 10" 是循环条件
   // do stuff
   x++;
}
```

可以被转化成一个递归函数和对其的调用：

```javascript
function loop(x) {
  if (x >= 10) // "x >= 10" 是退出条件（等同于 "!(x < 10)"）
    return;
  // 做些什么
  loop(x + 1); // 递归调用
}
loop(0);
```

不过，有些算法并不能简单的用迭代来实现。例如，获取树结构中所有的节点时，使用递归实现要容易得多：

```javascript
function walkTree(node) {
  if (node == null) // 
    return;
  // do something with node
  for (var i = 0; i < node.childNodes.length; i++) {
    walkTree(node.childNodes[i]);
  }
}
```

跟`loop`函数相比，这里每个递归调用都产生了更多的递归。

将递归算法转换为非递归算法是可能的，不过逻辑上通常会更加复杂，而且需要使用堆栈。事实上，递归函数就使用了堆栈：函数堆栈。

这种类似堆栈的行为可以在下例中看到：

```javascript
function foo(i) {
  if (i < 0)
    return;
  console.log('begin:' + i);
  foo(i - 1);
  console.log('end:' + i);
}
foo(3);

// 输出:

// begin:3
// begin:2
// begin:1
// begin:0
// end:0
// end:1
// end:2
// end:3
```

##### 嵌套函数和闭包

你可以在一个函数里面嵌套另外一个函数。嵌套（内部）函数对其容器（外部）函数是私有的。它自身也形成了一个闭包。一个闭包是一个可以自己拥有独立的环境与变量的的表达式（通常是函数）。

既然嵌套函数是一个闭包，就意味着一个嵌套函数可以”继承“容器函数的参数和变量。换句话说，内部函数包含外部函数的作用域。

可以总结如下：

- 内部函数只可以在外部函数中访问。
- 内部函数形成了一个闭包：它可以访问外部函数的参数和变量，但是外部函数却不能使用它的参数和变量。

下面的例子展示了嵌套函数：

```javascript
function addSquares(a, b) {
  function square(x) {
    return x * x;
  }
  return square(a) + square(b);
}
a = addSquares(2, 3); // returns 13
b = addSquares(3, 4); // returns 25
c = addSquares(4, 5); // returns 41
```

由于内部函数形成了闭包，因此你可以调用外部函数并为外部函数和内部函数指定参数：

```javascript
function outside(x) {
  function inside(y) {
    return x + y;
  }
  return inside;
}
fn_inside = outside(3); // Think of it like: give me a function that adds 3 to whatever you give it
result = fn_inside(5); // returns 8

result1 = outside(3)(5); // returns 8
```

##### 保存变量

注意到上例中 `inside` 被返回时 `x` 是怎么被保留下来的。一个闭包必须保存它可见作用域中所有参数和变量。因为每一次调用传入的参数都可能不同，每一次对外部函数的调用实际上重新创建了一遍这个闭包。只有当返回的 `inside` 没有再被引用时，内存才会被释放。

这与在其他对象中存储引用没什么不同，但是通常不太明显，因为并不能直接设置引用，也不能检查它们。

##### 多层嵌套函数

函数可以被多层嵌套。例如，函数A可以包含函数B，函数B可以再包含函数C。B和C都形成了闭包，所以B可以访问A，C可以访问B和A。因此，闭包可以包含多个作用域；他们递归式的包含了所有包含它的函数作用域。这个称之为作用*域链*。（稍后会详细解释）

思考一下下面的例子：

```javascript
function A(x) {
  function B(y) {
    function C(z) {
      console.log(x + y + z);
    }
    C(3);
  }
  B(2);
}
A(1); // logs 6 (1 + 2 + 3)
```

在这个例子里面，C可以访问B的y和A的x。这是因为：

1. B形成了一个包含A的闭包，B可以访问A的参数和变量
2. C形成了一个包含B的闭包
3. B包含A，所以C也包含A，C可以访问B和A的参数和变量。换言之，C用这个顺序链接了B和A的作用域

反过来却不是这样。A不能访问C，因为A看不到B中的参数和变量，C是B中的一个变量，所以C是B私有的。

##### 命名冲突

当同一个闭包作用域下两个参数或者变量同名时，就会产生命名冲突。更近的作用域有更高的优先权，所以最近的优先级最高，最远的优先级最低。这就是作用域链。链的第一个元素就是最里面的作用域，最后一个元素便是最外层的作用域。

看以下的例子：

```javascript
function outside() {
  var x = 5;
  function inside(x) {
    return x * 2;
  }
  return inside;
}

outside()(10); // returns 20 instead of 10
```

命名冲突发生在`return x`上，`inside`的参数`x`和`outside`变量`x`发生了冲突。这里的作用链域是{`inside`, `outside`, 全局对象}。因此`inside`的`x`具有最高优先权，返回了20（`inside`的`x`）而不是10（`outside`的`x`）。

#### 闭包

闭包是 JavaScript 中最强大的特性之一。JavaScript 允许函数嵌套，并且内部函数可以访问定义在外部函数中的所有变量和函数，以及外部函数能访问的所有变量和函数。但是，外部函数却不能够访问定义在内部函数中的变量和函数。这给内部函数的变量提供了一定的安全性。此外，由于内部函数可以访问外部函数的作用域，因此当内部函数生存周期大于外部函数时，外部函数中定义的变量和函数将的生存周期比内部函数执行时间长。当内部函数以某一种方式被任何一个外部函数作用域访问时，一个闭包就产生了。

```javascript
var pet = function(name) {          //外部函数定义了一个变量"name"
  var getName = function() {            
    //内部函数可以访问 外部函数定义的"name"
    return name; 
  }
  //返回这个内部函数，从而将其暴露在外部函数作用域
  return getName;               
};
myPet = pet("Vivie");
    
myPet();                            // 返回结果 "Vivie"
```

实际上可能会比上面的代码复杂的多。在下面这种情形中，返回了一个包含可以操作外部函数的内部变量方法的对象。

```javascript
var createPet = function(name) {
  var sex;
  
  return {
    setName: function(newName) {
      name = newName;
    },
    
    getName: function() {
      return name;
    },
    
    getSex: function() {
      return sex;
    },
    
    setSex: function(newSex) {
      if(typeof newSex == "string" 
        && (newSex.toLowerCase() == "male" || newSex.toLowerCase() == "female")) {
        sex = newSex;
      }
    }
  }
}

var pet = createPet("Vivie");
pet.getName();                  // Vivie

pet.setName("Oliver");
pet.setSex("male");
pet.getSex();                   // male
pet.getName();                  // Oliver
```

在上面的代码中，外部函数的`name`变量对内嵌函数来说是可取得的，而除了通过内嵌函数本身，没有其它任何方法可以取得内嵌的变量。内嵌函数的内嵌变量就像内嵌函数的保险柜。它们会为内嵌函数保留“稳定”——而又安全——的数据参与运行。而这些内嵌函数甚至不会被分配给一个变量，或者不必一定要有名字。

```javascript
var getCode = (function(){
  var secureCode = "0]Eal(eh&2";    // A code we do not want outsiders to be able to modify...
  
  return function () {
    return secureCode;
  };
})();

getCode();    // Returns the secret code
```

尽管有上述优点，使用闭包时仍然要小心避免一些陷阱。如果一个闭包的函数用外部函数的变量名定义了同样的变量，那在外部函数域将再也无法指向该变量。

```javascript
var createPet = function(name) {  // Outer function defines a variable called "name"
  return {
    setName: function(name) {    // Enclosed function also defines a variable called "name"
      name = name;               // ??? How do we access the "name" defined by the outer function ???
    }
  }
}
```

#### 使用 arguments 对象

函数的实际参数会被保存在一个类似数组的arguments对象中。在函数内，你可以按如下方式找出传入的参数：

```javascript
arguments[i]
```

其中`i`是参数的序数编号（译注：数组索引），以0开始。所以第一个传来的参数会是`arguments[0]`。参数的数量由`arguments.length`表示。

使用arguments对象，你可以处理比声明的更多的参数来调用函数。这在你事先不知道会需要将多少参数传递给函数时十分有用。你可以用`arguments.length`来获得实际传递给函数的参数的数量，然后用`arguments`对象来取得每个参数。

例如，设想有一个用来连接字符串的函数。唯一事先确定的参数是在连接后的字符串中用来分隔各个连接部分的字符（译注：比如例子里的分号“；”）。该函数定义如下：

```javascript
function myConcat(separator) {
   var result = ''; // 把值初始化成一个字符串，这样就可以用来保存字符串了！！
   var i;
   // iterate through arguments
   for (i = 1; i < arguments.length; i++) {
      result += arguments[i] + separator;
   }
   return result;
}
```

你可以给这个函数传递任意数量的参数，它会将各个参数连接成一个字符串“列表”：

```javascript
// returns "red, orange, blue, "
myConcat(", ", "red", "orange", "blue");

// returns "elephant; giraffe; lion; cheetah; "
myConcat("; ", "elephant", "giraffe", "lion", "cheetah");

// returns "sage. basil. oregano. pepper. parsley. "
myConcat(". ", "sage", "basil", "oregano", "pepper", "parsley");
```

**提示：**`arguments`变量只是 *”***类数组对象**“，并不是一个数组。称其为类数组对象是说它有一个索引编号和`length`属性。尽管如此，它并不拥有全部的Array对象的操作方法。

#### 函数参数

从ECMAScript 6开始，有两个新的类型的参数：默认参数，剩余参数。 

##### 默认参数

在JavaScript中，函数参数的默认值是`undefined`。然而，在某些情况下设置不同的默认值是有用的。这时默认参数可以提供帮助。

在过去，用于设定默认的一般策略是在函数的主体测试参数值是否为`undefined`，如果是则赋予一个值。如果在下面的例子中，调用函数时没有实参传递给`b`，那么它的值就是`undefined`，于是计算`a*b`得到、函数返回的是 `NaN`：

```javascript
function multiply(a, b) {
  b = (typeof b !== 'undefined') ?  b : 1;

  return a*b;
}

multiply(5); // 5
```

使用默认参数，在函数体的检查就不再需要了。现在，你可以在函数头简单地把1设定为`b`的默认值：

```javascript
function multiply(a, b = 1) {
  return a*b;
}

multiply(5); // 5
```

##### 剩余参数

[剩余参数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/rest_parameters)语法允许将不确定数量的参数表示为数组。在下面的例子中，使用剩余参数收集从第二个到最后参数。然后，我们将这个数组的每一个数与第一个参数相乘。这个例子是使用了一个箭头函数，这将在下一节介绍。

```javascript
function multiply(multiplier, ...theArgs) {
  return theArgs.map(x => multiplier * x);
}

var arr = multiply(2, 1, 2, 3);
console.log(arr); // [2, 4, 6]
```

#### 箭头函数

[箭头函数表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)（也称胖箭头函数）相比函数表达式具有较短的语法并以词法的方式绑定 `this`。箭头函数总是匿名的。另见 hacks.mozilla.org 的博文：“[深度了解ES6：箭头函数](https://hacks.mozilla.org/2015/06/es6-in-depth-arrow-functions/)”。

有两个因素会影响引入箭头函数：更简洁的函数和 `this`。

##### 更简洁的函数

有一些函数模式，更简洁的函数很受欢迎。对比一下：

```javascript
var a = [
  "Hydrogen",
  "Helium",
  "Lithium",
  "Beryllium"
];

var a2 = a.map(function(s){ return s.length });

console.log(a2); // logs [ 8, 6, 7, 9 ]

var a3 = a.map( s => s.length );

console.log(a3); // logs [ 8, 6, 7, 9 ]
```

### `this` 的词法

在箭头函数出现之前，每一个新函数都重新定义了自己的 [this](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this) 值（在严格模式下，一个新的对象在构造函数里是未定义的，以“对象方法”的方式调用的函数是上下文对象等）。以面向对象的编程风格，这样着实有点恼人。

```javascript
function Person() {
  // The Person() constructor defines `this` as itself.
  this.age = 0;

  setInterval(function growUp() {
    // In nonstrict mode, the growUp() function defines `this` 
    // as the global object, which is different from the `this`
    // defined by the Person() constructor.
    this.age++;
  }, 1000);
}

var p = new Person();
```

在ECMAScript 3/5里，通过把`this`的值赋值给一个变量可以修复这个问题。

```javascript
function Person() {
  var self = this; // Some choose `that` instead of `self`. 
                   // Choose one and be consistent.
  self.age = 0;

  setInterval(function growUp() {
    // The callback refers to the `self` variable of which
    // the value is the expected object.
    self.age++;
  }, 1000);
}
```

另外，创建一个[约束函数](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)可以使得 `this`值被正确传递给 `growUp()` 函数。

箭头功能捕捉闭包上下文的`this`值，所以下面的代码工作正常。

```javascript
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++; // |this| properly refers to the person object
  }, 1000);
}

var p = new Person();
```

#### 预定义函数

JavaScript语言有好些个顶级的内建函数：

- [`eval()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval)

  `**eval()**`方法会对一串字符串形式的JavaScript代码字符求值。

- [`uneval()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/uneval) 

  `**uneval()**`方法创建的一个[`Object`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)的源代码的字符串表示。

- [`isFinite()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isFinite)

  `**isFinite()**`函数判断传入的值是否是有限的数值。 如果需要的话，其参数首先被转换为一个数值。

- [`isNaN()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN)

  `**isNaN()**`函数判断一个值是否是[`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN)。注意：`isNaN`函数内部的`强制转换规则`十分有趣； 另一个可供选择的是ECMAScript 6 中定义[`Number.isNaN()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/isNaN) , 或者使用 `typeof`来判断数值类型。

- [`parseFloat()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseFloat)

  `**parseFloat()**` 函数解析字符串参数，并返回一个浮点数。

- [`parseInt()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt)

  `**parseInt()**` 函数解析字符串参数，并返回指定的基数（基础数学中的数制）的整数。

- [`decodeURI()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/decodeURI)

  `**decodeURI()**` 函数对先前经过[`encodeURI`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI)函数或者其他类似方法编码过的字符串进行解码。

- [`decodeURIComponent()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/decodeURIComponent)

  `**decodeURIComponent()**`方法对先前经过[`encodeURIComponent`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent)函数或者其他类似方法编码过的字符串进行解码。

- [`encodeURI()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURI)

  `**encodeURI()**`方法通过用以一个，两个，三个或四个转义序列表示字符的UTF-8编码替换统一资源标识符（URI）的某些字符来进行编码（每个字符对应四个转义序列，这四个序列组了两个”替代“字符）。

- [`encodeURIComponent()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent)

  `**encodeURIComponent()**` 方法通过用以一个，两个，三个或四个转义序列表示字符的UTF-8编码替换统一资源标识符（URI）的每个字符来进行编码（每个字符对应四个转义序列，这四个序列组了两个”替代“字符）。

- [`escape()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/escape) 已废弃的 `**unescape()**` 方法计算生成一个新的字符串，其中的十六进制转义序列将被其表示的字符替换。上述的转义序列就像[`escape`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/escape)里介绍的一样。因为 `unescape` 已经废弃，建议使用[`decodeURI()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/decodeURI)或者[`decodeURIComponent`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/decodeURIComponent) 替代本方法。

## ！！！表达式和运算符（待阅）

## 数字与日期

#### 数字

在 JavaScript 里面，数字均为双精度浮点类型[double-precision 64-bit binary format IEEE 754](https://en.wikipedia.org/wiki/Double-precision_floating-point_format) （也就是说一个数字的范围只能在 -(253 -1) 和 253 -1之间）。**整型数据也不例外。**除了能够表示浮点数，数字类型也还能表示三种符号值: `+`[`Infinity`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Infinity)（正无穷）、`-`[`Infinity`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Infinity)（负无穷）和 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN) (not-a-number非数字)。 

##### 十进制数字(Decimal numbers)

```javascript
1234567890
42

// 以零开头的数字的注意事项：

0888 // 888 将被当做十进制处理
0777 // 在非严格格式下会被当做八进制处理 (用十进制表示就是511)
```

请注意，十进制可以以0开头，后面接其他十进制数字，但是假如下一个接的十进制数字小于8，那么该数字将会被当做八进制处理。

##### 二进制数字(Binary numbers)

二进制数字语法是以零为开头，后面接一个小写或大写的拉丁文字母B(`0b或者是0B`)。 假如0b后面的数字不是0或者1，那么就会提示这样的语法错误（ `SyntaxError）：` "Missing binary digits after 0b(0b之后缺失二有效的二进制数据)"。

```javascript
var FLT_SIGNBIT  = 0b10000000000000000000000000000000; // 2147483648
var FLT_EXPONENT = 0b01111111100000000000000000000000; // 2139095040
var FLT_MANTISSA = 0B00000000011111111111111111111111; // 8388607
```

##### 八进制数字(Octal numbers)

八进制数字语法是以0为开头的。假如0后面的数字不在0到7的范围内，该数字将会被转换成十进制数字。

```javascript
var n = 0755; // 493
var m = 0644; // 420
```

在ECMAScript 5 严格模式下禁止使用八进制语法。八进制语法并不是ECMAScript 5规范的一部分，但是通过在八进制数字添加一个前缀0就可以被所有的浏览器支持：0644 === 420 而且 "\045" === "%"。在ECMAScript 6中使用八进制数字是需要给一个数字添加前缀"0o"。

```javascript
var a = 0o10; // ES6 :八进制
```

##### 十六进制(Hexadecimal numbers)

十六进制数字语法是以零为开头，后面接一个小写或大写的拉丁文字母X(`0x或者是0X`)。假如`0x`后面的数字超出规定范围(0123456789ABCDEF)，那么就会提示这样的语法错误(`SyntaxError)：`"Identifier starts immediately after numeric literal".

```javascript
0xFFFFFFFFFFFFFFFFF // 295147905179352830000
0x123456789ABCDEF   // 81985529216486900
0XA                 // 10
```

##### 指数形式(Exponentiation)

```javascript
1E3   // 1000
2e6   // 2000000
0.1e2 // 10
```

#### 数字对象

内置的[`Number`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number)对象有一些有关数字的常量属性，如最大值、不是一个数字和无穷大的。你不能改变这些属性，但可以按下边的方式使用它们：

```javascript
var biggestNum = Number.MAX_VALUE;
var smallestNum = Number.MIN_VALUE;
var infiniteNum = Number.POSITIVE_INFINITY;
var negInfiniteNum = Number.NEGATIVE_INFINITY;
var notANum = Number.NaN;
```

你永远只用从Number对象引用上边显示的属性，而不是你自己创建的Number对象的属性。

下面的表格汇总了数字对象的属性：

**数字的属性**

| 属性                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`Number.MAX_VALUE`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_VALUE) | 可表示的最大值                                               |
| [`Number.MIN_VALUE`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/MIN_VALUE) | 可表示的最小值                                               |
| [`Number.NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/NaN) | 特指”非数字“                                                 |
| [`Number.NEGATIVE_INFINITY`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/NEGATIVE_INFINITY) | 特指“负无穷”;在溢出时返回                                    |
| [`Number.POSITIVE_INFINITY`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/POSITIVE_INFINITY) | 特指“正无穷”;在溢出时返回                                    |
| [`Number.EPSILON`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/EPSILON) | 表示1和比最接近1且大于1的最小[`Number`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number)之间的差别 |
| [`Number.MIN_SAFE_INTEGER`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/MIN_SAFE_INTEGER) | JavaScript最小安全整数.                                      |
| [`Number.MAX_SAFE_INTEGER`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER) | JavaScript最大安全整数.                                      |

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`Number.parseFloat()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/parseFloat) | 把字符串参数解析成浮点数， 和全局方法 [`parseFloat()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseFloat) 作用一致. |
| [`Number.parseInt()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/parseInt) | 把字符串解析成特定基数对应的整型数字，和全局方法 [`parseInt()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/parseInt) 作用一致. |
| [`Number.isFinite()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/isFinite) | 判断传递的值是否为有限数字。                                 |
| [`Number.isInteger()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/isInteger) | 判断传递的值是否为整数。                                     |
| [`Number.isNaN()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/isNaN) | 判断传递的值是否为 [`NaN`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NaN). More robust version of the original global [`isNaN()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN). |
| [`Number.isSafeInteger()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/isSafeInteger) | 判断传递的值是否为安全整数。                                 |

数字的类型提供了不同格式的方法以从数字对象中检索信息。以下表格总结了 `数字类型原型上的方法。`

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`toExponential()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toExponential) | 返回一个数字的指数形式的字符串，形如：1.23e+2                |
| [`toFixed()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed) | 返回指定小数位数的表示形式，var a=123,b=a.toFixed(2)//b="123.00" |
| [`toPrecision()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Number/toPrecision) | 返回一个指定精度的数字。如下例子中，a=123中，3会由于精度限制消失var a=123,b=a.toPrecision(2)//b="1.2e+2" |

#### 数学对象（Math）

对于内置的[`Math`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math)数学常项和函数也有一些属性和方法。 比方说， `Math对象的` `PI` 属性会有属性值 pi (3.141...)，你可以像这样调用它：

```
Math.PI // π
```

同理，标准数学函数也是Math的方法。 这些包括三角函数，对数，指数，和其他函数。比方说你想使用三角函数 `sin`， 你可以这么写：

```
Math.sin(1.56)
```

需要注意的是Math的所有三角函数参数都是弧度制。

下面的表格总结了 `Math` 对象的方法。

Math的方法

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`abs()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/abs) | 绝对值                                                       |
| [`sin()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/sin), [`cos()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/cos), [`tan()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/tan) | 标准三角函数;参数为弧度                                      |
| [`asin()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/asin), [`acos()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/acos), [`atan()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/atan), [`atan2()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/atan2) | 反三角函数; 返回值为弧度                                     |
| [`sinh()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/sinh), [`cosh()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/cosh), [`tanh()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/tanh) | 双曲三角函数; 返回值为弧度.                                  |
| [`asinh()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/asinh), [`acosh()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/acosh), [`atanh()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/atanh) | 反双曲三角函数;返回值为弧度.                                 |
| [`pow()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/pow), [`exp()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/exp), [`expm1()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/expm1), [`log10()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/log10), [`log1p()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/log1p), [`log2()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/log2) | 指数与对数函数                                               |
| [`floor()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/floor), [`ceil()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/ceil) | 返回最大/最小整数小于/大于或等于参数                         |
| [`min()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/min), [`max()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/max) | 返回一个以逗号间隔的数字参数列表中的较小或较大值(分别地)     |
| [`random()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/random) | 返回0和1之间的随机数。                                       |
| [`round()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/round), [`fround()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/fround), [`trunc()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/trunc), | 四舍五入和截断函数                                           |
| [`sqrt()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/sqrt), [`cbrt()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/cbrt), [`hypot()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/hypot) | 平方根，立方根，平方参数的和的平方根 两个参数平方和的平方根  |
| [`sign()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/sign) | 数字的符号, 说明数字是否为正、负、零。                       |
| [`clz32()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/clz32), [`imul()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math/imul) | 在32位2进制表示中，开头的0的数量.*返回传入的两个参数相乘结果的类C的32位表现形式* |

和其他对象不同，你不能够创建一个自己的Math对象。你只能使用内置的Math对象。

#### 日期对象

JavaScript没有日期数据类型。但是你可以在你的程序里使用 [`Date`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Date) 对象和其方法来处理日期和时间。Date对象有大量的设置、获取和操作日期的方法。 它并不含有任何属性。

JavaScript 处理日期数据类似于Java。这两种语言有许多一样的处理日期的方法，也都是以**1970年1月1日00:00:00**以来的毫秒数来储存数据类型的。

`Date` 对象的范围是相对距离 UTC 1970年1月1日 的前后 100,000,000 天。

创建一个日期对象：

```javascript
var dateObjectName = new Date([parameters]);
```

这里的 dateObjectName 对象是所创建的Date对象的一个名字，它可以成为一个新的对象或者已存在的其他对象的一个属性。

不使用 *new* 关键字来调用Date对象将返回当前时间和日期的字符串

前边的语法中的参数（parameters）可以是一下任何一种：

- 无参数 : 创建今天的日期和时间，例如： `today = new Date();`.
- 一个符合以下格式的表示日期的字符串: "月 日, 年 时:分:秒." 例如： `var Xmas95 = new Date("December 25, 1995 13:30:00")。`如果你省略时、分、秒，那么他们的值将被设置为0。
- 一个年，月，日的整型值的集合，例如： `var Xmas95 = new Date(1995, 11, 25)。`
- 一个年，月，日，时，分，秒的集合，例如： `var Xmas95 = new Date(1995, 11, 25, 9, 30, 0);`.

##### Date对象的方法

处理日期时间的Date对象方法可分为以下几类：

- "set" 方法, 用于设置Date对象的日期和时间的值。
- "get" 方法,用于获取Date对象的日期和时间的值。
- "to" 方法,用于返回Date对象的字符串格式的值。
- parse 和UTC 方法, 用于解析Date字符串。

通过“get”和“set”方法，你可以分别设置和获取秒，分，时，日，星期，月份，年。这里有个getDay方法可以返回星期，但是没有相应的setDay方法用来设置星期，因为星期是自动设置的。这些方法用整数来代表以下这些值：

- 秒，分： 0 至 59
- 时： 0 至 23
- 星期： 0 (周日) 至 6 (周六)
- 日期：1 至 31 
- 月份： 0 (一月) to 11 (十二月)
- 年份： 从1900开始的年数

例如, 假设你定义了如下日期：

```javascript
var Xmas95 = new Date("December 25, 1995");
```

Then `Xmas95.getMonth()` 返回 11, and `Xmas95.getFullYear()` 返回 1995.

`getTime` 和 `setTime` 方法对于比较日期是非常有用的。`getTime`方法返回从1970年1月1日00:00:00的毫秒数。

例如，以下代码展示了今年剩下的天数：

```javascript
var today = new Date();
var endYear = new Date(1995, 11, 31, 23, 59, 59, 999); // 设置日和月，注意，月份是0-11
endYear.setFullYear(today.getFullYear()); // 把年设置为今年
var msPerDay = 24 * 60 * 60 * 1000; // 每天的毫秒数
var daysLeft = (endYear.getTime() - today.getTime()) / msPerDay;
var daysLeft = Math.round(daysLeft); //返回今年剩下的天数
```

这个例子中，创建了一个包含今天的日期的Date对象，并命名为today，然后创建了一个名为endYear的Date对象，并把年份设置为当前年份，接着使用每天的毫秒数和getTime分别获取今天和年底的毫秒数，计算出了今天到年底的天数，最后四舍五入得到今年剩下的天数。

parse方法对于从日期字符串赋值给现有的Date对象很有用，例如：以下代码使用parse和setTime分配了一个日期值给IPOdate对象：

```javascript
var IPOdate = new Date();
IPOdate.setTime(Date.parse("Aug 9, 1995"));
```

## 文本格式化

JavaScript中的 [String](https://developer.mozilla.org/en-US/docs/Glossary/String) 类型用于表示文本型的数据. 它是由无符号整数值（16bit）作为元素而组成的集合. 字符串中的每个元素在字符串中占据一个位置. 第一个元素的index值是0, 下一个元素的index值是1, 以此类推. 字符串的长度就是字符串中所含的元素个数.你可以通过**String字面值**或者**String对象**两种方式创建一个字符串。 

#### String字面量

可以使用单引号或双引号创建简单的字符串:

```javascript
'foo'
"bar"
```

可以使用转义序列来创建更复杂的字符串:

##### 16进制转义序列

\x之后的数值将被认为是一个16进制数.

```javascript
'\xA9' // "©"
```

##### Unicode转义序列

Unicode转义序列在\u之后需要至少4个字符.

```javascript
'\u00A9' // "©"
```

##### Unicode字元逸出

这是ECMAScript 6中的新特性。有了Unicode字元逸出，任何字符都可以用16进制数转义, 这使得通过Unicode转义表示大于`0x10FFFF`的字符成为可能。使用简单的Unicode转义时通常需要分别写字符相应的两个部分（译注：大于0x10FFFF的字符需要拆分为相应的两个小于0x10FFFF的部分）来达到同样的效果。

请参阅 [`String.fromCodePoint()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/fromCodePoint) 或 [`String.prototype.codePointAt()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/codePointAt)。

```javascript
'\u{2F804}'

// the same with simple Unicode escapes
'\uD87E\uDC04'
```

#### 字符串对象

[`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/String) 对象是对原始string类型的**封装** .

```javascript
var s = new String("foo"); // Creates a String object
console.log(s); // Displays: { '0': 'f', '1': 'o', '2': 'o'}
typeof s; // Returns 'object'
```

你可以在String字面值上使用String对象的任何方法—JavaScript自动把String字面值转换为一个临时的String对象, 然后调用其相应方法,最后丢弃此临时对象.在String字面值上也可以使用String.length属性.

除非必要, 应该尽量使用String字面值, 因为String对象的某些行为可能并不与直觉一致. 举例:

```javascript
var s1 = "2 + 2"; // Creates a string literal value
var s2 = new String("2 + 2"); // Creates a String object
eval(s1); // Returns the number 4
eval(s2); // Returns the string "2 + 2"
```

`String对象有一个属性`, `length`, 标识了字符串中的字符个数.举例, 下面的代码把13赋值给了`x`, 因为"Hello, World!"包含了13个字符（译注：注意W之前有个空格）:

```javascript
var mystring = "Hello, World!";
var x = mystring.length;
```

`String`对象有许多方法: 举例来说有些方法返回字符串本身的变体, 如 `substring` 和`toUpperCase`.

下表总结了 [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/String) 对象的方法.

| 方法                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`charAt`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/charAt), [`charCodeAt`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/charCodeAt), [`codePointAt`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/codePointAt) | 返回字符串指定位置的字符或者字符编码。                       |
| [`indexOf`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/indexOf), [`lastIndexOf`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/lastIndexOf) | 分别返回字符串中指定子串的位置或最后位置。                   |
| [`startsWith`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/startsWith), [`endsWith`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/endsWith), [`includes`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/includes) | 返回字符串是否以指定字符串开始、结束或包含指定字符串。       |
| [`concat`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/concat) | 连接两个字符串并返回新的字符串。                             |
| [`fromCharCode`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/fromCharCode), [`fromCodePoint`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/fromCodePoint) | 从指定的Unicode值序列构造一个字符串。这是一个String类方法，不是实例方法。 |
| [`split`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split) | 通过将字符串分离成一个个子串来把一个String对象分裂到一个字符串数组中。 |
| [`slice`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/slice) | 从一个字符串提取片段并作为新字符串返回。                     |
| [`substring`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substring), [`substr`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/substr) | 分别通过指定起始和结束位置，起始位置和长度来返回字符串的指定子集。 |
| [`match`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match), [`replace`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace), [`search`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/search) | 通过正则表达式来工作.                                        |
| [`toLowerCase`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase), [`toUpperCase`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/toUpperCase) | 分别返回字符串的小写表示和大写表示。                         |
| [`normalize`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/normalize) | 按照指定的一种 Unicode 正规形式将当前字符串正规化。          |
| [`repeat`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/repeat) | 将字符串内容重复指定次数后返回。                             |
| [`trim`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/trim) | 去掉字符串开头和结尾的空白字符。                             |

#### 多行模板字符串

模板字符串是一种允许内嵌表达式的String字面值. 可以用它实现多行字符串或者字符串内插等特性.

模板字符串使用反勾号 (\` \`) 包裹内容而不是单引号或双引号. 模板字符串可以包含占位符. 占位符用美元符号和花括号标识 (`${expression}`).

##### 多行

源代码中插入的任何新行开始字符都作为模板字符串的内容. 使用一般的字符串时, 为了创建多行的字符串不得不用如下语法:

```javascript
console.log("string text line 1\n\
string text line 2");
// "string text line 1
// string text line 2"
```

为了实现同样效果的多行字符串, 现在可以写成如下形式:

```javascript
console.log(`string text line 1
string text line 2`);
// "string text line 1
// string text line 2"
```

##### 嵌入表达式

为了在一般的字符串中嵌入表达式, 需要使用如下语法:

```javascript
var a = 5;
var b = 10;
console.log("Fifteen is " + (a + b) + " and\nnot " + (2 * a + b) + ".");
// "Fifteen is 15 and
// not 20."
```

现在, 使用模板字符串, 可以使用语法糖让类似功能的实现代码更具可读性:

```javascript
var a = 5;
var b = 10;
console.log(`Fifteen is ${a + b} and\nnot ${2 * a + b}.`);
// "Fifteen is 15 and
// not 20."
```

#### 国际化

Intl对象是ECMAScript国际化API的命名空间, 它提供了语言敏感的字符串比较，数字格式化和日期时间格式化功能.  [`Collator`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Collator), [`NumberFormat`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NumberFormat), 和 [`DateTimeFormat`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/DateTimeFormat) 对象的构造函数是`Intl`对象的属性. 

##### 日期和时间格式化

[`DateTimeFormat`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/DateTimeFormat) 对象在日期和时间的格式化方面很有用. 下面的代码把一个日期格式化为美式英语格式. (不同时区结果不同.)

```javascript
var msPerDay = 24 * 60 * 60 * 1000;
 
// July 17, 2014 00:00:00 UTC.
var july172014 = new Date(msPerDay * (44 * 365 + 11 + 197));//2014-1970=44年
//这样创建日期真是醉人。。。还要自己计算天数。。。11是闰年中多出的天数。。。
//197是6×30+16(7月的16天)+3(3个大月)-2(2月少2天)

var options = { year: "2-digit", month: "2-digit", day: "2-digit",
                hour: "2-digit", minute: "2-digit", timeZoneName: "short" };
var americanDateTime = new Intl.DateTimeFormat("en-US", options).format;
 
console.log(americanDateTime(july172014)); // 07/16/14, 5:00 PM PDT
```

##### 数字格式化

[`NumberFormat`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/NumberFormat) 对象在数字的格式化方面很有用, 比如货币数量值.

```javascript
var gasPrice = new Intl.NumberFormat("en-US",
                        { style: "currency", currency: "USD",
                          minimumFractionDigits: 3 });
 
console.log(gasPrice.format(5.259)); // $5.259

var hanDecimalRMBInChina = new Intl.NumberFormat("zh-CN-u-nu-hanidec",
                        { style: "currency", currency: "CNY" });
 
console.log(hanDecimalRMBInChina.format(1314.25)); // ￥ 一,三一四.二五
```

##### 定序

[`Collator`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Collator) 对象在字符串比较和排序方面很有用.

举例, 德语中*有两种不同的排序方式 电话本（phonebook）* 和 字典（*dictionary）*. 电话本排序强调发音, 比如在排序前 “ä”, “ö”等被扩展为 “ae”, “oe”等发音.

```javascript
var names = ["Hochberg", "Hönigswald", "Holzman"];
 
var germanPhonebook = new Intl.Collator("de-DE-u-co-phonebk");
 
// as if sorting ["Hochberg", "Hoenigswald", "Holzman"]:
console.log(names.sort(germanPhonebook.compare).join(", "));
// logs "Hochberg, Hönigswald, Holzman"
```

有些德语词包含变音, 所以在字典中忽略变音进行排序是合理的 (除非待排序的单词只有变音部分不同: *schon* 先于 *schön*).

```javascript
var germanDictionary = new Intl.Collator("de-DE-u-co-dict");
 
// as if sorting ["Hochberg", "Honigswald", "Holzman"]:
console.log(names.sort(germanDictionary.compare).join(", "));
// logs "Hochberg, Holzman, Hönigswald"
```

关于[`Intl`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Intl) API的更多信息, 请参考 [Introducing the JavaScript Internationalization API](https://hacks.mozilla.org/2014/12/introducing-the-javascript-internationalization-api/)。

## 正则表达式

正则表达式是用于匹配字符串中字符组合的模式。在 JavaScript中，正则表达式也是对象。这些模式被用于 [`RegExp`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/RegExp) 的 [`exec`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec) 和 [`test`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/test) 方法, 以及 [`String`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/String) 的 [`match`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/match)、[`replace`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)、[`search`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/search) 和 [`split`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split) 方法。本章介绍 JavaScript正则表达式。 

#### 创建一个正则表达式

你可以使用以下两种方法之一构建一个正则表达式：

使用一个**正则表达式字面量**，其由包含在斜杠之间的模式组成，如下所示：

```javascript
/*
   /pattern/flags
*/

const regex = /ab+c/;

const regex = /^[a-zA-Z]+[0-9]*\W?_$/gi;
```

在加载脚本后，正则表达式字面值提供正则表达式的编译。<u>当正则表达式保持不变时，使用此方法可获得更好的性能。</u>

或者调用**`RegExp`对象的构造函数**，如下所示：

```javascript
/* 
    new RegExp(pattern [, flags])
*/

let regex = new RegExp("ab+c");

let regex = new RegExp(/^[a-zA-Z]+[0-9]*\W?_$/, "gi");

let regex = new RegExp("^[a-zA-Z]+[0-9]*\\W?_$", "gi");
```

使用构造函数提供正则表达式的运行时编译。<u>使用构造函数，当你知道正则表达式模式将会改变，或者你不知道模式，并从另一个来源，如用户输入。</u>

#### 编写一个正则表达式的模式

一个正则表达式模式是由简单的字符所构成的，比如`/abc/`, 或者是简单和特殊字符的组合，比如 `/ab*c/` 或 `/Chapter (\d+)\.\d*/。后者`用到了括号，它在正则表达式中可以被用作是一个记忆设备。这一部分正则所匹配的字符将会被记住，在后面可以被利用。正如 [使用括号的子字符串匹配](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#%E4%BD%BF%E7%94%A8%E6%8B%AC%E5%8F%B7%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%8C%B9%E9%85%8D) 

##### 使用简单的模式

简单的模式是由你找到的直接匹配所构成的。比如，`/abc/`这个模式就匹配了在一个字符串中，仅仅字符 'abc' 同时出现并按照这个顺序。在 "Hi, do you know your abc's?" 和 "The latest airplane designs evolved from slabcraft." 就会匹配成功。在上面的两个实例中，匹配的是子字符串 'abc'。在字符串 "Grab crab" 中将不会被匹配，因为它不包含任何的 'abc' 子字符串。

##### 使用特殊字符

当你需要搜索一个比直接匹配需要更多条件的匹配时，比如寻找一个或多个 'b'，或者寻找空格，那么这时模式将要包含特殊字符。比如， 模式`/ab*c/`匹配了一个单独的 'a' 后面跟了零个或者多个 'b'（*的意思是前面一项出现了零个或者多个），且后面跟着 'c' 的任何字符组合。在字符串 "cbbabbbbcdebc" 中，这个模式匹配了子字符串 "abbbbc"。

下面的表格列出了一个我们在正则表达式中可以利用的特殊字符的完整列表和描述。

| 字符                                                         | 含义                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`\`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-backslash) | 匹配将依照下列规则：在非特殊字符之前的反斜杠表示下一个字符是特殊的，不能从字面上解释。例如，没有前面'\'的'b'通常匹配小写'b'，无论它们出现在哪里。如果加了'\',这个字符变成了一个特殊意义的字符，意思是匹配一个[字符边界](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#note)。反斜杠也可以将其后的特殊字符，转义为字面量。例如，模式 /a*/ 代表会匹配 0 个或者多个 a。相反，模式 /a\*/ 将 '*' 的特殊性移除，从而可以匹配像 "a*" 这样的字符串。使用 new RegExp("pattern") 的时候不要忘记将 \ 进行转义，因为 \ 在字符串里面也是一个转义字符。 |
| [`^`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-caret) | 匹配输入的开始。如果多行标志被设置为true，那么也匹配换行符后紧跟的位置。例如，/^A/ 并不会匹配 "an A" 中的 'A'，但是会匹配 "An E" 中的 'A'。当 '^' 作为第一个字符出现在一个字符集合模式时，它将会有不同的含义。[补充字符集合](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#note) 一节有详细介绍和示例。 |
| [`$`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-dollar) | 匹配输入的结束。如果多行标示被设置为true，那么也匹配换行符前的位置。例如，/t$/ 并不会匹配 "eater" 中的 't'，但是会匹配 "eat" 中的 't'。 |
| [`*`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-asterisk) | 匹配前一个表达式0次或多次。等价于 {0,}。例如，/bo*/会匹配 "A ghost boooooed" 中的 'booooo' 和 "A bird warbled" 中的 'b'，但是在 "A goat grunted" 中将不会匹配任何东西。 |
| [`+`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-plus) | 匹配前面一个表达式1次或者多次。等价于 {1,}。例如，/a+/匹配了在 "candy" 中的 'a'，和在 "caaaaaaandy" 中所有的 'a'。 |
| [`?`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-questionmark) | 匹配前面一个表达式0次或者1次。等价于 {0,1}。例如，/e?le?/ 匹配 "angel" 中的 'el'，和 "angle" 中的 'le' 以及"oslo' 中的'l'。如果**紧跟在任何量词 \*、 +、? 或 {} 的后面**，将会使量词变为**非贪婪**的（匹配尽量少的字符），和缺省使用的**贪婪模式**（匹配尽可能多的字符）正好相反。例如，对 "123abc" 应用 /\d+/ 将会返回 "123"，如果使用 /\d+?/,那么就只会匹配到 "1"。还可以运用于先行断言，如本表的 `x(?=y)` 和 `x(?!y)` 条目中所述。 |
| [`.`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-dot) | （小数点）匹配除换行符之外的任何单个字符。例如，/.n/将会匹配 "nay, an apple is on the tree" 中的 'an' 和 'on'，但是不会匹配 'nay'。 |
| [`(x)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-capturing-parentheses) | 匹配 'x' 并且记住匹配项，就像下面的例子展示的那样。括号被称为 *捕获括号*。模式`/(foo) (bar) \1 \2/`中的 '(foo)' 和 '(bar)' 匹配并记住字符串 "foo bar foo bar" 中前两个单词。模式中的 \1 和 \2 匹配字符串的后两个单词。注意 \1、\2、\n 是用在正则表达式的匹配环节。在正则表达式的替换环节，则要使用像 $1、$2、$n 这样的语法，例如，'bar foo'.replace( /(...) (...)/, '$2 $1' )。 |
| [`(?:x)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-non-capturing-parentheses) | 匹配 'x' 但是不记住匹配项。这种叫作非捕获括号，使得你能够定义为与正则表达式运算符一起使用的子表达式。来看示例表达式 /(?:foo){1,2}/。如果表达式是 /foo{1,2}/，{1,2}将只对 ‘foo’ 的最后一个字符 ’o‘ 生效。如果使用非捕获括号，则{1,2}会匹配整个 ‘foo’ 单词。 |
| [`x(?=y)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-lookahead) | 匹配'x'仅仅当'x'后面跟着'y'.这种叫做正向肯定查找。例如，/Jack(?=Sprat)/会匹配到'Jack'仅仅当它后面跟着'Sprat'。/Jack(?=Sprat\|Frost)/匹配‘Jack’仅仅当它后面跟着'Sprat'或者是‘Frost’。但是‘Sprat’和‘Frost’都不是匹配结果的一部分。 |
| [`x(?!y)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-negated-look-ahead) | 匹配'x'仅仅当'x'后面不跟着'y',这个叫做正向否定查找。例如，/\d+(?!\.)/匹配一个数字仅仅当这个数字后面没有跟小数点的时候。正则表达式/\d+(?!\.)/.exec("3.141")匹配‘141’但是不是‘3.141’ |
| [`x|y`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-or) | 匹配‘x’或者‘y’。例如，/green\|red/匹配“green apple”中的‘green’和“red apple”中的‘red’ |
| [`{n}`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-quantifier) | n是一个正整数，匹配了前面一个字符刚好发生了n次。比如，/a{2}/不会匹配“candy”中的'a',但是会匹配“caandy”中所有的a，以及“caaandy”中的前两个'a'。 |
| [`{n,m}`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-quantifier-range) | n 和 m 都是整数。匹配前面的字符至少n次，最多m次。如果 n 或者 m 的值是0， 这个值被忽略。例如，/a{1, 3}/ 并不匹配“cndy”中的任意字符，匹配“candy”中的a，匹配“caandy”中的前两个a，也匹配“caaaaaaandy”中的前三个a。注意，当匹配”caaaaaaandy“时，匹配的值是“aaa”，即使原始的字符串中有更多的a。 |
| `[xyz\]`                                                     | 一个字符集合。匹配方括号的中任意字符，包括[转义序列](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Grammar_and_types)。你可以使用破折号（-）来指定一个字符范围。对于点（.）和星号（*）这样的特殊符号在一个字符集中没有特殊的意义。他们不必进行转义，不过转义也是起作用的。例如，[abcd] 和[a-d]是一样的。他们都匹配"brisket"中得‘b’,也都匹配“city”中的‘c’。/[a-z.]+/ 和/[\w.]+/都匹配“test.i.ng”中的所有字符。 |
| `[^xyz\]`                                                    | 一个反向字符集。也就是说， 它匹配任何没有包含在方括号中的字符。你可以使用破折号（-）来指定一个字符范围。任何普通字符在这里都是起作用的。例如，[^abc] 和 [^a-c] 是一样的。他们匹配"brisket"中的‘r’，也匹配“chop”中的‘h’。 |
| `[\b\]`                                                      | 匹配一个退格(U+0008)。（不要和\b混淆了。）                   |
| [`\b`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-word-boundary) | 匹配一个词的边界。一个词的边界就是一个词不被另外一个“字”字符跟随的位置或者没有其他“字”字符在其前面的位置。注意，一个匹配的词的边界并不包含在匹配的内容中。换句话说，一个匹配的词的边界的内容的长度是0。（不要和[\b]混淆了）例子：/\bm/匹配“moon”中得‘m’；/oo\b/并不匹配"moon"中得'oo'，因为'oo'被一个“字”字符'n'紧跟着。/oon\b/匹配"moon"中得'oon'，因为'oon'是这个字符串的结束部分。这样他没有被一个“字”字符紧跟着。/\w\b\w/将不能匹配任何字符串，因为在一个单词中间的字符永远也不可能同时满足没有“字”字符跟随和有“字”字符跟随两种情况。**注意:** JavaScript的正则表达式引擎将[特定的字符集](http://www.ecma-international.org/ecma-262/5.1/#sec-15.10.2.6)定义为“字”字符。不在该集合中的任何字符都被认为是一个断词。这组字符相当有限：它只包括大写和小写的罗马字母，十进制数字和下划线字符。不幸的是，重要的字符，例如“é”或“ü”，被视为断词。 |
| [`\B`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-non-word-boundary) | 匹配一个非单词边界。他匹配一个前后字符都是相同类型的位置：都是“字”字符或者都不是“字”字符。一个字符串的开始和结尾都被认为不是“字”字符，或者空字符串。例如，/\B../匹配"noonday"中的'oo', 而/y\B../匹配"possibly yesterday"中的’ye‘ |
| [`\c*X*`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-control) | 当X是处于A到Z之间的字符的时候，匹配字符串中的一个控制符。例如，`/\cM/` 匹配字符串中的 control-M (U+000D)。 |
| [`\d`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-digit) | 匹配一个数字`。``等价于[0-9]`。例如， `/\d/` 或者 `/[0-9]/` 匹配"B2 is the suite number."中的'2'。 |
| [`\D`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-non-digit) | 匹配一个非数字字符`。``等价于[^0-9]`。例如， `/\D/` 或者 `/[^0-9]/` 匹配"B2 is the suite number."中的'B' 。 |
| [`\f`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-form-feed) | 匹配一个换页符 (U+000C)。                                    |
| [`\n`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-line-feed) | 匹配一个换行符 (U+000A)。                                    |
| [`\r`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-carriage-return) | 匹配一个回车符 (U+000D)。                                    |
| [`\s`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-white-space) | 匹配一个空白字符，包括空格、制表符、换页符和换行符。等价于[ \f\n\r\t\v\u00a0\u1680\u180e\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]。例如, `/\s\w*/` 匹配"foo bar."中的' bar'。 |
| [`\S`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-non-white-space) | 匹配一个非空白字符。等价于`[^ `\f\n\r\t\v\u00a0\u1680\u180e\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff`]`。例如， `/\S\w*/` 匹配"foo bar."中的'foo'。 |
| [`\t`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-tab) | 匹配一个水平制表符 (U+0009)。                                |
| [`\v`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-vertical-tab) | 匹配一个垂直制表符 (U+000B)。                                |
| [`\w`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-word) | 匹配一个单字字符（字母、数字或者下划线）。等价于`[A-Za-z0-9_]`。例如, `/\w/` 匹配 "apple," 中的 'a'，"$5.28,"中的 '5' 和 "3D." 中的 '3'。 |
| [`\W`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-non-word) | 匹配一个非单字字符。等价于`[^A-Za-z0-9_]`。例如, `/\W/` 或者 `/[^A-Za-z0-9_]/` 匹配 "50%." 中的 '%'。 |
| [`\*n*`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-backreference) | 在正则表达式中，它返回最后的第n个子捕获匹配的子字符串(捕获的数目以左括号计数)。比如 `/apple(,)\sorange\1/` 匹配"apple, orange, cherry, peach."中的'apple, orange,' 。 |
| [`\0`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-null) | 匹配 NULL (U+0000) 字符， 不要在这后面跟其它小数，因为 `\0<digits>` 是一个八进制转义序列。 |
| [`\xhh`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-hex-escape) | 与代码 hh 匹配字符（两个十六进制数字）                       |
| [`\uhhhh`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#special-unicode-escape) | 与代码 hhhh 匹配字符（四个十六进制数字）。                   |

| [`\u{hhhh}`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions$edit#special-unicode-escape-es6) | (仅当设置了u标志时) 使用Unicode值hhhh匹配字符 (十六进制数字). |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |

将用户输入转义为正则表达式中的一个字面字符串, 可以通过简单的替换来实现：

```javascript
function escapeRegExp(string){
    return string.replace(/([.*+?^=!:${}()|[\]\/\\])/g, "\\$&"); 
    //$&表示整个被匹配的字符串
}
```

##### 使用插入语

任何正则表达式的插入语都会使这部分匹配的副字符串被记忆。一旦被记忆，这个副字符串就可以被调用于其它用途，如同 [使用括号的子字符串匹配](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions#%E4%BD%BF%E7%94%A8%E6%8B%AC%E5%8F%B7%E7%9A%84%E5%AD%90%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%8C%B9%E9%85%8D)之中所述。

比如， `/Chapter (\d+)\.\d*/` 解释了额外转义的和特殊的字符，并说明了这部分pattern应该被记忆。它精确地匹配后面跟着一个以上数字字符的字符 'Chapter '  (`\d` 意为任何数字字符，`+ 意为1次以上`)，跟着一个小数点（在这个字符中本身也是一个特殊字符；小数点前的 \ 意味着这个pattern必须寻找字面字符 '.')，跟着任何数字字符0次以上。 (`\d` 意为数字字符， `*` 意为0次以上)。另外，插入语也用来记忆第一个匹配的数字字符。

此模式可以匹配字符串"Open Chapter 4.3, paragraph 6"，并且'4'将会被记住。此模式并不能匹配"Chapter 3 and 4"，因为在这个字符串中'3'的后面没有点号'.'。

括号中的"?:"，这种模式匹配的子字符串将不会被记住。比如，(?:\d+)匹配一次或多次数字字符，但是不能记住匹配的字符。