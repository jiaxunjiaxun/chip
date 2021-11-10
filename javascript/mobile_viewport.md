# 注意meta里的viewport

注意理解 width=device-width，这实际上就是把那个dpi的不同忽略了

- width 很常用，device-width 是设备宽，也可以是个数字（单位是像素 px）
- height 与 width 类似 device-height 是设备高
- initial-scale | minimum-scale | maximum-scale 缩放比例，默认 1.0
- user-scalable 是否可缩放 yes|no

```html
<meta
    name="viewport"
    content="
        width=[device-width | number(px)]
        height=[device-height | number(px)]
        initial-scale=1.0
        minimum-scale=1.0
        maximum-scale=1.0
        user-scalable=yes|no
    "
/>
```

```css
/* 按照设备宽度过滤 */
@media only screen and (max-device-width: 320px) {}
@media only screen and (max-device-width: 375px) {}
@media only screen and (max-device-width: 414px) {}

/* 按照最大（小）宽度 */
/* Tailwinds */
@media only screen and (min-width: 640px) {}
@media only screen and (min-width: 768px) {}
@media only screen and (min-width: 1024px) {}
@media only screen and (min-width: 1280px) {}
@media only screen and (min-width: 1536px) {}
/* Bootstrap */
@media only screen and (min-width: 576px) {}
@media only screen and (min-width: 768px) {}
@media only screen and (min-width: 992px) {}
@media only screen and (min-width: 1200px) {}
@media only screen and (min-width: 1400px) {}
@media only screen and (max-width: 575.98px) {}
@media only screen and (max-width: 767.98px) {}
@media only screen and (max-width: 991.98px) {}
@media only screen and (max-width: 1199.98px) {}
@media only screen and (max-width: 1399.98px) {}
```