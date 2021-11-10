1. 在弹出层的touchstart事件中调用preventDefault

缺点

- 如果弹出层本身是有滚动（条）的话，将会导致弹出层无法滚动，此时用这种方法无于饮鸩止渴。
- 一个很常见的场景，点击弹出层，弹出层消失掉，此时也无法触发弹出层的点击回调事。
- 弹出层内的任何事件都执行不了了。

2. 弹出层touchmove + preventDefault
```javascript
// 1. 弹出层里不能有需要滚动的内容
// 2. 如大段文字需要固定高度，显示滚动条也会被阻止
modal.addEventListener('touchmove', function(e) {
    e.preventDefault();
}, false);
```

3. 设置CSS

```css
html,body { overflow: hidden; }
```

在PC和移动端都能禁止掉下层的滚动。

4. 最好的解决方案

在body元素上设置position: fixed

```css
body.modal-open {
    position: fixed;
    width: 100%;
}
```

如果只是上面的 css，滚动条的位置同样会丢失,所以如果需要保持滚动条的位置需要用 js 保存滚动条位置关闭的时候还原滚动位置

```javascript
var ModalHelper = (function (bodyCls) {
    var scrollTop, doc, reg,
        bodyClassName = '',
        bodyEle = document.body;

    return {
        afterOpen: function () {
            doc = document.documentElement.scrollTop ? document.documentElement : bodyEle;
            scrollTop = doc.scrollTop;

            if (bodyEle.classList) {
                bodyEle.classList.add(bodyCls);
            } else {
                bodyEle.className += ' ' + bodyCls;
            }

            bodyEle.style.top = -scrollTop + 'px';
            bodyClassName = bodyEle.className;
        },
        beforeClose: function () {
            if (bodyEle.classList) {
                bodyEle.classList.remove(bodyCls);
            } else {
                reg = new RegExp('\\b' + bodyCls + '\\b', 'g', 'gi');
                if (reg.test(bodyClassName)) {
                    bodyClassName = bodyClassName.replace(reg, '');
                    bodyEle.className = bodyClassName;
                }
            }

            bodyEle.style.top = '';
            doc.scrollTop = scrollTop;
        }
    };
})('modal-open');

// -- or

/**
  * ModalHelper helpers resolve the modal scrolling issue on mobile devices
  * https://github.com/twbs/bootstrap/issues/15852
  * requires document.scrollingElement polyfill https://uedsky.com/demo/src/polyfills/document.scrollingElement.js
  */
var ModalHelper = (function (bodyCls) {
    var scrollTop;
    return {
        afterOpen: function () {
            scrollTop = document.scrollingElement.scrollTop;
            document.body.classList.add(bodyCls);
            document.body.style.top = -scrollTop + 'px';
        },
        beforeClose: function () {
            document.body.classList.remove(bodyCls);
            // scrollTop lost after set position:fixed, restore it back.
            document.scrollingElement.scrollTop = scrollTop;
        }
    };
})('modal-open');
```