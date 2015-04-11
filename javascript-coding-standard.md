## 1 前言

本文档的目标是使javascript代码风格保持一致，容易被理解和被维护。

虽然本文档是针对javascript设计的，但是在使用各种javascript的预编译器(如coffescript等)
时，适用的部分也应尽量遵循本文档的约定。

## 2 代码风格

### 2.1 文件

#### [建议] `CSS` 文件使用无 `BOM` 的 `UTF-8` 编码。

解释：

UTF-8 编码具有更广泛的适应性。BOM 在使用程序或工具处理文件时可能造成不必要的干扰。

### 2.2 结构

#### 2.2.1 缩进

##### [强制] 使用 `4` 个空格做为一个缩进层级，不允许使用 `2` 个空格 或 `tab` 字符。

##### [强制] switch 下的 case 和 default 必须增加一个缩进层级。

示例：

```javascript
// good
switch (variable) {

    case '1':
        // do...
        break;

    case '2':
        // do...
        break;

    default:
        // do...

}

// bad
switch (variable) {

case '1':
    // do...
    break;

case '2':
    // do...
    break;

default:
    // do...

}

```

##### [强制] `链式调用`较长时采用`缩进`进行调整。

```javascript

$('#items')
    .find('.selected')
    .highlight()
    .end();

```

#### 2.2.2 分号

##### [强制] 所有javascript表达式一律以`分号`结束，不可以省略。

#### 2.2.3 换行

##### [强制] 每个`独立语句`结束后必须换行。

##### [强制] 每行不得超过 `120` 个字符。

解释：超长的不可分割的代码允许例外，比如复杂的正则表达式。长字符串不在例外之列。


##### [强制] `方法`之间添加空行。

##### [强制] `单行`或`多行注释`前添加空行。

##### [建议] `逻辑块`之间应添加空行增加可读性。

示例：

```javascript
// 仅为按逻辑换行的示例，不代表setStyle的最优实现
function setStyle(element, property, value) {
    if (element == null) {
        return;
    }

    element.style[property] = value;
}
```

##### [强制] 在表示`语句块`开始的`{`之前不换行，在表示`语句块`结束的`}`后换行。

解释：特别的，对于 `if...else...`、`try...catch...finally` 等语句，在 `}` 
之后不换行，而是添加一个空格。

示例：

```javascript
function funcName() {
    // some statements;
}

if (...) {
    // some statements;
}

if (condition) {
    // some statements;
} else {
    // some statements;
}

try {
    // some statements;
} catch (ex) {
    // some statements;
}

```


##### [强制] `运算符`处换行时，`运算符`必须在新行的`行首`。

示例：

```javascript
// good
if (user.isAuthenticated()
    && user.isInRole('admin')
    && user.hasAuthority('add-admin')
    || user.hasAuthority('delete-admin')
) {
    // Code
}

var result = number1 + number2 + number3
    + number4 + number5;


// bad
if (user.isAuthenticated() &&
    user.isInRole('admin') &&
    user.hasAuthority('add-admin') ||
    user.hasAuthority('delete-admin')) {
    // Code
}

var result = number1 + number2 + number3 +
    number4 + number5;
```

##### [强制] 在`函数声明`、`函数表达式`、`函数调用`、`对象创建`、`数组创建`、`for语句`等场景中，不允许在 `,` 或 `;` 前换行。

示例：

```javascript
// good
var obj = {
    a: 1,
    b: 2,
    c: 3
};

foo(
    aVeryVeryLongArgument,
    anotherVeryLongArgument,
    callback
);


// bad
var obj = {
    a: 1
    , b: 2
    , c: 3
};

foo(
    aVeryVeryLongArgument
    , anotherVeryLongArgument
    , callback
);
```

##### [建议] 对于`HTML片段`的拼接，通过`换行`和`缩进`，保持和HTML相同的结构。

示例

```javascript
var html = '' // 此处用一个空字符串，以便整个HTML片段都在新行严格对齐
    + '<article>'
    +     '<h1>Title here</h1>'
    +     '<p>This is a paragraph</p>'
    +     '<footer>Complete</footer>'
    + '</article>';
```


#### 2.2.5 空格

##### [强制] `二元运算符`两侧必须有`一个空格`，`一元运算符`与`操作对象`之间不允许有空格。

示例：

```javascript
var a = !arr.length;
a++;
a = b + c;
```

##### [强制] 用作`代码块`起始的左花括号 `{` 前必须有一个空格。

示例：

```javascript
// good
if (condition) {
}

while (condition) {
}

function funcName() {
}

// bad
if (condition){
}

while (condition){
}

function funcName(){
}
```

##### [强制] `if / else / for / while / function / switch / do / try / catch / finally` 关键字后，必须有一个空格。

示例：

