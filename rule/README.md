# 规则 (Rule)

Surge 可以根据自定义规则将请求转发到另一个代理服务器，或直接连接到主机。

### 优先级 (Priority)

规则从第一条到最后一条按顺序匹配，顺序即它们在配置文件中出现的顺序。换句话说，列表顶部的规则比后面的规则具有更高的优先级。

### 组成部分 (Composition)

每条规则由 3 部分组成：规则类型、匹配器（FINAL 规则除外）和代理策略：

```ini
           类型(TYPE),  值(VALUE),       策略(POLICY)
例如:       DOMAIN-SUFFIX,apple.com,     DIRECT
           IP-CIDR,      192.168.0.0/16,ProxyA
```

Surge 支持多种类型的规则，请参阅此类别下特定规则的介绍。策略可以是内置策略、代理策略或策略组。有关详细信息，请参阅策略部分的说明。规则列表必须以一条 `FINAL` 规则结尾，以定义默认行为。

示例：

```ini
[Rule]
DOMAIN-SUFFIX,company.com,ProxyA
DOMAIN-KEYWORD,google,DIRECT
GEOIP,US,DIRECT
IP-CIDR,192.168.0.0/16,DIRECT
FINAL,ProxyB
```

`DOMAIN`、`DOMAIN-SUFFIX` 和 `DOMAIN-KEYWORD` 属于基于域名的规则 (domain based rules)。`IP-CIDR` 和 `GEOIP` 属于基于 IP 的规则 (IP based rules)。
