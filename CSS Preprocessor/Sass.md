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

