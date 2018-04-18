### 1、语法格式
同css代码
```javascript
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}
```
scss:
```javascript
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```
sass:
```javascript
$font-stack: Helvetica, sans-serif
$primary-color: #333

body 
  font: 100% $font-stack
  color: $primary-color
```
### 2、命令编译
单文件编译：
```javascript
sass <要编译的Sass文件路径>/style.scss:<要输出CSS文件路径>/style.css
```
多文件编译
```javascript
sass sass/:css/
//上面的命令表示将项目中“sass”文件夹中所有“.scss”(“.sass”)文件编译成“.css”文件，并且将这些 CSS 文件都放在项目中“css”文件夹中
```


### 3、基本特征
##### 3.1声明变量
Sass 的变量包括三个部分：
```javascript
声明变量的符号“$”
变量名称
赋予变量的值
```
##### 3.2普通变量与默认变量
- 普通变量
```javascript
$fontSize: 12px;
body{
    font-size:$fontSize;
}
```
编译后
```javascript
body{
    font-size:12px;
}
```
- 默认变量
sass 的默认变量仅需要在值后面加上 !default 
```javascript
$baseLineHeight:1.5 !default;
body{
    line-height: $baseLineHeight; 
}
```
编译后
```javascript
body{
    line-height:2;
}
```
##### 3.3变量的调用
比如在定义了变量
```javascript
$brand-primary : darken(#428bca, 6.5%) !default; // #337ab7
$btn-primary-color: #fff !default;
$btn-primary-bg : $brand-primary !default;
$btn-primary-border : darken($btn-primary-bg, 5%) !default;
```
在按钮 button 中调用，可以按下面的方式调用
```javascript
.btn-primary {
   background-color: $btn-primary-bg;
   color: $btn-primary-color;
   border: 1px solid $btn-primary-border;
}
```
编译为
```javascript
.btn-primary {
  background-color: #337ab7;
  color: #fff;
  border: 1px solid #2e6da4;
}
```


