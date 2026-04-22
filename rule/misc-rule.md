# 杂项规则 (Miscellaneous Rule)

### 端口号规则

端口号规则支持三种表达式：

*   直接写端口号，例如 `IN-PORT,6153`
*   端口号闭区间：例如 `DEST-PORT,10000-20000`
*   使用 `>`、`<`、`<=`、`>=` 运算符，例如 `SRC-PORT,>=50000` (iOS 5.8.4+ Mac 5.4.4+)

#### DEST-PORT

如果请求的目标端口匹配，则触发该规则。

```ini
DEST-PORT,80-81,DIRECT
```

#### IN-PORT

如果请求的传入端口匹配，则触发该规则。在 Surge 监听多个端口时很有用。

```ini
IN-PORT,6152,DIRECT
```

#### SRC-PORT iOS 5.8.4+ Mac 5.4.4+

如果请求的客户端端口号匹配，则触发该规则。

```ini
SRC-PORT,>=50000,DIRECT
```

### 其他

#### SRC-IP

如果请求的客户端 IP 地址匹配，则触发该规则。仅适用于远程机器。

```ini
SRC-IP,192.168.20.100,DIRECT
```

`SRC-IP` 规则也支持 CIDR 表示法。

```ini
SRC-IP,192.168.20.0/24,DIRECT
```

#### PROTOCOL

如果请求的协议匹配，则触发该规则。可能的值为 HTTP、HTTPS、TCP、UDP、DOH、DOH3、DOQ、QUIC、STUN。

```ini
PROTOCOL,HTTP,DIRECT
```

1.  由于 QUIC 存在多个草案版本，并非所有的 QUIC 流量都能被 Surge 识别。
2.  出于兼容性原因，`PROTOCOL,UDP` 也可以匹配 QUIC 流量。
3.  `PROTOCOL,TCP` 现在涵盖了 HTTP 和 HTTPS 连接，因此你可以为基于 TCP 的网络流量编写一条规则即可。
4.  协议关键字 `DOH`、`DOH3` 和 `DOQ` 仅用于匹配 Surge 自身发送的加密 DNS 请求。此功能需要与 `encrypted-dns-follow-outbound-mode=true` 结合使用。
5.  STUN 检测可用于过滤 P2P 流量；`PROTOCOL,STUN` 将专门拦截或转发 STUN 数据包。

#### SCRIPT

使用 Javascript 脚本来确定是否匹配。

```ini
SCRIPT,ScriptName,DIRECT
```

#### CELLULAR-RADIO (仅限 iOS)

如果当前网络的蜂窝无线电技术匹配，则触发该规则。可能的值有 GPRS, Edge, WCDMA, HSDPA, HSUPA, CDMA1x, CDMAEVDORev0, CDMAEVDORevA, CDMAEVDORevB, eHRPD, HRPD, LTE, NRNSA, NR

```ini
CELLULAR-RADIO,LTE,DIRECT
```

#### DEVICE-NAME

如果客户端的设备名称匹配，则触发该规则。

*   对于 Surge Ponte 访问，设备名称为客户端设备系统设置中的设备名称。
*   如果启用了 Surge DHCP，对于局域网设备访问，可以使用在设备视图上找到的自定义设备名称。

#### MAC-ADDRESS Mac 6.1.0+

可以匹配访问设备的 MAC 地址。请注意，这仅对同一局域网内的设备有效；如果请求是由网关转发的，则无法获取 MAC 地址。

#### HOSTNAME-TYPE Mac 5.7.3+

匹配请求中主机名的形式。支持的关键字有：

*   `IPv4`: 主机名是字面量 IPv4 地址。
*   `IPv6`: 主机名是字面量 IPv6 地址。
*   `DOMAIN`: 主机名包含点且是常规域名。
*   `SIMPLE`: 不包含点的主机名，例如 `localhost`。

示例：

```ini
HOSTNAME-TYPE,IPv6,REJECT
HOSTNAME-TYPE,SIMPLE,DIRECT
```