# CSS Snip

## gray

~~~ css
* {
    filter: progid:DXImageTransform.Microsoft.BasicImage(grayscale=1);
    filter: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg'><filter id='grayscale'><feColorMatrix type='saturate' values='0'/></filter></svg>#grayscale");
    -webkit-filter: grayscale(1);
    filter: grayscale(1);
    filter: gray;
}
~~~

> Note: "*" is not a good idea.\
> Using tag name or adding class name for DOM elements are better.
