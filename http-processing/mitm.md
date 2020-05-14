# HTTPS 解密

Surge 通过中间人攻击（MitM）解密 HTTPS 流量。你可以参考维基百科的这篇文章获取关于 MitM 的更多信息：[Wikipedia](https://en.wikipedia.org/wiki/Man-in-the-middle_attack)。

证书生成器会帮你生成一份受系统信任的用于开发的 CA 证书。这个在 macOS 和 iOS 版都能操作。生成的证书只会被保存在本地和系统的钥匙串。新生成的证书对应的 key 通过 OpenSSL 随机生成。

你也可以用既存的 CA 证书，需要以 PKCS\#12（.p12）格式导出并随附密码，然后就像下面这样导入。需要注意的是，系统不接受 ca-passphrase 为空的证书，所以这一项必须要填。密码可以用 base64 命令加密成一个 base64 类型的字符串。

```text
[MITM]
enable = true
ca-p12 = MIIJtQ.........
ca-passphrase = password
hostname = *google.com
```

打开 MitM 解密后，Surge 默认不会解密所有流量，只会解密被声明的流量。

* 可以使用像 \*、? 这样的通配符。
* 使用 - 排除域名。
* 默认情况下，只会解密 443 端口的请求。
  * 添加 :port 来解密其他端口的 HTTPS 流量。
  * 添加 suffix :0 来解密所有端口的 HTTPS 流量。

Example:

* `-*.apple.com`: 排除所有通过 443 端口发送给 \*.apple.com 的请求。
* `www.google.com`: 解密通过 443 端口的 www.google.com 的 HTTPS 流量。
* `www.google.com:8080`: 解密通过 8080 端口的 www.google.com 的 HTTPS 流量。
* `www.google.com:0`: 解密所有端口的 www.google.com 的 HTTPS 流量。
* `*:0`: 解密所有的 HTTPS 流量。

如果有多条，可以像下面这样写：

`hostname = -*.apple.com, -*.icloud.com, *`

> 某些程序有着严格的安全策略，不会认可自己生成的证书。此时，使用解密的话可能会导致一些问题。

## 选项

### **tcp-connection**

默认情况下，MitM 只会解密通过 Surge HTTP 代理的流量。如果使用 tcp-connection 选项，Surge 则会通过使用 VIF（增强模式）对原始 TCP 流量进行解密。但这样你仍然需要在 hostname 列表中声明解密的域名。

### **skip-server-cert-verify**

MitM 时不验证证书。

