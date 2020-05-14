# DNS over HTTPS

## 为特定域名使用 DoH

像下面这样设置即可：

```text
[Host]
example.com = server:https://cloudflare-dns.com/dns-query
```

## 为所有域名使用 DoH

## 如果直接使用 IP 地址

像下面这样设置即可：

```text
[General]
dns-server = https://9.9.9.9/dns-query
```

你可以指定多个 DoH 服务器（并不建议），甚至和普通 DNS 服务器地址放在一起。但由于 DoH 返回结果一般比普通 DNS 慢，所以反而会在并发查询中被丢弃。

## 如果要使用域名

这个情况就有点麻烦了，你必须像下面这样，先指定 IP 地址：

```text
[General]
dns-server = https://cloudflare-dns.com/dns-query

[Host]
cloudflare-dns.com = server:1.1.1.1
```

## **DNS over HTTPS 格式**

有两种 DoH 格式：JSON 和 DNS wireformat \(RFC1035\).

你要确认清楚你的 DoH 服务商提供的是什么格式。

* Surge iOS 4.1 和以下的版本、macOS 3.4.1 和以下的版本只支持 JSON 格式。
* Surge iOS 4.2 和之后的版本、macOS 3.5.0 和之后的版本 Surge 默认使用 DNS wireformat 格式，你也可以像下面这样特别指定使用 JSON 格式。

  ```text
      [General]
      doh-format=json
  ```

