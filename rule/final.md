# Final 规则

FINAL 规则必须写在所有其他类型的规则之后。当某条请求不匹配上面的任何一条规则，都会与 FINAL 规则匹配。

例子：

```
[Rule]
DOMAIN-SUFFIX,company.com,ProxyA
DOMAIN-KEYWORD,google,DIRECT
GEOIP,US,DIRECT
IP-CIDR,192.168.0.0/16,DIRECT
FINAL,ProxyB
```

## 选项

### 选项**: dns-failed**

当 DNS 查询失败后，也会匹配 FINAL 规则，前提是 FINAL 规则的策略不是 DIRECT。