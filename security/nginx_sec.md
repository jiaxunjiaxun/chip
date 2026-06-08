# Nginx 响应头安全配置

## 响应头文件安全策略

针对当前环境下，对网络安全的要求较高，平台的搭建从各个方面都在增强安全性。以下是从http头文件的方面，利用参数设置开启浏览器的安全策略，来实现相关的安全机制。由于目前的服务环境未nginx，所以配置都针对NGINX的设置，如果是tomcat，同理网上找对应的修改参数即可。

## 全部配置如下
``` nginx
add_header Content-Security-Policy "default-src 'self' xxx.xxx.com(允许的地址)
add_header X-Content-Type-Options "nosniff";
add_header X-XSS-Protection "1; mode=block";
add_header X-Frame-Options SAMEORIGIN;
add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
add_header 'Referrer-Policy' 'origin';
add_header X-Download-Options noopen;
add_header X-Permitted-Cross-Domain-Policies none;
```

## 说明如下

### Content-Security-Policy

内容网页安全策略，为了解决（缓解，实际上好像不能完全解决XSS攻击）制定的策略，要求请求头增加Content-Security-Policy配置，提供加载静态资源的白名单策略。

应对漏洞：XSS攻击

配置策略：

- script-src：外部脚本
- style-src：样式表
- img-src：图像
- media-src：媒体文件（音频和视频）
- font-src：字体文件
- object-src：插件（比如 Flash）
- child-src：框架
- frame-ancestors：嵌入的外部资源（比如：```<frame>```、```<iframe>```、```<embed>```和```<applet>```）
- connect-src：HTTP 连接（通过 XHR、WebSockets、EventSource 等）
- worker-src：worker 脚本
- manifest-src：manifest 文件

通过上诉配置，来控制平台只能执行同源的或者规定源（网址）的js、css等脚本。该配置由浏览器执行，启动后，不符合规则的外部资源就会被拒绝，从而一定程度屏蔽了XSS攻击。

示例：script-src 'self'; object-src 'none'; style-src cdn.example.org third-party.org; child-src https:

说明：script只接受同源的  ,object对象不接受，css只接受cdn.example.org third-party.org网址的 ，网站支持吃https的（这个好像不好使，后面有其他策略来实现这个）

nginx配置：add_header Content-Security-Policy "default-src 'self' xxx.xxx.com(允许的地址)  允许本域名和后边的域名脚本可以执行

此配置有很多细节的控制，详见http://www.ruanyifeng.com/blog/2016/09/csp.html

注意：此配置会屏蔽范围外的脚本，如果是已经上线的系统，需要考虑下是否有引入外部脚本的情况，避免盲目增加配置，导致生产事故。慎用。。

### X-Content-Type-Options

约定资源的响应头，屏蔽内容嗅探攻击。

应对漏洞：内容嗅探攻击

网络请求中，每个资源都有自己的类型，比如Content-Type:text/html 、image/png、 text/css。但是有一些资源的类型是未定义或者定义错了，导致浏览器会猜测资源类型，尝试解析内容，从而给了脚本攻击可乘之机。比如利用一个图片资源去执行一个恶意脚本。

通过设置 X-Content-Type-Options: nosniff ，浏览器严格匹配资源类型，会拒绝加载错误或者不匹配的资源类型。（PS：排查问题过程中，网上看到一个人用流的方式从后台给前台传图片，大概代码```<img src="xxxxx.xxxx.do">```由于后端未指定资源类型，导致增加该配置后无法显示图片）

nginx配置：add_header X-Content-Type-Options "nosniff";

### X-XSS-Protection

开启浏览器XSS防护(原理不明，待研究.好像是浏览器自己有个filter，能过滤xss攻击脚本)。开启后不会影响业务，无特殊情况，建议开启。

应对漏洞：XSS攻击

配置参数：

- X-XSS-Protection: 0 关闭防护
- X-XSS-Protection: 1 开启防护
- X-XSS-Protection: 1; mode=block 开启防护 如果被攻击，阻止脚本执行。

nginx配置：add_header X-XSS-Protection "1; mode=block";

### X-Frame-Options

开个此属性，控制页面是否可以用于ifream中，如果使用，是什么范围。也可以通过这个控制来避免自己的资源页面被其他页面引用。

应对漏洞：点击劫持

攻击者会用一个自己网站，用ifream或者fream嵌套的方式引入目标网站，诱使用户点击。从而劫持用户点击事件。

配置的三个参数：

- deny 标识该页面不允许在frame中展示，即便在相同域名的页面中嵌套也不行。

- sameorigin 可以在同域名的页面中frame中展示

- allow-form url 指定的frame中展示。

nginx配置:add_header X-Frame-Options SAMEORIGIN;

### Strict-Transport-Security

告诉浏览器只能通过https访问当前资源。

nginx配置：add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";

说明：在接下来的一年（即31536000秒）中，浏览器只要向xxx或其子域名发送HTTP请求时，必须采用HTTPS来发起连接。

### Referrer-Policy

用来规定什么情况下显示referer字段，以及referer字段显示的内容多少。

漏洞说明：

当用户在浏览器上点击一个链接时，会产生一个 HTTP 请求，用于获取新的页面内容，而在该请求的报头中，会包含一个 Referrer，用以指定该请求是从哪个页面跳转页来的，常被用于分析用户来源等信息。但是也成为了一个不安全的因素，所以就有了 Referrer-Policy，用于过滤 Referrer 报头内容，其可选的项有： no-referrer no-referrer-when-downgrade origin origin-when-cross-origin same-origin strict-origin strict-origin-when-cross-origin unsafe-url .

配置参数：

- no-referrer-when-downgrade：在同等安全等级下（例如https页面请求https地址），发送referer，但当请求方低于发送方（例如https页面请求http地址），不发送referer
- origin：仅仅发送origin，即protocal+host
- origin-when-cross-origin：跨域时发送origin
- same-origin：当双方origin相同时发送
- strict-origin：当双方origin相同且安全等级相同时发送
- unfafe-url：任何情况下都显示完整的referer

nginx配置：add_header 'Referrer-Policy' 'origin';

注意：此配置可能会导致某些系统访问率统计插件失效，如果有此类业务，一定要验证下。

### X-Permitted-Cross-Domain-Policies

一个针对flash的安全策略，用于指定当不能将"crossdomain.xml"文件（当需要从别的域名中的某个文件中读取 Flash 内容时用于进行必要设置的策略文件）放置在网站根目录等场合时采取的替代策略。

配置参数：

X-Permitted-Cross-Domain-Policies: master-only

- master-only 只允许使用主策略文件（/crossdomain.xml）

add_header X-Permitted-Cross-Domain-Policies none;

### X-Download-Options

用于控制浏览器下载文件是否支持直接打开，如果支持直接打开，可能会有安全隐患。

配置信息：X-Download-Options: noopen

- noopen 用于指定IE 8以上版本的用户不打开文件而直接保存文件。在下载对话框中不显示“打开”选项。

nginx配置：add_header X-Download-Options noopen;

cookie

在head中可以设置cookie的参数，来组合实现一些安全策略。

secure

Secure属性是说如果一个cookie被设置了Secure=true，那么这个cookie只能用https协议发送给服务器

HttpOnly

设置HttpOnly=true的cookie不能被js获取到，无法用document.cookie打出cookie的内容。

nginx:proxy_cookie_path / "/; Path=/; Secure; HttpOnly";