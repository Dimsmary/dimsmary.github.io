---
layout: post
title: JavaScript 概念入门
categories: [blog ]
tags: [JavaScript, Coding, ]
description: "JavaScript 基本知识点"
image:
  feature: windows.jpg
  credit: Azeril
  creditlink: azeril.com
---



## 概述

- 网景公司 1995 年发布 JavaScript;
- ECMAScript（European Computer Manufacturers Association）是一种语言标准，而 JavaScript 是网景公司对该标准的实现方式。最新的 ES6 发布于 2015 年 6 月。

JavaScript 能直接嵌入在网页任何地方，但通常放在网页的 <head> 中，由 <script></script> 包裹，能被浏览器直接执行。它也可以放在单独 .js 文件，通过 <script src="..."></script> 引入。

编写 JavaScript：使用诸如 Sublime Text 一类的编辑器。
运行：要让浏览器运行代码，需先有一个 HTML 页面，在浏览器中加载引入了 JavaScript 代码的页面，就可以执行代码。最初的学习阶段，不用管开发环境搭建的问题，直接在浏览器，比如 Chrome 提供的 Develop Tool 下的 控制台（Console）就可以运行程序代码了。

### 基本语法

JavaScript 的每个语句以` ；`结束，语句块以`{…}`来囊括。

赋值方式 `var = xx；`。

语句块中的语句缩进 4 空格，这不是 JavaScript 语法要求的，但可以让语法更直观易读。语句块的语句可以嵌套，形成层级结构。

语法的注释方式是将 `//` 放一行的开头，并在行末用同样的符号收尾。另一种注释方式是 `/*…*/` 包裹语句块或行。

### Data Type 数据类型 

* Number

- 整数
- 浮点数 0.42
- 科学计数法 3.14e
- 负数
- NaN Not a Number 当无法计算结果时的表达方式
- Infinity 超过 JavaScript 的数值能表达的最大值，用 Infinity 来表示无限大

Number 可以直接做基本的四则运算。（加 + 减 - 乘* 除 / 取余 %）

* 字符串

使用单引号或双引号括起的任意文本

* 布尔值

一个布尔值只有 true 或 false 两种表达。布尔值常用于条件判断中。

```
2 > 1; // true  
42 >= 43 // false  
```

在 `&&` （与）运算作判断时，只有相关条件都满足，运算结果才为 `true`。

```
true && true // ture  
true && false // false
```

在 `||` （或）运算时，只要满足一个条件为正确，输出即为 `true`。
在 `!` （非）运算时，原有的结果互相转换，`true` 变成 `false`，反之亦然。

* 比较运算符

在对 Number 做比较时，可以通过比较运算符获得布尔值，除此之外，JavaScript 也可以比较任意数据类型。（`===` 会比较数据类型，而 `==` 在比较时会转换数据类型，导致一些错误结果，这是 JavaScript 设计的问题。）

```
false == 0; //  true
false ===0; //  false
```

`Nan` 的判断方式，在比较时，它不与任何值相等，包括自身。唯一能判断的方式是用 `isNaN()` 函数。

```
isNaN(NaN); // true
```

浮点数的相等比较，浮点数在运算过程中会产生误差，计算机无法精确表示无限循环小数。

```
1 / 3 === (1 - 2 / 3); // false
```

要比较两个浮点数是否相等，只能计算它们之差的绝对值，看是否小于某个阈值：

```
Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true
```

* null 和 undefined

`null` 表示空值。与 0 或 `''` 空字符串不同。0 为数值，`''` 表示长度为零的字符串，`null` 表示为「空」。JavaScript 下还有一个类似的 `undefined`，表示为「未定义」。一般区分两者的意义不大，多数情况下使用前者，仅在判断函数参数是否传递时，用 `undefined`。

* Array 数组 

数组是一组按顺序排列的集合，用 `[]` 表示，其中的每个值称为元素，以 `,` 分隔。JavaScript 的数组可以包含任意数据类型。另一种创建数组的方式是通过 `Array()` 函数来实现：`new Array(1, 2, 4);`

数组的元素可以通过索引来访问，索引的起始值为 0。

* Object 对象 

对象是由一组由键-值组成的无需集合。对象的键都是字符串类型，值，或者说对象的属性，可以是任意数据类型。

```
var person = {
    name: 'Andy' ,
    age: 24,
    tages: ['js', 'python'],
};
```

对象属性，用 `object.property_name` 的方式来获取。类似，`person.name`。

* 变量

