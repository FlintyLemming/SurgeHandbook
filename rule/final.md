# 最终规则 (Final Rule)

`FINAL` 规则必须写在所有其他规则之后。它为未匹配任何其他规则的请求定义了默认策略。

示例：

```ini
[Rule]
DOMAIN-SUFFIX,company.com,ProxyA
DOMAIN-KEYWORD,google,DIRECT
GEOIP,US,DIRECT
IP-CIDR,192.168.0.0/16,DIRECT
FINAL,ProxyB
```

### 选项

#### 选项: dns-failed

如果在规则评估期间 DNS 查询失败，则使用 `FINAL` 规则。此选项仅在与非 `DIRECT` 策略结合使用时才有意义。
