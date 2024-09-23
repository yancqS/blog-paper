---
title: Sass入门学习
description: Sass入门学习
date: '2021-03-05'
cover: 'http://img.up-4ever.site/20210305230928.jpg'
top_img: 'http://img.up-4ever.site/infinity-12769278.jpg'
tags:
  - Sass
  - Scss
categories:
  - CSS
abbrlink: d2be4d57
---
# sass学习记录

## 前言

之前一直使用的`less`，也整理过一篇关于[less内置函数的文章](http://up-4ever.site/posts/1940b795/)，现在工作原因，需要了解一下`sass`。此篇文章记录一下学习过程，此篇为基础入门篇，后续会根据文档深入学习。

## 使用变量

sass使用`$`符号来标识变量。

### 变量的声明和引用

```scss
$highlight-clor: #F90;//声明变量
.selected {
    border: 1px solid $highlight-clor;//引用变量
}

//编译后

.selected {
    border: 1px solid #F90;
}
```

在声明变量时，变量值也可以引用其他变量。当你通过粒度区分，为不同的值取不同名字时，这相当有用。下例在独立的颜色值粒度上定义了一个变量，且在另一个更复杂的边框值粒度上也定义了一个变量：

```scss
$highlight-color: #F90;
$highlight-border: 1px solid $highlight-color;
.selected {
  border: $highlight-border;
}

//编译后

.selected {
  border: 1px solid #F90;
}
```

### 变量名是用中划线还是下划线分割

`sass`对这两种分割方法是相互兼容的。也就是说，用中划线声明的变量可以使用下划线的方式引用，反之亦然。

```scss
$link-color: blue;
a {
  color: $link_color;
}

//编译后

a {
  color: blue;
}
```

在上例中，$link-color和$link_color其实指向的是同一个变量。

## 嵌套CSS规则

css中重复写选择器是非常恼人的。如果要写一大串指向页面中同一块的样式时，往往需要 一遍又一遍地写同一个ID：

```css
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```

在Sass中，你可以像俄罗斯套娃那样在规则块中嵌套规则块。sass在输出css时会帮你把这些嵌套规则处理好，避免你的重复书写。

```scss
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  aside { background-color: #EEE }
}
```

编译后：

```css
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```

大多数情况下这种简单的嵌套都没问题，但是有些场景下不行，比如你想要在嵌套的选择器里边应用一个类似于：hover的伪类。为了解决这种以及其他情况，sass提供了一个特殊标识符`&`。

### 父选择器的标识符`&`

有一种情况是当你为链接之类的元素写:hover这种伪类时，你并不希望以后代选择器的方式连接。比如说，下面这种情况sass就**无法**正常工作：

```scss
article a {
  color: blue;
  :hover { color: red }
}
```

解决之道为使用一个特殊的sass选择器，即父选择器。在使用嵌套规则时，父选择器能对于嵌套规则如何解开提供更好的控制。它就是一个简单的&符号，且可以放在任何一个选择器可出现的地方:

```scss
article a {
  color: blue;
  &:hover { color: red }
}
```

当包含父选择器标识符的嵌套规则被打开时，它不会像后代选择器那样进行拼接，而是&被父选择器直接替换：

```css
article a { color: blue }
article a:hover { color: red }
```

父选择器标识符还有另外一种用法，你可以在父选择器之前添加选择器。举例来说，当用户在使用IE浏览器时，你会通过JavaScript在`<body>`标签上添加一个ie的类名，为这种情况编写特殊的样式如下：

```scss
#content aside {
  color: red;
  body.ie & { color: green }
}
```

编译后：

```css
#content aside {color: red};
body.ie #content aside { color: green }
```

### 群组选择器的嵌套

如果你需要在一个特定的容器元素内对这样一个群组选择器进行修饰，css的写法会让你在群组选择器中的每一个选择器前都重复一遍容器元素的选择器：

```css
.container h1, .container h2, .container h3 { margin-bottom: .8em }
```

sass的嵌套特性在这种场景下也非常有用。当sass解开一个群组选择器规则内嵌的规则时，它会把每一个内嵌选择器的规则都正确地解出来：

```scss
.container {
  h1, h2, h3 {margin-bottom: .8em}
}
```

编译后即为上面的普通css样式。

### 子组合选择器和同层组合选择器：`>`、`+`和`~`

组合选择器可以应用到sass的规则嵌套中。可以把它们放在外层选择器后边，或里层选择器前边：

```scss
article {
  ~ article { border-top: 1px dashed #ccc }
  > section { background: #eee }
  dl > {
    dt { color: #333 }
    dd { color: #555 }
  }
  nav + & { margin-top: 0 }
}
```

编译后：

```css
article ~ article { border-top: 1px dashed #ccc }
article > footer { background: #eee }
article dl > dt { color: #333 }
article dl > dd { color: #555 }
nav + article { margin-top: 0 }
```

### 嵌套属性

在sass中，除了CSS选择器，属性也可以进行嵌套。尽管编写属性涉及的重复不像编写选择器那么糟糕，但是要反复写`border-style`,`border-width`,`border-color`以及`border-*`等也是非常烦人的。在sass中，你只需敲写一遍border：

```scss
nav {
  border: {
    style: solid;
    width: 1px;
    color: #ccc;
  }
}
```

嵌套属性的规则是这样的：把属性名从中划线-的地方断开，在根属性后边添加一个冒号`:`，紧跟一个`{ }`块，把子属性部分写在这个`{ }`块中。

编译后：

```css
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
```

对于属性的缩写形式，你甚至可以像下边这样来嵌套，指明例外规则：

```scss
nav {
  border: 1px solid #ccc {
  left: 0px;
  right: 0px;
  }
}
```

编译后：


```css
nav {
  border: 1px solid #ccc;
  border-left: 0px;
  border-right: 0px;
}
```

## 导入SCSS文件

css有一个特别不常用的特性，即`@import`规则，它允许在一个css文件中导入其他css文件。然而，**后果是只有执行到`@import`时，浏览器才会去下载其他css文件，这导致页面加载起来特别慢**。

sass也有一个`@import`规则，但不同的是，sass的`@import`规则在生成css文件时就把相关文件导入进来。这意味着所有相关的样式被归纳到了同一个css文件中，而无需发起额外的下载请求。另外，所有在被导入文件中定义的变量和混合器均可在导入文件中使用。

使用sass的@import规则并不需要指明被导入文件的全名。你可以省略.sass或.scss文件后缀。

### 使用SCSS部分文件

当通过`@import`把sass样式分散到多个文件时，你通常只想生成少数几个css文件。那些专门为`@import`命令而编写的sass文件，并不需要生成对应的独立css文件，这样的sass文件称为**局部文件**。对此，sass有一个特殊的约定来命名这些文件。

此约定即，sass局部文件的文件名以下划线开头。这样，sass就不会在编译时单独编译这个文件输出css，而只把这个文件用作导入。当你`@import`一个局部文件时，还可以不写文件的全名，即省略文件名开头的下划线。举例来说，你想导入themes/_night-sky.scss这个局部文件里的变量，你只需在样式表中写`@import "themes/night-sky"`;。

### 默认变量值&嵌套导入

使用sass的`!default`标签可以实现变量默认值。

```scss
/* _blue-theme.scss */
$width: 400px !default;
aside {
    color: blue;
    width: $width;
    span {
        background: {
            color: #fff;
            repeat: no-repeat;
        };
    }
}

/* demo.scss */
$width: 250px; //有这个变量是width为250px，没有这个变量是width使用默认值400px
h1 {
    @import "blue-theme";
}
```

编译后：

```css
h1 aside {
  color: blue;
  width: 250px;
}

h1 aside span {
  background-color: #fff;
  background-repeat: no-repeat;
}
```

## 静默注释

sass另外提供了一种不同于css标准注释格式`/* ... */`的注释语法。**静默注释**的内容不会出现在生成的css文件中。静默注释的语法跟JavaScript、Java等类C的语言中单行注释的语法相同，它们以//开头，注释内容直到行末。

```scss
body {
  color: #333; // 这种注释内容不会出现在生成的css文件中
  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
}
```

## 混合器

如果你的整个网站中有几处小小的样式类似（例如一致的颜色和字体），那么使用变量来统一处理这种情况是非常不错的选择。但是当你的样式变得越来越复杂，你需要大段大段的重用样式的代码，独立的变量就没办法应付这种情况了。你可以通过sass的混合器实现大段样式的重用。

混合器使用`@mixin`标识符定义。混合器中不仅可以包含属性，也可以包含css规则，包含选择器和选择器中的属性。

```scss
@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}
```

然后就可以在你的样式表中通过@include来使用这个混合器，放在你希望的任何地方:

```scss
ul.plain {
  color: #444;
  @include no-bullets;
}
```

编译后：

```css
ul.plain {
  color: #444;
  list-style: none;
}
ul.plain li {
  list-style-image: none;
  list-style-type: none;
  margin-left: 0px;
}
```

### 给混合器传参

混合器并不一定总得生成相同的样式。可以通过在@include混合器时给混合器传参，来定制混合器生成的精确样式。

```scss
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```

当混合器被@include时，你可以把它当作一个css函数来传参。如果你像下边这样写

```css
a {
  @include link-colors(blue, red, green);
}

//Sass最终生成的是：

a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
```

当你`@include`混合器时，有时候可能会很难区分每个参数是什么意思，参数之间是一个什么样的顺序。为了解决这个问题，sass允许通过语法`$name: value`的形式指定每个参数的值。这种形式的传参，参数顺序就不必再在乎了，只需要保证没有漏掉参数即可：

```scss
a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}
```

## 选择器继承

使用sass的时候，最后一个减少重复的主要特性就是`选择器继承`。选择器继承是说一个选择器可以继承为另一个选择器定义的所有样式。这个通过`@extend`语法实现，如下代码：

```scss
//通过选择器继承继承样式
.error {
  border: 1px solid red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```
以`class="seriousError"` 修饰的html元素最终的展示效果就好像是`class="seriousError error"`。

.seriousError不仅会继承.error自身的所有样式，任何跟.error有关的组合选择器样式也会被.seriousError以组合选择器的形式继承，如下代码:

```scss
.error_1 {
  border: 1px solid red;
  background-color: #fdd;
}

h1 .error_1 {
  font-size: 1.2em;
}
.seriousError_1 {
  @extend .error_1;
  border-width: 3px;
}
```

编译后：


```css
.error_1, .seriousError_1 {
  border: 1px solid red;
  background-color: #fdd;
}

h1 .error_1, h1 .seriousError_1 {
  font-size: 1.2em;
}

.seriousError_1 {
  border-width: 3px;
}
```

### 继承的工作细节

跟变量和混合器不同，继承不是仅仅用css样式替换@extend处的代码那么简单。为了不让你对生成的css感觉奇怪，对这背后的工作原理有一定了解是非常重要的。

`@extend`背后最基本的想法是，如果`.seriousError @extend .error`， 那么样式表中的任何一处`.error`都用`.error.seriousError`这一选择器组进行替换。这就意味着相关样式会如预期那样应用到.error和.seriousError。

## 最后

简单入个门，了解语法和规则。一些进阶的功能会通过阅读[文档](https://www.sass.hk/docs/)来了解。

## 参考文章
- [如何安装Sass](https://www.sass.hk/install/)
- [Visual Studio Code插件Live Sass Compiler可将Sass或Scss实时编译为CSS](https://www.sass.hk/skill/sass154.html)
- [sass快速入门](https://www.sass.hk/guide/)