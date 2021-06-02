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

### use-default-policy-if-wifi-not-primary （仅macOS）

如果 Wi-Fi 不是主接口，则使用SSID组的默认策略。

```
use-default-policy-if-wifi-not-primary = false
```

### proxy-settings-interface （仅macOS）

助手代理接口设置。

```
proxy-settings-interface = Wi-Fi
```

### always-real-ip

```
always-real-ip = *.apple.com
```

当 Surge VIF 处理 DNS 问题时，这个选项要求 Surge 返回一个真实的 IP 地址，而不是一个假的 IP 地址（Fake-IP）。

DNS 请求包将被转发到上游的 DNS 服务器。

### hijack-dns

```
hijack-dns = 8.8.8.8:53
```

默认情况下，Surge 只对发送至 Surge DNS地址(198.18.0.2)的 DNS 查询返回 Fake-IP 地址。发送到标准 DNS 的查询将被转发。

有些设备或软件始终使用硬编码的 DNS 服务器。例如，谷歌的一款音箱总是使用 8.8.8.8）。你可以使用这个选项来劫持查询以获得一个 Fake-IP 地址。

你可以使用 `hijack-dns = *:53` 来劫持所有 DNS 请求。

### force-http-engine-hosts

```
force-http-engine-hosts = example.com:80
```

让 Surge 将 TCP 连接视为 HTTP 请求。Surge 的 HTTP 引擎将处理这些请求，并且所有的高级功能都可以使用，如捕获、重写和脚本。

- 通配符支持 `*` 和 `?`。
- 使用前缀 `-` 排除域名。
- 默认情况下，只有去往 80 端口的请求会被解码。
  + 使用后缀 `:port` 启用指定端口
  + 使用后缀 `:0` 启用所有端口。

例如：

- `-*.apple.com`: 排除所有去往 *.apple.com 的 80 端口。
- `www.google.com`: 强制 HTTP 引擎处理通往 www.google.com 的 80 端口的请求。
- `www.google.com:8080`: 强制 HTTP 引擎处理通往 www.google.com 的 8080 端口的请求。
- `www.google.com:0`: 强制 HTTP 引擎处理通往 www.google.com 所有端口的请求。
- `*:0`: 强制 HTTP 引擎处理通往所有主机的所有端口请求。

### tun-excluded-routes

```
tun-excluded-routes = 192.168.0.0/16, 10.0.0.0/8, 172.16.0.0/12
```

Surge VIF 只能处理 TCP 和 UDP 协议。使用此选项可以绕过特定的 IP 范围，允许所有流量通过。

> 注意：此选项只适用于 Surge VIF。由 Surge 代理服务器处理的请求不受影响。结合`skip-proxy` 和 `tun-excluded-routes` 来确保特定的 HTTP 流量绕过 Surge。  
> 这个选项可能会导致系统错误 ENOMEM（无法分配内存）。这似乎是 iOS 系统的一个 Bug。如果可能的话，请不要使用这个选项。

### tun-included-routes

```
tun-included-routes = 192.168.1.12/32
```

在默认情况下，Surge VIF 声明自己为默认路由。但是，由于 Wi-Fi 接口的路由较小，有些流量可能不会通过 Surge VIF 接口。使用此选项可以添加一个较小的路由。


### compatibility-mode （仅iOS）

### allow-wifi-access （仅iOS）

### wifi-access-http-port （仅iOS）

### wifi-access-socks5-port （仅iOS）

### show-error-page-for-reject

### exclude-simple-hostnames

### socks5-listen （仅macOS）

### http-listen （仅macOS）

### proxy-test-url

### test-timeout

### internet-test-url

### read-etc-hosts

### network-framework

### tls-provider

### debug-cpu-usage

### debug-memory-usage
