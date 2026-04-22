# REJECT 策略

为了满足不同的需求，Surge 有多个内置的 REJECT 策略。在大多数情况下，直接使用 `REJECT` 已经足够。如果存在特殊需求，可以考虑使用派生策略。

#### REJECT

拒绝请求，如果该请求是 HTTP 类型，则将返回一个错误页面。此行为可以通过 `show-error-page-for-reject` 参数进行控制。

#### REJECT-DROP

拒绝请求。与 REJECT 不同，此策略将静默丢弃连接。某些应用程序具有非常激进的重试逻辑，当它们在连接失败后立即重试时，会导致请求风暴。使用此策略可以缓解该问题。

#### REJECT-NO-DROP

如果在短时间内对某个主机名的大量请求触发了 `REJECT/REJECT-TINYGIF` 策略（当前版本中的阈值为 30 秒内 50 次），Surge 将自动把策略升级为 `REJECT-DROP` 以避免浪费大量资源。

你可以使用 `REJECT-NO-DROP` 策略来避免这种行为。

#### REJECT-TINYGIF

拒绝请求，如果该请求是 HTTP 类型，则返回一个 1 像素的透明 GIF，用于广告拦截。

### 预匹配拒绝 (Pre-matching Reject) iOS 5.14.0+ Mac 5.9.0+

由于 Surge 规则系统可以评估的属性范围很广，因此规则的判定只能在接收到第一个 TCP 数据包后进行。这在处理风暴请求或广告拦截需求时会导致产生过多的不必要开销。

在新版本中，Surge 添加了预匹配 (Pre-matching) 功能，以低开销快速拒绝请求。对于使用 REJECT 策略的规则，可以通过 `pre-matching` 标记启用此功能。

```ini
[Rule]
DOMAIN,ad.com,REJECT,pre-matching
```

标记为 `pre-matching` 的规则将在正常的规则匹配过程之前生效，因此具有最高优先级。

所有标记为 `pre-matching` 的规则都将被提取以进行优先匹配，并在 DNS 解析和 TCP SYN 阶段执行。如果匹配了 DNS 域名，则直接返回无记录 (No Record)；如果在 TCP SYN 阶段匹配，则立即生成 TCP RST 响应。在大量请求的情况下，它会升级为丢包，UDP 的处理方式也类似。

此外，每条规则每 5 分钟只会在最近请求列表中出现一次，以避免因大量请求而导致列表泛滥。

可以标记为 `pre-matching` 的规则类型包括：

*   DOMAIN 类型：DOMAIN、DOMAIN-SUFFIX、DOMAIN-KEYWORD、DOMAIN-SET、DOMAIN-WILDCARD。
*   IP 类型：IP-CIDR、IP-CIDR6、GEOIP、IP-ASN。
*   逻辑规则：AND、OR、NOT
*   其他：SUBNET、DEST-PORT、SRC-PORT、SRC-IP

`RULE-SET` 也可以被使用，但其内容同样受上述限制。

### 预匹配技术细节

为了获得最佳的用户体验，拒绝在预匹配阶段进行，并在使用不同的派生规则时会存在一些细节上的差异。

#### 对于 DNS 查询

*   如果匹配了 REJECT 策略，Surge 将返回一个无记录的 DNS 响应。如果触发了内置于 REJECT 策略的频率限制，DNS 查询将被直接丢弃而不产生响应。
*   如果匹配了 REJECT-DROP 策略，DNS 查询将被直接丢弃而不产生响应。
*   如果匹配了 REJECT-NO-DROP 策略，它将返回一个特殊的 IP 地址 198.18.0.244；Surge 将为所有访问该地址的 TCP 连接生成 TCP RST 响应。

#### 对于使用 IP 的 TCP 请求

*   如果匹配了 REJECT 策略，将直接生成 TCP RST 响应；如果触发了内置于 REJECT 策略的频率限制，它将丢弃相应的 TCP SYN 握手数据包。
*   如果匹配了 REJECT-DROP 策略，将直接丢弃 TCP SYN 握手数据包。
*   如果匹配了 REJECT-NO-DROP 策略，将直接生成 TCP RST 响应。

请注意，由于某些软件可能具有激进的重试逻辑，在请求失败后立即重试从而导致异常的 CPU 占用，因此即使对于 REJECT-NO-DROP 策略，当 Surge 在短时间内产生大量的 TCP RST 数据包时（当前版本的阈值为 3 秒内 100 次），也会触发保护机制，暂停返回 TCP RST 而直接执行丢包处理。

#### 对于 UDP 数据包

由于 UDP 数据包没有握手开销，因此不存在预匹配阶段；它们使用主规则集进行直接匹配：

*   如果匹配了 REJECT 策略，将生成 ICMP Administratively Prohibited (管理上被禁止) 的响应；如果触发了内置于 REJECT 策略的频率限制，数据包将被直接丢弃。
*   如果匹配了 REJECT-DROP 策略，数据包将被直接丢弃。
*   如果匹配了 REJECT-NO-DROP 策略，将生成 ICMP Administratively Prohibited 的响应。
