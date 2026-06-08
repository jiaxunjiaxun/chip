# Mobile CSS

## import

```html
<link href="css/reset.css" rel="stylesheet" type="text/css" media="screen" />
<link href="css/style.css" rel="stylesheet" type="text/css" media="all" />
<link href="css/print.css" rel="stylesheet" type="text/css" media="print" />
```

### iPhone
```html
<link rel="stylesheet" media="only screen and (-webkit-min-device-pixel-ratio: 2)" type="text/css" href="iphone4.css" />
```

### iPad

```html
<link rel="stylesheet" media="all and (orientation:portrait)" href="portrait.css" />
<link rel="stylesheet" media="all and (orientation:landscape)" href="landscape.css" />
```


## Style

```css
@media screen {
    selector { style-attribute: style-attribute-value; }
}
@media screen and (max-width: 600px) { ... }
@media screen and (max-device-width: 480px) { ... }

html {
    /* viewport device-ratio */
    font-size: [12 * device-ratio]px;

    /* viewport device-width */
    font-size: 625%
}

@media screen and (min-width: 350px) { html { font-size: 291.667%; } }
@media screen and (min-width: 410px) { html { font-size: 341.667%; } }
@media screen and (min-width: 480px) { html { font-size: 400%; } }
@media screen and (min-width: 640px) { html { font-size: 533.333%; } }
@media screen and (min-width: 750px) { html { font-size: 625%; } }
html { font-size: 312.5%; }

body {
    -webkit-text-size-adjust: none;
    -webkit-user-select: none;
    -webkit-touch-callout: none;
    -webkit-user-drag: none;

    /* viewport device-ratio */
    font: 1rem/1.5rem 'Helvetica Neue', 'Helvetica', 'Tahoma', 'Arial', sans-serif;

    /* viewport device-width */
    font: 0.12rem/0.18rem 'Georgia', 'STHeiti', 'SimHei', sans-serif
}
```

#### Bootstrap
```css
html { font-size: 16px; }
@media (min-width: 576px) {}
@media (min-width: 768px) {}
@media (min-width: 992px) {}
@media (min-width: 1200px) {}
@media (min-width: 1400px) {}
```

### iPhone Font Size

```css
/* iPhone, portrait & landscape. */
@media all and (max-device-width: 480px) {
    html, body { -webkit-text-size-adjust: none; }
}
/* iPad, portrait & landscape. */
@media all and (min-device-width: 768px) and (max-device-width: 1024px) {
    html, body { -webkit-text-size-adjust: none; }
}
```

### Mobile Font

| Windows     |                    |                            |
| ----------- | ------------------ | -------------------------- |
| [宋体]      | SimSun             | \5b8b\4f53                 |
| [黑体]      | SimHei             | \9ed1\4f53                 |
| [微软雅黑]  | Microsoft YaHei    | \5fae\8f6f\96c5\9ed1       |
| 微软正黑体  | Microsoft JhengHei | \5fae\x8f6f\6b63\9ed1\4f53 |
| 新宋体      | NSimSun            | \65b0\5b8b\4f53            |
| 新细明体    | PMingLiU           | \65b0\7ec6\660e\4f53       |
| 细明体      | MingLiU            | \7ec6\660e\4f53            |
| 标楷体      | DFKai-SB           | \6807\6977\4f53            |
| 仿宋        | FangSong           | \4eff\5b8b                 |
| 楷体        | KaiTi              | \6977\4f53                 |
| 仿宋_GB2312 | FangSong_GB2312    | \4eff\5b8b_GB2312          |
| 楷体_GB2312 | KaiTi_GB2312       | \6977\4f53_GB2312          |



| Mac        |                         |                           |
| ---------- | ----------------------- | ------------------------- |
| [华文细黑] | STHeiti Light [STXihei] | \534e\6587\7ec6\9ed1      |
| [华文黑体] | STHeiti                 | \534e\6587\9ed1\4f53      |
| 华文楷体   | STKaiti                 | \534e\6587\6977\4f53      |
| 华文宋体   | STSong                  | \534e\6587\5b8b\4f53      |
| 华文仿宋   | STFangsong              | \534e\6587\4eff\5b8b      |
| 丽黑 Pro   | LiHei Pro Medium        | \4e3d\9ed1 Pro            |
| 丽宋 Pro   | LiSong Pro Light        | \4e3d\5b8b Pro            |
| 标楷体     | BiauKai                 | \6807\6977\4f53           |
| 苹果丽中黑 | Apple LiGothic Medium   | \82f9\679c\4e3d\4e2d\9ed1 |
| 苹果丽细宋 | Apple LiSung Light      | \82f9\679c\4e3d\7ec6\5b8b |

#### Web Safe
- Arial/Helvetica
- Arial Black
- Comic Sans MS
- Courier/Courier New
- Georgia
- Impact
- Times/Times New Roman
- Trebuchet MS
- Verdana

#### Mobile Safe
- Arial/Helvetica
- Courier/Courier New
- Georgia
- Times/Times New Roman
- Trebuchet MS
- Verdana
