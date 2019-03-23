​	

学习目标:



  - 掌握编程jQuery的基本使用
  - 选择器、属性操作、样式操作、节点操作、动画、事件、插件
  - 掌握jQuery插件的使用
  - 能够开发jQuery插件
typora-copy-images-to: media

# jQuery 

## jQuery简介

### JavaScript库的概念

- JavaScript库
  - 把一些浏览器兼容性，或者是一些常用的函数封装到一个js文件中，就是JavaScript库
  - 我们自己封装的animate.js，就是JavaScript库
- 常见的JavaScript库： jQuery、Prototype、MooTools
  - 其中jQuery使用最流行
  - 2014年统计有57%以上的网站使用jQuery

### jQuery

- jQuery 是一个高效、精简并且功能丰富的 JavaScript 工具库。
- 它提供的 API 易于使用且兼容众多浏览器
- 方便HTML 文档遍历和操作，事件处理、动画和 Ajax 操作更加简单。

### jQuery的优点

- jQuery的宗旨
  - Write Less，Do More
- 强大的选择器
- 链式编程
- 隐式迭代
- 丰富的插件，可以自己编写插件
- 开源

### jQuery的版本

- 1.x 版本 

  - 兼容IE6,7,8

- 2.x 版本

  - 不兼容IE6,7,8

- 3.x 版本

  - 更加精简
  - 提供了精简版本，slim

  > 国内很多网站还在使用1.x版本

