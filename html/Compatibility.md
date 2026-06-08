# 浏览器的兼容问题及解决方案整理

## 前言

> 有一天 leader 突然找到我说，咱们之后可能会给一些老系统做兼容问题处理。我还很天真地问：“有多老呢？”
>
> “哦，也没多老，就是用 jsp 语法写的，原本可能只是兼容 IE7 的，现在要兼容 IE8、IE9 和火狐、谷歌之类的浏览器”
>
> jsp ？ IE7 ？？ IE8 ？？？ IE9 ？？？？ 兼容问题？？？？？那我走？
>
> 走是不可能走的，不过我想听到要给 IE 浏览器做兼容的时候，可能大多数前端开发都会表面笑嘻嘻，心里 MMP 吧。
>
> 然后 leader 发了一些 word 文档给我，是一些兼容问题及解决方案，不过很混乱，让我整理一下。呼！那就整理呗，也安慰自己这也是一个难得的接触早期系统开发的机会。
>
> 整理完之后 leader 又来新信息了，说不用我们做兼容处理，只要整理好的这个文档和做一下怎么解决兼容问题的演示指导就好了，也就是只要有问题的时候技术支援一下就好。哈哈哈，峰回路转，非常开心！
>
> 我就不需要给 jsp 老系统做 IE 浏览器的兼容了，哈哈，希望你也不用做！

下面是整理的资料：

## 一、了解浏览器

主流浏览器 有五个：IE(Trident内核)、Firefox(火狐：Gecko内核)、Safari(苹果：webkit内核)、Google Chrome(谷歌：Blink内核)、Opera(欧朋：Blink内核)

四大内核：Trident(IE内核)、Gecko(Firefox内核)、webkit内核、Blink(Chrome内核)

## 二、了解兼容问题

W3C 对标准的推进，Firefox、Chrome、Safari、Opera 的出现，结束了 IE 雄霸天下的日子。

然而，这对开发者来说，是好事，也是坏事。说它是好事，是因为浏览器厂商为了取得更多的市场份额，会促使各浏览器更符合 W3C 标准，而得到更好的兼容性，并且，不同浏览器的扩展功能(例如：-moz-、-webkit- 开头的样式)，对 W3C 标准也是个推进；说它是坏事，因为，多个浏览器同时存在，这些浏览器在处理一个相同的页面时，表现有时会有差异。这种差异可能很小，甚至不会被注意到；也可能很大，甚至造成在某个浏览器下无法正常浏览。我们把引起这些差异的问题统称为“浏览器兼容性问题”。而正是这些“浏览器兼容性问题”，无形中给我们的开发增加了不少难度。

1. 什么是兼容问题？

    答：同样的代码，在不同的浏览器上显示的页面效果不一样

2. 不一样的原因是什么？

    答：浏览器各浏览器使用了不同的内核，并且它们处理同一件事情的时候思路不同。

3. 为什么浏览器会存在兼容问题？

    同一浏览器，版本越老，存在 bug 越多，相对于版本越新的浏览器，对新属性和标签、新特性支持越少。

    不同浏览器，核心技术(内核)不同，标准不同，实现方式也有差异，最终呈现出来在大众面前的效果也是会有差异。

    设计师写出了不规范的代码，不规范的代码会使不兼容现象更加突出。

从浏览器内核的角度来看，浏览器兼容性问题可分为以下三类：

渲染相关：和 样式 相关的问题，即体现在布局效果上的问题。

脚本相关：和 脚本 相关的问题，包括JavaScript和DOM、BOM方面的问题。对于某些浏览器的功能方面的特性，也属于这一类。

其他类别：除以上两类问题外的功能性问题，一般是浏览器自身提供的功能，在内核层之上的。

不规则的嵌套：

```html
<div>
   <li>新闻标题一</li>
   <li>新闻标题一</li>
   <li>新闻标题一</li>
</div>
```

