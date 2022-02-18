# 杂项规则

## DEST-PORT

规则会匹配相应端口的出站请求。

```
DEST-PORT,80,DIRECT
```
## SRC-IP

规则会匹配相应来源 IP 的入站请求。

```
SRC-IP,192.168.20.100,DIRECT
```

## IN-PORT

规则会匹配相应端口的入站请求。 在 Surge 监听多个端口时会有用。

```
IN-PORT,6152,DIRECT
```

## PROTOCOL

规则会匹配与协议参数相符合的请求。可选值：HTTP，HTTPS，TCP，UDP，DOH。

```
PROTOCOL,HTTP,DIRECT
```

## SUBNET

如果 SSID / BSSID / 路由器IP地址 匹配，则规则匹配。支持通配符模式。

```
SUBNET,MyHome,DIRECT
```

## SCRIPT

使用一段 JavaScript 脚本判断是否匹配。

```
SCRIPT,ScriptName,DIRECT
```

## CELLULAR-RADIO（仅 iOS）

如果与当前网络使用的无线通信技术匹配，则规则匹配。可选值：GPRS, Edge, WCDMA, HSDPA, HSUPA, CDMA1x, CDMAEVDORev0, CDMAEVDORevA, CDMAEVDORevB, eHRPD, HRPD, LTE, NRNSA, NR

```
CELLULAR-RADIO,LTE,DIRECT
```

## CELLULAR-CARRIER（仅 iOS）

如果与当前网络所使用的运营商匹配，则规则匹配。可选值为 MCC-MNC 风格代码。

```
CELLULAR-CARRIER,289-67,DIRECT
```

> 更多代码，请参考[这里](https://zh.m.wikipedia.org/zh-hans/%E7%A7%BB%E5%8A%A8%E8%AE%BE%E5%A4%87%E7%BD%91%E7%BB%9C%E4%BB%A3%E7%A0%81)。