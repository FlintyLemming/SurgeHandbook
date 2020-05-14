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

