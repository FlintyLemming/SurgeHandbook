# URL 重写 (URL Rewrite)

Surge 可以使用 2 种不同的方法重写请求的 URL，或者根据 URL 拒绝某些请求。

示例：

```ini
[URL Rewrite]
^http://www\.google\.cn http://www.google.com header
^http://yachen\.com https://yach.me 302
^http://ad\.com/ad\.png _ reject
```

重写规则由三个部分组成：正则表达式、替换内容和类型。

### Header 模式

Surge 会修改请求头，并在必要时将请求重定向到另一个主机。客户端不会感知到这个重写操作。

请求头中的 `Host` 字段将被修改以匹配新的 URL。

```ini
[URL Rewrite]
^http://www\.google\.cn http://www.google.com header
```

### 302 模式

Surge 将直接返回一个 302 重定向响应。如果为主机名启用了 MitM，HTTPS 请求也可以被重定向。

```ini
[URL Rewrite]
^http://yachen\.com https://yach.me 302
```

### Reject 模式

如果模式匹配，则拒绝该请求。替换参数将被忽略。如果为主机名启用了 MitM，HTTPS 请求将被拒绝。

```ini
[URL Rewrite]
^http://ad\.com/ad\.png _ reject
```