变量不止是数字，也可以是任意数据类型。变量名是英文（包括大小写）、数字、「$」和「_」的组合。但组合时开头不用用数字，变量名也不能是 JavaScript 的关键字，如「if」、「while」等

在语法中，使用等号 `=` 对变量赋值，可以是任意数据类型。同一变量可以反复赋值（这种变量本身类型不固定的语言称之为动态语言，与之对应的是静态语言。），且可为不同类型，但只能用 `var` 申明一次。

```
var a = 42; // Number  
a = “ABC” // 字符串  
```

* Strict 模式

如果某一变量没有通过 `var` 申明就被使用，那么该变量就自动被申明为全局变量。而使用了 `var` 申明的变量则不是全局变量，它的范围被限制在该变量被申明的函数中，同名变量在不同函数中互不冲突。

因为早期 JavaScript 在设计时并不强制要求使用 `var`，后来为修补这一缺陷，ECMA 在后续规范中推出了 `strict` 模式。启用的方式是，在 JavaScript 代码的首行中标注上 `'use strict';` 这样一段字符串。当它运行时，会强制通过 `var` 申明变量，未使用就使用的话，将导致运行错误。

### String 字符串

字符串使用 `''` 或 `”"` 括起的字符表示。如「'」本身就是字符，可以用双引号括起。如果也包含双引号，则使用转义符`\` 标识。

> 'I\'m \"OK";

转义符的用途，`\n` 表示换行，`\t` 表示制表符，如果「\」本身也要转义，则用 `\\` 表示。

* 多行字符串

一般以 `\n` 书写，因为费事，最新的 ES6 新增使用反引号 \`…\` 的多行字符串表示法。

* 模板字符串

多个字符串使用 `+` 连接。ES6 新增了一种模板字符串，使用方法类似多行字符串，但它会自动替换字符串变量。

* 操作字符串

常见的字符串操作：

```
var word =  "Hello,world!";  
var.length;
```

获取字符串某指定位置的字符（注意索引从 0 开始）。字符串是不可变的，不能通过索引的方式去重新赋值，虽然那样做不会报错。

字符串操作：

- `toUpperCase()` 将一个字符串全部变为大写；
- `toLowerCase()` 全部变为小写；
- `indexOf()`  搜索特定字符串出现位置；
- `substring()` 返回指定索引区间的子串；


### Array 数组

Array 可以包含任意数据类型，并通过索引来访问每个元素。

使用 `length` 属性可获取 `Array` 长度。如果直接给 `Array` 的 `length` 赋新值，会导致其大小变化。可以通过索引将对应的元素修改为新值。而当赋值时索引超出范围，`Array` 大小依然会变化。只不过 JavaScript 不会报错。编写代码时，不建议直接修改 Array 大小，访问索引时确保索引不超范围。

- `indexOf` 用于搜索 `Array` 中指定元素的位置；
- `slice` 对应 String 的 `substring` 的的版本，截取元素并返回新的 `Array`。
- `push` & `pop` 前者向`Array` 的末端添加元素，后者则用于将最后一个元素移除掉。
- unshift & shift 前者，往 `Array` 头部添加元素，后者则是移除同位置的元素；
- `sort` 排序。直接调用，按照默认顺序进行排序；
- `reverse` 反转元素；
- `splice` 从指定索引删除元素，再从该位置开始添加元素；
- `concat` 将当前 `Array` 与另一个 `Array` 连接，并返回一个新的；
- `join` 将当前 `Array` 的元素用指定字符串连起来，返回连接后的字符串；

`Array` 可以是多层嵌套的，即 `Array` 可以囊括其它的 `Array`。


### 对象 Object

JavaScript 的对象是无序集合数据类型，由若干键值构成。对象用于描述现实中的某对象。对象用 `{…}` 表示，键值则以 `xxx: xxx` 的形式申明，各个键值以 `,` 间隔，最后一个键值末尾不需添加。

通过对象和属性名获取属性的方法：`objectName.propertyName`。访问属性通过 `.` 操作符完成。属性名称如包含特殊字符，必须用 `''` 括起。因为 JavaScript 的对象是动态类型，可以自由地为对象添加和删除属性。

> objectName.propertyName = xxx;
> delete `objectName.propertyName`;

使用 `in` 可以监测某个对象是否拥有某一属性。`in` 判断属性存在，属性不一定属于该对象，也可能是该对象继承得来的。（`toString` 的例子）判断某以属性是否为对象自身拥有而非继承，可以用 `hasOwnProperty()` 函数来确认。

objectName.hasOwnProperty('propertyName');

书写代码的时候，属性名应尽量用标准变量名。

### Judgment 条件判断

条件判断的语句： if () {…} else {…} 。特点是执行时候二选一，如果某一条件成立，就不再继续判断。

如果语句块只包含一个语句，`{}` 可以省略。不过这在实际操作中可能导致后续添加语句，遗忘了添加 `{}`，判断语句也会出现问题。一般建议都写上。

多行判断

`if …else…` 组合使用。

if () {…}; else if () {…}; else {…};

条件判断的顺序很重要。

当 if 语句条件判断结果不是 `true` 或 `false` 时，JavaScript 如何判定。null、undefined、0、NaN 和空字符串都会被视为 `false`，其他值一概视为 `true`。


### Loop 循环

循环

两种循环：`for` 和 `while`

`for` 循环，通过初始条件，结束条件和递增条件来循环执行语句块。

`for` 循环最常用于利用索引来遍历数组。需要注意，`for` 循环的 3 个条件都是可省略的，如无退出循环的判断条件，就必须用 `break` 语句退出循环。

```
var x = 0;
for (;;) { // 将无限循环下去
    if (x > 100) {
        break; // 通过if判断来退出循环
    }
    x ++;
}
```

变体，`for…in` 用于将一个对象所有属性依次循环出来（搭配 `hasOwnProperty()` ，实现过滤掉对象继承的属性）。

```
var Andy = {

 name: 'Andy',
 birth: 1989,
 school: 'Haining No.1 middle school',
 height: 1.70, 
 weight: 55,
 score: null
};

