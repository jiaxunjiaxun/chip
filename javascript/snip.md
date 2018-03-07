# Snip

## A good template for javascript

~~~ javascript
(function (bootstrap) {
    'use strict';
    bootstrap(window);
}(function (window) {
    'use strict';

    var CIIC = {};

    CIIC.template = function (tpl, data) {
        var attr, result = tpl;
        for (attr in data) {
            if (data.hasOwnProperty(attr)) {
                result = result.replace(new RegExp('{' + attr + '}', 'img'), data[attr]);
            }
        }
        return result;
    };

    // CIIC.embed_script=function(url){var h=document.head||document.getElementsByTagName("head")[0]||document.documentElement,s=document.createElement("script");s.src=url;s.charset="UTF-8";s.async=true;s.onload=s.onreadystatechange=function(){if(!s.readyState||/loaded|complete/.test(s.readyState)){s.onload=s.onreadystatechange=null;if(s.parentNode){s.parentNode.removeChild(s)}s=null}};h.insertBefore(s,h.firstChild)};
    CIIC.embed_script = function (url) {
        var e_head = document.head || document.getElementsByTagName("head")[0] || document.documentElement,
            e_script = document.createElement("script");
        e_script.src = url;
        e_script.charset = "UTF-8";
        e_script.async = true;
        e_script.onload = e_script.onreadystatechange = function (e) {
            if (!e_script.readyState || /loaded|complete/.test(e_script.readyState)) {
                e_script.onload = e_script.onreadystatechange = null;
                if (e_script.parentNode) {
                    e_script.parentNode.removeChild(e_script);
                }
                e_script = null;
            }
        };
        e_head.insertBefore(e_script, e_head.firstChild);
    };

    CIIC.getScript = function (url) {
        var head = document.head || document.getElementsByTagName('head')[0] || document.documentElement,
            script = document.createElement("script");
        script.async = true;
        script.src = url;
        script.onload = script.onreadystatechange = function (e) {
            if (!script.readyState || /loaded|complete/.test(script.readyState)) {
                script.onload = script.onreadystatechange = null;
                if (script.parentNode) {
                    script.parentNode.removeChild(script);
                }
                script = null;
            }
        };
        head.insertBefore(script, head.firstChild);
    };

    CIIC.trim = function (text) {
        return text === null ? '' : String(text).replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, '');
    };

    CIIC.is_mobile = {
        Android: function () {
            return navigator.userAgent.match(/Android/i);
        },
        BlackBerry: function () {
            return navigator.userAgent.match(/BlackBerry/i);
        },
        iOS: function () {
            return navigator.userAgent.match(/iPhone|iPad|iPod/i);
        },
        Opera: function () {
            return navigator.userAgent.match(/Opera Mini/i);
        },
        Windows: function () {
            return navigator.userAgent.match(/IEMobile/i);
        },
        any: function () {
            return (isMobile.Android() || isMobile.BlackBerry() || isMobile.iOS() || isMobile.Opera() || isMobile.Windows());
        }
    };

    CIIC.is_mobile_simple = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);

    /*
    CIIC.globalEval = function (data) {
        var executor = window.execScript || function (data) {
            window["eval"].call(window, data);
        };
        if (data && CIIC.trim(data)) {
            executor(data);
        }
    };
    */

    // AMD define
    var obj = {};
    if ('function' === typeof define && define.amd) {
        window.define('module-name', [/*require-list*/], function() {
            return obj;
        });
    }
    return obj;

    window.CIIC = CIIC;
}));
~~~

## About class

~~~ javascript
// define
(function (window) {
    // private
    var _attr = { /* ... */ };

    // constructor
    var __ClassName__ = function( args ) { /* ... */ };

    // prototype initiate
    // warning: pay attention to an existed class
    __ClassName__.prototype = {};

    // prototype method
    __ClassName__.prototype.__Method__ = function( args ) { /* ... */ };

    // static variable or method
    __ClassName__.__StaticAttribute__ = {}; /* ... */
    __ClassName__.__StaticMethod__ = function( args ) { /* ... */ };

    // redefine constructor
    __ClassName__.prototype.constructor = __ClassName__;

    // export
    window.__ClassName__ = __ClassName__;
}(window));

// extends
(function (window) {

    // private
    var _attr = { /* ... */ };

    // constructor
    var __ParentClassName__ = window.__ParentClassName__ || {};
    var __ClassName__ = function( args ) {
        __ParentClassName__.apply(this, args);
        /* ... */
    };

    // prototype initiate
    // warning: pay attention to an existed class
    __ClassName__.prototype = new __ParentClassName__;

    /* same as class define */
}(window));
~~~

## CMD Principle

### define(factory)
~~~ javascript
define(function(require, exports, module) { ... });
~~~

### define(id?, deps?, factory)
~~~ javascript
define(id?, deps?, function(require, exports, module) { ... });
~~~

### define.cmd
~~~ javascript
if (typeof define === "function" && define.cmd) { ... }
~~~

### require(id)
~~~ javascript
define(function(require, exports) {
    var module = require('./module');
    module.action();
});
~~~

### require.async(id, callback?)
~~~ javascript
define(function(require, exports, module) {

    // Single module
    require.async('./async_module', function(async_module) {
        async_module.doSomething();
    });

    // Multi module
    require.async(['./async_1', './async_2'], function(async_1, async_2) {
        async_1.action();
        async_2.action();
    });
});
~~~

### require.resolve(id)
~~~ javascript
define(function(require, exports) {
    // --> http://example.com/path/to/module.js
    console.log(require.resolve('./module'));
});
~~~

### exports

Use 'return' or define module.exports.

**warning**: exports is just a reference of module.exports.

~~~ javascript
exports = { /* ... */ } // wrong
module.exports = { /* ... */ } // correct.
~~~

### module

**module.id** (string): Module identity.

**module.uri** (string): Module URI.

**module.dependencies** (array): Module dependencies.

**module.exports** (object): Module exports

## Some function

### Get URL query parameters

~~~javascript
function getQueryParameters(str) {
    var i, len, pair, params = {},
        search_string = (str || document.location.search).replace(/(^\?)/, ''),
        search_split = search_string.split('&');
    for (i = 0, len = search_split.length; i < len; i++) {
        pair = search_split[i].split('=');
        params[pair[0]] = window.decodeURIComponent(pair[1]);
    }
    return params;
}

jQuery.extend({
    getQueryParameters: function(str) {
        return (str || document.location.search).replace(/(^\?)/, '').split('&').map(function(n) {
            return n = n.split('='), this[n[0]] = n[1], this
        }.bind({}))[0];
    }
});
~~~