[jQuery官网](http://jquery.com)

### 体验jQuery

- 下载jQuery

  https://code.jquery.com/jquery/

- 案例：让div显示与设置内容

  - 使用JavaScript开发过程中，有许多不便之处
    - 查找元素的方法太少，麻烦。
    - 遍历伪数组很麻烦，通常要嵌套一大堆的for循环。
    - 有兼容性问题。
    - 想要实现简单的动画效果，也很麻烦
    - 代码冗余。
- 演示jQuery开发

```javascript
$(document).ready(function () {
    $('#btn1').click(function () {
      	// 隐式迭代：偷偷的遍历，在jQuery中，不需要手动写for循环了，会自动进行遍历。
        $('div').show(200);
    });

    $('#btn2').click(function () {
        $('div').text('我是内容');
    });
});
```

- 优点总结：
  - 查找元素的方法多种多样，非常灵活。
  - 拥有隐式迭代特性，因此不再需要手写for循环了。
  - 没有兼容性问题。
  - 实现动画非常简单，而且功能更加的强大。
  - 代码简单、粗暴。

### jQuery中顶级对象

jQuery中的顶级对象是$或jQuery

- 获取jQuery对象
- 入口函数

>  注意：jQuery中的$和jQuery关键字本身为同一对象；

### jQuery中页面加载事件

使用jQuery的三个步骤：

- 引入jQuery文件
- 入口函数
- 功能实现

关于jQuery的入口函数：

```javascript
// 第一种写法
$(document).ready(function() {
});
// 第二种写法
$(function() {
});
```

jQuery入口函数与window.onload的对比

- JavaScript的入口函数要等到页面中所有资源（包括图片、文件）加载完成才开始执行。
- jQuery的入口函数只会等待文档树加载完成就开始执行，并不会等待图片、文件的加载。
- window.onload只能注册一次，jQuery入口函数可以注册多次

$的本质

```js
console.log(typeof $);
```

​	三种常见用法，传对象、传函数、传字符串

## jQuery对象和DOM对象

### DOM对象

- 用原生JavaScript获取的DOM对象
  - 通过document.getElementById()  返回的是元素(DOM对象)


- 通过document.getElementsByTagName()获取到的是什么？
  - 伪数组(集合)，集合中的每一个对象是DOM对象

### jQuery对象

jQuery对象又可以叫做包装集(包装的DOM对象的集合)

- jQuery对象用$()的方式获取的对象
- length属性，获取对象内部的DOM元素个数

### jQuery对象和DOM对象的区别

- **jQuery对象不能使用DOM对象的成员，DOM对象不能使用jQuery对象的成员**

```javascript
// DOM对象
var box = document.getElementById('box');
// 错误
box.text('hello');
// 正确
box.innerText = 'hello';

// jQuery对象，jQuery对象加前缀$，用以区分DOM对象
var $box = $('#box');
// 错误
$box.innerText = 'hello';
// 正确
$box.text('hello');
```

### jQuery对象和DOM对象的相互转换

- DOM对象转换成jQuery对象

  把DOM对象转换成jQuery对象，让我们可以使用jQuery提供的众多的方法**方便操作**。

  - $(DOM对象) 


- jQuery对象转换成DOM对象 

  jQuery对象是包装集(集合)，从集合中取数据可以使用索引的方式

  - jQuery对象[索引值]
  - jQuery对象.get(索引值)

### 案例

- 开关灯

```javascript
// 仅仅演示对象之间的转换，代码不推荐这么写
jQuery(document).ready(function () {
  // 获取元素；
  var inpArr = document.getElementsByTagName('input');
  // 获取第一个按钮，然后绑定事件；
  $(inpArr[0]).click(function () {
    // 设置body的背景色
    $('body')[0].style.background = '#fff';
  });
  // 获取第二个按钮，然后绑定事件；
  $(inpArr[1]).click(function () {
    // 设置body的背景色
    $('body').get(0).style.background = '#000';
  });
});
```

## 选择器

- jQuery可以使用css的选择器来**获取**想要的**元素**，极大的方便了我们查找元素
  - ES5中提供的querySelector()和querySelectorAll()就是借鉴jQuery中的方式。


- jQuery选择器有很多，基本兼容了CSS1到CSS3所有的选择器

- jQuery还添加了很多更加复杂的选择器。（查看jQuery文档）

  >  注意：jQuery选择器返回的是jQuery对象。
  >
  >  jQuery选择器虽然很多，但是选择器之间可以相互替代，就是说获取一个元素，你会有很多种方法获取到。所以我们平时真正能用到的只是少数的最常用的选择器。

### jQuery基本选择器

| 名称    | 用法                | 描述                     |
| ----- | ----------------- | :--------------------- |
| ID选择器 | $('#id')          | 获取指定ID的元素              |
| 类选择器  | $('.class')       | 获取同一类class的元素          |
| 标签选择器 | $('div')          | 获取同一类标签的所有元素           |
| 并集选择器 | $('div,p,li')     | 使用逗号分隔，只要符合条件之一就可。     |
| 交集选择器 | $('div.redClass') | 获取class为redClass的div元素 |

- 总结：跟css的选择器用法一模一样。



### jQuery层级选择器

| 名称    | 用法           | 描述                              |
| ----- | ------------ | :------------------------------ |
| 子代选择器 | $('ul > li') | 使用>号，获取儿子层级的元素，注意，并不会获取孙子层级的元素  |
| 后代选择器 | $('ul li')   | 使用空格，代表后代选择器，获取ul下的所有li元素，包括孙子等 |

如果是事件体内进行选择，也可以这样写

~~~javascript
$('ul').click(function () {
    var index = $(this).index();
    $('li',this)  // ?
})
~~~

- 跟CSS的选择器一模一样。

### jQuery过滤选择器

- 这类选择器都带冒号:

| 名称         | 用法                                | 描述                                 |
| ---------- | --------------------------------- | :--------------------------------- |
| :eq(index) | $('li:eq(2)').css('color', 'red') | 获取到的li元素中，选择索引号为2的元素，索引号index从0开始。 |
| :odd       | $('li:odd').css('color', 'red')   | 获取到的li元素中，选择索引号为奇数的元素              |
| :even      | $('li:even').css('color', 'red')  | 获取到的li元素中，选择索引号为偶数的元素              |

### jQuery筛选选择器(方法)

- 筛选选择器的功能与过滤选择器有点类似，但是用法不一样，筛选选择器主要是方法。

| 名称                 | 用法                                       | 描述                     |
| ------------------ | ---------------------------------------- | :--------------------- |
| children(selector) | \$('ul').children('li')                  | 相当于\$('ull > i')，子类选择器 |
| find(selector)     | \$('ul').find('li')                      | 相当于\$('ul li'),后代选择器   |
| siblings(selector) | $('#first').siblings('li')               | 查找兄弟节点，不包括自己本身。        |
| parent()           | $('#first').parent()                     | 查找父元素                 |
| eq(index)          | $('li').eq(2)| 相当于\$('li:eq(2)'),index从0开始 | 取得index位置的jQuery对象 |
| next()             | $('li').next()                           | 找下一个兄弟                 |
| prev()             | $('li').prev()                           | 找上一个兄弟                |
| get(index) | $("img").get(0); | 取得index位置的DOM元素【对象】 |
| parents() | $('#first').parents('id \| class') | 查找所有父元素中的特定元素 |

#### **get([index])**

> 取得其中一个匹配的元素。 num表示取得第几个匹配的元素。这能够让你选择一个实际的DOM 元素并且对他直接操作，而不是通过 jQuery 函数。$(this).get(0)与\$(this)[0]等价。

~~~javascript
$(this).get(0)与$(this)[0]等价。
<img src="test1.jpg"/> <img src="test2.jpg"/>
$("img").get(0);  // 有参数，返回一个DOM元素 img
$("img").get();  // 没参数，返回一组DOM元素 [img, img]
~~~



#### **index([selector|element])**

 **selector** :   一个选择器，代表一个jQuery对象，将会从这个对象中查找元素。

**element**  :  获得 index  位置的元素。可以是 DOM 元素或 jQuery 选择器。

~~~javascript
<ul>
  <li id="foo">foo</li>
  <li id="bar">bar</li>
  <li id="baz">baz</li>
</ul>
$('#bar').index(); //1，不传递参数，返回这个元素在同辈中的索引位置，算上其他不适li的兄弟元素。
$('#bar').index('li'); //1，传递一个选择器，返回#bar在所有li中的索引位置 

$('li').index(document.getElementById('bar')); //1，传递一个DOM对象，返回这个对象在原先集合中的索引位置
$('li').index($('#bar')); //1，传递一个jQuery对象，返回这个对象在原先集合中的索引位置
$('li').index($('li:gt(0)')); //1，传递一组jQuery对象，返回这个对象中第一个元素在原先集合中的索引位置
~~~



### 案例

- 下拉菜单
- 鼠标进入高亮显示(#CFDFFF  #EFEFFF 鼠标所在行#FFEFBF)
- 鼠标放上突出展示
- 手风琴
- 淘宝服饰精品

## jQuery样式操作

### CSS操作

- 功能：设置或者修改样式，操作的是style属性。

- 操作单个样式

```javascript
// name：需要设置的样式名称
// value：对应的样式值
$obj.css(name, value);
// 使用案例
$('#one').css('background','gray');// 将背景色修改为灰色
```

- 设置多个样式

```javascript
// 参数是一个对象，对象中包含了需要设置的样式名和样式值
$obj.css(obj);
// 使用案例
$('#one').css({
    'background-color':'gray',
    'width':400,
    'height':400,
});
```

- 获取样式

```javascript
// name:需要获取的样式名称
$obj.css(name);
// 案例
$('div').css('background-color');
```

**注意**：获取操作的时候，如果是多个元素，那么只会返回第一个元素的值。

隐式迭代：

1. 设置操作的时候，如果是多个元素，那么给所有的元素设置相同的值

### class操作

- 添加样式类

```javascript
// name：需要添 加的样式类名，注意参数不要带点.
$obj.addClass(name);
// 例子,给所有的div添加one的样式。
$('div').addClass('one');
```

- 移除样式类

```javascript
// name:需要移除的样式类名
$obj.removeClass('name');
// 例子，移除div中one的样式类名
$('div').removeClass('one');
```

- 判断是否有某个样式类

```javascript
// name:用于判断的样式类名，返回值为true false
$obj.hasClass(name)
// 例子，判断第一个div是否有one的样式类
$('div').hasClass('one');
```

- 切换样式类

```javascript
// name:需要切换的样式类名，如果有，移除该样式，如果没有，添加该样式。
$obj.toggleClass(name);
// 例子
$('div').toggleClass('one');
$('div').toggleClass('one two'); // one 和 two两个类名互换
```

### 案例

- 点击按钮控制div显示隐藏
- tab栏切换案例 
- 旋转木马

## jQuery操作属性

### attr操作

- 设置单个属性

```javascript
// 第一个参数：需要设置的属性名
// 第二个参数：对应的属性值
$obj.attr(name, value);
// 用法举例
$('img').attr('title','哎哟，不错哦');
$('img').attr('alt','哎哟，不错哦');
```

- 设置多个属性

```javascript
// 参数是一个对象，包含了需要设置的属性名和属性值
$obj.attr(obj)
// 用法举例
$('img').attr({
    title:'哎哟，不错哦',
    alt:'哎哟，不错哦',
    style:'opacity:.5'
});
```

- 获取属性

```javascript
// 传需要获取的属性名称，返回对应的属性值
$obj.attr(name)
// 用法举例
var oTitle = $('img').attr('title');
alert(oTitle);
```

- 移除属性

```javascript
// 参数：需要移除的属性名，
$obj.removeAttr(name);
// 用法举例
$('img').removeAttr('title');
```

**案例**：美女相册

### prop操作

- 在jQuery1.6之后，对于checked、selected、disabled这类boolean类型的属性来说，不能用attr方法，只能用prop方法。

```javascript
// 设置属性
$(':checked').prop('checked', true);
// 获取属性
$(':checked').prop('checked');// 返回true或者false
```

**案例**：

- 表格全选反选  
  - 选择器   input:checked   获取所有选中的input标签
  - 属性选择器  input[type=checkbox]

### val()/text()/html()

```javascript
$obj.val()		获取或者设置表单元素的value属性的值
$obj.html() 	对应innerHTML
$obj.text()		对应innerText/textContent，处理了浏览器的兼容性
```

案例：

~~~javascript
    $('#btn').click(function () {
      // 一、val()表单元素value值的联系
      // 设置value值
      $("input[type='text']").val('abc');
      // 清除value值
      $("input[type='text']").val('');

      $('.box').text('<h3>hello</h3>');  // 页面显示：<h3>hello</h3>
      $('.box').html('<h3>hello</h3>');  // 页面显示：hello
      console.log($('.box').html());     // 控制台：<h3>hello</h3>
      console.log($('.box').text());     // 控制台：hello
    });
~~~



## jQuery动画

- jQuery提供了三组基本动画，这些动画都是标准的、有规律的效果，jQuery还提供了自定义动画的功能。
- 演示动画效果  

### 三组基本动画

- 显示(show)与隐藏(hide)是一组动画切换(toggle)
- 滑入(slideUp)与滑出(slideDown)与切换(slideToggle)，效果与卷帘门类似
- 淡入(fadeIn)与淡出(fadeOut)与切换(fadeToggle)

```javascript
$obj.show([speed], [callback]);
// speed(可选)：动画的执行时间
	 // 1.如果不传，就没有动画效果。如果是slide和fade系列，会默认为normal
	 // 2.毫秒值(比如1000),动画在1000毫秒执行完成(推荐)
     // 3.固定字符串，slow(600)、normal(400)、fast(200)，如果传其他字符串，则默认为normal。
// callback(可选):执行完动画后执行的回调函数

slideDown()/slideUp()/slideToggle();同理
fadeIn()/fadeOut()/fadeToggle();同理
```

代码：

~~~javascript
    $('#btn').mouseover(function () {
      $('.box').show(1000);
      $('.box').slideDown(1000);
      $('.box').fadeOut();
    });

    $('#btn').mouseout(function () {
      $('.box').hide('fast');
      $('.box').slideUp('normal');
      $('.box').fadeIn('slow');
    });

    $('#btn').click(function () {
      $('.box').toggle(1000);
      $('.box').slideToggle(1000);
      $('.box').fadeToggle(1000);
    });
~~~

**案例**：

- 京东轮播图

### 自定义动画

- animate: 自定义动画

```javascript
$(selector).animate({params},[speed],[easing],[callback]);
// {params}：要执行动画的CSS属性，带数字（必选）
// speed：执行动画时长（可选），默认400
// easing:执行效果，默认为swing（缓动）慢快慢  可以是linear（匀速）
// callback：动画执行完后立即执行的回调函数（可选）
```

代码：

~~~javascript
    $('#btn').click(function () {
 // 按顺序执行，第一步，一步执行完毕后执行下一步
      $('.box').animate({
        left: 300,
      }, 1000);
 // 第二步
      $('.box').animate({
        top: 300,
      }, 1000);
 // 第三部，代码的链式编程写法
      $('.box').animate({
        left: 0,
      }, 1000).animate({
        top: 30,
      }, 1000).animate({
        width: 300,
        height: 300,
      }, 1000, 'swing')
      .animate({
        width: 100,
        height: 100,
      }, 2000, 'linear', 
      function () {
        $('.box').text('动画执行完毕'); // 回执函数,当点击两次发生动画队列的时候，第一次就会回执
      });
    });
// animate() 返回值 jQuery对象，谁调用返回谁
    var $n = $('.box:eq(1)').animate({
    left: 1000
    }, 5000, 'linear');
    console.log($n);               // 控制台：jQuery.fn.init(1)
    console.log($('.box:eq(1)'));  // 控制台：jQuery.fn.init(1)

~~~



### 动画队列与停止动画

- 在同一个元素上执行多个动画，那么对于这个动画来说，后面的动画会被放到动画队列中，等前面的动画执行完成了才会执行（联想：火车进站）。

```javascript
// stop方法：停止动画效果
stop(clearQueue, jumpToEnd);
// 第一个参数：是否清除队列
// 第二个参数：是否跳转到最终效果

 $('#btn1').click(function () {
   // 1. 功能 停止动画
   // 2. 参数
   // clearQueue 是否清空动画队列 默认 false
   // jumpToEnd  是否跳到本次动画结束的位置 false
   // 3. 返回值

   // 1 停止当前动画，继续从当前位置执行下一次动画
   $('.box').stop();

   // 2 清空动画队列
   // 清空动画队列，并且停止当前位置
   $('.box').stop(true);

   // 3 跳到动画结束的位置
   // 第二个参数为true  停止当前动画，并且跳到当前动画结束的位置
   // 停止当前动画并且跳到当前动画结束的位置，继续执行下一次动画
   $('.box').stop(false, true);

   // 4 第一个参数为true  清空动画队列
   // 清空动画队列，停止当前动画并且跳到当前动画结束的位置
   $('.box').stop(true, true);
   });
```

### 案例

- 下拉菜单-动画

~~~javascript
  <div class="box">
    <ul>
      <li class="lis">
        <a href="">一级菜单1</a>
        <ul>
          <li>二级菜单1</li>
          <li>二级菜单1</li>
        </ul>
     </li>
      <li class="lis">
        <a href="">一级菜单1</a>
        <ul>
          <li>二级菜单1</li>
          <li>二级菜单1</li>
        </ul>
      </li>
      <li class="lis">
        <a href="">一级菜单1</a>
        <ul>
          <li>二级菜单1</li>
          <li>二级菜单1</li>
        </ul>
      </li>
    </ul>
  </div>

$('.lis > a').mouseover(function () {
   $(this).siblings('ul').stop().slideDown(300); // 鼠标进入是先.stop()上一个的动画，然后开始这个动画
   // $(this).next().slideToggle();
})
   $('.lis > a').mouseout(function () {
   $(this).next().stop().slideUp(300);  // 鼠标进入是先.stop()上一个的动画，然后开始这个动画
   // $(this).next().slideToggle();
})
~~~



- 作业：开关机动画
- 手风琴特效

## jQuery节点操作

### 创建节点

```javascript
// $(htmlStr)
// htmlStr：html格式的字符串 返回jQuery对象
$('<span>这是一个span元素</span>');
```

### 添加节点

```javascript
append  appendTo	在被选元素的结尾插入内容
prepend prependTo	在被选元素的开头插入内容
before				在被选元素之前插入内容
after				在被选元素之后插入内容

// 添加节点的方法
// 1、创建节点
  var $span = $('<br><span>这是下一个span元素</span><br>');
  var $span1 = $('<br><span>这是上一个span元素</span><br>');
  var $span2 = $('<br><span>这是外部上一个span元素</span><br>');
  var $span3 = $('<br><span>这是外部下一个span元素</span><br>');
// 2、将创建的节点分别添加到不同的位置
  $('#box').append($span);     // 添加到box内部内容之后，第一种写法
  $('#box').prepend($span1);   // 添加到box内部内容之前，第一种写法
  $span.appendTo($('#box'));   // 添加到box内部内容之后
  $span.appendTo('#box');      // 添加到box内部内容之后，第二种写法
  $span1.prependTo('#box');	   // 添加到box内部内容之前，第二种写法
  $('#box').before($span2);    // 添加到box外部内容之前
  $('#box').after($span3);     // 添加到box外部内容之后
```

**案例**：

- 动态生成英雄列表

~~~javascript
  // 模拟后台拉取数据
  var arr = ['鲁班', '刘备', '张飞'];
  // 创建ul
  var $ul = $('<ul></ul>');
  $('.box').append($ul);
  // 遍历数组在box盒子中分别添加数组元素到li标签中
  arr.forEach(function (item) {
      var $li = $('<li>' + item + '</li>');
      $ul.append($li);
  });
  // 点击添加按钮
  $('#btn').click(function () {
     // 获取输入框中的内容
      var $val = $('input:eq(0)').val();
     // 创建一个li标签并添加到li标签中
      var $li = $('<li>' + $val + '</li>');
      $ul.append($li);   // 添加到内容前边
      $ul.prepend($li);  // 添加到内容后边
     // 清空输入框中的内容
      $('input:eq(0)').val('');
  });
~~~


- 作业：城市选择案例
- 作业：动态创建表格，学生成绩表

### 清空节点与删除节点

- empty：清空指定节点的所有元素，自身保留(清理门户)

```javascript
$('div').empty(); // 清空div的所有内容（推荐使用，会清除子元素上绑定的内容，源码）
$('div').html('');// 使用html方法来清空元素。
```

- remove：相比于empty，自身也删除（自杀）

```javascript
$('div').remove();
```

### 克隆节点

- 作用：复制匹配的元素

```javascript
// 复制$(selector)所匹配到的元素（深度复制）
// cloneNode(true)
// 返回值为复制的新元素，和原来的元素没有任何关系了。即修改新元素，不会影响到原来的元素。
$(selector).clone();

var $span4 = $('#box').clone();  // 克隆包括文本和内部标签
$('#box').before($span4);
```

### 案例

- 删除和添加表格数据
- 根据数据生成表格

## jQuery尺寸和位置操作

### width方法与height方法

- 设置或者获取高度，不包括内边距、边框和外边距
- 可以是字符串或者数字，还可以是一个**函数**，返回要设置的数值。函数接受两个参数，第一个参数是元素在原先集合中的**索引位置**，第二个参数为原先的**高度**。 

```javascript
// 带参数表示设置高度
$('img').height(200);
// 不带参数获取高度
$('img').height();
// 参数为函数function(index, height)
$("button").click(function(){
    $("p").height(function(n,c){
    return c+10;
    });
 });
```

获取网页的可视区宽高

```javascript
// 获取可视区宽度
$(window).width();
// 获取可视区高度
$(window).height();
```

### innerWidth/innerHeight/outerWidth/outerHeight

> 作用：只能获取元素宽高，不能设置元素宽高

```javascript
innerWidth()/innerHeight()	方法返回元素的宽度/高度（包括内边距）。
outerWidth()/outerHeight()  方法返回元素的宽度/高度（包括内边距和边框）。
outerWidth(true)/outerHeight(true)  方法返回元素的宽度/高度（包括内边距、边框和外边距）,最近定位父元素的坐标。
```

### scrollTop( val )与scrollLeft( val )

- 设置或者获取垂直滚动条的位置

```javascript
获取：
// 获取页面被卷曲的高度
$(window).scrollTop();
// 获取页面被卷曲的宽度
$(window).scrollLeft();
设置：
// 也可以填入一个参数数字，设置相对滚动条顶部偏移的距离
   $('#totop').click(function () {
        $('html,body').scrollTop(0);  // 点击跳回顶部
    })
// scroll当页面滚动时触发事件，输出滚动值
$(window).scroll(function () {
    console.log($(window).scrollTop());
})

// 设置滚动动画
$('body,html').animate({
    scrollTop: 0   // 点击以动画的方式运动回顶部
}, 100)
```

### resize()

jQuery监听 浏览器窗口大小的变化事件

~~~js
$(window).resize(function () {          //当浏览器大小变化时
    alert($(window).height());          //浏览器时下窗口可视区域高度
    alert($(document).height());        //浏览器时下窗口文档的高度
    alert($(document.body).height());   //浏览器时下窗口文档body的高度
    alert($(document.body).outerHeight(true)); //浏览器时下窗口文档body的总高度 包括border padding margin
});
~~~

### offset方法与position方法

- offset方法获取元素距离document的位置，position方法获取的是元素距离有定位的父元素(offsetParent)的位置。

```javascript
// 获取或设置元素距离document的位置,返回值为对象：{left:100, top:100}
// 指的是从边框外边到文档边的距离，不包括自身的border
$(selector).offset();
// 获取相对于其最近的有定位的父元素的位置,可以设置。 
// 注意： 获取的是该元素从margin开始计算的坐标
设置：
$(selector).offset({
    top: 10px,
    left: 10px,
});
$(selector).position();       // 只能获取位置，不能设置位置
// 获取最近定位的父元素
$(selector).offsetParent();   // jQuery.fn.init [div#box1]
代码：
  <style>
    body {
      height: 1000;
      margin: 0;
      padding: 0;
    }
    #box1 {
      width: 500px;
      height: 500px;
      background-color: burlywood;
      margin: 50px;
      border: 1px solid red;
    }

    #box2 {
      width: 300px;
      height: 300px;
      background-color: antiquewhite;
      margin: 40px;
      padding: 20px;
      border: 2px solid red;
    }
  </style>
<body>
  <div id="box1">
    <div id="box2"></div>
  </div>
  <script src="./jquery-1.12.4.js"></script>
  <script>
      console.log($('#box2').offset());  // 结果：对象{top: 91, left: 91}
      console.log($('#box2').offset().top);  // 结果：91
      console.log($('#box2').offset().left);  //  结果：91 
	
      console.log($('#box2').position()); 
      // 父级没有定位，返回：对象{top: 51, left: 51}
	 // 父级定位position：relative，返回：{top: 0, left: 0}    
     ※ 获取的是该元素从margin开始计算的坐标
  </script>
```

案例：固定导航栏 
案例：电梯导航

## jQuery事件机制

- JavaScript中已经学习过了事件，jQuery对JavaScript事件进行了封装，增加并扩展了事件处理机制。jQuery不仅提供了更加优雅的事件处理语法，而且极大的增强了事件的处理能力。

### jQuery事件发展历程(了解)

简单事件绑定--bind事件绑定--delegate事件绑定--on事件绑定(推荐)

- 简单事件注册

```javascript
// handler表示执行的事件函数
click(handler)		    单击事件
mouseenter(handler)		鼠标进入事件
mouseleave(handler)		鼠标离开事件
代码：
// 链式编程，事件也可以连写，都起作用
	$('#u li')
    .click(function () {
      console.log('xxxxx');
    })
    .mouseover(function () {  
      console.log('xxxxx');
    });	
```

缺点：不能同时注册多个事件 bind方式注册事件

```javascript
// 第一个参数：事件类型
// 第二个参数：事件处理程序
$('p').bind('click mouseenter', function(){
    // 事件响应方法
});

代码：
// 注册单个事件
  $('#u li').bind('click', function () {
    console.log('0000');
  })
// 让多个事件，同时注册同一个事件处理函数
  $('#u li').bind('click mouseover', function () {
    console.log('0000');
  });
// 注册多个事件处理函数，以给对象添加方法的方式
  $('#u li').bind({
    click: function () {
     console.log('这是点击事件');
    },
    mouseover: function () {
     console.log('这是鼠标移上事件');
    }
  });
```

缺点：不支持动态事件绑定

- delegate注册委托事件

```javascript
// 第一个参数：selector，要绑定事件的元素
// 第二个参数：事件类型
// 第三个参数：事件处理函数
$('.parentBox').delegate('p', 'click', function(){
    // 为 .parentBox下面的所有的p标签绑定事件
});
```

缺点：只能注册委托事件，因此注册时间需要记得方法太多了

- on注册事件

### on注册事件(重点)

- jQuery1.7之后，jQuery用on统一了所有事件的处理方法。
- 最现代的方式，兼容zepto(移动端类似jQuery的一个库)，强烈建议使用。

on注册事件

```javascript
// 表示给$(selector)绑定事件，并且由自己触发，不支持动态绑定。
$(selector).on( 'click mouseover', function() {});

// 第一个参数：events，绑定事件的名称可以是由空格分隔的多个事件（标准事件或者自定义事件）
// 第二个参数：selector, 执行事件的后代元素（可选），如果没有后代元素，那么事件将有自己执行。
// 第三个参数：data，传递给处理函数的数据，事件触发的时候通过event.data来使用（不常使用）
// 第四个参数：handler，事件处理函数
$(selector).on(events[,selector][,data],handler);

代码：
// 注册单个事件
  $('#u li').bind('click', function () {
    console.log('0000');
  })
// 让多个事件，同时注册同一个事件处理函数
  $('#u li').bind('click mouseover', function () {
    console.log('0000');
  });
// 注册多个事件处理函数，以给对象添加方法的方式
  $('#u li').bind({
    click: function () {
     console.log('这是点击事件');
    },
    mouseover: function () {
     console.log('这是鼠标移上事件');
    }
  });
```

on注册事件委托

```javascript
// 表示给$(selector)绑定代理事件，当必须是它的内部元素span才能触发这个事件，支持动态绑定
$(selector).on( 'click', 'span', function() {});
```

事件委托原理

```javascript
// 事件委托的原理
var ul = document.querySelector('#ul');
ul.onclick = function (e) {
  // console.log(e.target.tagName);
  if (e.target.tagName.toLowerCase() === 'li') {
    console.log(e.target);
  }
}
```

**案例**：表格删除案例

- 通过源码查看 bind click delegate on 注册事件的区别

### 事件解绑

- unbind方式（不用）

```javascript
$(selector).unbind(); // 解绑所有的事件
$(selector).unbind('click'); // 解绑指定的事件
```

- undelegate方式（不用）

```javascript
$( selector ).undelegate(); // 解绑所有的delegate事件
$( selector).undelegate( 'click' ); // 解绑所有的click事件
```

- off方式（推荐）

```javascript
// 解绑匹配元素的所有事件
$(selector).off();
// 解绑匹配元素的所有click事件
$(selector).off('click');
// 解绑所有li元素的所有click事件
$(selector).off('click', 'li');
```

### 触发事件？

### trigger('click')

语法：$(selector).trigger(event,[param1,param2,...])

```javascript
触发事件
$(selector).click();  // 触发 click事件
$(selector).trigger('click');  // click内部调用了trigger
```

| 参数                    | 描述                                                         |
| ----------------------- | ------------------------------------------------------------ |
| event                   | 必需。规定指定元素要触发的事件。可以使自定义事件（使用 bind() 函数来附加），或者任何标准事件。 |
| [*param1*,*param2*,...] | 可选。传递到事件处理程序的额外参数。额外的参数对自定义事件特别有用。 |

实例：

~~~js

<script type="text/javascript" src="/jquery/jquery.js"></script>
<script type="text/javascript">
$(document).ready(function(){
  $("input").select(function(){
    $("input").after("文本被选中！");
  });
  $("button").click(function(){
    $("input").trigger("select");
  });
});
</script>

<body>
    <input type="text" name="FirstName" value="Hello World" />
    <br />
    <button>激活 input 域的 select 事件</button>
</body>
~~~



### Hover

语法：

hover([over,]out)

```javascript
// over:鼠标移到元素上要触发的函数
// out:鼠标移出元素要触发的函数
$(selector).hover(fnEnter, fnLeave);
案例：
<div class="box"></div>
$('.box').hover(function () {
   $(this).css('backgroundColor', 'red');
}, function () {
   $(this).css('backgroundColor', '');
})
// 下面的简写形式
$(selector).mouseenter(function () {
}).mouseleave(function () {
})

```

### jQuery事件对象

jQuery事件对象其实就是js事件对象的一个封装，处理了兼容性。

```javascript
不常用，可以用来获取鼠标，手指，等相对整个设备显示屏的坐标
// screenX和screenY	对应屏幕最左上角的值
经常用来获取元素中的坐标
// offsetX和offsetY  获取鼠标在元素中的位置
不推荐用，鼠标滚动后就会出现问题
// clientX和clientY	距离页面左上角的位置（忽视滚动条）
推荐常用
// pageX和pageY	距离页面最顶部的左上角的位置（会计算滚动条的距离）

// event.keyCode	按下的键盘代码
// event.data	存储绑定事件时传递的附加数据
    第一个：事件类型(名称)
    第二个：选择器 - 事件委托
    第三个：data - 传入事件处理函数内部的数据
    第四个：事件处理函数
    $('#btn').on('click', { left: 10, top: 20 }, function (e) {
      console.log(e.data);
    });

// event.stopPropagation()	阻止事件冒泡行为
// event.preventDefault()	阻止浏览器默认行为
// return false:既能阻止事件冒泡，又能阻止浏览器默认行为。
			   一般阻止不了事件监听注册的标签默认行为
```

### 案例

- 按键变色

## jQuery补充知识点

### 链式编程

- 通常情况下，只有设置操作才能把链式编程延续下去。因为获取操作的时候，会返回获取到的相应的值，无法返回 jQuery对象。

```javascript
end(); // 筛选选择器会改变jQuery对象的DOM对象，想要回复到上一次的状态，并且返回匹配元素之前的状态。
```

### each方法

- jQuery的隐式迭代会对所有的DOM对象设置相同的值，但是如果我们需要给每一个对象设置不同的值的时候，就需要自己进行迭代了。
- 每次执行传递进来的函数时，函数中的this关键字都指向一个不同的DOM元素（每次都是一个不同的匹配元素）。 

作用：遍历jQuery对象集合，为每个匹配的元素执行一个函数

```javascript
// 参数一：index表示当前元素在所有匹配元素中的索引号
// 参数二：element表示当前元素（DOM对象）
$(selector).each(function(index,element){});
代码：
	<img/>
    <img/>
    $("img").each(function(i){
   		this.src = "test" + i + ".jpg";
 	});
结果：<img src="test0.jpg" />, <img src="test1.jpg" /> 
注：    
如果你想得到 jQuery对象，可以使用 /* $(this) */ 函数。
可以使用 'return' 来提前跳出 each() 循环。
```

### 多库共存

- jQuery使用$作为标示符，但是如果与其他框架中的$冲突时，jQuery可以释放$符的控制权.

```javascript
var c = $.noConflict();// 释放$的控制权,并且把$的能力给了c
```

### 案例

- 五角星评分案例

## 插件

怎么获取插件

​	百度搜索、github搜索

​	看技术文章

​	跟别人交流

插件怎么用

​	看demo，知道插件的功能

​	看文档，readme.md

​	通过demo/文档，快速的实践一下

### 常用插件

- 弹出层插件 layer
  - [layer插件](https://github.com/sentsin/layer)
- 放大镜插件
  - [jQuery.zoom](http://www.jacklmoore.com/zoom/)
- 轮播图插件
  - [http://sorgalla.com/jcarousel/](http://sorgalla.com/jcarousel/)
  - [https://github.com/OwlCarousel2/OwlCarousel2](https://github.com/OwlCarousel2/OwlCarousel2)
- 图片懒加载插件
  - [jQuery.lazyload](https://github.com/tuupola/jquery_lazyload)
- jQueryUI
  - 常用的2-3个功能演示
  - [jQueryUI](https://jqueryui.com/)
- 查看jQuery插件的源码

### 自己探索插件

- [artDialog](https://github.com/aui/artDialog)
- [图片放大](https://github.com/fat/zoom.js)
- [github上搜索](http://www.github.com)


## jQuery插件开发

- 给jQuery增加方法的两种方式

```javascript
$.method = fn		静态方法
$.fn.method = fn	实例方法
```

- 增加一个静态方法，实现两个数的和，插件

```javascript
(function ($) {
  $.add = function (a, b) {
    return a + b;
  }
}(jQuery))

$.add(5, 6);
```

- tab栏插件

```javascript
(function ($) {
  // {tabMenu: '#aa'}
  $.tab = function (options) {
    // 默认参数
    var defaults = {
      tabMenu: '#tab',
      activeClass: 'active',
      tabMain: '#tab-main',
      tabMainSub: '.main',
      selectedClass: 'selected'
    }
    // 把options中的属性，把对应属性的值赋给defaults对应的属性
    // defaults.tabMenu = options.tabMenu || defaults.tabMenu;
    // for(var key in options) {
    //   defaults[key] = options[key];
    // }
    $.extend(defaults, options);

    $(defaults.tabMenu).on('click', 'li', function () {
      $(this)
        .addClass(defaults.activeClass)
        .siblings()
        .removeClass(defaults.activeClass);

      //
      var index = $(this).index();
      //
      $(defaults.tabMain + ' ' + defaults.tabMainSub)
        .eq(index)
        .addClass(defaults.selectedClass)
        .siblings()
        .removeClass(defaults.selectedClass);
    })
  }
}(window.jQuery))
```

- 表格插件

```javascript
(function($) {
  // 内部的变量，外部无法访问，防止变量名冲突
  var a = 0;
  // 给$增加了一个实例方法
  $.fn.table = function (header, data) {
    var array = [];
    array.push('<table>');
    array.push('<tr>');

    // 生成表头
    $.each(header, function () {
      array.push('<th>' + this + '</th>');
    })
    array.push('</tr>');


    // 生成数据行
    $.each(data, function (index) {
      // this是当前遍历到的数组中的每一个对象
      // 拼数据行
      array.push('<tr>');
      array.push('<td>' + (index + 1) + '</td>');

      // 遍历对象，拼表格
      for (var key in this) {
        array.push('<td>' + this[key] + '</td>');
      }

      array.push('</tr>');
    })
    array.push('</table>');

    this.append(array.join(''));
  }

}(window.jQuery))
```

- 插件开发的原理