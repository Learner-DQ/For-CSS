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
