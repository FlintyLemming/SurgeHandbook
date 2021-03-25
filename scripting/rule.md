# 脚本规则

使用脚本进行规则判定，该类型下第二参数为规则名。

```ini
script = type=rule,script-path=rule.js
```

然后在 \[Rule\] 中加入规则：

```text
SCRIPT,ssid-rule,DIRECT
```

脚本返回一个词典，字段 `matched<Boolean>` 表示是否匹配该规则。

传入参数有：

* `$request.hostname<String>`
* `$request.destPort<Number>`
* `$request.processPath<String>`
* `$request.userAgent<String>`
* `$request.url<String>`
* `$request.sourceIP<String>`
* `$request.listenPort<Number>`
* `$request.dnsResult<Object>`

默认情况下，SCRIPT 规则不会触发 DNS 解析，如果需要进行 DNS 解析，可使用 requires-resolve 修饰规则

```text
SCRIPT,ssid-rule,DIRECT,requires-resolve
```

DNS 结果将出现在 `$request.dnsResult` 字段。

一个简单样例：

```javascript
var hostnameMatched = ($request.hostname === 'home.com');
var ssidMatched = ($network.wifi.ssid === 'My Home');

$done({matched: (hostnameMatched && ssidMatched)});
```
