# 代理策略

一个代理类型的策略，会将当前规则匹配到的请求发送到指定的代理服务器。Surge 支持以下几种协议的代理：HTTP、HTTPS、SOCKS5、SOCKS5-TLS。

配置文件中，\[Prosy\] 这一部分定义代理类型的策略（即代理服务器列表）。你可以填写多个代理服务器，这样就可以让不同的规则使用不同的服务器代理策略。

例子：

```text
[Proxy]
ProxyHTTP = http, 1.2.3.4, 443, username, password
ProxyHTTPS = https, 1.2.3.4, 443, username, password
ProxySOCKS5 = socks5, 1.2.3.4, 443, username, password
ProxySOCKS5TLS = socks5-tls, 1.2.3.4, 443, username, password, skip-common-name-verify=true
```

## 代理类型

Surge 支持标准的的几个代理协议。

* HTTP Proxy：`ProxyHTTP = http, 1.2.3.4, 443, username, password`
* HTTPS Proxy \(HTTP Proxy via TLS\)：`ProxyHTTPS = https, 1.2.3.4, 443, username, password`
* SOCKS5：`ProxySOCKS5 = socks5, 1.2.3.4, 443, username, password`
* SOCKS5 via TLS：`ProxySOCKS5TLS = socks5-tls, 1.2.3.4, 443, username, password`

Surge 也支持几个非标准的代理协议。

* Snell：`ProxySnell = snell, 1.2.3.4, 8000, psk=password`
* Shadowsocks：`ProxySS = ss, 1.2.3.4, 8000, encrypt-method=chacha20-ietf-poly1305, password=abcd1234`
* VMess：`ProxyVMess = vmess, 1.2.3.4, 8000, username=0233d11c-15a4-47d3-ade3-48ffca0ce119`
* Trojan：`ProxyTrojan = trojan, 192.168.20.6, 443, password=password1`

其中 Snell 是由我们开发的一个轻量化的加密代理协议。你可以在这里查看服务端的代码 [https://github.com/surge-networks/snell](https://github.com/surge-networks/snell)。

## 参数

### 所有类型的代理策略都包含的

* interface：可选（默认值： N/A）。

  让该代理强制使用某个网络接口（只在 macOS 中有效）。请确保指定的网络接口到目的地址有正确的路由表。

  ```text
      ProxyHTTP = http, 1.2.3.4, 443, username, password, interface = en2
  ```

* allow-other-interface：可选（默认值：false）。

  ```text
      ProxyHTTP = http, 1.2.3.4, 443, username, password, interface = en2, allow-other-interface=true
  ```

  除此之外，你还可以在 interface 基础上增加 allow-other-interface 参数。这样当指定的网络接口不可用时，会使用默认的网络接口，不然则会导致错误。

* tfo
* mptcp
* no-error-alert

### 对于通过 **TLS 代理（HTTP、SOCKS5-TLS、VMess、Trojan）的代理**

* skip-cert-verify：可选项，"true" 或 "false" （默认值为 "false"）。

  如果该选项设置为 true，Surge 则不会验证服务器的证书是否有效。

* sni （默认值：hostname）

  你可以在 TLS 握手时自定义 Server Name Indication（SNI）。通过添加 sni=off 可以关闭 SNI。 默认情况下，Surge 像大多数浏览器一样，会发送域名作为 SNI。

### **HTTP/HTTPS 协议的代理**

* always-use-connect

### 对于支持混淆的代理（**Shadowsocks, Snell）**

* obfs
* obfs-host
* obfs-uri

### 对于 Snell

* psk
* version

### 对于 Shadowsocks

* udp-relay

### 对于 VMess 协议

* ws
* ws-path
* ws-headers
* encrypt-method

## **TLS 代理的客户端证书**

Surge 支持基于 TLS 代理的客户端证书验证。

```bash
[Proxy] Proxy = https, example.com, 443, client-cert=cert1
```

```bash
[Keystore] cert1 = base64=, password=123456
```

