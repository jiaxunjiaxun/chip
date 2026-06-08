# Apache 安全头信息

``` apache

Header set Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"

Header always set Content-Security-Policy "default-src 'self'; font-src *;img-src * data:; script-src *; style-src *;"

Header set X-XSS-Protection "1; mode=block"

Header always set X-Frame-Options "SAMEORIGIN"

Header always set X-Content-Type-Options "nosniff"

Header always set Referrer-Policy "strict-origin"

Header always set Permissions-Policy "geolocation=(),midi=(),sync-xhr=(),microphone=(),camera=(),magnetometer=(),gyroscope=(),fullscreen=(self),payment=()"

```

## Host头攻击

中危

https://blog.csdn.net/doulicau/article/details/106685476

## Apache byterange filter 拒绝服务

https://www.cnblogs.com/princessd8251/articles/3853421.html


https://cloud.tencent.com/developer/article/2020474