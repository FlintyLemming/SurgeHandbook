# 代理规则

Surge 可以根据自定义规则，决定将请求转发给代理服务器，还是直接发送给原始服务器。

## 优先度

规则按照配置文件里的顺序，按照从头到尾的顺序进行匹配。换句话说，第一条规则的优先级最高。

## 编辑规则

每一条规则由三部分组成：规则类型、匹配的流量（FINAL 规则没有这一项）以及所采取的策略。即：TYPR, VALUE, POLICY。比如：DOMAIN-SUFFIX,apple.com, DIRECT IP-CIDR, 192.168.0.0/16,ProxyA

Surge 支持 6 中不同种类的规则：DOMAIN、DOMAIN-SUFFIX、DOMAIN-KEYWORD、GEOIP、IP-CIDR 和 FINAL。对于所采取的策略，可以是一个代理服务器，也可以是一个策略组，或者是内置的“DIRECT”和“REJECT”。不管使用什么样的规则、写了多少条规则，都必须要以一个 FINAL 规则结束，作为对所写规则以外情况的判断。

例子：

```text
[Rule]
DOMAIN-SUFFIX,company.com,ProxyA
DOMAIN-KEYWORD,google,DIRECT
GEOIP,US,DIRECT
IP-CIDR,192.168.0.0/16,DIRECT
FINAL,ProxyB
```

DOMAIN、DOMAIN-SUFFIX 和 DOMAIN-KEYWORD 是[按域名判断的规则](https://surge-wiki.headto.icu/rule/domain-based)。 而 IP-CIDR 和 GEOIP 则是[按 IP 地址判断的规则](https://surge-wiki.headto.icu/rule/ip-based)。

