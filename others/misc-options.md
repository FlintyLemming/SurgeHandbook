# 杂项

## General 部分

```ini
[General]
ipv6 = false
loglevel = notify
skip-proxy = 127.0.0.1, 192.168.0.0/16, 10.0.0.0/8, 172.16.0.0/12, 100.64.0.0/10, localhost, *.local
tun-excluded-routes = 192.168.0.0/16, 10.0.0.0/8, 172.16.0.0/12
tun-included-routes = 192.168.1.12/32
```

### ipv6 默认值：false

启用完整的IPv6支持

```
ipv6 = true
```

### loglevel 默认值：notify

```
loglevel = notify
```

可选值为 verbose info notify 和 warning。不建议在日常使用中开启 verbose，因为这会消耗过多的资源使系统变慢。

loglevel = notify

### skip-proxy

```
skip-proxy = 127.0.0.1, 192.168.0.0/16, 10.0.0.0/8, 172.16.0.0/12, 100.64.0.0/10, localhost, *.local
```

在 iOS 平台，这个选项中的域名和IP段将由 Surge TUN 处理，而不是 Surge Proxy。  
在 macOS 平台，这个选项仅在“设置为系统代理”开启时生效，用于解决某些应用的兼容问题。
指定某个域名。例如，`apple.com`  
指定网站的所有域名，在域名前添加星号。例如，`*apple.com`  
指定某个域的特定部分。例如，`store.appple.com`
通过 IP 地址指定某个主机或网络，输入 IP 地址或地址范围。例如，`192.168.2.*` `192.168.2.0/24`

> 注意: 如果设置了IP地址或地址范围，仅在使用该地址连接主机时绕过代理，连接到解析后的地址时无法生效。