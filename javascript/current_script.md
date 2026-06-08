# How to get the current script element:

## 1. Use document.currentScript

document.currentScript will return the `<script>` element whose script is currently being processed.

```html
<script>
    var me = document.currentScript;
</script>
```

### Benefits

- Simple and explicit. Reliable.
- Don't need to modify the script tag
- Works with asynchronous scripts (defer & async)
- Works with scripts inserted dynamically

### Problems

- Will not work in older browsers and IE.
- Does not work with modules `<script type="module">`

## 2. Select script by id

Giving the script an id attribute will let you easily select it by id from within using document.getElementById().

```html
<script id="myscript">
    var me = document.getElementById('myscript');
</script>
```

### Benefits

- Simple and explicit. Reliable.
- Almost universally supported
- Works with asynchronous scripts (defer & async)
- Works with scripts inserted dynamically

### Problems

- Requires adding a custom attribute to the script tag
- id attribute may cause weird behaviour for scripts in some browsers for some edge cases

## 3. Select the script using a data-* attribute

Giving the script a data-* attribute will let you easily select it from within.

```html
<script data-name="myscript">
    var me = document.querySelector('script[data-name="myscript"]');
</script>
```

This has few benefits over the previous option.

### Benefits

- Simple and explicit.
- Works with asynchronous scripts (defer & async)
- Works with scripts inserted dynamically

### Problems

- Requires adding a custom attribute to the script tag
- HTML5, and querySelector() not compliant in all browsers
- Less widely supported than using the id attribute
- Will get around `<script>` with id edge cases.
- May get confused if another element has the same data attribute and value on the page.

## 4. Select the script by src

Instead of using the data attributes, you can use the selector to choose the script by source:

```html
<script src="//example.com/embed.js"></script>
```

In embed.js:

```javascript
var me = document.querySelector('script[src="//example.com/embed.js"]');
```

### Benefits

- Reliable
- Works with asynchronous scripts (defer & async)
- Works with scripts inserted dynamically
- No custom attributes or id needed

### Problems

- Does not work for local scripts
- Will cause problems in different environments, like Development and Production
- Static and fragile. Changing the location of the script file will require modifying the script
- Less widely supported than using the id attribute
- Will cause problems if you load the same script twice

## 5. Loop over all scripts to find the one you want

We can also loop over every script element and check each individually to select the one we want:

```html
<script>
    var i, me = null,
        scripts = document.getElementsByTagName('script');
    for (i = 0; i < scripts.length; ++i) {
        if (isMe(scripts[i])) {
            me = scripts[i];
        }
    }
</script>
```

This lets us use both previous techniques in older browsers that don't support querySelector() well with attributes. For example:

```javascript
function isMe(scriptElem) {
    return scriptElem.getAttribute('src') === "//example.com/embed.js";
}
```

This inherits the benefits and problems of whatever approach is taken, but does not rely on querySelector() so will work in older browsers.

## 6. Get the last executed script

Since the scripts are executed sequentially, the last script element will very often be the currently running script:

```html
<script>
    var scripts = document.getElementsByTagName('script'),
        me = scripts[scripts.length - 1];
</script>
```

### Benefits

- Simple.
- Almost universally supported
- No custom attributes or id needed

### Problems

- Does not work with asynchronous scripts (defer & async)
- Does not work with scripts inserted dynamically