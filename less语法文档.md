# Overview 概述

> An in-depth guide to features of the LESS language. See the [Overview](https://less.bootcss.com/#overview) for a quick summary of Less.
> 深入了解LESS语言的特性。有关Less的快速摘要，请参阅概述。

*For an in-depth guide to installing and setting up a Less environment, as well as documentation on developing for Less, see: Using Less.js.*
有关安装和设置Less环境的深入指南，以及关于为Less开发的文档，请参见:Using Less.js。



# Variables变量

> Control commonly used values in a single location.
> 控件在单个位置中常用的值。

### Overview概述

It's not uncommon to see the same value repeated dozens *if not hundreds of times* across your stylesheets:
相同的值在样式表中重复出现几十次甚至上百次也很常见:

```less
a,
.link {
  color: #428bca;
}
.widget {
  color: #fff;
  background: #428bca;
}
```

Variables make your code easier to maintain by giving you a way to control those values from a single location:
变量通过提供一种从单一位置控制这些值的方法使代码更容易维护:

```
// Variables
@link-color:        #428bca; // sea blue
@link-color-hover:  darken(@link-color, 10%);

// Usage
a,
.link {
  color: @link-color;
}
a:hover {
  color: @link-color-hover;
}
.widget {
  color: #fff;
  background: @link-color;
}
```

### Variable Interpolation变量插值

The examples above focused on using variables to control *values in CSS rules*, but they can also be used in other places as well, such as selector names, property names, URLs and `@import` statements.
上面的例子着重于在CSS规则中使用变量来控制值，但是它们也可以在其他地方使用，例如选择器名称、属性名称、url和`@import`语句。

#### Selectors选择器

*v1.4.0*

```less
// Variables变量
@my-selector: banner;

// Usage用法
.@{my-selector} {
  font-weight: bold;
  line-height: 40px;
  margin: 0 auto;
}
```

Compiles to:编译: 

```
.banner {
  font-weight: bold;
  line-height: 40px;
  margin: 0 auto;
}
```

#### URLs资源引用

```less
// Variables变量
@images: "../img";

// Usage用法
body {
  color: #444;
  background: url("@{images}/white-sand.png");
}
```

#### Import Statements导入语句

*v1.4.0*

Syntax: `@import "@{themes}/tidal-wave.less";`
语法:`@ import“@ {主题} / tidal-wave.less”;`

Note that before v2.0.0, only variables which have been declared in the root or current scope were considered and that only the current file and calling files were considered when looking for a variable.
注意，在v2.0.0之前，只考虑在根或当前作用域中声明的变量，并且在查找变量时只考虑当前文件和调用文件。

Example:例子:

```less
// Variables变量
@themes: "../../src/themes";

// Usage引用
@import "@{themes}/tidal-wave.less";
```

#### Properties属性

*v1.6.0*

```less
@property: color;

.widget {
  @{property}: #0ee;
  background-@{property}: #999;
}
```

Compiles to:编译：

```less
.widget {
  color: #0ee;
  background-color: #999;
}
```

### Variable Variables变量的变量

In Less, you can define a variable's name using another variable.
在Less中，您可以使用另一个变量定义变量的名称。

```less
@primary:  green;
@secondary: blue;

.section {
  @color: primary;

  .element {
    color: @@color;
  }
}
```

compiles to:编译：

```less
.section .element {
  color: green;
}
```

### Lazy Evaluation懒惰的评价

> Variables do not have to be declared before being used.
> 变量在使用之前不必声明。

Valid Less snippet:有效减少代码片段:

```
.lazy-eval {
  width: @var;
}

@var: @a;
@a: 9%;
```

this is valid Less too:这也是不太有效的:

```
.lazy-eval {
  width: @var;
  @a: 9%;
}

@var: @a;
@a: 100%;
```

both compile into:这两个编译成: 

```
.lazy-eval {
  width: 9%;
}
```

When defining a variable twice, the last definition of the variable is used, searching from the current scope upwards. This is similar to css itself where the last property inside a definition is used to determine the value.
当定义一个变量两次时，将使用该变量的最后一个定义，从当前范围向上搜索。这类似于css本身，定义中的最后一个属性用于确定值。

For instance:例如:

```less
@var: 0;
.class {
  @var: 1;
  .brass {
    @var: 2;
    three: @var;
    @var: 3;
  }
  one: @var;
}
```

Compiles to:

```
.class {
  one: 1;
}
.class .brass {
  three: 3;
}
```

Essentially, each scope has a "final" value, similar to properties in the browser, like this example using custom properties:
本质上，每个作用域都有一个“最终”值，类似于浏览器中的属性，就像这个使用自定义属性的例子: 

```less
.header {
  --color: white;
  color: var(--color);  // the color is black
  --color: black;
}
```

This means that, unlike other CSS pre-processing languages, Less variables behave very much like CSS's.
这意味着，与其他CSS预处理语言不同，更少的变量的行为与CSS非常相似。

### Properties as Variables **(NEW!)**属性作为变量(新的!)

*v3.0.0*

You can easily treat properties like variables using the `$prop` syntax. Sometimes this can make your code a little lighter.
您可以使用$prop语法轻松地将属性视为变量。有时这会使您的代码更轻松。

```less
.widget {
  color: #efefef;
  background-color: $color;
}
```

Compiles to:

```less
.widget {
  color: #efefef;
  background-color: #efefef;
}
```

Note that, like variables, Less will choose the last property within the current/parent scope as being the "final" value.
注意，与变量一样，Less将选择当前/父范围内的最后一个属性作为“最终”值。

```less
.block {
  color: red; 
  .inner {
    background-color: $color; 
  }
  color: blue;  
} 
```

Compiles to:

```less
.block {
  color: red; 
  color: blue;  
} 
.block .inner {
  background-color: blue; 
}
```

### Default Variables默认的变量

We sometimes get requests for default variables - an ability to set a variable only if it is not already set. This feature is not required because you can easily override a variable by putting the definition afterwards.
我们有时会收到对默认变量的请求——只有在尚未设置变量的情况下才能设置该变量。这个特性不是必需的，因为您可以通过在后面放置定义来轻松覆盖变量。

For instance:

```less
// library库
@base-color: green;
@dark-color: darken(@base-color, 10%);

// use of library使用的库
@import "library.less";
@base-color: red;
```

This works fine because of [Lazy Loading](https://less.bootcss.com/features/#variables-feature-lazy-loading) - `@base-color` is overridden and `@dark-color` is a dark red.
这是因为[延迟加载](https://less.bootcss.com/features/#variabls-feature-lazy加载)—“@base-color”被覆盖，“@dark-color”是暗红色。

 

# Parent Selectors父选择器

> Referencing parent selectors with `&`
> 使用' & '引用父选择器

The `&` operator represents the parent selectors of a [nested rule](https://less.bootcss.com/features/#features-overview-feature-nested-rules) and is most commonly used when applying a modifying class or pseudo-class to an existing selector:
`&`操作符表示[嵌套规则]的父选择器(https://less.bootcss.com/features/features-overview-features-nested.rules)，最常用的是在对现有选择器应用修改类或伪类时:
```less
a {
  color: blue;
  &:hover {
    color: green;
  }
}
```

results in:结果是:

```less
a {
  color: blue;
}

a:hover {
  color: green;
}
```

Notice that without the `&`, the above example would result in `a :hover` rule (a descendant selector that matches hovered elements inside of `<a>` tags) and this is not what we typically would want with the nested `:hover`.
注意，如果没有' & '，上面的示例将导致' a:hover '规则(匹配' <a> '标记中悬停元素的后代选择器)，而这不是嵌套的':hover '通常需要的。

The "parent selectors" operator has a variety of uses. Basically any time you need the selectors of the nested rules to be combined in other ways than the default. For example another typical use of the `&` is to produce repetitive class names:
“父选择器”操作符有多种用途。基本上，在任何时候，您都需要以其他方式组合嵌套规则的选择器，而不是以默认方式组合。例如，' & '的另一个典型用法是产生重复的类名:

```less
.button {
  &-ok {
    background-image: url("ok.png");
  }
  &-cancel {
    background-image: url("cancel.png");
  }

  &-custom {
    background-image: url("custom.png");
  }
}
```

output:输出：

```less
.button-ok {
  background-image: url("ok.png");
}
.button-cancel {
  background-image: url("cancel.png");
}
.button-custom {
  background-image: url("custom.png");
}
```

### Multiple `&`多个`&`

`&` may appear more than once within a selector. This makes it possible to repeatedly refer to a parent selector without repeating its name.
`&`可能在选择器中出现不止一次。这使得重复引用父选择器而不重复其名称成为可能。

```less
.link {
  & + & {
    color: red;
  }

  & & {
    color: green;
  }

  && {
    color: blue;
  }

  &, &ish {
    color: cyan;
  }
}
```

will output:将输出

```less
.link + .link {
  color: red;
}
.link .link {
  color: green;
}
.link.link {
  color: blue;
}
.link, .linkish {
  color: cyan;
}
```

Note that `&` represents all parent selectors (not just the nearest ancestor) so the following example:
注意， `&` 表示所有父选择器(不仅仅是最近的祖先)，因此下面的例子:

```less
.grand {
  .parent {
    & > & {
      color: red;
    }

    & & {
      color: green;
    }

    && {
      color: blue;
    }

    &, &ish {
      color: cyan;
    }
  }
}
```

results in:

```less
.grand .parent > .grand .parent {
  color: red;
}
.grand .parent .grand .parent {
  color: green;
}
.grand .parent.grand .parent {
  color: blue;
}
.grand .parent,
.grand .parentish {
  color: cyan;
}
```

### Changing Selector Order改变选择器顺序

It can be useful to prepend a selector to the inherited (parent) selectors. This can be done by putting the `&` after current selector. For example, when using Modernizr, you might want to specify different rules based on supported features:
将选择器前置到继承(父)选择器可能很有用。这可以通过将' & '放在当前选择器之后来实现。例如，在使用Modernizr时，您可能希望根据支持的特性指定不同的规则:

```less
.header {
  .menu {
    border-radius: 5px;
    .no-borderradius & {
      background-image: url('images/button-background.png');
    }
  }
}
```

The selector `.no-borderradius &` will prepend `.no-borderradius` to its parent `.header .menu` to form the`.no-borderradius .header .menu` on output:
选择器`.no-borderradius &`将预先考虑 `.no-borderradius`到它的父节点`.header .menu` 形成了`.no-borderradius .header .menu`输出:

```less
.header .menu {
  border-radius: 5px;
}
.no-borderradius .header .menu {
  background-image: url('images/button-background.png');
}
```

### Combinatorial Explosion组合爆炸

`&` can also be used to generate every possible permutation of selectors in a comma separated list:
`&` 也可以用于在逗号分隔的列表中生成选择器的所有可能排列:

```less
p, a, ul, li {
  border-top: 2px dotted #366;
  & + & {
    border-top: 0;
  }
}
```

This expands to all possible (16) combinations of the specified elements:
这扩展到指定元素的所有可能(16)组合:

```less
p,
a,
ul,
li {
  border-top: 2px dotted #366;
}
p + p,
p + a,
p + ul,
p + li,
a + p,
a + a,
a + ul,
a + li,
ul + p,
ul + a,
ul + ul,
ul + li,
li + p,
li + a,
li + ul,
li + li {
  border-top: 0;
}
```

 

# Extend扩展

> Extend is a Less pseudo-class which merges the selector it is put on with ones that match what it references.
> 扩展是一个伪类，它将它所使用的选择器与它所引用的选择器合并在一起。

Released [v1.4.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)
发布[v1.4.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)

```less
nav ul {
  &:extend(.inline);
  background: blue;
}
```

In the rule set above, the `:extend` selector will apply the "extending selector" (`nav ul`) onto the `.inline` class *wherever the .inline class appears*. The declaration block will be kept as-is, but without any reference to the extend (because extend isn't css).
在上面的规则集中，扩展选择器将在`.inline`类出现的任何地方应用“扩展选择器” (`nav ul`) 。声明块将保持原样，但不引用扩展(因为扩展不是css)。

So the following:所以以下:

```less
nav ul {
  &:extend(.inline);
  background: blue;
}
.inline {
  color: red;
}
```

Outputs输出

```less
nav ul {
  background: blue;
}
.inline,
nav ul {
  color: red;
}
```

Notice how the `nav ul:extend(.inline)` selector gets output as `nav ul` - the extend gets removed before output and the selector block left as-is. If no properties are put in that block then it gets removed from the output (but the extend still may affect other selectors).
请注意`nav ul:extend(.inline)` 选择器如何以 `nav ul` 的形式获得输出—在输出之前删除扩展，选择器块保持原样。如果在该块中没有放入属性，那么它将从输出中删除(但是扩展仍然可能影响其他选择器)。

### Extend Syntax扩展语法

The extend is either attached to a selector or placed into a ruleset. It looks like a pseudo-class with selector parameter optionally followed by the keyword `all`:
扩展要么附加到选择器，要么放置到规则集中。它看起来像一个伪类，带有选择器参数，可选地后跟关键字 `all`:

Example:例子:

```less
.a:extend(.b) {}

// the above block does the same thing as the below block
// 上面的块和下面的块做同样的事情
.a {
  &:extend(.b);
}
```

```less
.c:extend(.d all) {
  // extends all instances of ".d" e.g. ".x.d" or ".d.x"
  // 扩展“的所有实例".d" e.g. ".x.d" or ".d.x"
}
.c:extend(.d) {
  // extends only instances where the selector will be output as just ".d"
  // c:扩展(.d) {}
  // 只扩展选择器输出为“。d”的实例
}
```

It can contain one or more classes to extend, separated by commas.

Example:

```
.e:extend(.f) {}
.e:extend(.g) {}

// the above an the below do the same thing
.e:extend(.f, .g) {}
```

### Extend Attached to Selector扩展附加选择器

Extend attached to a selector looks like an ordinary pseudo-class with selector as a parameter. A selector can contain multiple extend clauses, but all extends must be at the end of the selector.
附加到选择器的扩展看起来像一个普通的伪类，选择器是一个参数。选择器可以包含多个扩展子句，但所有扩展必须位于选择器的末尾。

- Extend after the selector: `pre:hover:extend(div pre)`.
- Space between selector and extend is allowed: `pre:hover :extend(div pre)`.
- Multiple extends are allowed: `pre:hover:extend(div pre):extend(.bucket tr)` - Note this is the same as `pre:hover:extend(div pre, .bucket tr)`
- This is NOT allowed: `pre:hover:extend(div pre).nth-child(odd)`. Extend must be last.

- 扩展后的选择器:`pre:hover:extend(div pre)`。
- 选择器和扩展之间的空间是允许的:`pre:hover :extend(div pre)`。
- 允许多个扩展: `pre:hover:extend(div pre):extend(.bucket tr)`(注意，这与`pre:hover:extend(div pre, .bucket tr)`相同)
- 这是不允许的:`pre:hover:extend(div pre)`。扩展必须是最后一个。

If a ruleset contains multiple selectors, any of them can have the extend keyword. Multiple selectors with extend in one ruleset:
如果一个规则集包含多个选择器，它们中的任何一个都可以具有extend关键字。多个选择器与扩展在一个规则集:

```less
.big-division,
.big-bag:extend(.bag),
.big-bucket:extend(.bucket) {
  // body
}
```

### Extend Inside Ruleset扩展内部规则集

Extend can be placed into a ruleset's body using `&:extend(selector)` syntax. Placing extend into a body is a shortcut for placing it into every single selector of that ruleset.
可以使用`&:extend(selector)` 语法将Extend放置到规则集的主体中。将extend放置到主体中是将其放置到该规则集的每个选择器中的快捷方式。

Extend inside a body:
伸展身体:

```less
pre:hover,
.some-class {
  &:extend(div pre);
}
```

is exactly the same as adding an extend after each selector:
与在每个选择器后添加扩展完全相同: 

```less
pre:hover:extend(div pre),
.some-class:extend(div pre) {}
```

### Extending Nested Selectors扩展嵌套选择器

Extend is able to match nested selectors. Following less:
扩展能够匹配嵌套的选择器。跟踪less:

Example:

```less
.bucket {
  tr { // nested ruleset with target selector
    color: blue;
  }
}
.some-class:extend(.bucket tr) {} // nested ruleset is recognized
```

Outputs

```less
.bucket tr,
.some-class {
  color: blue;
}
```

Essentially the extend looks at the compiled css, not the original less.
从本质上说，扩展查看的是编译后的css，而不是原始的css。

Example:

```less
.bucket {
  tr & { // nested ruleset with target selector
    color: blue;
  }
}
.some-class:extend(tr .bucket) {} // nested ruleset is recognized
```

Outputs

```less
tr .bucket,
.some-class {
  color: blue;
}
```

### Exact Matching with Extend扩展精确匹配

Extend by default looks for exact match between selectors. It does matter whether selector uses leading star or not. It does not matter that two nth-expressions have the same meaning, they need to have to same form in order to be matched. The only exception are quotes in attribute selector, less knows they have the same meaning and matches them.
在默认情况下，Extend会在选择器之间寻找精确匹配。选择器是否使用前导星号并不重要。两个n -表达式含义相同没有关系，它们必须具有相同的形式才能匹配。唯一的例外是属性选择器中的引号，less知道它们有相同的含义并匹配它们。


Example:

```less
.a.class,
.class.a,
.class > .a {
  color: blue;
}
.test:extend(.class) {} // this will NOT match the any selectors above
```

Leading star does matter. Selectors `*.class` and `.class` are equivalent, but extend will not match them:
主角很重要。选择器的`*.class` 和`.class`是等价的，但扩展不匹配:

```less
*.class {
  color: blue;
}
.noStar:extend(.class) {} // this will NOT match the *.class selector
```

Outputs

```less
*.class {
  color: blue;
}
```

Order of pseudo-classes does matter. Selectors `link:hover:visited` and `link:visited:hover` match the same set of elements, but extend treats them as different:
伪类的顺序很重要。选择器的link:hover:visited和link:visited:hover匹配相同的元素集，但extend将它们视为不同的元素:

```less
link:hover:visited {
  color: blue;
}
.selector:extend(link:visited:hover) {}
```

Outputs

```less
link:hover:visited {
  color: blue;
}
```

### nth Expressio nth表达式

Nth expression form does matter. Nth-expressions `1n+3` and `n+3` are equivalent, but extend will not match them:
第n个表达式很重要。第n个表达式“1n+3”和“n+3”是等价的，但扩展不匹配:

```less
:nth-child(1n+3) {
  color: blue;
}
.child:extend(:nth-child(n+3)) {}
```

Outputs

```less
:nth-child(1n+3) {
  color: blue;
}
```

Quote type in attribute selector does not matter. All of the following are equivalent.
属性选择器中的引号类型不重要。下面所有的都是等价的。

```less
[title=identifier] {
  color: blue;
}
[title='identifier'] {
  color: blue;
}
[title="identifier"] {
  color: blue;
}

.noQuote:extend([title=identifier]) {}
.singleQuote:extend([title='identifier']) {}
.doubleQuote:extend([title="identifier"]) {}
```

Outputs

```less
[title=identifier],
.noQuote,
.singleQuote,
.doubleQuote {
  color: blue;
}

[title='identifier'],
.noQuote,
.singleQuote,
.doubleQuote {
  color: blue;
}

[title="identifier"],
.noQuote,
.singleQuote,
.doubleQuote {
  color: blue;
}
```

### Extend "all"扩展“all”

When you specify the all keyword last in an extend argument it tells Less to match that selector as part of another selector. The selector will be copied and the matched part of the selector only will then be replaced with the extend, making a new selector.
当您在扩展参数中指定all关键字last时，它会告诉Less作为另一个选择器的一部分来匹配该选择器。选择器将被复制，并且选择器的匹配部分将被扩展替换，从而生成一个新的选择器。

Example:

```
.a.b.test,
.test.c {
  color: orange;
}
.test {
  &:hover {
    color: green;
  }
}

.replacement:extend(.test all) {}
```

Outputs

```
.a.b.test,
.test.c,
.a.b.replacement,
.replacement.c {
  color: orange;
}
.test:hover,
.replacement:hover {
  color: green;
}
```

*You can think of this mode of operation as essentially doing a non-destructive search and replace.*
你可以把这种操作模式看作是一种无损的搜索和替换

### Selector Interpolation with Extend选择器插值与扩展

> Extend is **not** able to match selectors with variables. If selector contains variable, extend will ignore it.
> 扩展是**not**匹配选择器与变量。如果选择器包含变量，extend将忽略它。

However, extend can be attached to interpolated selector.
然而，扩展可以附加到插值选择器。

Selector with variable will not be matched:
选择器与变量将不匹配:
```less
@variable: .bucket;
@{variable} { // interpolated selector
  color: blue;
}
.some-class:extend(.bucket) {} // does nothing, no match is found
```

and extend with variable in target selector matches nothing:
在目标选择器中使用变量进行扩展，不匹配:

```less
.bucket {
  color: blue;
}
.some-class:extend(@{variable}) {} // interpolated selector matches nothing
@variable: .bucket;
```

Both of the above examples compile into:
以上两个例子都可以编译成:

```less
.bucket {
  color: blue;
}
```

However, `:extend` attached to an interpolated selector works:
然而，`:extend` 附加到一个插值选择器工作:

```less
.bucket {
  color: blue;
}
@{variable}:extend(.bucket) {}
@variable: .selector;
```

compiles to:

```
.bucket, .selector {
  color: blue;
}
```

### Scoping / Extend Inside @media作用域/扩展@media内部

Currently, an `:extend` inside a `@media` declaration will only match selectors inside the same media declaration:
目前，“@media”声明中的“:extend”只匹配同一媒体声明中的选择器:

```less
@media print {
  .screenClass:extend(.selector) {} // extend inside media
  .selector { // this will be matched - it is in the same media
    color: black;
  }
}
.selector { // ruleset on top of style sheet - extend ignores it
  color: red;
}
@media screen {
  .selector {  // ruleset inside another media - extend ignores it
    color: blue;
  }
}
```

compiles into:编译成

```less
@media print {
  .selector,
  .screenClass { /*  ruleset inside the same media was extended */
    color: black;
  }
}
.selector { /* ruleset on top of style sheet was ignored */
  color: red;
}
@media screen {
  .selector { /* ruleset inside another media was ignored */
    color: blue;
  }
}
```

Note: extending does not match selectors inside a nested `@media` declaration:
注意:扩展与嵌套的“@media”声明中的选择器不匹配:

```less
@media screen {
  .screenClass:extend(.selector) {} // extend inside media
  @media (min-width: 1023px) {
    .selector {  // ruleset inside nested media - extend ignores it
      color: blue;
    }
  }
}
```

This compiles into:

```less
@media screen and (min-width: 1023px) {
  .selector { /* ruleset inside another nested media was ignored */
    color: blue;
  }
}
```

Top level extend matches everything including selectors inside nested media:
顶级扩展匹配所有内容，包括嵌套媒体中的选择器:

```less
@media screen {
  .selector {  /* ruleset inside nested media - top level extend works */
    color: blue;
  }
  @media (min-width: 1023px) {
    .selector {  /* ruleset inside nested media - top level extend works */
      color: blue;
    }
  }
}

.topLevel:extend(.selector) {} /* top level extend matches everything */
```

compiles into:

```less
@media screen {
  .selector,
  .topLevel { /* ruleset inside media was extended */
    color: blue;
  }
}
@media screen and (min-width: 1023px) {
  .selector,
  .topLevel { /* ruleset inside nested media was extended */
    color: blue;
  }
}
```

### Duplication Detection重复检测

Currently there is no duplication detection.
目前没有重复检测。

Example:

```less
.alert-info,
.widget {
  /* declarations */
}

.alert:extend(.alert-info, .widget) {}
```

Outputs

```less
.alert-info,
.widget,
.alert,
.alert {
  /* declarations */
}
```

### Use Cases for Extend扩展用例

#### Classic Use Case经典用例

The classic use case is to avoid adding a base class. For example, if you have
经典用例是避免添加基类。例如，如果你有

```less
.animal {
  background-color: black;
  color: white;
}
```

and you want to have a subtype of animal which overrides the background color then you have two options, firstly change your HTML
你想要一个动物的子类，它会覆盖背景颜色，然后你有两个选择，首先改变你的HTML

```less
<a class="animal bear">Bear</a>
```

```less
.animal {
  background-color: black;
  color: white;
}
.bear {
  background-color: brown;
}
```

or have simplified html and use extend in your less. e.g.
或者简化了html并在您的less中使用了extend。如。

```less
<a class="bear">Bear</a>
```

```less
.animal {
  background-color: black;
  color: white;
}
.bear {
  &:extend(.animal);
  background-color: brown;
}
```

#### Reducing CSS Size减少CSS大小

Mixins copy all of the properties into a selector, which can lead to unnecessary duplication. Therefore you can use extends instead of mixins to move the selector up to the properties you wish to use, which leads to less CSS being generated.
mixin将所有属性复制到选择器中，这会导致不必要的复制。因此，您可以使用extends而不是mixin来将选择器移动到您希望使用的属性，这将导致生成更少的CSS。

Example - with mixin:
例子-使用mixin:

```less
.my-inline-block() {
  display: inline-block;
  font-size: 0;
}
.thing1 {
  .my-inline-block;
}
.thing2 {
  .my-inline-block;
}
```

Outputs

```less
.thing1 {
  display: inline-block;
  font-size: 0;
}
.thing2 {
  display: inline-block;
  font-size: 0;
}
```

Example (with extends):示例(扩展)

```less
.my-inline-block {
  display: inline-block;
  font-size: 0;
}
.thing1 {
  &:extend(.my-inline-block);
}
.thing2 {
  &:extend(.my-inline-block);
}
```

Outputs

```less
.my-inline-block,
.thing1,
.thing2 {
  display: inline-block;
  font-size: 0;
}
```

#### Combining Styles / A More Advanced Mixin
组合样式/更高级的混合

Another use-case is as an alternative for a mixin - because mixins can only be used with simple selectors, if you have two different blocks of html, but need to apply the same styles to both you can use extends to relate two areas.
另一个用例是mixin的替代——因为mixin只能与简单的选择器一起使用，如果您有两个不同的html块，但是需要将相同的样式应用到这两个块上，那么您可以使用extends来关联两个区域。

Example:

```less
li.list > a {
  // list styles
}
button.list-style {
  &:extend(li.list > a); // use the same list styles
}
```

 

# Merge合并

> Combine properties属性结合起来

The `merge` feature allows for aggregating values from multiple properties into a comma or space separated list under a single property. `merge` is useful for properties such as background and transform.
“合并”特性允许将多个属性的值聚合到单个属性下的逗号或空格分隔的列表中。“合并”对于背景和转换等属性很有用。

### Comma逗号

> Append property value with comma用逗号附加属性值

Released发布 [v1.5.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)

Example:

```less
.mixin() {
  box-shadow+: inset 0 0 10px #555;
}
.myclass {
  .mixin();
  box-shadow+: 0 0 20px black;
}
```

Outputs

```less
.myclass {
  box-shadow: inset 0 0 10px #555, 0 0 20px black;
}
```

### Space空间

> Append property value with space附加属性值与空格

Released发布 [v1.7.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)

Example:

```less
.mixin() {
  transform+_: scale(2);
}
.myclass {
  .mixin();
  transform+_: rotate(15deg);
}
```

Outputs

```less
.myclass {
  transform: scale(2) rotate(15deg);
}
```

To avoid any unintentional joins, `merge` requires an explicit `+` or `+_` flag on each join pending declaration.
为了避免任何无意的连接，`merge`在每个连接挂起声明上都需要显式的`+` 或`+_`标志。


# Mixins

> "mix-in" properties from existing styles来自现有样式的>“混搭”属性

You can mix-in class selectors and id selectors, e.g.
您可以混合类选择器和id选择器，例如:

```less
.a, #b {
  color: red;
}
.mixin-class {
  .a();
}
.mixin-id {
  #b();
}
```

which results in:

```less
.a, #b {
  color: red;
}
.mixin-class {
  color: red;
}
.mixin-id {
  color: red;
}
```

Currently and historically, the parentheses in a mixin call are optional, but optional parentheses are deprecated and will be required in a future release.
当前和历史上，mixin调用中的括号都是可选的，但是可选括号是不推荐的，在将来的版本中会用到。

```less
.a(); 
.a;  // currently works, but deprecated; don't use
```

## Not Outputting the Mixin没有输出混合物

If you want to create a mixin but you do not want that mixin to be in your CSS output, put parentheses after the mixin definition.
如果您希望创建一个mixin，但又不希望该mixin出现在CSS输出中，请在mixin定义之后加上括号。

```less
.my-mixin {
  color: black;
}
.my-other-mixin() {
  background: white;
}
.class {
  .my-mixin();
  .my-other-mixin();
}
```

outputs

```less
.my-mixin {
  color: black;
}
.class {
  color: black;
  background: white;
}
```

## Selectors in Mixins在mixins选择器

Mixins can contain more than just properties, they can contain selectors too.
mixins不仅可以包含属性，还可以包含选择器。

For example:

```less
.my-hover-mixin() {
  &:hover {
    border: 1px solid red;
  }
}
button {
  .my-hover-mixin();
}
```

Outputs

```
button:hover {
  border: 1px solid red;
}
```

## Namespaces
名称空间

If you want to mixin properties inside a more complicated selector, you can stack up multiple id's or classes.
如果希望在更复杂的选择器中混合属性，可以将多个id或类堆叠在一起。

```less
#outer() {
  .inner {
    color: red;
  }
}

.c {
  #outer > .inner();
}
```

Both `>` and whitespace are optional“>”和空格都是可选的

```less
// all do the same thing
#outer > .inner();
#outer .inner();
#outer.inner();
```

Namespacing your mixins like reduces conflict with other library mixins or user mixins, but it can also be a way to "organize" groups of mixins.
给您的混合程序命名空间，比如减少与其他库混合程序或用户混合程序的冲突，但是它也可以是“组织”混合程序组的一种方法。

Example:

```less
#my-library {
  .my-mixin() {
    color: black;
  }
}
// which can be used like this
.class {
  #my-library.my-mixin();
}
```

## Guarded Namespaces

If a namespace has a guard, mixins defined by it are used only if the guard condition returns true. A namespace guard is evaluated exactly the same way as a guard on mixin, so the following two mixins work the same way:
如果名称空间具有保护，则只有在保护条件返回true时才使用由其定义的mixins。名称空间保护的计算方式与mixins上的保护完全相同，因此以下两个mixin的工作方式相同:

```less
#namespace when (@mode = huge) {
  .mixin() { /* */ }
}

#namespace {
  .mixin() when (@mode = huge) { /* */ }
}
```

The `default` function is assumed to have the same value for all nested namespaces and mixin. Following mixin is never evaluated, one of its guards is guaranteed to be false:
假定“默认”函数对于所有嵌套名称空间和mixins具有相同的值。以下mixins从未被评估，它的一个守卫被保证是假的:

```less
#sp_1 when (default()) {
  #sp_2 when (default()) {
    .mixin() when not(default()) { /* */ }
  }
}
```

## The `!important` keyword

Use the `!important` keyword after mixin call to mark all properties inherited by it as `!important`:
使用 `!important` 调用mixin后的关键字，将它继承的所有属性标记为“!important”:

Example:

```less
.foo (@bg: #f5f5f5, @color: #900) {
  background: @bg;
  color: @color;
}
.unimportant {
  .foo();
}
.important {
  .foo() !important;
}
```

Results in:

```less
.unimportant {
  background: #f5f5f5;
  color: #900;
}
.important {
  background: #f5f5f5 !important;
  color: #900 !important;
}
```

 

## Parametric Mixins参数混合

> How to pass arguments to mixins如何将参数传递给mixin

Mixins can also take arguments, which are variables passed to the block of selectors when it is mixed in.
mixins还可以接受参数，这些参数是在混合时传递给选择器块的变量。

For example:

```less
.border-radius(@radius) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
          border-radius: @radius;
}
```

And here's how we can mix it into various rulesets:
下面是我们如何将它融入到各种规则集中:

```less
#header {
  .border-radius(4px);
}
.button {
  .border-radius(6px);
}
```

Parametric mixins can also have default values for their parameters:
参数混合也可以有其参数的默认值:

```less
.border-radius(@radius: 5px) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
          border-radius: @radius;
}
```

We can invoke it like this now:
我们现在可以这样调用它:

```
#header {
  .border-radius();
}
```

And it will include a 5px border-radius.它将包含一个5px的边界半径。

You can also use parametric mixins which don't take parameters. This is useful if you want to hide the ruleset from the CSS output, but want to include its properties in other rulesets:
你也可以使用不带参数的参数混合。如果您想在CSS输出中隐藏规则集，但又想在其他规则集中包含其属性，那么这一点非常有用:

```less
.wrap() {
  text-wrap: wrap;
  white-space: -moz-pre-wrap;
  white-space: pre-wrap;
  word-wrap: break-word;
}

pre { .wrap() }
```

Which would output:输出

```less
pre {
  text-wrap: wrap;
  white-space: -moz-pre-wrap;
  white-space: pre-wrap;
  word-wrap: break-word;
}
```

#### Mixins with Multiple Parameters
具有多个参数的混合

Parameters are either *semicolon* or *comma* separated. It is recommended to use *semicolon*. The symbol comma has double meaning: it can be interpreted either as a mixin parameters separator or css list separator.
参数可以是*分号*或*逗号*分隔。建议使用*分号*。符号逗号有双重含义:它可以解释为mixin参数分隔符或css列表分隔符。

Using comma as mixin separator makes it impossible to create comma separated lists as an argument. On the other hand, if the compiler sees at least one semicolon inside mixin call or declaration, it assumes that arguments are separated by semicolons and all commas belong to css lists:
使用逗号作为混合分隔符使得不可能创建逗号分隔的列表作为参数。另一方面，如果编译器在mixin调用或声明中看到至少一个分号，它假定参数由分号分隔，所有逗号都属于css列表:

- two arguments and each contains comma separated list: `.name(1, 2, 3; something, else)`,
- three arguments and each contains one number: `.name(1, 2, 3)`,
- use dummy semicolon to create mixin call with one argument containing comma separated css list: `.name(1, 2, 3;)`,
- comma separated default value: `.name(@param1: red, blue;)`.
- 两个参数，每个参数包含逗号分隔的列表:的东西,否则)”,
- 三个参数，每个参数包含一个数字:
- 使用伪分号创建mixin调用，其中一个参数包含逗号分隔的css列表:
- 逗号分隔的默认值:“.name(@param1: red, blue;)”

It is legal to define multiple mixins with the same name and number of parameters. Less will use properties of all that can apply. If you used the mixin with one parameter e.g. `.mixin(green);`, then properties of all mixins with exactly one mandatory parameter will be used:
定义具有相同名称和参数数量的多个mixin是合法的。较少会使用所有可以应用的属性。如果混合料只有一个参数，例如`.mixin(green);`，则会使用具有一个强制参数的所有mixin的属性:

```less
.mixin(@color) {
  color-1: @color;
}
.mixin(@color; @padding: 2) {
  color-2: @color;
  padding-2: @padding;
}
.mixin(@color; @padding; @margin: 2) {
  color-3: @color;
  padding-3: @padding;
  margin: @margin @margin @margin @margin;
}
.some .selector div {
  .mixin(#008000);
}
```

compiles into:

```less
.some .selector div {
  color-1: #008000;
  color-2: #008000;
  padding-2: 2;
}
```

#### Named Parameters命名参数

A mixin reference can supply parameters values by their names instead of just positions. Any parameter can be referenced by its name and they do not have to be in any special order:
mixin引用可以通过它们的名称而不仅仅是位置来提供参数值。任何参数都可以通过它的名称来引用，它们不需要以任何特殊的顺序:

```less
.mixin(@color: black; @margin: 10px; @padding: 20px) {
  color: @color;
  margin: @margin;
  padding: @padding;
}
.class1 {
  .mixin(@margin: 20px; @color: #33acfe);
}
.class2 {
  .mixin(#efca44; @padding: 40px);
}
```

compiles into:

```less
.class1 {
  color: #33acfe;
  margin: 20px;
  padding: 20px;
}
.class2 {
  color: #efca44;
  margin: 10px;
  padding: 40px;
}
```

#### The `@arguments` Variable“@arguments”变量

`@arguments` has a special meaning inside mixins, it contains all the arguments passed, when the mixin was called. This is useful if you don't want to deal with individual parameters:
“@arguments”在mixin中有特殊的含义，它包含调用mixin时传递的所有参数。如果您不想处理单个参数，这是有用的:

```less
.box-shadow(@x: 0; @y: 0; @blur: 1px; @color: #000) {
  -webkit-box-shadow: @arguments;
     -moz-box-shadow: @arguments;
          box-shadow: @arguments;
}
.big-block {
  .box-shadow(2px; 5px);
}
```

Which results in:

```less
.big-block {
  -webkit-box-shadow: 2px 5px 1px #000;
     -moz-box-shadow: 2px 5px 1px #000;
          box-shadow: 2px 5px 1px #000;
}
```

#### Advanced Arguments and the `@rest` Variable
高级参数和“@rest”变量

You can use `...` if you want your mixin to take a variable number of arguments. Using this after a variable name will assign those arguments to the variable.
你可以用“……”如果你想让你的mixin使用可变数量的参数。在变量名之后使用它将把这些参数分配给变量。

```less
.mixin(...) {        // matches 0-N arguments
.mixin() {           // matches exactly 0 arguments
.mixin(@a: 1) {      // matches 0-1 arguments
.mixin(@a: 1; ...) { // matches 0-N arguments
.mixin(@a; ...) {    // matches 1-N arguments
```

Furthermore:此外

```less
.mixin(@a; @rest...) {
   // @rest is bound to arguments after @a
   // @arguments is bound to all arguments
}
```

## Pattern-matching模式匹配

Sometimes, you may want to change the behavior of a mixin, based on the parameters you pass to it. Let's start with something basic:
有时，您可能希望根据传递给它的参数更改mixin的行为。让我们从一些基本的东西开始:

```less
.mixin(@s; @color) { ... }

.class {
  .mixin(@switch; #888);
}
```

Now let's say we want `.mixin` to behave differently, based on the value of `@switch`, we could define `.mixin` as such:
现在假设我们想要。我们可以根据‘@switch’的值来定义‘mixin’。mixin的这样:

```less
.mixin(dark; @color) {
  color: darken(@color, 10%);
}
.mixin(light; @color) {
  color: lighten(@color, 10%);
}
.mixin(@_; @color) {
  display: block;
}
```

Now, if we run:现在，如果运行:

```less
@switch: light;

.class {
  .mixin(@switch; #888);
}
```

We will get the following CSS:

```less
.class {
  color: #a2a2a2;
  display: block;
}
```

Where the color passed to `.mixin` was lightened. If the value of `@switch` was `dark`, the result would be a darker color.

Here's what happened:

- The first mixin definition didn't match because it expected `dark` as the first argument.
- The second mixin definition matched, because it expected `light`.
- The third mixin definition matched because it expected any value.

Only mixin definitions which matched were used. Variables match and bind to any value. Anything other than a variable matches only with a value equal to itself.

We can also match on arity, here's an example:

```
.mixin(@a) {
  color: @a;
}
.mixin(@a; @b) {
  color: fade(@a; @b);
}
```

Now if we call `.mixin` with a single argument, we will get the output of the first definition, but if we call it with *two*arguments, we will get the second definition, namely `@a` faded to `@b`.

 

## Return values from mixins

> Return variables or mixins from mixins

Starting in Less 3.5, you can use property/variable accessors to get a "return value" from a mixin, essentially using it like a function.

Example:

```
.average(@x, @y) {
  @result: ((@x + @y) / 2);
}

div {
  padding: .average(16px, 50px)[@result];  // call a mixin and look up its "@return" value
}
```

Results in:

```
div {
  padding: 33px;
}
```

#### Variables in caller scope

**DEPRECATED - Use Property / Value Accessors**

Variables and mixins defined in a mixin are visible and can be used in caller's scope. There is only one exception: a variable is not copied if the caller contains a variable with the same name (that includes variables defined by another mixin call). Only variables present in callers local scope are protected. Variables inherited from parent scopes are overridden.

Example:

```
.mixin() {
  @width:  100%;
  @height: 200px;
}

.caller {
  .mixin();
  width:  @width;
  height: @height;
}
```

Results in:

```
.caller {
  width:  100%;
  height: 200px;
}
```

Variables defined directly in callers scope cannot be overridden. However, variables defined in callers parent scope is not protected and will be overridden:

```
.mixin() {
  @size: in-mixin;
  @definedOnlyInMixin: in-mixin;
}

.class {
  margin: @size @definedOnlyInMixin;
  .mixin();
}

@size: globaly-defined-value; // callers parent scope - no protection
```

Results in:

```
.class {
  margin: in-mixin in-mixin;
}
```

Finally, mixin defined in mixin acts as return value too:

```
.unlock(@value) { // outer mixin
  .doSomething() { // nested mixin
    declaration: @value;
  }
}

#namespace {
  .unlock(5); // unlock doSomething mixin
  .doSomething(); //nested mixin was copied here and is usable
}
```

Results in:

```
#namespace {
  declaration: 5;
}
```

 

## Recursive mixins

> Creating loops

In Less a mixin can call itself. Such recursive mixins, when combined with [Guard Expressions](https://less.bootcss.com/features/#mixin-guards-feature) and [Pattern Matching](https://less.bootcss.com/features/#mixins-parametric-feature-pattern-matching), can be used to create various iterative/loop structures.

Example:

```
.loop(@counter) when (@counter > 0) {
  .loop((@counter - 1));    // next iteration
  width: (10px * @counter); // code for each iteration
}

div {
  .loop(5); // launch the loop
}
```

Output:

```
div {
  width: 10px;
  width: 20px;
  width: 30px;
  width: 40px;
  width: 50px;
}
```

A generic example of using a recursive loop to generate CSS grid classes:

```
.generate-columns(4);

.generate-columns(@n, @i: 1) when (@i =< @n) {
  .column-@{i} {
    width: (@i * 100% / @n);
  }
  .generate-columns(@n, (@i + 1));
}
```

Output:

```
.column-1 {
  width: 25%;
}
.column-2 {
  width: 50%;
}
.column-3 {
  width: 75%;
}
.column-4 {
  width: 100%;
}
```

 

## Mixin Guards

Guards are useful when you want to match on *expressions*, as opposed to simple values or arity. If you are familiar with functional programming, you have probably encountered them already.

In trying to stay as close as possible to the declarative nature of CSS, Less has opted to implement conditional execution via **guarded mixins** instead of `if`/`else` statements, in the vein of `@media` query feature specifications.

Let's start with an example:

```
.mixin(@a) when (lightness(@a) >= 50%) {
  background-color: black;
}
.mixin(@a) when (lightness(@a) < 50%) {
  background-color: white;
}
.mixin(@a) {
  color: @a;
}
```

The key is the `when` keyword, which introduces a guard sequence (here with only one guard). Now if we run the following code:

```
.class1 { .mixin(#ddd) }
.class2 { .mixin(#555) }
```

Here's what we'll get:

```
.class1 {
  background-color: black;
  color: #ddd;
}
.class2 {
  background-color: white;
  color: #555;
}
```

#### Guard Comparison Operators

The full list of comparison operators usable in guards are: `>`, `>=`, `=`, `=<`, `<`. Additionally, the keyword `true` is the only truthy value, making these two mixins equivalent:

```
.truth(@a) when (@a) { ... }
.truth(@a) when (@a = true) { ... }
```

Any value other than the keyword `true` is falsy:

```
.class {
  .truth(40); // Will not match any of the above definitions.
}
```

Note that you can also compare arguments with each other, or with non-arguments:

```
@media: mobile;

.mixin(@a) when (@media = mobile) { ... }
.mixin(@a) when (@media = desktop) { ... }

.max(@a; @b) when (@a > @b) { width: @a }
.max(@a; @b) when (@a < @b) { width: @b }
```

#### Guard Logical Operators

You can use logical operators with guards. The syntax is based on CSS media queries.

Use the `and` keyword to combine guards:

```
.mixin(@a) when (isnumber(@a)) and (@a > 0) { ... }
```

You can emulate the *or* operator by separating guards with a comma `,`. If any of the guards evaluate to true, it's considered a match:

```
.mixin(@a) when (@a > 10), (@a < -10) { ... }
```

Use the `not` keyword to negate conditions:

```
.mixin(@b) when not (@b > 0) { ... }
```

#### Type Checking Functions

Lastly, if you want to match mixins based on value type, you can use the `is` functions:

```
.mixin(@a; @b: 0) when (isnumber(@b)) { ... }
.mixin(@a; @b: black) when (iscolor(@b)) { ... }
```

Here are the basic type checking functions:

- `iscolor`
- `isnumber`
- `isstring`
- `iskeyword`
- `isurl`

If you want to check if a value is in a specific unit in addition to being a number, you may use one of:

- `ispixel`
- `ispercentage`
- `isem`
- `isunit`

 

## Aliasing Mixins

Released [v3.5.0-beta.4](https://github.com/less/less.js/blob/master/CHANGELOG.md)

> Assigning mixin calls to a variable

Mixins can be assigned to a variable to be called as a variable call, or can be used for map lookup.

```
#theme.dark.navbar {
  .colors(light) {
    primary: purple;
  }
  .colors(dark) {
    primary: black;
    secondary: grey;
  }
}

.navbar {
  @colors: #theme.dark.navbar.colors(dark);
  background: @colors[primary];
  border: 1px solid @colors[secondary];
}
```

This would output:

```
.navbar {
  background: black;
  border: 1px solid grey;
}
```

#### Variable calls

Entire mixin calls can be aliased and called as variable calls. As in:

```
#library() {
  .rules() {
    background: green;
  }
}
.box {
  @alias: #library.rules();
  @alias();
}
```

Outputs:

```
.box {
  background: green;
}
```

Note, unlike mixins used in root, mixin calls assigned to variables and *called with no arguments* always require parentheses. The following is not valid.

```
#library() {
  .rules() {
    background: green;
  }
}
.box {
  @alias: #library.colors;
  @alias();   // ERROR: Could not evaluate variable call @alias
}
```

This is because it's ambiguous if variable is assigned a list of selectors or a mixin call. For example, in Less 3.5+, this variable could be used this way.

```
.box {
  @alias: #library.colors;
  @{alias} {
    a: b;
  }
}
```

The above would output:

```
.box #library.colors {
  a: b;
}
```

 

# CSS Guards

> "if"'s around selectors

Released [v1.5.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)

Like Mixin Guards, guards can also be applied to css selectors, which is syntactic sugar for declaring the mixin and then calling it immediately.

For instance, before 1.5.0 you would have had to do this:

```
.my-optional-style() when (@my-option = true) {
  button {
    color: white;
  }
}
.my-optional-style();
```

Now, you can apply the guard directly to a style.

```
button when (@my-option = true) {
  color: white;
}
```

You can also achieve an `if` type statement by combining this with the `&` feature, allowing you to group multiple guards.

```
& when (@my-option = true) {
  button {
    color: white;
  }
  a {
    color: blue;
  }
}
```

 

# Detached Rulesets

> Assign a ruleset to a variable

Released [v1.7.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)

A detached ruleset is a group of css properties, nested rulesets, media declarations or anything else stored in a variable. You can include it into a ruleset or another structure and all its properties are going to be copied there. You can also use it as a mixin argument and pass it around as any other variable.

Simple example:

```
// declare detached ruleset
@detached-ruleset: { background: red; }; // semi-colon is optional in 3.5.0+

// use detached ruleset
.top {
    @detached-ruleset(); 
}
```

compiles into:

```
.top {
  background: red;
}
```

Parentheses after a detached ruleset call are mandatory. The call `@detached-ruleset;` would NOT work.

It is useful when you want to define a mixin that abstracts out either wrapping a piece of code in a media query or a non-supported browser class name. The rulesets can be passed to mixin so that the mixin can wrap the content, e.g.

```
.desktop-and-old-ie(@rules) {
  @media screen and (min-width: 1200px) { @rules(); }
  html.lt-ie9 &                         { @rules(); }
}

header {
  background-color: blue;

  .desktop-and-old-ie({
    background-color: red;
  });
}
```

Here the `desktop-and-old-ie` mixin defines the media query and root class so that you can use a mixin to wrap a piece of code. This will output

```
header {
  background-color: blue;
}
@media screen and (min-width: 1200px) {
  header {
    background-color: red;
  }
}
html.lt-ie9 header {
  background-color: red;
}
```

A ruleset can be now assigned to a variable or passed in to a mixin and can contain the full set of Less features, e.g.

```
@my-ruleset: {
    .my-selector {
      background-color: black;
    }
  };
```

You can even take advantage of [media query bubbling](https://less.bootcss.com/features/#features-overview-feature-media-query-bubbling-and-nested-media-queries), for instance

```
@my-ruleset: {
    .my-selector {
      @media tv {
        background-color: black;
      }
    }
  };
@media (orientation:portrait) {
    @my-ruleset();
}
```

which will output

```
@media (orientation: portrait) and tv {
  .my-selector {
    background-color: black;
  }
}
```

A detached ruleset call unlocks (returns) all its mixins into caller the same way as mixin calls do. However, it does **not** return variables.

Returned mixin:

```
// detached ruleset with a mixin
@detached-ruleset: { 
    .mixin() {
        color:blue;
    }
};
// call detached ruleset
.caller {
    @detached-ruleset(); 
    .mixin();
}
```

Results in:

```
.caller {
  color: blue;
}
```

Private variables:

```
@detached-ruleset: { 
    @color:blue; // this variable is private
};
.caller {
    color: @color; // syntax error
}
```

## Scoping

A detached ruleset can use all variables and mixins accessible where it is *defined* and where it is *called*. Otherwise said, both definition and caller scopes are available to it. If both scopes contains the same variable or mixin, declaration scope value takes precedence.

*Declaration scope* is the one where detached ruleset body is defined. Copying a detached ruleset from one variable into another cannot modify its scope. The ruleset does not gain access to new scopes just by being referenced there.

Lastly, a detached ruleset can gain access to scope by being unlocked (imported) into it.

#### Definition and Caller Scope Visibility

A detached ruleset sees the caller's variables and mixins:

```
@detached-ruleset: {
  caller-variable: @caller-variable; // variable is undefined here
  .caller-mixin(); // mixin is undefined here
};

selector {
  // use detached ruleset
  @detached-ruleset(); 

  // define variable and mixin needed inside the detached ruleset
  @caller-variable: value;
  .caller-mixin() {
    variable: declaration;
  }
}
```

compiles into:

```
selector {
  caller-variable: value;
  variable: declaration;
}
```

Variable and mixins accessible form definition win over those available in the caller:

```
@variable: global;
@detached-ruleset: {
  // will use global variable, because it is accessible
  // from detached-ruleset definition
  variable: @variable; 
};

selector {
  @detached-ruleset();
  @variable: value; // variable defined in caller - will be ignored
}
```

compiles into:

```
selector {
  variable: global;
}
```

#### Referencing *Won't* Modify Detached Ruleset Scope

A ruleset does not gain access to new scopes just by being referenced there:

```
@detached-1: { scope-detached: @one @two; };
.one {
  @one: visible;
  .two {
    @detached-2: @detached-1; // copying/renaming ruleset 
    @two: visible; // ruleset can not see this variable
  }
}

.use-place {
  .one > .two(); 
  @detached-2();
}
```

throws an error:

```
ERROR 1:32 The variable "@one" was not declared.
```

#### Unlocking *Will* Modify Detached Ruleset Scope

A detached ruleset gains access by being unlocked (imported) inside a scope:

```
#space {
  .importer-1() {
    @detached: { scope-detached: @variable; }; // define detached ruleset
  }
}

.importer-2() {
  @variable: value; // unlocked detached ruleset CAN see this variable
  #space > .importer-1(); // unlock/import detached ruleset
}

.use-place {
  .importer-2(); // unlock/import detached ruleset second time
   @detached();
}
```

compiles into:

```
.use-place {
  scope-detached: value;
}
```

 

# @import At-Rules

> Import styles from other style sheets

In standard CSS, `@import` at-rules must precede all other types of rules. But Less doesn't care where you put `@import` statements.

Example:

```
.foo {
  background: #900;
}
@import "this-is-valid.less";
```

## File Extensions

`@import` statements may be treated differently by Less depending on the file extension:

- If the file has a `.css` extension it will be treated as CSS and the `@import` statement left as-is (see the [inline option](https://less.bootcss.com/features/#import-options-inline)below).
- If it has *any other extension* it will be treated as Less and imported.
- If it does not have an extension, `.less` will be appended and it will be included as a imported Less file.

Examples:

```
@import "foo";      // foo.less is imported
@import "foo.less"; // foo.less is imported
@import "foo.php";  // foo.php imported as a Less file
@import "foo.css";  // statement left in place, as-is
```

The following options can be used to override this behavior.

## Import Options

> Less offers several extensions to the CSS `@import` CSS at-rule to provide more flexibility over what you can do with external files.

Syntax: `@import (keyword) "filename";`

The following import options have been implemented:

- `reference`: use a Less file but do not output it
- `inline`: include the source file in the output but do not process it
- `less`: treat the file as a Less file, no matter what the file extension
- `css`: treat the file as a CSS file, no matter what the file extension
- `once`: only include the file once (this is default behavior)
- `multiple`: include the file multiple times
- `optional`: continue compiling when file is not found

> More than one keyword per `@import` is allowed, you will have to use commas to separate the keywords:

Example: `@import (optional, reference) "foo.less";`

### reference

Use `@import (reference)` to import external files, but without adding the imported styles to the compiled output unless referenced.

Released [v1.5.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)

Example: `@import (reference) "foo.less";`

Imagine that `reference` marks every at-rule and selector with a *reference flag* in the imported file, imports as normal, but when the CSS is generated, "reference" selectors (as well as any media queries containing only reference selectors) are not output. `reference` styles will not show up in your generated CSS unless the reference styles are used as [mixins](https://less.bootcss.com/features/#mixins-feature) or [extended](https://less.bootcss.com/features/#extend-feature).

Additionally, **reference** produces different results depending on which method was used (mixin or extend):

- **extend**: When a selector is extended, only the new selector is marked as *not referenced*, and it is pulled in at the position of the reference `@import` statement.
- **mixins**: When a `reference` style is used as an [implicit mixin](https://less.bootcss.com/features/#mixins-feature), its rules are mixed-in, marked "not reference", and appear in the referenced place as normal.

#### reference example

This allows you to pull in only specific, targeted styles from a library such as [Bootstrap](https://github.com/twbs/bootstrap) by doing something like this:

```
.navbar:extend(.navbar all) {}
```

And you will pull in only `.navbar` related styles from Bootstrap.

### inline

Use `@import (inline)` to include external files, but not process them.

Released [v1.5.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)

Example: `@import (inline) "not-less-compatible.css";`

You will use this when a CSS file may not be Less compatible; this is because although Less supports most known standards CSS, it does not support comments in some places and does not support all known CSS hacks without modifying the CSS.

So you can use this to include the file in the output so that all CSS will be in one file.

### less

Use `@import (less)` to treat imported files as Less, regardless of file extension.

Released [v1.4.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)

Example:

```
@import (less) "foo.css";
```

### css

Use `@import (css)` to treat imported files as regular CSS, regardless of file extension. This means the import statement will be left as it is.

Released [v1.4.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)

Example:

```
@import (css) "foo.less";
```

outputs

```
@import "foo.less";
```

### once

The default behavior of `@import` statements. It means the file is imported only once and subsequent import statements for that file will be ignored.

Released [v1.4.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)

This is the default behavior of `@import` statements.

Example:

```
@import (once) "foo.less";
@import (once) "foo.less"; // this statement will be ignored
```

### multiple

Use `@import (multiple)` to allow importing of multiple files with the same name. This is the opposite behavior to once.

Released [v1.4.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)

Example:

```
// file: foo.less
.a {
  color: green;
}
// file: main.less
@import (multiple) "foo.less";
@import (multiple) "foo.less";
```

Outputs

```
.a {
  color: green;
}
.a {
  color: green;
}
```

### optional

Use `@import (optional)` to allow importing of a file only when it exists. Without the `optional` keyword Less throws a FileError and stops compiling when importing a file that can not be found.

Released [v2.3.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)

 

# @plugin At-Rules

Released [v2.5.0](https://github.com/less/less.js/blob/master/CHANGELOG.md)

> Import JavaScript plugins to add Less.js functions and features

## Writing your first plugin

Using a `@plugin` at-rule is similar to using an `@import` for your `.less` files.

```
@plugin "my-plugin";  // automatically appends .js if no extension
```

Since Less plugins are evaluated within the Less scope, the plugin definition can be quite simple.

```
registerPlugin({
    install: function(less, pluginManager, functions) {
        functions.add('pi', function() {
            return Math.PI;
        });
    }
})
```

or you can use `module.exports` (shimmed to work in browser as well as Node.js).

```
module.exports = {
    install: function(less, pluginManager, functions) {
        functions.add('pi', function() {
            return Math.PI;
        });
    }
};
```

Note that other Node.js CommonJS conventions, like `require()` are not available in the browser. Keep this in mind when writing cross-platform plugins.

What can you do with a plugin? A lot, but let's start with the basics. We'll focus first on what you might put inside the `install` function. Let's say you write this:

```
// my-plugin.js
install: function(less, pluginManager, functions) {
    functions.add('pi', function() {
        return Math.PI;
    });
}
// etc
```

Congratulations! You've written a Less plugin!

If you were to use this in your stylesheet:

```
@plugin "my-plugin";
.show-me-pi {
  value: pi();
}
```

You would get:

```
.show-me-pi {
  value: 3.141592653589793;
}
```

However, you would need to return a proper Less node if you wanted to, say, multiply that against other values or do other Less operations. Otherwise the output in your stylesheet is plain text (which may be fine for your purposes).

Meaning, this is more correct:

```
functions.add('pi', function() {
    return less.dimension(Math.PI);
});
```

*Note: A dimension is a number with or without a unit, like "10px", which would be less.Dimension(10, "px"). For a list of units, see the Less API.*

Now you can use your function in operations.

```
@plugin "my-plugin";
.show-me-pi {
  value: pi() * 2;
}
```

You may have noticed that there are available globals for your plugin file, namely a function registry (`functions`object), and the `less` object. These are there for convenience.

## Plugin Scope

Functions added by a `@plugin` at-rule adheres to Less scoping rules. This is great for Less library authors that want to add functionality without introducing naming conflicts.

For instance, say you have 2 plugins from two third-party libraries that both have a function named "foo".

```
// lib1.js
// ...
    functions.add('foo', function() {
        return "foo";
    });
// ...

// lib2.js
// ...
    functions.add('foo', function() {
        return "bar";
    });
// ...
```

That's ok! You can choose which library's function creates which output.

```
.el-1 {
    @plugin "lib1";
    value: foo();
}
.el-2 {
    @plugin "lib2";
    value: foo();
}
```

This will produce:

```
.el-1 {
    value: foo;
}
.el-2 {
    value: bar;
}
```

For plugin authors sharing their plugins, that means you can also effectively make private functions by placing them in a particular scope. As in, this will cause an error:

```
.el {
    @plugin "lib1";
}
@value: foo();
```

As of Less 3.0, functions can return any kind of Node type, and can be called at any level.

Meaning, this would throw an error in 2.x, as functions had to be part of the value of a property or variable assignment:

```
.block {
    color: blue;
    my-function-rules();
}
```

In 3.x, that's no longer the case, and functions can return At-Rules, Rulesets, any other Less node, strings, and numbers (the latter two are converted to Anonymous nodes).

## Null Functions

There are times when you may want to call a function, but you don't want anything output (such as storing a value for later use). In that case, you just need to return `false` from the function.

```
var collection = [];

functions.add('store', function(val) {
    collection.push(val);  // imma store this for later
    return false;
});
```

```
@plugin "collections";
@var: 32;
store(@var);
```

Later you could do something like:

```
functions.add('retrieve', function(val) {
    return less.value(collection);
});
```

```
.get-my-values {
    @plugin "collections";
    values: retrieve();   
}
```

## The Less.js Plugin Object

A Less.js plugin should export an object that has one or more of these properties.

```
{
    /* Called immediately after the plugin is 
     * first imported, only once. */
    install: function(less, pluginManager, functions) { },

    /* Called for each instance of your @plugin. */
    use: function(context) { },

    /* Called for each instance of your @plugin, 
     * when rules are being evaluated.
     * It's just later in the evaluation lifecycle */
    eval: function(context) { },

    /* Passes an arbitrary string to your plugin 
     * e.g. @plugin (args) "file";
     * This string is not parsed for you, 
     * so it can contain (almost) anything */
    setOptions: function(argumentString) { },

    /* Set a minimum Less compatibility string
     * You can also use an array, as in [3, 0] */
    minVersion: ['3.0'],

    /* Used for lessc only, to explain 
     * options in a Terminal */
    printUsage: function() { },

}
```

The PluginManager instance for the `install()` function provides methods for adding visitors, file managers, and post-processors.

Here are some example repos showing the different plugin types.

- post-processor: <https://github.com/less/less-plugin-clean-css>
- visitor: <https://github.com/less/less-plugin-inline-urls>
- file-manager: <https://github.com/less/less-plugin-npm-import>

## Pre-Loaded Plugins

While a `@plugin` call works well for most scenarios, there are times when you might want to load a plugin before parsing starts.

See: [Pre-Loaded Plugins](https://less.bootcss.com/usage/#plugins) in the "Using Less.js" section for how to do that.

 

# Maps (NEW!)

Released [v3.5.0-beta.4](https://github.com/less/less.js/blob/master/CHANGELOG.md)

> Use rulesets and mixins as maps of values

By combining namespacing with the lookup `[]` syntax, you can turn your rulesets / mixins into maps.

```
@sizes: {
  mobile: 320px;
  tablet: 768px;
  desktop: 1024px;
}

.navbar {
  display: block;

  @media (min-width: @sizes[tablet]) {
    display: inline-block;
  }
}
```

Outputs:

```
.navbar {
  display: block;
}
@media (min-width: 768px) {
  .navbar {
    display: inline-block;
  }
}
```

Mixins are a little more versatile as maps because of namespacing and the ability to overload mixins.

```
#library() {
  .colors() {
    primary: green;
    secondary: blue;
  }
}

#library() {
  .colors() { primary: grey; }
}

.button {
  color: #library.colors[primary];
  border-color: #library.colors[secondary];
}
```

Outputs:

```
.button {
  color: grey;
  border-color: blue;
}
```

You can also make this easier by [aliasing mixins](https://less.bootcss.com/features/#mixins-feature-mixin-aliasing-feature). That is:

```
.button {
  @colors: #library.colors();
  color: @colors[primary];
  border-color: @colors[secondary];
}
```

Note, if a lookup value produces another ruleset, you can append a second `[]` lookup, as in:

```
@config: {
  @options: {
    library-on: true
  }
}

& when (@config[@options][library-on] = true) {
  .produce-ruleset {
    prop: val;
  }
}
```

In this way, rulesets and variable calls can emulate a type of "namespacing", similar to mixins.

As far as whether to use mixins or rulesets assigned to variables as maps, it's up to you. You may want to replace entire maps by re-declaring a variable assigned to a rulset. Or you may want to "merge" individual key/value pairs, in which case mixins as maps might be more appropriate.

### Using variable variables in lookups

One important thing to notice is that the value in `[@lookup]` is the key (variable) name `@lookup`, and is not evaluated as a variable. If you want the key name itself to be variable, you can use the `@@variable` syntax.

E.g.

```
.foods() {
  @dessert: ice cream;
}

@key-to-lookup: dessert;

.lunch {
  treat: .foods[@@key-to-lookup];
}
```

This would output:

```
.lunch {
  treat: ice cream;
}
```

 