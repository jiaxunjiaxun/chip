# jQuery Plugin

~~~ javascript
(function ($) {

    // plugin function
    $.fn.__plugin_name__ = function (options) {
        // merge settings
        options = $.extend({}, $.fn.__plugin__.defaults, options);
        // initiate
        var _param = options['param'];
        // do what you want
        return this.each(function (index, el) {
            // for each element
        });
    };

    // if it is just a function
    $.fn.__plugin_name__ = function (args) {
        // do what you want
    };

    // default settings
    $.fn.__plugin_name__.defaults = {
        param: ''
        //,param: ''...
    };
})(window.jQuery);
~~~