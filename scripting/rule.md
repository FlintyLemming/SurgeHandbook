### 规则 (rule)

使用脚本作为规则。value 字段将被用作规则名称。

`rule ssid-rule script-path=ssid-rule.js`

然后在 `[Rule]` 段落中添加一行：

`SCRIPT,ssid-rule,DIRECT`

脚本应该返回一个带有 `matched` 属性的对象，以指示是否匹配。

传入的参数包括：

*   `$request.hostname<String>`
*   `$request.destPort<Number>`
*   `$request.processPath<String>`
*   `$request.userAgent<String>`
*   `$request.url<String>`
*   `$request.sourceIP<String>`
*   `$request.listenPort<Number>`
*   `$request.dnsResult<Object>`
*   `$request.srcPort<Number>` iOS 5.8.4+ Mac 5.4.4+
*   `$request.protocol<Number>` iOS 5.8.4+ Mac 5.4.4+

默认情况下，`SCRIPT` 规则不会触发 DNS 查询。您可以使用 `requires-resolve` 选项来更改此行为。

`SCRIPT,ssid-rule,DIRECT,requires-resolve`

DNS 结果包含在 `$request.dnsResult` 中。

一个简单的示例：

```javascript
var hostnameMatched = ($request.hostname === 'home.com');
var ssidMatched = ($network.wifi.ssid === 'My Home');

$done({matched: (hostnameMatched && ssidMatched)});
```
