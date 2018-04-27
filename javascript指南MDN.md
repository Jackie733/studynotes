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