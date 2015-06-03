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
