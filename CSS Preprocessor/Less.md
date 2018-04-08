# Less
- From<a href="https://less.bootcss.com/" >快速入门Less</a>
### 语法
#### 1、变量
```javascript
@nice-blue: #5B83AD;
@light-blue: @nice-blue + #111;

#header {
  color: @light-blue;
}
```
编译为
```javascript
#header {
  color: #6c94be;
}
```
#### 2、混合
混合是一种将一组属性从一个规则集合（另一个规则集合）（“混入”）的方式。
例:
```javascript
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```
我们希望在其他规则集内使用这些属性。那么，我们只需要放下我们想要属性的类的名称，如下所示：
```javascript
#menu a {
  color: #111;
  .bordered;
}

.post a {
  color: red;
  .bordered;
}
//在的特性.bordered类现在会出现在#menu a和.post a。
```
#### 3、嵌套
Less使您能够使用嵌套代替或与级联结合使用。例:
```javascript
#header {
  color: black;
}
#header .navigation {
  font-size: 12px;
}
#header .logo {
  width: 300px;
}
```
生成的代码更简洁，并模仿HTML的结构,可这样写
```javascript
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```
也可以使用此方法将伪选择器与mixin捆绑在一起。这里是经典的clearfix hack，重写为mixin（&代表当前选择器父代）：
```javascript
.clearfix {
  display: block;
  zoom: 1;

  &:after {
    content: " ";
    display: block;
    font-size: 0;
    height: 0;
    clear: both;
    visibility: hidden;
  }
}
```
###### 嵌套的规则和冒泡
像规则一样，@media或者@supports可以像选择器一样嵌套。规则放在顶部，相对规则集中的其他元素的相对顺序保持不变。这叫做冒泡。
```javascript
.component {
  width: 300px;
  @media (min-width: 768px) {
    width: 600px;
    @media  (min-resolution: 192dpi) {
      background-image: url(/img/retina2x.png);
    }
  }
  @media (min-width: 1280px) {
    width: 800px;
  }
}
```
编译为：
```javascript
.component {
  width: 300px;
}
@media (min-width: 768px) {
  .component {
    width: 600px;
  }
}
@media (min-width: 768px) and (min-resolution: 192dpi) {
  .component {
    background-image: url(/img/retina2x.png);
  }
}
@media (min-width: 1280px) {
  .component {
    width: 800px;
  }
}
```
#### 4、运算
算术运算+，-，*，/可以在任意数量，颜色或可变的操作。如果可能的话，数学运算将单位考虑在内并在加，减或比较它们之前转换数字。结果最左边明确指出单位类型。如果转换不可行或不具有意义，则单位将被忽略。无法进行转换的示例：px为cm或rad为％。
```javascript
// numbers are converted into the same units
@conversion-1: 5cm + 10mm; // result is 6cm
@conversion-2: 2 - 3cm - 5mm; // result is -1.5cm

// conversion is impossible
@incompatible-units: 2 + 5px - 3cm; // result is 4px

// example with variables
@base: 5%;
@filler: @base * 2; // result is 10%
@other: @base + @filler; // result is 15%
```
乘法和除法不会转换数字。在大多数情况下，它不会有意义 - 长度乘以长度会产生一个区域，而css不支持指定区域。较少将按照原样操作数字，并将明确指定的单位类型分配给结果。
```javascript
@base: 2cm * 3mm; // result is 6cm
```
也可以对颜色进行反制：
```javascript
@color: #224488 / 2; //results in #112244
background-color: #112244 + #111; // result is #223355
```
###### calc() 特例
为了与 CSS 保持兼容，`calc()` 并不对数学表达式进行计算，但是在嵌套函数中会计算变量和数学公式的值。
```javascript
@var: 50vh/2;
width: calc(50% + (@var - 20px));  // result is calc(50% + (25vh - 20px))
```
#### 5、函数
Less 内置了多种函数用于转换颜色、处理字符串、算术运算等。这些函数在函数手册中有详细介绍。

函数的用法非常简单。下面这个例子将介绍如何利用`percentage`函数将 0.5 转换为 50%，将颜色饱和度增加 5%，以及颜色亮度降低 25% 并且色相值增加 8 等用法：
```javascript
@base: #f04615;
@width: 0.5;

.class {
  width: percentage(@width); // returns `50%`
  color: saturate(@base, 5%);
  background-color: spin(lighten(@base, 25%), 8);
}
```
#### 6、逃离
转义允许使用任何任意字符串作为属性或变量值。内部~"anything"或内部的任何内容~'anything'除插值外没有任何更改。
```javascript
@min768: ~"(min-width: 768px)";
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}
```
编译为
```javascript
@media (min-width: 768px) {
  .element {
    font-size: 1.2rem;
  }
}
```
#### 7、命名空间和访问器
有时候，可能想要将mixin分组，或为了组织目的，或者只是提供一些封装。可以在Less中很直观地做到这一点。想要将一些mixin和变量捆绑在一起#bundle，以便以后重用或分发：
```javascript
#bundle() {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover {
      background-color: white
    }
  }
  .tab { ... }
  .citation { ... }
}
```
现在，如果想.button在class中混合#header a，可以这样做：
```javascript
#header a {
  color: orange;
  #bundle > .button;  // 也可以写为#bundle.button
}
```
注意：追加()到你的名字空间，如果你不希望它出现在你的CSS输出即#bundle .tab。

还要注意，在命名空间中声明的变量将仅限于该命名空间，并且将不会在范围之外通过与用于引用mixin（#Namespace > .mixin-name）的语法相同的语法提供。所以，例如，你不能做到以下几点：#Namespace > @this-will-not-work。
#### 8、作用域
Less中的范围与编程语言非常相似。变量和mixin首先在本地查找，如果找不到，编译器将查找父范围，依此类推。
```javascript
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}
```
Mixin和变量定义不必放在引用它们的行之前。所以下面的Less代码和前面的例子是一样的：
```javascript
@var: red;

#page {
  #header {
    color: @var; // white
  }
  @var: white;
}
```
#### 9、注释
```javascript
/* 一个块注释
 * style comment! */
@var: red;

// 这一行被注释掉了！
@var: white;
```
#### 10、导入
“导入”的工作方式和你预期的一样。你可以导入一个 .less 文件，此文件中的所有变量就可以全部使用了。如果导入的文件是 .less 扩展名，则可以将扩展名省略掉：
```javascript
@import "library"; // library.less
@import "typo.css";
```

