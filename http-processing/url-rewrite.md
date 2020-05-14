# 重写 URL

Surge 可以通过两种方式来重写请求的 URL，或者根据 URL 拒绝特定的请求。

例子：

```
[URL Rewrite]
^http://www\.google\.cn http://www.google.com header
^http://yachen\.com https://yach.me 302
^http://ad\.com/ad\.png _ reject
```

一条重写规则由 3 部分组成：正则表达式、替换的内容和类型。

### 重写请求头的域名

Surge 能重写请求头的域名，指向一个新的地址发送请求。发送请求的客户端不会发现这个操作。

例子：

```
[URL Rewrite]
^http://www\.google\.cn http://www.google.com header
```

> 不能使用 HTTPS Scheme 重定向。而且也不能重定向 HTTPS 请求。

### **302 重定向**

Surge 会直接返回一个 302 重定向的响应。如果启用了 MitM，并将域名添加到了解密列表，那么这个方法可以使用 HTTPS。

```
[URL Rewrite]
^http://yachen\.com https://yach.me 302
```

### 拒绝请求

拒绝符合正则表达式的请求。这种就不用写替换的内容了。如果启用了 MitM，并将域名添加到了解密列表，那么这个方法可以使用 HTTPS。

```
[URL Rewrite]
^http://ad\.com/ad\.png _ reject
```