div 中直接嵌套 li 元素是不合标准的，li 应该处于 ul/ol 内。此类问题常见的还有 p 中嵌套 div、table 等元素。

不规范的DOM接口和属性设置：

```javascript
document.all.a_name.style.top = 35;
```

上面代码中 top 的值，其实应该是一个字符串值，需有单位。例如：35px。

总之，人为的原因也占很大一部分。而人为造成兼容性问题的原因，除了粗心之外，大都源于浏览器 bug 的存在，和开发者对标准的不了解。

比如，如果要做一个功能，功能是想让鼠标悬停在 img 元素上方时，可以出现提示信息，经常针对 IE 做开发的人，可能会使用 img 元素的 "alt" 属性，但其他浏览器中就是不给 "alt" 属性面子。因为 W3C 标准中规定要去做这件事的属性是 "title"，大多浏览器符合标准，IE 不符合，这是 IE 浏览器内核的问题；开发者不知道 "title" ，不遵循标准去写代码，是开发者的问题。

所以，一个问题分两半，浏览器和开发者都有责任。既然都有责任，就都有义务去解决兼容性问题。那么，从浏览器的角度来讲，它的厂商应该修复浏览器的 bug 和不合标准的地方，当某一天 IE 的 "alt" 不能用于提示了，还有人用这个错误的属性去显示提示么？从开发者角度来讲，多了解标准，了解浏览器兼容性问题，就可以在开发的过程中，有效的避开兼容性问题，让你的页面在所有浏览器中畅通无阻。

## 三、处理兼容问题的思路

### 1、要不要做？

1. 从产品的角度看：产品的受众、受众的浏览器比例、效果优先还是基本功能优先。

2. 成本的角度：有无必要做这个兼容。

### 2、做到什么程度？

答：让哪些浏览器支持哪些效果。

### 3、如何做？

1. 根据兼容需求选择技术框架/库(如  jquery 1.x.x )。

2. 根据兼容需求选择兼容工具：html5shiv、Respond.js、CSS Reset、normalize.css、Modernizr.js、postcss。

3. 条件注释、CSS Hack、js能力检测做一些修补。Hack：CSS 中，Hack 是指一种兼容 CSS 在不同浏览器中正确显示的技巧方法，修补 bug 的方法 Filter ：表示过滤器的意思，它是一种对特定的浏览器或浏览器组显示或隐藏规则或声明的方法。本质上讲，Filter 是 hack 方法中的一种。

### 4、渐进增强和优雅降级

1. 渐进增强：针对低版本浏览器进行构建页面，保证最基本的功能，然后再针对高级浏览器进行效果、交互等改进和追加功能达到更好的用户体验。

2. 优雅降级：一开始就构建完整的功能，然后再针对低版本浏览器进行兼容。

## 四、JavaScript兼容

### 1、```addEventListener``` 与 ```attachEvent``` 区别

```attachEvent``` ——兼容：IE7、IE8；不兼容firefox、chrome、IE9、IE10、IE11、safari、opera

```addEventListener``` ——兼容：firefox、chrome、IE、safari、opera；不兼容IE7、IE8

解决方案：

```javascript
function addEvent(elm, evType, fn, useCapture) {
    if (elm.addEventListener) { // W3C标准
        elm.addEventListener(evType, fn, useCapture);
        return true;
    } else if (elm.attachEvent) { // IE
        var r = elm.attachEvent('on' + evType, fn); // IE5+
        return r;
    } else {
        elm['on' + evType] = fn; // DOM事件
    }
}
```

### 2、```document.formName.item("itemName")``` 问题

问题说明：

IE下，可以使用 ```document.formName.item("itemName")``` 或 ```document.formName.elements ["elementName"]```

Firefox 下，只能使用 ```document.formName.elements["elementName"]```。

解决方案：统一使用 ```document.formName.elements["elementName"]```。

### 3、集合类对象问题

问题说明：

IE下，可以使用 () 或 [] 获取集合类对象

