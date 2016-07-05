# Mobile CSS

## import

&lt;link href="css/reset.css" rel="stylesheet" type="text/css" media="screen" /&gt;

&lt;link href="css/style.css" rel="stylesheet" type="text/css" media="all" /&gt;

&lt;link href="css/print.css" rel="stylesheet" type="text/css" media="print" /&gt;

### iPhone
&lt;link rel="stylesheet" media="only screen and (-webkit-min-device-pixel-ratio: 2)" type="text/css" href="iphone4.css" /&gt;

### iPad

&lt;link rel="stylesheet" media="all and (orientation:portrait)" href="portrait.css" /&gt;
&lt;link rel="stylesheet" media="all and (orientation:landscape)" href="landscape.css" /&gt;

## Style

    @media screen {
        selector {
            style-attribute: style-attribute-value;
        }
    }
    @media screen and (max-width: 600px) { ... }
    @media screen and (max-device-width: 480px) { ... }
    
    html {
        /* viewport device-ratio */
        font-size: [12 * device-ratio]px;
        
        /* viewport device-width */
        font-size: 625%
    }
    
    @media screen and (min-width: 350px) {
        html { font-size: 291.667%; }
    }
    @media screen and (min-width: 410px) {
        html { font-size: 341.667%; }
    }
    @media screen and (min-width: 480px) {
        html { font-size: 400%; }
    }
    @media screen and (min-width: 640px) {
        html { font-size: 533.333%; }
    }
    @media screen and (min-width: 750px) {
        html { font-size: 625%; }
    }
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

### iPhone Font Size

    /* iPhone, portrait & landscape. */
    @media all and (max-device-width: 480px) {
        html,
        body {
            -webkit-text-size-adjust: none;
        }
    }
    /* iPad, portrait & landscape. */
    @media all and (min-device-width: 768px) and (max-device-width: 1024px) {
        html,
        body {
            -webkit-text-size-adjust: none;
        }
    }

### Mobile Font

<table>
    <caption>Windows</caption>
    <tr><td>[宋体]</td><td>SimSun</td><td>\5b8b\4f53</td></tr>
    <tr><td>[黑体]</td><td>SimHei</td><td>\9ed1\4f53</td></tr>
    <tr><td>[微软雅黑]</td><td>Microsoft YaHei</td><td>\5fae\8f6f\96c5\9ed1</td></tr>
    <tr><td>微软正黑体</td><td>Microsoft JhengHei</td><td>\5fae\x8f6f\6b63\9ed1\4f53</td></tr>
    <tr><td>新宋体</td><td>NSimSun</td><td>\65b0\5b8b\4f53</td></tr>
    <tr><td>新细明体</td><td>PMingLiU</td><td>\65b0\7ec6\660e\4f53</td></tr>
    <tr><td>细明体</td><td>MingLiU</td><td>\7ec6\660e\4f53</td></tr>
    <tr><td>标楷体</td><td>DFKai-SB</td><td>\6807\6977\4f53</td></tr>
    <tr><td>仿宋</td><td>FangSong</td><td>\4eff\5b8b</td></tr>
    <tr><td>楷体</td><td>KaiTi</td><td>\6977\4f53</td></tr>
    <tr><td>仿宋_GB2312</td><td>FangSong_GB2312</td><td>\4eff\5b8b_GB2312</td></tr>
    <tr><td>楷体_GB2312</td><td>KaiTi_GB2312</td><td>\6977\4f53_GB2312</td></tr>
</table>

<table>
    <caption>Mac</caption>
    <tr><td>[华文细黑]</td><td>STHeiti Light [STXihei]</td><td>\534e\6587\7ec6\9ed1</td></tr>
    <tr><td>[华文黑体]</td><td>STHeiti</td><td>\534e\6587\9ed1\4f53</td></tr>
    <tr><td>华文楷体</td><td>STKaiti</td><td>\534e\6587\6977\4f53</td></tr>
    <tr><td>华文宋体</td><td>STSong</td><td>\534e\6587\5b8b\4f53</td></tr>
    <tr><td>华文仿宋</td><td>STFangsong</td><td>\534e\6587\4eff\5b8b</td></tr>
    <tr><td>丽黑 Pro</td><td>LiHei Pro Medium</td><td>\4e3d\9ed1 Pro</td></tr>
    <tr><td>丽宋 Pro</td><td>LiSong Pro Light</td><td>\4e3d\5b8b Pro</td></tr>
    <tr><td>标楷体</td><td>BiauKai</td><td>\6807\6977\4f53</td></tr>
    <tr><td>苹果丽中黑</td><td>Apple LiGothic Medium</td><td>\82f9\679c\4e3d\4e2d\9ed1</td></tr>
    <tr><td>苹果丽细宋</td><td>Apple LiSung Light</td><td>\82f9\679c\4e3d\7ec6\5b8b</td></tr>
</table>

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
