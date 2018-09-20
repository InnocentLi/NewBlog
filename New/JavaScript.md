# 简介

作者：李文昊

一下均是从书籍或博客整理然后修饰总结而来

JavaScript系列参考书籍:《JavaScript高级程序设计》

CSS:《CSS权威指南》

ES6:深入理解ES6

HTTP:《HTTP权威指南》

Vue源码：

React源码

计算机网络:《》

数据结构算法:计蒜客

混合应用:慕课网

操作系统:计蒜客,博客

# JavaScript

## script标签

### 脚本引入

其实只需要记住scripr标签的引入就好了

scripr标签有6种属性

 async：可选。表示应该**立即下载脚本**，但不应妨碍页面中的其他操作，比如下载其他资源或等待加载其他脚本。只对外部脚本文件有效。

 charset：可选。表示通过src 属性指定的代码的字符集。由于大多数浏览器会忽略它的值，因此这个属性很少有人用。

 defer：可选。表示**可以延迟到文档完全被解析和显示之后再执行**。只对外部脚本文件有效。

~~ language：已废弃。原来用于表示编写代码使用的脚本语言（如JavaScript、JavaScript1.2或VBScript）。大多数浏览器会忽略这个属性，因此也没有必要再用了。~~

 src：可选。表示包含要执行代码的外部文件。

 type：可选。可以看成是language 的替代属性；表示编写代码使用的脚本语言的内容类型（也称为MIME 类型）。服务器在传送JavaScript 文件时使用的MIME 类型通常是application/x–javascript，默认值text/javascript

#### 正常脚本

只要不存在defer 和async 属性，浏览器都会按照<script>元素在页面中出现的先后顺序对它们依次进行解析。换句话说，在第一个<script>元素包含的代码解析完成后，第二个<script>包含的代码才会被解析，然后才是第三个、第四个……

#### 延迟脚本defer 

##### 立即下载延迟执行

这个属性的用途是表明脚本在执行时不会影响页面的构造。也就是说，脚本会被延迟到整个页面都解析完毕后再运行。因此，在<script>元素中设置defer 属性，相当于告诉浏览器**立即下载，但延迟执行**。

##### 例子

```html
<!DOCTYPE html>
<html>
<head>
<title>Example HTML Page</title>
<script type="text/javascript" defer="defer" src="example1.js"></script>
<script type="text/javascript" defer="defer" src="example2.js"></script>
</head>
<body>
<!-- 这里放内容 -->
</body>
</html>
```

虽然我们把<script>元素放在了文档的<head>元素中，但其中包含的脚本将延迟到浏览器遇到</html>标签后再执行。HTML5 规范要求脚本按照它们出现的先后顺序执行，因此第一个延迟脚本会先于第二个延迟脚本执行，而这两个脚本会先于DOMContentLoaded 事件（详见第13 章）执行。实际上延迟脚本并不一定会按照顺序执行，也不一定会在DOMContentLoaded 事件触发前执行，因此**最好只包含一个延迟脚本**。所以把延迟脚本放在页面底部仍然是最佳选择。

#### 异步脚本

##### 立即下载文件

```html
<html>
<head>
<title>Example HTML Page</title>
<script type="text/javascript" async src="example1.js"></script>
<script type="text/javascript" async src="example2.js"></script>
</head>
<body>
<!-- 这里放内容 
Firefox 3.6、Safari 5 和Chrome -->
</body>
</html>
```

第二个脚本文件可能会在第一个脚本文件之前执行。因此，确保**两者之间互不依赖非常重要**。指定async 属性的目的是不让页面等待两个脚本下载和执行，从而异步加载页面其他内容。为此，建议异步脚本不要在加载期间修改DOM。异步脚本一定会在页面的load 事件前执行，但可能会在DOMContentLoaded 事件触发之前或之后执行。

### 文档模式

IE5.5 引入了文档模式的概念，而这个概念是通过使用文档类型（doctype）切换实现的。最初的两种文档模式是：混杂模式（quirks mode）①和标准模式（standards mode）。混杂模式会让IE 的行为与（包含非标准特性的）IE5 相同，而标准模式则让IE 的行为更接近标准行为。虽然这两种模式**主要影响CSS内容的呈**现，但在某些情况下**也会影响到JavaScript 的解释执行**。

<!-- HTML 5 -->

```html
<!DOCTYPE html>
```

大家用这个就行

~~<!-- HTML 4.01 严格型 -->~~

~~<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"~~

~~"http://www.w3.org/TR/html4/strict.dtd">~~

~~<!-- XHTML 1.0 严格型 -->~~

~~<!DOCTYPE html PUBLIC~~

~~"-//W3C//DTD XHTML 1.0 Strict//EN"~~

~~"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">~~

~~而对于准标准模式，则可以通过使用过渡型（transitional）或框架集型（frameset）文档类型来触发，~~

~~如下所示：~~

~~<!-- HTML 4.01 过渡型 -->~~

```html
<!DOCTYPE HTML PUBLIC>
```

~~"-//W3C//DTD HTML 4.01 Transitional//EN"~~

~~"http://www.w3.org/TR/html4/loose.dtd">~~

~~<!-- HTML 4.01 框架集型 -->~~

~~<!DOCTYPE HTML PUBLIC~~

~~"-//W3C//DTD HTML 4.01 Frameset//EN"~~

~~"http://www.w3.org/TR/html4/frameset.dtd">~~

~~<!-- XHTML 1.0 过渡型 -->~~

~~<!DOCTYPE html PUBLIC~~

~~"-//W3C//DTD XHTML 1.0 Transitional//EN"~~

~~"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">~~

~~<!-- XHTML 1.0 框架集型 -->~~

~~<!DOCTYPE html PUBLIC~~

~~"-//W3C//DTD XHTML 1.0 Frameset//EN"~~

~~"http://www.w3.org/TR/xhtml1/DTD/xhtml1-frameset.dtd">~~

## 基本类型(基本概念)

### typeof操作符

"undefined"——如果这个值未定义

 "boolean"——如果这个值是布尔值

 "string"——如果这个值是字符串

 "number"——如果这个值是数值

 "object"——如果这个值是对象或null

 "function"——如果这个值是函数

**typeof 操作符检测null 值时会返回object**

### 判断是否为空对象

```javascript
//1.格式化
var data = {};
var b = (JSON.stringify(data) == "{}");
alert(b);//true

//2.for in
var obj = {};
var b = function() {
for(var key in obj) {
return false;
}
return true;
}
alert(b());//true

//3.Object.getOwnPropertyNames()方法
//此方法是使用Object对象的getOwnPropertyNames方法，获取到对象中的属性名，存到一个数组中，返回数组对
//象，我们可以通过判断数组的length来判断此对象是否为空
var data = {};
var arr = Object.getOwnPropertyNames(data);
alert(arr.length == 0);//true


//4.使用ES6的Object.keys()方法
//与4方法类似，是ES6的新方法, 返回值也是对象中属性名组成的数组
var data = {};
var arr = Object.keys(data);
alert(arr.length == 0);//true

//5. for in 方法
```





### Undefined类型

Undefined 类型只有一个值，即特殊的undefined。在使用var 声明变量但未对其加以初始化时就为Undefined

```javascript
var a;10
a //Undefined
```

### Null类型