Firefox下，只能使用 [] 获取集合类对象。

解决方案：统一使用 [] 获取集合类对象。

### 4、自定义属性问题.

问题说明：

IE下，可以使用获取常规属性的方法来获取自定义属性，也可以使用 ```getAttribute()``` 获取自定义属性

Firefox下，只能使用 ```getAttribute()``` 获取自定义属性。

解决方案：统一通过 ```getAttribute()``` 获取自定义属性。

### 5、```eval("idName")``` 问题

问题说明：

IE下，可以使用 ```eval("idName")``` 或 ```getElementById("idName")``` 来取得 id 为 idName 的 HTML 对象

Firefox下，只能使用 ```getElementById("idName")``` 来取得 id 为 idName 的 HTML 对象。

解决方案：统一用 ```getElementById("idName")``` 来取得 id 为 idName 的 HTML 对象。

### 6、变量名与某 HTML 对象 ID 相同的问题

问题说明：

IE下，HTML 对象的 ID 可以作为 document 的下属对象变量名直接使用，Firefox下则不能

Firefox下，可以使用与 HTML 对象 ID 相同的变量名，IE下则不能

解决方案：使用 ```document.getElementById("idName")``` 代替 ```document.idName```。最好不要取 HTML 对象 ID 相同的变量名，以减少错误；在声明变量时，一律加上 ```var``` 关键字，以避免歧义。

### 7、const 问题

问题说明：

Firefox下，可以使用 ```const``` 关键字或 ```var``` 关键字来定义常量

IE下，只能使用 ```var``` 关键字来定义常量

解决方案：统一使用 ```var``` 关键字来定义常量。

### 8、```input.type``` 属性问题

问题说明：

IE下 ```input.type``` 属性为只读

但是Firefox下 ```input.type``` 属性为读写

解决方案：不修改 ```input.type``` 属性。如果必须要修改，可以先隐藏原来的 ```input```，然后在同样的位置再插入一个新的 ```input``` 元素。

### 9、```window.event``` 问题

问题说明：

```window.event``` 只能在IE下运行，而不能在Firefox下运行，这是因为Firefox的 event 只能在事件发生的现场使用。解决方案：在事件发生的函数上加上 event 参数，在函数体内(假设形参为 evt )使用 ```var myEvent = evt ? evt : (window.event ? window.event : null)```

示例：
```html
<input type="button" onclick="doSomething(event)"/>
<script language="javascript">
function doSomething(evt) {
    var myEvent = evt ? evt : (window.event ? window.event : null);
    // ...
}
</script>
```

### 10、event.x 与 event.y 问题

问题说明：

IE下，event 对象有 x、y 属性，但是没有 pageX、pageY 属性

Firefox下，event 对象有 pageX、pageY 属性，但是没有 x、y 属性

解决方案：```var myX = event.x ? event.x : event.pageX; var myY = event.y ? event.y:event.pageY;``` 如果考虑第 9 条问题，就改用 myEvent 代替 event 即可。

### 11、event.srcElement 问题

问题说明：IE下，event 对象有 srcElement 属性，但是没有 target 属性；Firefox下，event 对象有 target 属性，但是没有 srcElement 属性。解决方案：使用 ```srcObj = event.srcElement ? event.srcElement : event.target;``` 如果考虑第 9 条问题，就改用 myEvent 代替 event 即可。

### 12、window.location.href 问题

问题说明：IE或者Firefox 2.0.x下，可以使用 ```window.location``` 或 ```window.location.href```；Firefox 1.5.x下，只能使用 ```window.location```。解决方案：使用 ```window.location``` 来代替 ```window.location.href``` 。当然也可以考虑使用 ```location.replace()``` 方法。

### 13、模态和非模态窗口问题

问题说明：

IE下，可以通过 ```showModalDialog``` 和 ```showModelessDialog``` 打开模态和非模态窗口

Firefox下则不能。