for (var key in Andy) {
 if (Andy.hasOwnProperty(key)) {
 console.log(key);
 }
}
```

由于 `Array` 也是对象，而它的元素的索引被视为对象的属性，因此，`for...in` 循环可直接循环出 `Array` 的索引。

while

`while` 循环只有一个判断条件，满足条件则不断循环，不满足时就退出。

`do...while` 循环与 `while` 循环唯一的区别在于，不是在每次循环开始时判断条件，而是在每次循环完成后进行判断。也因此，`do...while` 循环至少会被执行依次，而 `for` 和 `while` 循环则可能一次都不执行。

```
var i = 0；
do {
   i = i + 1; 
} while (n < 42);```
console.log(n);
```


## Map & Set

JavaScript 的对象有个缺陷，对象的键必须为字符串，而现实的使用中整数或其它数据类型作为键也是合理的。由此，ES6 引入了新的数据类型 `Map`。

`Map` 是一组键值对（key-value）的结构，对查找来说效率很高。 

```
`Map`: `var x = new Map();`  // 新建 
x.set('key',value);  // 添加
x.has('key'); // 查询是否存在
x.get()  // 获取值
x.delete() // 删除 key
```

```
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
for (var x of m) { // 遍历Map
    alert(x[0] + '=' + x[1]);
}
```


一个 key 只能对应一个值，设定新值会覆盖掉原值。

Set 

`Set` 与 `Map` 类似，是一组 key 的集合，但不存储 Value。此外，由于 Key 不能重复，`Set` 里没重复的 key。

```
var x = new Set(); // 空 Set
x.add(); // 新增 key
x.delete(); // 删除
```

### iterable

ES6 新引入的 `Map` 和 `Set` 无法像 `Array` 一样通过下标循环来循环。为统一集合类型，引入了 `iterable` 类型，通用于 `Array`、`Map` 和 `Set`。具备此类型的集合可通过同样是新引入的 `for … of` 循环来遍历。

`for … of`` 与 `for … in` 循环的区别。

`for … in` 遍历的是对象属性名称，`Array` 数组也被视作对象，它的每个索引被视为一个个属性。在操作时，为 `Array` 对象添加额外属性后，使用 `for...in` 循环会带来一个问题，循环会把 `name` 包括在内，但 `Array` 的 `length` 属性却不会包括。`for...of` 循环的引入修复了这些问题。它只循环集合本身的元素。

```
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x of a) {
   console.log(x); 
}
```

* Array

```
var a = ['A', 'B', 'C'];
a.forEach(function (element, index, array) {
    // element: 指向当前元素的值
    // index: 指向当前索引
    // array: 指向Array对象本身
    alert(element);
});
```

* Set

```
var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {
    alert(element);
});
```

* Map

```
var m = new Map([['Microsoft', 4200],['Facebook', 3200],['Snap', 240],['Twitter', 134]]);
m.forEach(function (value, key, map) {console.log(value);});
```