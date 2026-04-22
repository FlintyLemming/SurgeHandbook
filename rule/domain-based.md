# 基于域名的规则 (Domain-based Rule)

共有三种基于域名的规则类型。

#### DOMAIN

`DOMAIN,www.apple.com,Proxy`

如果请求的域名完全匹配，则触发该规则。

#### DOMAIN-SUFFIX

`DOMAIN-SUFFIX,apple.com,Proxy`

如果请求的域名匹配该后缀，则触发该规则。例如：'google.com' 会匹配 'www.google.com'、'mail.google.com' 和 'google.com'，但**不**匹配 'content-google.com'。

#### DOMAIN-KEYWORD

`DOMAIN-KEYWORD,google,Proxy`

如果请求的域名包含该关键字，则触发该规则。

#### DOMAIN-SET

专为大量域名设计，支持数千条记录的快速搜索。文件中的每一行都是一个域名，如果一行以 `.` 开头，则匹配所有子域名以及该域名本身。这可用于广告过滤。

### 基于域名的规则参数

#### extended-matching iOS 5.8.0+ Mac 5.4.0+

当启用此参数时，规则将尝试同时匹配 SNI 和 HTTP Host 请求头 (或 `:authority`)。

此参数仅适用于 `DOMAIN`、`DOMAIN-SUFFIX`、`DOMAIN-KEYWORD` 规则，你可以通过将该参数追加到相应的 `RULE-SET`/`DOMAIN-SET` 规则行，将其传播到 `DOMAIN-SET` 的条目中。