解决方案：直接使用 ```window.open(pageURL,name,parameters)``` 方式打开新窗口。如果需要将子窗口中的参数传递回父窗口，可以在子窗口中使用 ```window.opener``` 来访问父窗口。如果需要父窗口控制子窗口的话，使用 ```var subWindow = window.open(pageURL,name,parameters);``` 来获得新开的窗口对象。

### 14、frame 和 iframe 问题

以下面的 frame 为例：
1. 访问 frame 对象

    IE：使用 ```window.frameId``` 或者 ```window.frameName``` 来访问这个 frame 对象

    Firefox：使用 ```window.frameName``` 来访问这个 frame 对象

    解决方案：统一使用 ```window.document.getElementById("frameId")``` 来访问这个 frame 对象

2. 切换 frame 内容在IE和Firefox中都可以使用 ```window.document.getElementById("frameId").src = "webjx.com.html"```  或 ```window.frameName.location = "webjx.com.html"``` 来切换 frame 的内容；如果需要将 frame 中的参数传回父窗口，可以在 frame 中使用 parent 关键字来访问父窗口。

### 15、事件委托方法

问题说明：

IE下，使用 ```document.body.onload = inject;```，其中 ```function inject()``` 在这之前已被实现

在Firefox下，使用 ```document.body.onload = inject();```

解决方案：统一使用 ```document.body.onload = new Function("inject()");``` 或者 ```document.body.onload = function(){}```

[**注意**] Function 和 function 的区别

### 16、用 ```setAttribute``` 设置事件

```javascript
var obj = document.getElementById("objId");
obj.setAttribute("onclick", "funcitonname()");
```

FireFox支持，IE不支持。

解决方案：IE中必须用点记法来引用所需的事件处理程序,并且要用赋予匿名函数；如下：

```javascript
var obj = document.getElementById("objId");
obj.onclick = function() { fucntionname(); };
```

这种方法所有浏览器都支持

### 17、访问的父元素的区别

问题说明：
在IE下，使用 ```obj.parentElement``` 或 ```obj.parentNode``` 访问 obj 的父结点

在FireFox下，使用 ```obj.parentNode``` 访问 obj 的父结点

解决方案：因为FireFox与IE都支持DOM，因此统一使用 ```obj.parentNode``` 来访问 obj 的父结点。

### 18、innerText的问题

问题说明：

innerText 在IE中能正常工作，但是 innerText 在FireFox中却不行。

解决方案：在非IE浏览器中使用 textContent 代替 innerText。示例：

```javascript
if (navigator.appName.indexOf("Explorer") > -1) {
    document.getElementById("element").innerText = "my text";
} else {
    document.getElementById("element").textContent = "my text";
}
```

[注] innerHTML 同时被IE、FireFox等浏览器支持，其他的，如 outerHTML 等只被IE支持，最好不用。

### 19、Table 操作问题

问题说明：

IE、FireFox以及其它浏览器对于 table 标签的操作都各不相同，在IE中不允许对 table 和 tr 的 innerHTML 赋值，使用 js 增加一个 tr 时，使用 appendChild 方法也不管用。```document.appendChild``` 在往表里插入行时FireFox支持，IE不支持解决方案：把行插入到 tbody 中，不要直接插入到表解决方法：

```javascript
// 向table追加一个空行：
var row = otable.insertRow(-1);
var cell = document.createElement("td");
cell.innerHTML = "";
cell.className = "XXXX";
row.appendChild(cell);
```

[注] 建议使用JS框架集来操作 table，如 JQuery。

### 20、对象宽高赋值问题

问题说明：FireFox中类似 ```obj.style.height = imgObj.height``` 的语句无效。解决方案：统一使用 ```obj.style.height = imgObj.height + "px";```

### 21、```setAttribute("style", "color: red;")```