```javascript
// good
if (condition) {
}

while (condition) {
}

(function () {
})();

// bad
if(condition) {
}

while(condition) {
}

(function() {
})();
```

##### [强制] 在对象创建时，属性中的 `:` 之后必须有空格，`:` 之前不允许有空格。

示例：

```javascript
// good
var obj = {
    a: 1,
    b: 2,
    c: 3
};

// bad
var obj = {
    a : 1,
    b:2,
    c :3
};
```

##### [强制] `函数声明`、`具名函数表达式`、`函数调用中`，`函数名`和 `(` 之间`不允许`有空格。

示例：

```javascript
// good
function funcName() {
}

funcName();

// bad
function funcName () {
}

funcName ();
```

##### [强制] 使用`函数表达式`声明函数或使用`匿名函数`时，关键词`function`之后应有空格。

示例：

```javascript
// good
var funcName = function () {
};
var value = (function () {
}());

// bad
var funcName = function() {
};
var value = (function() {
}());
```

##### [强制] `,` 和 `;` 前不允许有空格，`,`之后应有一个空格。

示例：

```javascript
// good
callFunc(a, b);

// bad
callFunc(a , b) ;

callFunc(a,b);
```

##### [强制] 在函数调用、函数声明、括号表达式、属性访问、`if / for / while / switch / catch` 等语句中，`()` 和 `[]` 内紧贴括号部分不允许有空格。

示例：

```javascript
// good

callFunc(param1, param2, param3);

save(this.list[this.indexes[i]]);

needIncream && (variable += increament);

if (num > list.length) {
}

while (len--) {
}


// bad

callFunc( param1, param2, param3 );

save( this.list[ this.indexes[ i ] ] );

needIncreament && ( variable += increament );

if ( num > list.length ) {
}

while ( len-- ) {
}
```

##### [强制] 单行声明的`数组`与`对象`，如果包含元素，`{}` 和 `[]` 内紧贴括号部分`不允许`包含空格。

解释：

声明包含元素的数组与对象，只有当内部元素的形式较为简单时，才允许写在一行。元素复杂的情况，还是应该换行书写。


示例：

```javascript
// good
var arr1 = [];
var arr2 = [1, 2, 3];
var obj1 = {};
var obj2 = {name: 'obj'};
var obj3 = {
    name: 'obj',
    age: 20,
    sex: 1
};

// bad
var arr1 = [ ];
var arr2 = [ 1, 2, 3 ];
var obj1 = { };
var obj2 = { name: 'obj' };
var obj3 = {name: 'obj', age: 20, sex: 1};
```

##### [强制] `行尾`不得有多余的空格。

### 2.3 命名

#### 2.3.1 命名风格

##### [强制] 变量使用`下划线`命名法。

示例：

```javascript
var value_name_like_this;
```
##### [强制] 函数名使用`Camel`命名法

```javascript
function funcNameLikeThis(){
    //code....
}
```

##### [强制] `常量命名`全部字母`大写`，单词间用`下划线`分隔。

示例：

```javascript
var CONSTANT_NAME_LIKE_THIS;
```

##### [强制] `函数`的`参数`使用`下划线`命名法。

示例：

```javascript
function funcName(param_name_like_this) {
    //code....
}
```


##### [强制] `类名`（构造函数）使用`Pascal`命名法。

示例：

```javascript
function ClassNameLikeThis(){
    //code....
}
```

##### [强制] `类的方法`（属性）使用`Camel`命名法。

示例：

```javascript
function ClassNameLikeThis(){
    this.valueNameLikeThis = value;
    this.functionInClass = function () {
    //code....
    }
}
```

##### [强制] `枚举变量`使用`Pascal`命名法，枚举的`属性`使用`全部字母大写`，单词间`下划线分隔`的命名方式。

示例：

```javascript
var TargetState = {
    READING: 1,
    READED: 2,
    APPLIED: 3,
    READY: 4
};
```

##### [强制] `命名空间`使用`Camel`命名法。

示例：

```javascript
var nameSpace = {};
```

##### [强制] 由多个单词组成的`缩写词`，在命名中，根据当前命名法和出现的位置，所有字母的大小写与首字母的大小写`保持一致`。

示例：

```javascript
function XMLParser() {
}

function insertHTML(element, html) {
}

var httpRequest = new HTTPRequest();
```

#### 2.3.2 命名语义

##### [强制] `类名`使用`名词`。

示例：

```javascript
function Engine(options) {
}
```

##### [建议] `函数名` 使用 `动宾短语`。

示例：

```javascript
function getStyle(element) {
}
```

##### [建议] `boolean` 类型的变量使用 `is` 或 `has` 开头。

示例：

```javascript
var isReady = false;
var hasMoreCommands = false;
```

##### [建议] `Promise对象` 用 `动宾短语的进行时` 表达。

示例：

```javascript
var loadingData = ajax.get('url');
loadingData.then(callback);
```
