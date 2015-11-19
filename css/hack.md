# IE CSS

## expression

    top: expression(eval(document.documentElement.scrollTop + document.documentElement.clientHeight - [height])));

    _top: expression(eval(document.documentElement.scrollTop + document.documentElement.clientHeight - [height]));

    html {
        _background-image: url(about/blank);
        _background-attachment: fixed;
    }

## IE

### comment

    <!--[if IE]>...<![endif]-->
    <!--[if !IE]>...<![endif]-->
    <!--[if IE 6]>...<![endif]-->
    <!--[if !IE 6]>...<![endif]-->
    <!--[if lt IE 6]>...<![endif]-->
    <!--[if gt IE 8]>...<![endif]-->
    <!--[if lte IE 8]>...<![endif]-->
    <!--[if gte IE 9]>...<![endif]-->
    

### IE 6

    selector { -property: value; }
    selector { _property: value; }
    _selector { property: value; }
    selector { property: value; property: value!important; } // IE6 不支持同一选择符中的!important

### IE 6,7

    selector { +property: value; }
    selector { #property: value; }
    selector { *property: value; }
    +selector { property: value; }
    #selector { property: value; }
    *selector { property: value; }

### IE 8,9,10

    selector { property: value\0; }

### IE 6,7,8,9,10

    selector { property: value\9; }