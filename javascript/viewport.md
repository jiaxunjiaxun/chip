# For different device pixel ratio

```javascript
!function (viewport) {
    'use strict';
    viewport(window);
}(function (window) {
    'use strict';
    var BASE_FONT_SIZE = 12,
        el_html = document.getElementsByTagName('html')[0],
        viewport = document.getElementById('viewport'),
        pixel_ratio = !!window.devicePixelRatio ? window.devicePixelRatio : 1,
        scale = 1 / pixel_ratio,
        viewport_content = 'initial-scale=' + scale + ',maximum-scale=' + scale + ',minimum-scale=' + scale + ',user-scalable=no';
    viewport.setAttribute('content', viewport_content);
    el_html.style.fontSize = BASE_FONT_SIZE * pixel_ratio + 'px';
});
```

```javascript
!function (doc, win) {
    var edge = 750, rate = 7.5, docEl = doc.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        recalc = function () {
            var is_responsive = docEl.clientWidth < edge,
                base = is_responsive ? docEl.clientWidth / rate : 100,
                width = is_responsive ? 'auto' : '750px';

            docEl.style.fontSize = base + 'px';
            doc.getElementById('app').style.width = 'auto';
        };
    if (!doc.addEventListener) return;
    win.addEventListener(resizeEvt, recalc, false);
    doc.addEventListener('DOMContentLoaded', recalc, false);
}(document, window);
```

```javascript
!(function (win, doc) {
    'use strict';

    var font_size_baseline = 750,
        options = { width: font_size_baseline, dpr: win.devicePixelRatio },
        html = doc.documentElement,
        width = html.getAttribute('data-width') || options.width,
        dpr = html.getAttribute('data-dpr') || options.dpr,
        viewPort = doc.querySelector('meta[name="viewport"]'),
        rotate = win.onorientationchange ? 'orientationchange' : 'resize';

    // 设置html fontSize
    function setSize() {
        var winWidth = win.innerWidth || html.clientWidth,
            font_size = 100 * winWidth / width;
        html.style.fontSize = font_size + 'px';

        console.log(winWidth, width, font_size);
    };

    // 设置 initial-scale
    function setScale() {
        setSize();
        var viewContent = viewPort.getAttribute('content');
        var reg = /initial-scale=(\d(.\d+)?)/i;
        var matchRes = viewContent.match(reg);
        var scale = 1 / parseInt(dpr);
        if (matchRes && matchRes[1] == scale) {
            return;
        }
        var newContent = viewContent.replace(reg, function (a, b) {
            return a.replace(/\d(.\d+)?/i, scale);
        });
        viewPort.setAttribute('content', newContent);
    };

    win.addEventListener(rotate, setSize);
    window.requestAnimationFrame = window.requestAnimationFrame || window.webkitRequestAnimationFrame;
    requestAnimationFrame(setScale);
}(window, document));
```

## Meta viewport

### For percentage and 'rem'
&lt;meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,minimum-scale=1.0,user-scalable=no" /&gt;

### For viewport, pixel ratio and 'rem'
&lt;meta name="viewport" content="initial-scale=1.0,maximum-scale=1.0,minimum-scale=1.0,user-scalable=no" /&gt;