Null 类型是第二个只有一个值的数据类型，这个特殊的值是null。从逻辑角度来看，null 值表示一个空对象指针，而这也正是使用typeof 操作符检测null 值时会返回"object"的原因

```javascript
var car = null;
alert(typeof car); // "object"
```

如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为null 而不是其他值。这样

一来，只要直接检查null 值就可以知道相应的变量是否已经保存了一个对象的引用.

```javascript
if (car != null){
// 对car 对象执行某些操作
}

```

实际上，**undefined 值是派生自null 值的**，因此ECMA-262 规定对它们的相等性测试要返回true：

```javascript
alert(null == undefined); //true
```

这里，位于null 和undefined 之间的相等操作符（==）总是返回true，不过要注意的是，这个操作符出于比较的目的**会转换其操作数**，尽管null 和undefined 有这样的关系，但它们的用途完全不同。如前所述，无论在什么情况下都没有必要把一个变量的值显式地设置为undefined，可是同样的规则对null 却不适用。换句话说，只要意在保存对象的变量还没有真正保存对象，就应该明确地让该变量保存null 值。这样做不仅可以体现**null 作为空对象指针**的惯例，而且也有助于进一步区分null 和undefined。

也就是说👇

```javascript
null == undefined  //true
null === undefined  //false
```

### Boolean类型

该类型只有两个字面值：true 和false。

true 不一定等于1，而false 也不一定等于0

| 数据类型  | 转换为true的值               | 转换为false的值 |
| --------- | ---------------------------- | --------------- |
| Boolean   | true                         | false           |
| String    | 任何非空字符串               | ""（空字符串）  |
| Number    | 任何非零数字值（包括无穷大） | 0和NaN）        |
| Object    | 任何对象                     | null            |
| Undefined | 不存在这种情况               | undefined       |

### Number类型   IEEE754

最基本的数值字面量格式是十进制整数，十进制整数可以像下面这样直接在代码中输入：

var intNum = 55; // 整数 

除了以十进制表示外，整数还可以通过八进制（以8 为基数）或十六进制（以16 为基数）的字面值来表示。其中，八进制字面值的第一位必须是零（0），然后是八进制数字序列（0～7）。如果字面值中的数值超出了范围，那么前导零将被忽略，后面的数值将被当作十进制数值解析。

```javascript
var octalNum1 = 070; // 八进制的56
var octalNum2 = 079; // 无效的八进制数值——解析为79
var octalNum3 = 08; // 无效的八进制数值——解析为8
```

**八进制字面量在严格模式下是无效的，会导致支持的JavaScript 引擎抛出错误。**

十六进制字面值的前两位必须是0x，后跟任何十六进制数字（0～9 及A～F）。

其中，字母A～F可以大写，也可以小写。如下面的例子所示：

```javascript
var hexNum1 = 0xA; // 十六进制的10
var hexNum2 = 0x1f; // 十六进制的31
```

在进行算术计算时，所有以八进制和十六进制表示的数值最终都将被转换成十进制数值

#### JavaScript 浮点数陷阱及解法

浏览器在做浮点运算的时候

 0.1+0.2=0.30000000000000004

1-0.9=0.09999999999999998

JavaScript 中所有数字包括整数和小数都只有一种类型为Number，即使用 64 位固定长度来表示，也就是标准的 double 双精度浮点数(相关的还有float 32位单精度)。

- 符号位S：第 1 位是正负数符号位（sign），0代表正数，1代表负数

- 指数位E：中间的 11 位存储指数（exponent），用来表示次方数

- 尾数位M：最后的 52 位是尾数（mantissa），超出的部分自动进一舍零

$ V = (-1)^{S}\times M \times 2^{E} $





