# DNS

使用脚本去执行 DNS 解析操作，该类型下第二参数为脚本名。

```
dns dnspod script-path=dnspod.js
```

之后需要在 `[Host]` 中对相应域名进行配置。

```
[Host]
baidu.com = script:dnspod
*.baidu.com = script:dnspod
```

传入参数为 `$domain`，当前查询的域名。

返回结果可为下列中的任意一个：

- 当返回 `address<String>` 时，直接使用该 IP 地址为结果，必须是一个有效的 IPv4 或者 IPv6 地址的字符串表示。
- 当返回 `addresses<Array>` 时，使用多个IP地址作为结果。
- 当返回 `server<String>` 时，表示应交给特定的 DNS 服务器进行查询。
- 当返回 `servesr<Array>` 时，表示指定多个上游 DNS 服务器进行查询。

当返回 `address<String>` 或者 `addresses<Array>` 时，可以额外返回 TTL 字段，将本次的结果记入 DNS 缓存避免重复查询，单位为秒。

以下样例使用了 DNSPod 的公开 HTTP DNS API，通过脚本扩展的方式让 Surge 可以随意支持各种协议的 DNS 查询

```
$httpClient.get('http://119.29.29.29/d?dn=' + $domain, function(error, response, data){
  if (error) {
    $done({}); // Fallback to standard DND query
  } else {
$done({addresses: data.split(';'), ttl: 600});
  }
});
```
