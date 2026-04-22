# 子网规则 (Subnet Rule)

### 子网表达式 (Subnet Expression)

子网表达式可以是以下之一：

*   使用 `SSID:value` 匹配 Wi-Fi SSID，允许使用通配符。
*   使用 `BSSID:value` 匹配 Wi-Fi BSSID，允许使用通配符。
*   使用 `ROUTER:value` 匹配路由器 IP 地址。
*   使用 `TYPE:WIFI` 匹配所有 Wi-Fi 网络。
*   使用 `TYPE:WIRED` 匹配所有有线网络。
*   使用 `TYPE:CELLULAR` 匹配所有蜂窝网络。
*   如果未提供前缀，它将尝试匹配 SSID/BSSID/Router，以保持旧版本兼容性。

#### SUBNET

如果子网表达式匹配，则触发该规则。

```ini
SUBNET,TYPE:WIRED,DIRECT
SUBNET,SSID:MyHome,Proxy
```
