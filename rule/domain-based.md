# 根据域名判断的规则
# 域名规则

有三种根据域名判断的规则

#### DOMAIN

`DOMAIN,www.apple.com,Proxy`

规则会匹配与请求完全相同的地址

#### DOMAIN-SUFFIX

`DOMAIN-SUFFIX,apple.com,Proxy`

规则会匹配与请求与主域名相同的地址

比如，'[google.com](http://google.com)' 匹配 '[www.google.com](http://www.google.com)', '[mail.google.com](http://mail.google.com)' 和 '[google.com](http://google.com)' 本身，**但**不会匹配 '[content-google.com](http://content-google.com)'

#### DOMAIN-KEYWORD

`DOMAIN-KEYWORD,google,Proxy`

规则会匹配包含相应关键字的域名

> 除了 "apple" 会匹配 "[www.apple.com](http://www.apple.com)"，"app" 同样也会匹配到

#### DOMAIN-SET

为支持快速在千万级域名列表文件中搜索而设计。文件中的每一行都是一个域名，如果某一行以.开头，则匹配所有子域名和域名本身。这可用于广告过滤。

> 也支持在文件中使用 IPv4 和 IPv6 地址，但不支持 CIDR 的形式。