# For different device pixel ratio

    (function (viewport) {
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
    }));

## Meta viewport

### For percentage and 'rem'
&lt;meta name="viewport" content="width=device-width,initial-scale=1.0,maximum-scale=1.0,minimum-scale=1.0,user-scalable=no" /&gt;

### For viewport, pixel ratio and 'rem'
&lt;meta name="viewport" content="initial-scale=1.0,maximum-scale=1.0,minimum-scale=1.0,user-scalable=no" /&gt;