FireFox支持(除了IE，现在所有浏览器都支持)，IE不支持解决方案：不用 ```setAttribute("style", "color : red;")``` 而用 ```object.style.cssText = "color: red;"``` (这写法也有例外)最好的办法是上面种方法都用上，万无一失

### 22、类名设置 ```setAttribute("class", "styleClass")```

FireFox支持，IE不支持(指定属性名为 class，IE不会设置元素的 class 属性，相反只使用 setAttribute 时IE自动识 classname 属性)解决方案：```setAttribute("class", "styleClass")``` ```setAttribute("className", "styleClass")``` 或者直接 ```object.className= "styleClass";``` IE和FF都支持 ```object.className```。

### 23、建立单选钮

```javascript
// IE以外的浏览器
var rdo = document.createElement("input");
rdo.setAttribute("type", "radio");
rdo.setAttribute("name", "radiobtn");
rdo.setAttribute("value", "checked");
// IE
var rdo = document.createElement("<input name="radiobtn" type="radio" value="checked" />");
```

解决方案：这一点区别和前面的都不一样。这次完全不同，所以找不到共同的办法来解决，那么只有 IF-ELSE了万幸的是，IE可以识别出 document 的 uniqueID 属性，别的浏览器都不可以识别出这一属性。问题解决。​

## 五、CSS兼容

### 1、CSS Hack

使用 hacker 可以把浏览器分为3类：IE6；IE7和遨游；其他（IE8 Chrome ff Safari opera等）
1. IE6认识的 hacker 是 下划线 _  和星号 *
2. IE7和遨游认识的 hacker 是 星号 * 如：

```css
height: 300px;
*height: 200px;
_height: 100px;

/*
1. IE6浏览器在读到 height: 300px 的时候会认为高时 300px 继续往下读，他也认识 *heihgt， 所以当IE6读到 *height: 200px 的时候会覆盖掉前一条的相冲突设置，认为高度是 200px。 继续往下读，IE6还认识 _height,所以他又会覆盖掉 200px 高的设置，把高度设置为 100px。
2. IE7和遨游也是一样的从高度 300px 的设置往下读。当它们读到 *height: 200px 的时候就停下了，因为它们不认识 _height。 所以它们会把高度解析为 200px ，剩下的浏览器只认识第一个 height: 300px; 所以他们会把高度解析为 300px。因为优先级相同且想冲突的属性设置后一个会覆盖掉前一个，所以书写的次序是很重要的。
*/
```

### 2、CSS 样式兼容不同浏览器问题

所有浏览器通用：height: 100px;

IE6 专用：_height: 100px; 或者 *height: 100px;

IE7 专用：*+height: 100px;

IE7、FF 共用：height: 100px !important;

以下两种方法几乎能解决现今所有兼容：

1. !important

随着IE7对 !important 的支持, !important 方法现在只针对IE6的兼容(注意写法，记得该声明位置需要提前)

```css
.box {
    width: 100px !important; /* IE7+FF */
    width: 80px; /* IE6 */
}
```

2. IE6/IE7对FireFox

*+html 与 *html 是IE特有的标签，FireFox 暂不支持。而 *+html 又为 IE7 特有标签。

```css
.box { width: 120px; } /* FireFox */
*html .box { width: 80px;} /* ie6 fixed */
*+html .box { width: 60px;} /* ie7 fixed, 注意顺序 */
```

### 3、万能 float 闭合(非常重要) 可以用这个解决多个 div 对齐时的间距不对

将以下代码加入 Global CSS  中，给需要闭合的 div 加上 class="clearfix"  即可，屡试不爽。代码：

```css
/* Clear Fix */
.clearfix:after {
    content: ".";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
}
.clearfix { display: inline-block; }
/* Hide from IE Mac \*/
.clearfix { display: block; }
/* End hide from IE Mac */
/* end of clearfix */
```

### 4、其他兼容技巧

1. Firefox下给 div 设置 padding 后会导致 width 和 height 增加, 但IE不会(可用 !important 解决)

