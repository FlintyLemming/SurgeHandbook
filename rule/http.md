# HTTP 规则

有两种 HTTP 规则类型。HTTP 规则对 HTTP 以及 HTTPS 请求有效，但对 TCP 连接无效。

## **USER-AGENT**

`USER-AGENT,Instagram*,DIRECT`

这个规则匹配相应的 USER-AGENT，可以使用像 \* 或者 ? 这样的通配符。

## **URL-REGEX**

`URL-REGEX,^http://google\.com,DIRECT`

这个规则匹配符合正则表达式的 URL。

