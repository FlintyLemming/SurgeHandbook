### DNS

使用脚本作为 DNS 解析器。value 字段将被用作名称。

`dns dnspod script-path=dnspod.js`

然后在 `[Host]` 段落中添加一行：

```ini
[Host]
example.com = script:dnspod
*.example.com = script:dnspod
```

传入的参数为 `$domain`。

脚本应该返回以下内容**之一**：

*   `address<String>`: 使用此 IP 地址作为结果。它必须是字符串格式的有效 IPv4/IPv6 地址。
*   `addresses<Array>`: 使用多个 IP 地址作为结果。
*   `server<String>`: 要求 Surge 通过指定的上游 DNS 服务器解析该域名。它必须是字符串格式的有效 IPv4/IPv6 地址。
*   `servers<Array>`: 要求 Surge 通过多个指定的上游 DNS 服务器解析该域名。

当返回 `address<String>` 或 `addresses<Array>` 时，还可以返回一个额外的 `ttl`，以将结果添加到缓存中并避免重复查询。单位是秒。

以下是一个示例，使用 DNSPod 的公共 HTTP DNS API 作为 Surge 的解析器：

```javascript
$httpClient.get('http://119.29.29.29/d?dn=' + $domain, function(error, response, data){
  if (error) {
    $done({}); // 回退到标准 DNS 查询
  } else {
    $done({addresses: data.split(';'), ttl: 600});
  }
});
```