2. 居中问题：

    1. 垂直居中将 line-height  设置为当前 div  相同的高度, 再通过 vetical-align: middle ( 注意内容不要换行）

    2. 水平居中： margin: 0 auto; (当然不是万能)

3. 若需给 a  标签内内容加上 样式，需要设置 display: block; (常见于导航标签)

4. Firefox 和 IE 对 BOX 理解的差异导致相差 2px  的还有设为 float 的 div 在 ie下 margin 加倍等问题

5. ul 标签在 FF 下面默认有 list-style  和 padding 最好事先声明，以避免不必要的麻烦(常见于导航标签和内容列表)

6. 作为外部 wrapper  的 div  不要定死高度，最好还加上 overflow: hidden 以达到高度自适应

### 5、兼容代码：兼容最推荐的模式。

```css
/* FF */
.submitbutton {
    float: left;
    width: 40px;
    height: 57px;
    margin-top: 24px;
    margin-right: 12px;
}
/* IE6 */
*html .submitbutton {
    margin-top: 21px;
}
/* IE7 */
*+html .submitbutton {
    margin-top: 21px;
}
```

### 6、兼容 CSS 方法
做兼容页面的方法是：每写一小段代码（布局中的一行或者一块）我们都要在不同的浏览器中看是否兼容，当然熟练到一定的程度就没这么麻烦了。建议经常会碰到兼容性问题的新手使用。很多兼容性问题都是因为浏览器对标签的默认属性解析不同造成的，只要我们稍加设置都能轻松地解决这些兼容问题。如果我们熟悉标签的默认属性的话，就能很好的理解为什么会出现兼容问题以及怎么去解决这些兼容问题。

## 六、其它CSS兼容

### 1、不同浏览器的标签默认margin 和 padding 不同

问题说明：随便写几个标签，不加样式控制的情况下，各自的 margin 和 padding 差异较大。解决方案： CSS: *{margin: 0;padding:0;} 

### 2、块属性标签 float 后，又有横行的 margin 情况下，在IE6显示 margin 比设置的大

问题说明：常见症状是IE6中后面的一块被顶到下一行（ float 布局最常见的浏览器兼容问题）解决方案：在 float 的标签样式控制中加入 display:inline;  将其转化为行内属性备注：我们最常用的就是 div+CSS 布局了，而 div就是一个典型的块属性标签，横向布局的时候我们通常都是用 div float 实现的，横向的间距设置如果用 margin 实现，这就是一个必然会碰到的兼容性问题。

### 3、较小高度标签（一般小于 10px ）问题

问题说明：IE6、7和遨游里这个标签的高度不受控制，超出自己设置的高度解决方案：给超出高度的标签设置 overflow:hidden; 或者设置行高 line-height  小于你设置的高度。备注：这种情况一般出现在我们设置小圆角背景的标签里。出现这个问题的原因是IE8之前的浏览器都会给标签一个最小默认的行高的高度。即使你的标签是空的，这个标签的高度还是会达到默认的行高。

### 4、行内属性标签，设置 display:block 后采用 float 布局，又有横行的 margin 的情况，IE6间距 bug

问题说明：IE6里的间距比超过设置的间距解决方案：在 display:block; 后面加入 display:inline;display:table; 备注：行内属性标签，为了设置宽高，我们需要设置 display:block; (除了 input 标签比较特殊)。在用 float 布局并有横向的 margin 后，在IE6下，他就具有了块属性 float 后的横向 margin 的 bug 。不过因为它本身就是行内属性标签，所以我们再加上 display:inline 的话，它的高宽就不可设了。这时候我们还需要在 display:inline 后面加入 display:talbe 。

### 5、图片默认有间距

问题说明：几个 img 标签放在一起的时候，有些浏览器会有默认的间距，加了问题一中提到的通配符也不起作用。解决方案：使用 float 属性为 img 布局备注：因为 img 标签是行内属性标签，所以只要不超出容器宽度， img 标签都会排在一行里，但是部分浏览器的 img 标签之间会有个间距。去掉这个间距使用 float 是正道。

### 6、标签最低高度设置 min-height 不兼容

问题说明：因为 min-height 本身就是一个不兼容的 CSS 属性，所以设置 min-height 时不能很好的被各个浏览器兼容解决方案：如果我们要设置一个标签的最小高度 200px ，需要进行的设置为：

```css
{
    min-height: 200px;
    height: auto !important;
    height: 200px;
    overflow:visible;
}
```

备注：在B/S系统前端开时，有很多情况下我们又这种需求。当内容小于一个值（如 300px ）时。容器的高度为 300px ；当内容高度大于这个值时，容器高度被撑高，而不是出现滚动条。这时候我们就会面临这个兼容性问题。

### 7、CSS 布局中的居中问题

问题说明： 首先在父级元素定义 text-align: center; 这个的意思就是在父级元素内的内容居中；对于IE这样设定就已经可以了。但在mozilla中不能居中。解决办法：在子元素定义时候设定时再加上 margin-right: auto; margin-left: auto;  

### 8、IE浮动产生的双倍距离

```css
#box {
    float: left;
    width: 100px;
    margin: 0 0 0 100px; /* 这种情况之下IE会产生200px的距离 */
    display: inline; /* 使浮动忽略 */
}
```

这里细说一下 block，inline 两个元素Block 元素的特点是：总是在新行上开始，高度、宽度、行高、边距都可以控制(块元素)Inline 元素的特点是：和其他元素在同一行上，不可控制(内嵌元素)

```css
#box {
  display: block; /* 可以为内嵌元素模拟为块元素 */
  display: inline; /* 实现同一行排列的的效果 */
  diplay: table;
}
```

### 9、IE与宽度和高度的问题

IE不认得 min- 这个定义，但实际上它把正常的 width 和 height 当作有 min 的情况来使。这样问题就大了，如果只用宽度和高度，正常的浏览器里这两个值就不会变，如果只用 min-width 和 min-height 的话，IE下面根本等于没有设置宽度和高度。比如要设置背景图片，这个宽度是比较重 要的。解决方案：要解决这个问题，可以这样：

```css
#box {
  width: 80px;
  height: 35px;
}
html>body #box {
  width: auto;
  height: auto;
  min-width: 80px;
  min-height: 35px;
}
```

### 10、页面的最小宽度

min-width 是个非常方便的 CSS 命令，它可以指定元素最小也不能小于某个宽度，这样就能保证排版一直正确。但IE不认得这个，而它实际上把 width 当做最小宽度来使。为了让这一命令在IE上也能用，可以把一个 div 放到 body 标签下，然后为 div 指定一个类，然后 CSS 这样设计：

```css
#containe r{
    min-width: 600px;
    width: e-xpression(document.body.clientWidth < 600? "600px": "auto");
}
```

第一个 min-width 是正常的；但第2行的 width 使用了 JavaScript ，这只有IE才认得，这也会让你的 HTML 文档不太正规。它实际上通过 JavaScript 的判断来实现最小宽度。

### 11、清除浮动

```css
.box {
  display: table; /* 将对象作为块元素级的表格显示 */
}
/* 或者 */
.box {
  clear: both;
}
```

或者加入  :after （伪对象）,设置在对象后发生的内容，通常和 content 配合使用，IE不支持此伪对象，非IE 浏览器支持，所以并不影响到IE/WIN浏览器。

```css
#box:after {
  content: “.”;
  display: block;
  height: 0;
  clear: both;
  visibility: hidden;
}
```

### 12、 DIV 浮动IE文本产生 3px 的 bug 

左边对象浮动，右边采用外补丁的左边距来定位，右边对象内的文本会离左边有 3px 的间距。

```css
#box {
  float: left;
  width: 800px;
}
#left {
  float: left;
  width: 50%;
}
#right {
  width: 50%;
}
*html #left {
  margin-right: -3px; // 这句是关键
}
```

### 13、IE捉迷藏的问题

当 div 应用复杂的时候每个栏中又有一些链接， div 等这个时候容易发生捉迷藏的问题。有些内容显示不出来，当鼠标选择这个区域是发现内容确实在页面。解决方案：对 #layout 使用 line-height 属性或者给 #layout 使用固定高和宽。页面结构尽量简单。

### 14、高度不适应

高度不适应是当内层对象的高度发生变化时外层高度不能自动进行调节，特别是当内层对象使用 margin  或 padding  时。例：

```html
<div id=”box”>
	<p>p对象中的内容</p>
</div>
```

```css
#box {
  background-color: #eee;
}
#box p {
  margin-top: 20px;
  margin-bottom: 20px;
  text-align: center;
}
```

解决方案：在 p 对象上下各加 2 个空的 div 对象 CSS 代码： .1{ height: 0px; overflow: hidden; } 或者为 DIV 加上 border 属性。

### 15、cursor 属性问题

cursor: hand; VS cursor: pointer; 问题说明：FireFox不支持 hand，但IE支持 pointer 。解决方法：统一使用 pointer。

### 16、对盒模型的解析不一致

对 border 的解析不一致，如：box.style { width: 100px; border: 1px; }；IE理解为 box.width = 100px;；Firefox 理解为 box.width = 100 + 1*2 = 102px;  // 加上边框2px

解决方案：div { margin: 30px !important; margin: 28px; } 注意这两个 margin 的顺序一定不能写反， IE不能识别 !important 这个属性，但别的浏览器可以识别。所以在IE下其实解释成这样：div{ maring: 30px; margin: 28px; } 重复定义的话按照最后一个来执行，所以不可以只写 margin: XXpx !important;

IE5 和 IE6 的盒模型解释不一致。IE5下 div{ width: 300px; margin: 0 10px 0 10px; } 其中 div 的宽度会被解释为 300px-10px(右填充)-10px(左填充)，最终 div 的宽度为 280px ，而在IE6和其他浏览器上宽度则是以 300px+10px(右填充)+10px(左填充)=320px 来计算的。

解决方案：做如下修改 div { width: 300px !important; width /**/: 340px; margin: 0 10px 0 10px; }17、ul 和 ol 列表缩进问题消除 ul、ol 等列表的缩进时，样式应写成：list-style: none; margin: 0px; padding: 0px; 经验证，在IE中，设置 margin: 0px; 可以去除列表的上下左右缩进、空白以及列表编号或圆点，设置 padding 对样式没有影响；在 Firefox 中，设置 margin: 0px; 仅仅可以去除上下的空白，设置 padding: 0px; 后仅仅可以去掉左右缩进，还必须设置 list- style: none; 才能去除列表编号或圆点。也就是说，在IE中仅仅设置 margin: 0px; 即可达到最终效果，而在Firefox中必须同时设置 margin: 0px;、 padding: 0px; 以及 list-style: none; 三项才能达到最终效果。

### 17、为什么无法定义 1px 左右高度的容器

IE6下这个问题是因为默认的行高造成的；解决的技巧也有很多：例如：overflow: hidden;  zoom: 0.08  line-height: 1px;

### 18、链接(a标签)的边框与背景

a 链接加边框和背景色，需设置 display: block; 同时设置 float: left;  保证不换行。参照 menubar, 给 a 和 menubar 设置高度是为了避免底边显示错位，若不设 height, 可以在 menubar 中插入一个空格。19、超链接访问过后 hover 样式就不出现的问题问题说明：被点击访问过的超链接样式不在具有 hover 和 active了解决技巧是改变CSS属性的排列顺序:  L-V-H-A；

```html
<style type="text/css">
    <!--
    a:link {}
    a:visited {}
    a:hover {}
    a:active {}
    -->
</style>
```
