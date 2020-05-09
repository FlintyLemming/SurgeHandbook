# 根据 IP 地址匹配的规则

有两个根据 IP 匹配的规则。如果请求的主机名是一个域名，那么根据 IP 的规则将触发 DNS 查询。如果 DNS 查询失败，Surge 会停止规则测试并报错。

#### IP-CIDR

```
IP-CIDR,192.168.0.0/16,DIRECT
IP-CIDR,10.0.0.0/8,DIRECT
IP-CIDR,172.16.0.0/12,DIRECT
IP-CIDR,127.0.0.1/8,DIRECT
```

规则会匹配规则范围内请求的 IP 地址

#### GEOIP

`GEOIP,US,DIRECT`

规则会匹配相应国家和地区的 IP 地址

#### 根据 IP 判断的规则的可选项

#### 选项: no-resolve

```
GEOIP,US,DIRECT,no-resolve
IP-CIDR,172.16.0.0/12,DIRECT,no-resolve
```

当 GEOIP 或者 IP-CIDR 规则被触发，Surge 会发送一个解析请求去检查主机名是否为一个域名。你可以加上 “no-resolve” 以跳过这个过程。

> 请注意: 如果本地 DNS 服务器无法解析某些域名，请
确保在这个规则前没有基于 IP 的规则会匹配到这个域名。否则，由于 DNS 解析错误，规则测试将失败。
你可以用“不no-resolve”来解决这个问题。