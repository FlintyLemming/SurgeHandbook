# HTTP 规则

共有两种 HTTP 规则类型。HTTP 规则适用于 HTTP 请求或 HTTPS 请求。它不会影响 TCP 连接。

#### USER-AGENT

```ini
USER-AGENT,Instagram*,DIRECT
```

如果请求的 User-Agent 匹配，则触发该规则。支持通配符 `*` 和 `?`。

#### URL-REGEX

`URL-REGEX,^http://google\.com,DIRECT`

如果 URL 匹配该正则表达式，则触发该规则。

你可以追加 `extended-matching` 参数以同时测试 HTTP Host 请求头（或 `:authority`）和 SNI，这在需要 TLS 握手信息时非常有用：

```ini
URL-REGEX,^https://example\.com,Proxy,extended-matching
```
