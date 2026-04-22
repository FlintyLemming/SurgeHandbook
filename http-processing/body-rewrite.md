# 请求体重写 (Body Rewrite) iOS 5.10.0+ Mac 5.6.0+

Surge 可以重写 HTTP 请求或响应的请求体，使用正则表达式替换原始内容。示例：

```ini
[Body Rewrite]
http-request ^http(s)?://example\.com value abc
http-response ^http(s)?://example\.com documents Surge
```

### 语法

每行包含一条重写规则，参数之间以空格分隔，并以 `http-request` 或 `http-response` 开头。第二个参数是用于匹配生效 URL 的正则表达式。第三个参数是用于替换的正则表达式，第四个参数是替换的内容。

`http-response ^https?://example\.com/ regex replacement`

允许在后面继续添加正则表达式和替换内容，以进行连续替换。

```ini
http-response ^https?://example\.com/ regex1 replacement1 regex2 replacement2
http-response ^https?://example\.com/ regex1 replacement1 regex2 replacement2 regex3 replacement3
…
```

1.  如果一个请求匹配到多个请求体重写规则，它们将被按顺序执行。
2.  即使原始请求不包含请求体，仍然可以通过请求体重写生成新内容，例如使用 `^$` 表达式。

### JQ 请求体重写 iOS 5.14.0+ Mac 5.9.0+

你可以使用 JQ 表达式来操作 JSON 请求体。

```ini
http-request-jq url-pattern jq-expexpression
http-response-jq url-pattern jq-expexpression
```

例如：

```ini
http-response-jq ^http://httpbingo.org/anything '.headers |= with_entries(select(.key | test("^X-") | not))'
```