![](https://camo.githubusercontent.com/af8c1cdd9aedced18be47e40d27208b671b4a18d/687474703a2f2f617461322d696d672e636e2d68616e677a686f752e696d672d7075622e616c6979756e2d696e632e636f6d2f37323637613538623239383932633362373233653364366333663733393035612e706e67)

所以会造成精度损失，所以可以采用toPrecision(12)来保持精度

parseFloat(1.4000000000000001.toPrecision(12)) === 1.4  // True22

这个我们不做详细的讨论

#### 数值范围

由于内存的限制，ECMAScript 并不能保存世界上所有的数值。ECMAScript 能够表示的最小数值保存在Number.MIN_VALUE 中——在大多数浏览器中，这个值是5e-324；能够表示的最大数值保存在Number.MAX_VALUE 中——在大多数浏览器中，这个值是1.7976931348623157e+308。如果某次计算的结果得到了一个超出JavaScript 数值范围的值，那么这个数值将被自动转换成特殊的Infinity 值。具体来说，如果这个数值是负数，则会被转换成-Infinity（负无穷），如果这个数值是正数，则会被转换成Infinity（正无穷）。如上所述，如果某次计算返回了正或负的Infinity 值，那么该值将无法继续参与下一次的计算，因为Infinity 不是能够参与计算的数值。要想确定一个数值是不是有穷的（换句话说，是不是位于最小和最大的数值之间），可以使用**isFinite**()函数。这个函数在参数位于最小与最大数值之间时会返回true，如下面的例子所示：

```javascript
var result = Number.MAX_VALUE + Number.MAX_VALUE;
alert(isFinite(result)); //false
```

尽管在计算中很少出现某些值超出表示范围的情况，但在执行极小或极大数值的计算时，检测监控这些值是可能的，也是必需的。

参考淘宝订单以前的订单曾是用数字表示的，然后随着订单增多，就超出了最大值，虽然后来改成了字符串模式，但是这样十分影响性能。所以js提出了一个大数的概念.

#### NaN

即非数值（Not a Number）是一个特殊的数值，这个数值用于表示一个本来要返回数值的操作数未返回数值的情况（这样就不会抛出错误了）。例如，在其他编程语言中，任何数值除以0 都会导致错误，从而停止代码执行。但在ECMAScript 中，任何**数值除以0 会返回NaN**，因此不会影响其他代码的执行。NaN 本身有两个非同寻常的特点。首先，任何**涉及NaN 的操作（例如NaN/10）都会返回NaN**，这特点在多步计算中有可能导致问题。其次，**NaN 与任何值都不相等，包括NaN 本身**。

**但实际上只有0 除以0 才会返回NaN，正数除以0 返回Infinity，负数除以0 返回-Infinity**。



##### isNaN()函数

这个函数接受一个参数，该参数可以是任何类型，而函数会帮我们确定这个参数是否“不是数值”。isNaN()在接收到一个值之后，会尝试将这个值转换为数值。某些不是数值的值会直接转换为数值，例如字符串"10"或Boolean 值。而任何不能被转换为数值的值都会导致这个函数返回true。

```javascript
alert(isNaN(NaN)); //true
alert(isNaN(10)); //false（10 是一个数值）
alert(isNaN("10")); //false（可以被转换成数值10）
alert(isNaN("blue")); //true（不能转换成数值）
alert(isNaN(true)); //false（可以被转换成数值1）
```

#### 数值转换

有三个方法 Number(),parseInt()和parseFloat()。

##### Number()函数

Number()函数的转换规则如下。

1.如果是Boolean 值，true 和false 将分别被转换为1 和0。

2.如果是数字值，只是简单的传入和返回。

3.如果是null 值，返回0。

4.如果是undefined，返回NaN。

5.如果是字符串，遵循下列规则：

6.如果字符串中只包含数字（包括前面带正号或负号的情况），则将其转换为十进制数值，即"1"会变成1，"123"会变成123，而"011"会变成11（注意：前导的零被忽略了）；

7.如果字符串中包含有效的浮点格式，如"1.1"，则将其转换为对应的浮点数值（同样，也会忽略前导零）；

8.如果字符串中包含有效的十六进制格式，例如"0xf"，则将其转换为相同大小的十进制整数值；

9.如果字符串是空的（不包含任何字符），则将其转换为0；

10.如果字符串中包含除上述格式之外的字符，则将其转换为NaN。

11.如果是对象，则调用对象的valueOf()方法，然后依照前面的规则转换返回的值。如果转换的结果是NaN，则调用对象的toString()方法，然后再次依照前面的规则转换返回的字符串值。

### String类型

String 类型用于表示由零或多个16 位Unicode 字符组成的字符序列，即字符串。字符串可以由双引号（"）或单引号（'）表示，因此下面两种字符串的写法都是有效的：

```javascript
var firstName = "Nicholas";
var lastName = 'Zakas';
```

ECMAScript 中的字符串是不可变的，也就是说，字符串一旦创建，它们的值就不能改变。要改变

某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量，

例如：

```javascript
var lang = "Java";
lang = lang + "Script";
```

以上示例中的变量lang 开始时包含字符串"Java"。而第二行代码把lang 的值重新定义为"Java"与"Script"的组合，即"JavaScript"。实现这个操作的过程如下：首先创建一个能容纳10 个字符的新字符串，然后在这个字符串中填充"Java"和"Script"，最后一步是销毁原来的字符串"Java"和字符串"Script"，因为这两个字符串已经没用了。这个过程是在后台发生的，而这也是在某些旧版本的

ECMAScript 中的字符串是不可变的，也就是说，字符串一旦创建，它们的值就不能改变。要改变某个变量保存的字符串，首先要销毁原来的字符串，然后再用另一个包含新值的字符串填充该变量，

```javascript
var lang = "Java";
lang = lang + "Script";
```

以上示例中的变量lang 开始时包含字符串"Java"。而第二行代码把lang 的值重新定义为"Java"与"Script"的组合，即"JavaScript"。实现这个操作的过程如下：首先创建一个能容纳10 个字符的新字符串，然后在这个字符串中填充"Java"和"Script"，最后一步是销毁原来的字符串"Java"和字符串"Script"，因为这两个字符串已经没用了。这个过程是在后台发生的，而这也是在某些旧版本的浏览器（例如版本低于1.0 的Firefox、IE6 等）中拼接字符串时速度很慢的原因所在。但这些浏览器后来的版本已经解决了这个低效率问题。

#### 转换为字符串

要把一个值转换为一个字符串有两种方式。第一种是使用几乎每个值都有的toString()方法。这个方法唯一要做的就是返回相应值的字符串表现。来看下面的例子：

```javascript
var age = 11;
var ageAsString = age.toString(); // 字符串"11"
var found = true;
var foundAsString = found.toString(); // 字符串"true"
```

数值、布尔值、对象和字符串值（没错，每个字符串也都有一个toString()方法，该方法返回字符串的一个副本）都有toString()方法。但null 和undefined 值没有这个方法。多数情况下，调用toString()方法不必传递参数。但是，在调用数值的toString()方法时，可以传递一个参数：输出数值的基数。默认情况下，toString()方法以十进制格式返回数值的字符串表示。而通过传递基数，toString()可以输出以二进制、八进制、十六进制，乃至其他任意有效进制格式表示的字符串值。下面给出几个例子：

```javascript
var num = 10;
alert(num.toString()); // "10"
alert(num.toString(2)); // "1010"
alert(num.toString(8)); // "12"
alert(num.toString(10)); // "10"
alert(num.toString(16)); // "a"
```

通过这个例子可以看出，通过指定基数，**toString()方法会改变输出的值**。而数值10 根据基数的不同，可以在输出时被转换为不同的数值格式。注意，默认的（没有参数的）输出值与指定基数10 时的输出值相同。在不知道要转换的值是不是null 或undefined 的情况下，还可以使用转型函数String()，这个函数能够将任何类型的值转换为字符串。String()函数遵循下列转换规则：

如果值有toString()方法，则调用该方法（没有参数）并返回相应的结果；

如果值是null，则返回"null"；

如果值是undefined，则返回"undefined"。

```javascript
var value1 = 10;
var value2 = true;
var value3 = null;
var value4;
alert(String(value1)); // "10"
alert(String(value2)); // "true"
alert(String(value3)); // "null"
alert(String(value4)); // "undefined"
```

因为null 和undefined 没有toString()方法，所以String()函数就返回了这两个值的字面量。

### Object类型

可以通过new创建

```javascript
var o = new Object();
var o = new Object; // 有效，但不推荐省略圆括号
```

 在ECMAScript 中，如果不给构造函数传递参数，则可以省略后面的那一对圆括号。也就是说，在像前面这个示例一样不传递参数的情况下，完全可以省略那对圆括号

Object 的每个实例都具有下列属性和方法。

1 constructor：保存着用于创建当前对象的函数。对于前面的例子而言，构造函数（constructor）就是Object()。

2 hasOwnProperty(propertyName)：用于检查给定的属性在当前对象实例中（而不是在实例的原型中）是否存在。其中，作为参数的属性名（propertyName）必须以字符串形式指定（o.hasOwnProperty("name")）。

3 isPrototypeOf(object)：用于检查传入的对象是否是传入对象的原型。

4 propertyIsEnumerable(propertyName)：用于检查给定的属性是否能够使用for-in 语句来枚举。与hasOwnProperty()方法一样，作为参数的属性名必须以字符串形式指定。

5 toLocaleString()：返回对象的字符串表示，该字符串与执行环境的地区对应。

6 toString()：返回对象的字符串表示。

7 valueOf()：返回对象的字符串、数值或布尔值表示。通常与toString()方法的返回值相同。



从技术角度讲，ECMA-262 中对象的行为不一定适用于JavaScript 中的其他对象。浏览器环境中的对象，比如BOM 和DOM 中的对象，都属于**宿主对象**，因为它们是由宿主实现提供和定义的。ECMA-262 不负责定义宿主对象，因此宿主对象可能会也可能不会继承Object。



#### toLocaleString和toString区别

```javascript
var a=1234
a.toString()
"1234"
a.toLocaleString()
"1,234"  //国外表示法

var sd=new Date()
sd
Fri Sep 14 2018 00:24:09 GMT+0800 (中国标准时间)
sd.toLocaleString()
"2018/9/14 上午12:24:09"
sd.toString()
"Fri Sep 14 2018 00:24:09 GMT+0800 (中国标准时间)"
```

说白了就是地区是什么就用什么地区的方法去表示

### for..in和Object.keys()和Object.getOwnPropertyNames()的区别

```javascript
var arr = ['a', 'b', 'c', 'd'];

// 使用for..in
for (var i in arr) {
  console.log('索引：' + i + '，值：' + arr[i]);
}

// 使用for循环
for (var j = 0; j < arr.length; j++) {
  console.log('索引：' + j + '，值：' + arr[j]);
}

/* 两种方式都输出：
 * ----------------
 * 索引：0，值：a
 * 索引：1，值：b
 * 索引：2，值：c
 * 索引：3，值：d
 * ----------------
 */
```

但是有几种特殊的情况一定不能用for in 

具体请查看

https://stackoverflow.com/questions/500504/why-is-using-for-in-with-array-iteration-a-bad-idea

```javascript
var a = []; // Create a new empty array.
a[5] = 5;   // Perfectly legal JavaScript that resizes the array.

for (var i = 0; i < a.length; i++) {
    // Iterate over numeric indexes from 0 to 5, as everyone expects.
    console.log(a[i]);
}

/* Will display:
   undefined
   undefined
   undefined
   undefined
   undefined
   5
*/

var a = [];
a[5] = 5;
for (var x in a) {
    // Shows only the explicitly set index of "5", and ignores 0-4
    console.log(x);
}

/* Will display:
   5
*/



// Somewhere deep in your JavaScript library...
Array.prototype.foo = 1;

// Now you have no idea what the below code will do.
var a = [1, 2, 3, 4, 5];
for (var x in a){
    // Now foo is a part of EVERY array and 
    // will show up here as a value of 'x'.
    console.log(x);
}

/* Will display:
   0
   1
   2
   3
   4
   foo
*/
```

所以我们可以利用Object.definepropty来禁止遍历

```javascript
var colors = [1, 2, 3];
Object.defineProperty(Array.prototype, 'demo', {
  enumerable: false,
  value: function() {}
});

Array.prototype.propertyIsEnumerable('demo'); // false 
Object.getOwnPropertyDescriptor(Array.prototype, 'demo'); 
// {writable: false, enumerable: false, configurable: false}
//以上两个属性都是判断对象属性的

for (var i in colors) {
  console.log(i); // 输出：0 1 2
}
```

**in操作符只要通过对象能访问到属性就返回true。hasOwnProperty()只在属性存在于实例中时才返回true。**

所以可以通过hasOwnProperty()进行过滤操作

```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.getName = function() {
  return this.name;
}

// 实例化
var jenemy = new Person('jenemy', 25);

for (var prop in Person) {
  console.log(prop); // name age getName
}

var hasOwn = Object.prototype.hasOwnProperty;
for (var prop2 in jenemy) {
  if (hasOwn.call(jenemy, prop2)) {  //这段代码的理解在下面
    console.log(prop2); // name age
  }
}
```

in和 hasOwnProperty的区别

```javascript
function Person(){
}
	Person.prototype.name = "Nicholas";
	Person.prototype.age = 29;
	Person.prototype.job = "Software Engineer";
	Person.prototype.sayName = function(){
    alert(this.name);
};

var person1 = new Person();
var person2 = new Person();

alert(person1.hasOwnProperty("name"));//false
alert("name" in person1);//true

person1.name = "Greg";
alert(person1.name);//"Greg"
alert(person1.hasOwnProperty("name"));//true
alert("name" in person1);//true

delete person1.name;
alert(person1.name);//"Nicholas"
alert(person1.hasOwnProperty("name"));//false
alert("name" in person1);//true
```

所以以上代码可以过滤掉了

#### Object.keys()

**用于获取对象自身所有的可枚举的属性值**，**但不包括原型中的属性**

**注意它同for..in一样不能保证属性按对象原来的顺序输出。**

```javascript
// 遍历数组
var colors = ['red', 'green', 'blue'];
colors.length = 10;
colors.push('yellow');
Array.prototype.demo = function () {};

Object.keys(colors); // 0 1 2 10

// 遍历对象
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.demo = function() {};

var jenemy = new Person('jenemy', 25);

Object.keys(jenemy); // name age  并没有demo
```

注意在 ES5 环境，如果传入的参数不是一个对象，而是一个字符串，那么它会报 TypeError。在 ES6 环境，如果传入的是一个非对象参数，内部会对参数作一次强制对象转换，如果转换不成功会抛出 TypeError。

```javascript
// 在 ES5 环境
Object.keys('foo'); // TypeError: "foo" is not an object
// 在 ES6 环境
Object.keys('foo'); // ["0", "1", "2"]
// 传入 null 对象
Object.keys(null); // Uncaught TypeError: Cannot convert undefined or null to object
// 传入 undefined
Object.keys(undefined); // Uncaught TypeError: Cannot convert undefined or null to object
```

要在原生不支持的旧环境中添加兼容的`Object.keys`，请复制以下代码段：(原生Objext.keys手写)

```javascript
if (!Object.keys) {
  Object.keys = (function () {
    var hasOwnProperty = Object.prototype.hasOwnProperty,
        hasDontEnumBug = !({toString: null}).propertyIsEnumerable('toString'),
        dontEnums = [
          'toString',
          'toLocaleString',
          'valueOf',
          'hasOwnProperty',
          'isPrototypeOf',
          'propertyIsEnumerable',
          'constructor'
        ],
        dontEnumsLength = dontEnums.length;

    return function (obj) {
      if (typeof obj !== 'object' && typeof obj !== 'function' || obj === null) throw new TypeError('Object.keys called on non-object');

      var result = [];

      for (var prop in obj) {
        if (hasOwnProperty.call(obj, prop)) result.push(prop);
      }

      if (hasDontEnumBug) {
        for (var i=0; i < dontEnumsLength; i++) {
          if (hasOwnProperty.call(obj, dontEnums[i])) result.push(dontEnums[i]);
        }
      }
      return result;
    }
  })()
};
```

**发现Object keys 也是通过 in 搭配 hasOwnProperty 来实现的**

所以浏览器底层代码实现可能也是这种方式

#### Object.prototype.propertyIsEnumerable()

#### 基本操作

解释propertyIsEnumerable()方法返回一个布尔值，表示指定的属性是否可枚举。

这里可以参考一下js高级里的对象属性

```javascript
var o = {};
var a = [];
o.prop = 'is enumerable';
a[0] = 'is enumerable';

o.propertyIsEnumerable('prop');   //  返回 true
a.propertyIsEnumerable(0);        // 返回 true
```

#### 用户自定义对象和引擎内置对象

```javascript
var a = ['is enumerable'];

a.propertyIsEnumerable(0);          // 返回 true
a.propertyIsEnumerable('length');   // 返回 false

Math.propertyIsEnumerable('random');   // 返回 false
this.propertyIsEnumerable('Math');     // 返回 false
```

并未显示引擎内置对象。只是显示了用户自定义对象，

#### 自身属性和继承属性

```javascript
var a = [];
a.propertyIsEnumerable('constructor');         // 返回 false

function firstConstructor() {
  this.property = 'is not enumerable';
}

firstConstructor.prototype.firstMethod = function() {};

function secondConstructor() {
  this.method = function method() { return 'is enumerable'; };
}

secondConstructor.prototype = new firstConstructor;
secondConstructor.prototype.constructor = secondConstructor;

var o = new secondConstructor();
o.arbitraryProperty = 'is enumerable';

o.propertyIsEnumerable('arbitraryProperty');   // 返回 true
o.propertyIsEnumerable('method');              // 返回 true
o.propertyIsEnumerable('property');            // 返回 false

o.property = 'is enumerable';

o.propertyIsEnumerable('property');            // 返回 true

// 这些返回fasle，是因为，在原型链上propertyIsEnumerable不被考虑
// (尽管最后两个在for-in循环中可以被循环出来)。
o.propertyIsEnumerable('prototype');   // 返回 false (根据 JS 1.8.1/FF3.6)
o.propertyIsEnumerable('constructor'); // 返回 false
o.propertyIsEnumerable('firstMethod'); // 返回 false
```

注意⚠️原型链上propertyIsEnumerable不被考虑，尽管最后两个在for-in循环中可以被循环出来

#### Object.getOwnPropertyNames()

Object.getOwnPropertyNames()方法返回对象的所有自身属性的属性名,**包括不可枚举的属性**组成的数组，但不会获取原型链上的属性。

```javascript
function A(a,aa) {
  this.a = a;
  this.aa = aa;
  this.getA = function() {
    return this.a;
  }
}
// 原型方法
A.prototype.aaa = function () {};

var B = new A('b', 'bb');
B.myMethodA = function() {};
// 不可枚举方法
Object.defineProperty(B, 'myMethodB', {
  enumerable: false,
  value: function() {}
});

Object.getOwnPropertyNames(B); // ["a", "aa", "getA", "myMethodA", "myMethodB"]
```

#### ES6    for..of

```javascript
var colors = ['red', 'green', 'blue'];
colors.length = 5;
colors.push('yellow');

for (var i in colors) {
  console.log(colors[i]); // red green blue yellow
}

for (var j of colors) {
  console.log(j); // red green blue undefined undefined yellow
}
```

### 其他语句

#### do-while和while

do-while 语句是一种后测试循环语句，即只有在循环体中的代码执行之后，才会测试出口条件。换句话说，在对条件表达式求值之前，循环体内的代码至少会被执行一次。

while 语句属于前测试循环语句，也就是说，在循环体内的代码被执行之前，就会对出口条件求值。因此，循环体内的代码有可能永远不会被执行

所以do-while可以至少保证一次执行操作

#### break和continue语句

break 和continue 语句用于在循环中精确地控制代码的执行。其中，break 语句会立即退出循环，强制继续执行循环后面的语句。而continue 语句虽然也是立即退出循环，但退出循环后会从循环的顶部继续执行。

```javascript
var num = 0;
for(var i=1; i < 10; i++){
if (i%5==0) {
   break;
}
   num++;
}

alert(num); //4
```

这个例子中的for 循环会将变量i 由1 递增至10。在循环体内，有一个if 语句检查i 的值是否可以被5 整除（使用求模操作符）。如果是，则执行break 语句退出循环。另一方面，变量num 从0 开始，用于记录循环执行的次数。在执行break 语句之后，要执行的下一行代码是alert()函数，结果显示4。也就是说，在变量i 等于5 时，循环总共执行了4 次；而break 语句的执行，导致了循环在num 再次递增之前就退出了。如果在这里把break 替换为continue 的话

```javascript
var num = 0;
for (var i=1; i < 10; i++) {
if (i % 5 == 0) {
continue;
}
num++;
}
alert(num); //8
```

也就是循环总共执行了8 次。当变量i 等于5 时，循环会在num 再次递增之前退出，但接下来执行的是下一次循环，即i 的值等于6 的循环。于是，循环又继续执行，直到i 等于10 时自然结束。而num 的最终值之所以是8，是因为continue 语句导致它少递增了一次。

#### break 和continue 语句与label 语句联合

```javascript
var num = 0;
outermost:
for (var i=0; i < 10; i++) {
  for (var j=0; j < 10; j++) {
    if (i == 5 && j == 5) {
      break outermost;
      }
     num++;
   }
}

alert(num); //55


var num = 0;

for (var i=0; i < 10; i++) {
  for (var j=0; j < 10; j++) {
    if (i == 5 && j == 5) {
      
      }
     num++;
   }
}
alert(num); //100



var num = 0;

for (var i=0; i < 10; i++) {
  for (var j=0; j < 10; j++) {
    if (i == 5 && j == 5) {
      break ;
      }
      num++;
   }
}

alert(num); //95
```

### with                                                                                                                                                                                                                                                                                                                                                                                    

```javascript
var qs = location.search.substring(1);
var hostName = location.hostname;
var url = location.href;

等同于

with(location){
var qs = search.substring(1);
var hostName = hostname;
var url = href;
}
```

每个变量首先被认为是一个局部变量，而如果在局部环境中找不到该变量的定义，就会查询location 对象中是否有同名的属性。如果发现了同名属性，则以location 对象属性的值作为变量的值。

**严格模式下不允许使用with 语句，否则将视为语法错误**。

### 函数

function关键字

没什么可说的，注意以下代码永远不会执行

```javascript
function sayHi(name, message) {
return;
alert("Hello " + name + "," + message); //永远不会调用
}
```

#### 理解参数

ECMAScript 函数**不介意传递进来多少个参数**，也不在乎传进来参数是什么数据类型。也就是说，即便你定义的函数只接收两个参数，在调用这个函数时也未必一定要传递两个参数。可以传递一个、三个甚至不传递参数，ECMAScript 中的**参数在内部是用一个数组来表示的**。函数接收到的始终都是这个数组，而不关心数组中包含哪些参数（如果有参数的话）。如果这个数组中不包含任何元素，无所谓；如果包含多个元素，也没有问题。实际上，在函数体内可以通过**arguments 对象来访问这个参数数组**，从而获取传递给函数的每一个参数。

arguments 对象只是与数组类似（**它并不是Array 的实例**），因为可以使用方括号语法访问它的每一个元素（即第一个元素是arguments[0]，第二个元素是argumetns[1]）使用length 属性来确定传递进来多少个参数。在前面的例子中，sayHi()函数的第一个参数的名字叫name，而该参数的值也可以通过访问arguments[0]来获取。因此，那个函数也可以像下面这样重写，即不显式地使用命名参数：

```javascript
function sayHi(name, message) {
	alert("Hello " + name + "," + message); //永远不会调用
}

等价于

function sayHi() {
alert("Hello " + arguments[0] + "," + arguments[1]);
}
```

所以**命名的参数只提供便利，但不是必需的**

```javascript
// 可以获取参数长度
function howManyArgs() {
	alert(arguments.length);
}
howManyArgs("string", 45); //2
howManyArgs(); //0
howManyArgs(12); //1


function doAdd() {
if(arguments.length == 1) {
	alert(arguments[0] + 10);
} else if (arguments.length == 2) {
	alert(arguments[0] + arguments[1]);
	}
}
doAdd(10); //20
doAdd(30, 20); //50

function doAdd(num1, num2) {
if(arguments.length == 1) {
	alert(num1 + 10);
} else if (arguments.length == 2) {
	alert(arguments[0] + num2);
}
}

//覆盖
function doAdd(num1, num2) {
	arguments[1] = 10;
	alert(arguments[0] + num2);
}
//如果只传入了一个参数，那么为arguments[1]设置的值不会反应到命名参数中。这是因为arguments 对象的长度是由传入的参数个数决定的，不是由定义函数时的命名参数的个数决定的。
```

#### 没有重载

ECMAScript 函数不能像传统意义上那样实现重载

```javascript
function addSomeNumber(num){
	return num + 100;
}
function addSomeNumber(num) {
	return num + 200;
}
var result = addSomeNumber(100); //30
```

## 变量作用域内存

### 变量

包含两种不同数据类型的值：**基本类型值**和**引用类型值**。基本类型值指的是简单的数据段，而引用类型值指那些可能由多个值构成的对象。

#### 动态的属性

定义基本类型值和引用类型值的方式是类似的

```javascript
var person = new Object();
person.name = "Nicholas";
alert(person.name); //"Nicholas"
```

以上代码创建了一个对象并将其保存在了变量person 中。然后，我们为该对象添加了一个名为name 的属性，并将字符串值"Nicholas"赋给了这个属性。紧接着，又通过alert()函数访问了这个新属性。如果对象不被销毁或者这个属性不被删除，则这个属性将一直存在。

#### 复制变量值

##### 基本类型值

除了保存的方式不同之外，在从**一个变量向另一个变量复制基本类型值和引用类型值时，也存在不同**。如果从一个变量向另一个变量复制基本类型的值，会在变量对象上创建一个新值，然后把该值复制到为新变量分配的位置上。来看一个例子：

```javascript
var num1 = 5;
var num2 = num1;
```

在此，num1 中保存的值是5。当使用num1 的值来初始化num2 时，num2 中也保存了值5。但num2中的5 和num1 中的5 是完全独立的，该值只是num1 中5 的一个副本。此后，这两个变量**可以参与任何操作而不会相互影响**

![](/Users/innocence/Desktop/书籍/img/变量保存.png)



##### 引用类型值

当从一个变量向另一个变量复制引用类型的值时，同样也会将存储在变量对象中的值复制一份放到为新变量分配的空间中。不同的是，**这个值的副本实际上是一个指针，而这个指针指向存储在堆中的一个对象**。复制操作结束后，两个变量实际上将引用同一个对象。因此，改变其中一个变量，就会影响另一个变量。

```javascript
var obj1 = new Object();
var obj2 = obj1;
obj1.name = "Nicholas";
alert(obj2.name); //"Nicholas" ❗️
```

首先，变量obj1 保存了一个对象的新实例。然后，这个值被复制到了obj2 中；换句话说，obj1和obj2 都指向同一个对象。这样，当为obj1 添加name 属性后，可以通过obj2 来访问这个属性，因为这**两个变量引用的都是同一个对象。**

![](/Users/innocence/Desktop/书籍/img/堆.png)

#### 传递参数

ECMAScript 中**所有函数的参数都是按值传递**的。也就是说，把函数外部的值复制给函数内部的参数，就和把值从一个变量复制到另一个变量一样。**基本类型值的传递如同基本类型变量的复制一样**，而引用类型值的传递，则如同引用类型变量的复制一样。访问变量有按值和按引用两种方式，而参数只能按值传递。

在向参数传递基本类型的值时，被传递的值会被复制给一个局部变量（即命名参数，或者用ECMAScript 的概念来说，就是arguments 对象中的一个元素）。在向参数传递引用类型的值时，会把这个值在内存中的地址复制给一个局部变量，因此这个局部变量的变化会反映在函数的外部。



```javascript
function addTen(num) {
	num += 10;
	return num;
}

var count = 20;
var result = addTen(count);
alert(count); //20，没有变化
alert(result); //30

```

这里的函数addTen()有一个参数num，而参数实际上是函数的局部变量。在调用这个函数时，变量count 作为参数被传递给函数，这个变量的值是20。于是，数值20 被复制给参数num 以便在addTen()中使用。在函数内部，参数num 的值被加上了10，但这一变化不会影响函数外部的count 变量。参数num 与变量count 互不相识，它们仅仅是具有相同的值。假如num 是按引用传递的话，那么变量count的值也将变成30，从而反映函数内部的修改。当然，使用数值等基本类型值来说明按值传递参数比较简单。

````javascript
function setName(obj) {
	obj.name = "Nicholas";
}
var person = new Object();
setName(person);
alert(person.name); //"Nicholas"
````

以上代码中创建一个对象，并将其保存在了变量person 中。然后，这个变量被传递到setName()函数中之后就被复制给了obj。在这个函数内部，obj 和person 引用的是同一个对象。换句话说，即使这个变量是按值传递的，obj 也会按引用来访问同一个对象。于是，当在函数内部为obj 添加name属性后，函数外部的person 也将有所反映；因为person 指向的对象在堆内存中只有一个，而且是全局对象。

```javascript
function setName(obj) {
	obj.name = "Nicholas";
	obj = new Object();
	obj.name = "Greg";
}
var person = new Object();
setName(person);
alert(person.name); //"Nicholas"
```

这个例子与前一个例子的唯一区别，就是在setName()函数中添加了两行代码：一行代码为obj重新定义了一个对象，另一行代码为该对象定义了一个带有不同值的name 属性。在把person 传递给setName()后，其name 属性被设置为"Nicholas"。然后，又将一个新对象赋给变量obj，同时将其name属性设置为"Greg"。如果person 是按引用传递的，那么person 就会自动被修改为指向其name 属性值为"Greg"的新对象。但是，当接下来再访问person.name 时，显示的值仍然是"Nicholas"。这说明即使在函数内部修改了参数的值，但原始的引用仍然保持未变。实际上，当在函数内部重写obj 时，这个变量引用的就是一个局部对象了。而这个局部对象会在函数执行完毕后立即被销毁。

#### 检测类型

````javascript
var s = "Nicholas";
var b = true;
var i = 22;
var u;
var n = null;
var o = new Object();
alert(typeof s); //string
alert(typeof i); //number
alert(typeof b); //boolean
alert(typeof u); //undefined
alert(typeof n); //object
alert(typeof o); //object
````

虽然在检测基本数据类型时typeof 是非常得力的助手，但在**检测引用类型的值时，这个操作符的用处不大**。通常，我们并不是想知道某个值是对象，而是想知道它是什么类型的对象。为此，ECMAScript提供了instanceof 操作符，其语法如下所示：

```javascript
result = variable instanceof constructor
```

如果变量是给定引用类型（根据它的原型链来识别；第6 章将介绍原型链）的实例，那么instanceof 操作符就会返回true。请看下面的例子：

```javascript
alert(person instanceof Object); // 变量person 是Object 吗？
alert(colors instanceof Array); // 变量colors 是Array 吗？
alert(pattern instanceof RegExp); // 变量pattern 是RegExp 吗？
```

根据规定，所有引用类型的值都是Object 的实例。在检测一个引用类型值和Object 构造函数时，instanceof 操作符始终会返回true。当然，如果使用instanceof 操作符检测基本类型的值，则该操作符始终会返回false，因为基本类型不是对象。

## 作用域和执行环境

执行环境是JavaScript 中最为重要的一个概念。执行环境定义了变量或函数有权访问的其他数据，决定了它们各自的行为。每个执行环境都有一个与之关联的**变量对象**。环境中定义的所有变量和函数都保存在这个对象中。虽然我们编写的代码无法访问这个对象，但解析器在处理数据时会在后台使用它。全局执行环境是最外围的一个执行环境。根据ECMAScript 实现所在的宿主环境不同，表示执行环境的对象也不一样。在Web 浏览器中，全局执行环境被认为是window 对象（，因此所有全局变量和函数都是作为window 对象的属性和方法创建的。某个执行环境中的所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数定义也随之销毁（全局执行环境直到应用程序退出——例如关闭网页或浏览器——时才会被销毁）。每个函数都有自己的执行环境。当执行流进入一个函数时，函数的环境就会被推入一个环境栈中。而在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境。ECMAScript 程序中的执行流正是由这个方便的机制控制着。当代码在一个环境中执行时，会创建变量对象的一个作用域链（scope chain）。作用域链的用途，是保证对执行环境有权访问的所有变量和函数的有序访问。作用域链的前端，始终都是当前执行的代码所在环境的变量对象。如果这个环境是函数，则将其活动对象（activation object）作为变量对象。活动对象在最开始时只包含一个变量，即arguments 对象（这个对象在全局环境中是不存在的）。**作用域链中的下一个变量对象来自包含（外部）环境**，而再下一个变量对象则来自下一个包含环境。这样，一直延续到全局执行环境；全局执行环境的变量对象始终都是作用域链中的最后一个对象。标识符解析是沿着作用域链一级一级地搜索标识符的过程。搜索过程始终从作用域链的前端开始，然后逐级地向后回溯，直至找到标识符为止（如果找不到标识符，通常会导致错误发生）。



```javascript
var color = "blue";
function changeColor(){
	if(color === "blue"){
		color = "red";
    }else{
		color = "blue";
	}
}
changeColor();
alert("Color is now " + color);
```

在这个简单的例子中，函数changeColor()的作用域链包含两个对象：它自己的变量对象（其中定义着arguments 对象）和全局环境的变量对象。可以在函数内部访问变量color，就是因为可以在这个作用域链中找到它。此外，在局部作用域中定义的变量可以在局部环境中与全局变量互换使用

```javascript
var color = "blue";
function changeColor(){
	var anotherColor = "red";
	function swapColors(){
		var tempColor = anotherColor;
		anotherColor = color;
		color = tempColor;
		// 这里可以访问color、anotherColor 和tempColor
	}
	// 这里可以访问color 和anotherColor，但不能访问tempColor
	swapColors();
}
// 这里只能访问color
changeColor();
```

以上代码有3 个执行环境：全局环境、changeColor()的局部环境和swapColors()的局部环境。全局环境中有一个变量color 和一个函数changeColor()。changeColor()的局部环境中有一个名为anotherColor 的变量和一个名为swapColors()的函数，但它也可以访问全局环境中的变量color。swapColors()的局部环境中有一个变量tempColor该变量只能在这个环境中访问到。无论全局环境还是changeColor()的局部环境都无权访问tempColor然而，在swapColors()内部则可以访问其他两个环境中的所有变量，因为那两个环境是它的父执行环境。

其中，内部环境可以通过作用域链访问所有的外部环境，但外部环境不能访问内部环境中的任何变量和函数。这些环境之间的联系是线性、有次序的。每个环境都可以向上搜索作用域链，以查询变量和函数名；但任何环境都不能通过向下搜索作用域链而进入另一个执行环境。对于这个例子中的swapColors()而言，其作用域链中包含3 个对象：swapColors()的变量对象、changeColor()的变量对象和全局变量对象。swapColors()的局部环境开始时会先在自己的变量对象中搜索变量和函数名，如果搜索不到则再搜索上一级作用域链。changeColor()的作用域链中只包含两个对象：它自己的变量对象和全局变量对象。这也就是说，它不能访问swapColors()的环境。

### 延长作用域链

虽然执行环境的类型总共只有两种——全局和局部（函数），但还是有其他办法来延长作用域链。这么说是因为有些语句可以在作用域链的前端临时增加一个变量对象，该变量对象会在代码执行后被移除。在两种情况下会发生这种现象。具体来说，就是当执行流进入下列任何一个语句时，作用域链就会得到加长：

 try-catch 语句的catch 块；

 with 语句。

这两个语句都会在作用域链的前端添加一个变量对象。对with 语句来说，会将指定的对象添加到作用域链中。对catch 语句来说，会创建一个新的变量对象，其中包含的是被抛出的错误对象的声明。

```javascript
function buildUrl() {
	var qs = "?debug=true";
	with(location){
	var url = href + qs;
    }
	return url;
}
```

在此，with 语句接收的是location 对象，因此其变量对象中就包含了location 对象的所有属性和方法，而这个变量对象被添加到了作用域链的前端。buildUrl()函数中定义了一个变量qs。当在with 语句中引用变量href 时（实际引用的是location.href），可以在当前执行环境的变量对象中找到。当引用变量qs 时，引用的则是在buildUrl()中定义的那个变量，而该变量位于函数环境的变量对象中。至于with 语句内部，则定义了一个名为url 的变量，因而url 就成了函数执行环境的一部分，所以可以作为函数的值被返回。

在此，with 语句接收的是location 对象，因此其变量对象中就包含了location 对象的所有属性和方法，而这个变量对象被添加到了作用域链的前端。buildUrl()函数中定义了一个变量qs。当在with 语句中引用变量href 时（实际引用的是location.href），可以在当前执行环境的变量对象中找到。当引用变量qs 时，引用的则是在buildUrl()中定义的那个变量，而该变量位于函数环境的变量对象中。至于with 语句内部，则定义了一个名为url 的变量，因而url 就成了函数执行环境的一部分，所以可以作为函数的值被返回。

### 没有块级作用域

JavaScript 没有块级作用域经常会导致理解上的困惑。在其他类C 的语言中，由花括号封闭的代码块都有自己的作用域（如果用ECMAScript 的话来讲，就是它们自己的执行环境），因而支持根据条件来定义变量。例如，下面的代码在JavaScript 中并不会得到想象中的结果：

```javascript
if (true) {
	var color = "blue";
}
alert(color); //"blue"
```

这里是在一个if 语句中定义了变量color。如果是在C、C++或Java 中，color 会在if 语句执行完毕后被销毁。但在JavaScript 中，if 语句中的变量声明会将变量添加到当前的执行环境（在这里是全局环境）中。在使用for 语句时尤其要牢记这一差异，例如：

```javascript
for (var i=0; i < 10; i++){
	doSomething(i);
}
alert(i); //10
```

对于有块级作用域的语言来说，for 语句初始化变量的表达式所定义的变量，只会存在于循环的环境之中。而对于JavaScript 来说，由for 语句创建的变量i 即使在for 循环执行结束后，也依旧会存在于循环外部的执行环境中。



### 总结



## 引用类型



## 面向对象

## 函数表达式

## BOM



## DOM

## 事件

## 表单脚本

## canvas vs svg



### 图像处理的滤镜算法

https://juejin.im/post/5b9e127df265da0a9a395e01

灰度滤镜

将颜色的RGB设置为相同的值即可使得图片为灰色，一般处理方法有： 1、取三种颜色的平均值 2、取三种颜色的最大值（最小值） 3、加权平均值：0.3*R + 0.59*G + 0.11*B

```
for(var i = 0; i < data.length; i+=4) {
     var grey = (data[i] + data[i+1] + data[i+2]) / 3;
     data[i] = data[i+1] = data[i+2] = grey;
}

复制代码
```

黑白滤镜

顾名思义，就是图片的颜色只有黑色和白色，可以计算rgb的平均值arg，arg>=100，r=g=b=255，否则均为0

```
for(var i = 0; i < data.length; i += 4) {
     var avg = (data[i] + data[i+1] + data[i+2]) / 3;
     data[i] = data[i+1] = data[i+2] = avg >= 100 ? 255 : 0;
}

```

反向滤镜

就是RGB三种颜色分别取255的差值。

```
for(var i = 0; i < data.length; i+= 4) {
      data[i] = 255 - data[i];
      data[i + 1] = 255 - data[i + 1];
      data[i + 2] = 255 - data[i + 2];
}
复制代码
```

去色滤镜

rgb三种颜色取三种颜色的最值的平均值。

```
for(var i = 0; i < data.length; i++) {
   var avg = Math.floor((Math.min(data[i], data[i+1], data[i+2]) + Math.max(data[i], data[i+1], data[i+2])) / 2 );
   data[i] = data[i+1] = data[i+2] = avg;
}
复制代码
```

单色滤镜

就是只保留一种颜色，其他颜色设为0

```
for(var i = 0; i < canvas.height * canvas.width; i++) {
    data[i*4 + 2] = 0;
    data[i*4 + 1] = 0;
}
复制代码
```

高斯模糊滤镜

高斯模糊的原理就是根据正态分布使得每个像素点周围的像素点的权重不一致，将各个权重（各个权重值和为1）与对应的色值相乘，所得结果求和为中心像素点新的色值。

```
function gaussBlur(imgData, radius, sigma) {
    var pixes = imgData.data,
        height = imgData.height,
        width = imgData.width,
        radius = radius || 5;
        sigma = sigma || radius / 3;
    
    var gaussEdge = radius * 2 + 1;

    var gaussMatrix = [],
        gaussSum = 0,
        a = 1 / (2 * sigma * sigma * Math.PI),
        b = -a * Math.PI;
    
    for(var i = -radius; i <= radius; i++) {
        for(var j = -radius; j <= radius; j++) {
            var gxy = a * Math.exp((i * i + j * j) * b);
            gaussMatrix.push(gxy);
            gaussSum += gxy;
        }
    }
    var gaussNum = (radius + 1) * (radius + 1);
    for(var i = 0; i < gaussNum; i++) {
        gaussMatrix[i] /= gaussSum;
    }

    for(var x = 0; x < width; x++) {
        for(var y = 0; y < height; y++) {
            var r = g = b = 0;
            for(var i = -radius; i<=radius; i++) {
                var m = handleEdge(i, x, width);
                for(var j = -radius; j <= radius; j++) {
                    var mm = handleEdge(j, y, height);
                    var currentPixId = (mm * width + m) * 4;
                    var jj = j + radius;
                    var ii = i + radius;
                    r += pixes[currentPixId] * gaussMatrix[jj * gaussEdge + ii];
                    g += pixes[currentPixId + 1] * gaussMatrix[jj * gaussEdge + ii];
                    b += pixes[currentPixId + 2] * gaussMatrix[jj * gaussEdge + ii];
                }
            }
            var pixId = (y * width + x) * 4;

            pixes[pixId] = ~~r;
            pixes[pixId + 1] = ~~g;
            pixes[pixId + 2] = ~~b;
        }
    }
    imgData.data = pixes;
    return imgData;
}

function handleEdge(i, x, w) {
    var m = x + i;
    if(m < 0) {
        m = -m;
    } else if(m >= w) {
        m = w + i -x;
    }
    return m;
}
复制代码
```

怀旧滤镜

怀旧滤镜公式 

```
for(var i = 0; i < imgData.height * imgData.width; i++) {
    var r = imgData.data[i*4],
        g = imgData.data[i*4+1],
        b = imgData.data[i*4+2];

    var newR = (0.393 * r + 0.769 * g + 0.189 * b);
    var newG = (0.349 * r + 0.686 * g + 0.168 * b);
    var newB = (0.272 * r + 0.534 * g + 0.131 * b);
    var rgbArr = [newR, newG, newB].map((e) => {
        return e < 0 ? 0 : e > 255 ? 255 : e;
    });
    [imgData.data[i*4], imgData.data[i*4+1], imgData.data[i*4+2]] = rgbArr;
}
复制代码
```

熔铸滤镜

公式： r = r*128/(g+b +1); g = g*128/(r+b +1); b = b*128/(g+r +1);

```
for(var i = 0; i < imgData.height * imgData.width; i++) {
    var r = imgData.data[i*4],
        g = imgData.data[i*4+1],
        b = imgData.data[i*4+2];

    var newR = r * 128 / (g + b + 1);
    var newG = g * 128 / (r + b + 1);
    var newB = b * 128 / (g + r + 1);
    var rgbArr = [newR, newG, newB].map((e) => {
        return e < 0 ? 0 : e > 255 ? 255 : e;
    });
    [imgData.data[i*4], imgData.data[i*4+1], imgData.data[i*4+2]] = rgbArr;
}
复制代码
```

冰冻滤镜

公式： r = (r-g-b)*3/2; g = (g-r-b)*3/2; b = (b-g-r)*3/2;

```javascript
for(var i = 0; i < imgData.height * imgData.width; i++) {
    var r = imgData.data[i*4],
        g = imgData.data[i*4+1],
        b = imgData.data[i*4+2];

    var newR = (r - g -b) * 3 /2;
    var newG = (g - r -b) * 3 /2;
    var newB = (b - g -r) * 3 /2;
    var rgbArr = [newR, newG, newB].map((e) => {
        return e < 0 ? 0 : e > 255 ? 255 : e;
    });
    [imgData.data[i*4], imgData.data[i*4+1], imgData.data[i*4+2]] = rgbArr;
}
```

连环画滤镜

公式： R = |g – b + g + r| * r / 256

G = |b – g + b + r| * r / 256;

B = |b – g + b + r| * g / 256;

```javascript
for(var i = 0; i < imgData.height * imgData.width; i++) {
    var r = imgData.data[i*4],
        g = imgData.data[i*4+1],
        b = imgData.data[i*4+2];

    var newR = Math.abs(g - b + g + r) * r / 256;
    var newG = Math.abs(b -g + b + r) * r / 256;
    var newB =  Math.abs(b -g + b + r) * g / 256;
    var rgbArr = [newR, newG, newB];
    [imgData.data[i*4], imgData.data[i*4+1], imgData.data[i*4+2]] = rgbArr;
}
```

褐色滤镜

公式： r = r * 0.393 + g * 0.769 + b * 0.189; g = r * 0.349 + g * 0.686 + b * 0.168; b = r * 0.272 + g * 0.534 + b * 0.131;

```javascript
for (var i = 0; i < imgData.height * imgData.width; i++) {
    var r = imgData.data[i * 4],
        g = imgData.data[i * 4 + 1],
        b = imgData.data[i * 4 + 2];

    var newR = r * 0.393 + g * 0.769 + b * 0.189;
    var newG = r * 0.349 + g * 0.686 + b * 0.168;
    var newB =  r * 0.272 + g * 0.534 + b * 0.131;
    var rgbArr = [newR, newG, newB];
    [imgData.data[i * 4], imgData.data[i * 4 + 1], imgData.data[i * 4 + 2]] = rgbArr;
}

```

## HTML5

## JSON

## Ajax和Cornet

## 高级

