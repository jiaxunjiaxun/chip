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
