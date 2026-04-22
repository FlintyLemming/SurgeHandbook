# 规则集 (Ruleset)

你可以使用来自文件或 URL 的一系列规则。Surge 也提供了两个内部规则集。

## 内部规则集 (Internal Ruleset)

内部规则集的内容可能会随着 Surge 版本更新而改变。请前往规则集设置查看最新的子规则。

### SYSTEM

`RULE-SET,SYSTEM,DIRECT`

包含 macOS 和 iOS 自身发送的大多数请求的规则。不包含由 App Store、iTunes 和其他内容服务发送的请求。

```ini
USER-AGENT,*com.apple.mobileme.fmip1
USER-AGENT,*WeatherFoundation*
USER-AGENT,%E5%9C%B0%E5%9B%BE*
USER-AGENT,%E8%AE%BE%E7%BD%AE*
USER-AGENT,com.apple.geod*
USER-AGENT,com.apple.Maps
USER-AGENT,FindMyFriends*
USER-AGENT,FindMyiPhone*
USER-AGENT,FMDClient*
USER-AGENT,FMFD*
USER-AGENT,fmflocatord*
USER-AGENT,geod*
USER-AGENT,locationd*
USER-AGENT,Maps*
DOMAIN,api.smoot.apple.com
DOMAIN,captive.apple.com
DOMAIN,configuration.apple.com
DOMAIN,guzzoni.apple.com
DOMAIN,smp-device-content.apple.com
DOMAIN,xp.apple.com
DOMAIN-SUFFIX,ess.apple.com
DOMAIN-SUFFIX,push-apple.com.akadns.net
DOMAIN-SUFFIX,push.apple.com
DOMAIN,aod.itunes.apple.com
DOMAIN,mesu.apple.com
DOMAIN,api.smoot.apple.cn
DOMAIN,gs-loc.apple.com
DOMAIN,mvod.itunes.apple.com
DOMAIN,streamingaudio.itunes.apple.com
DOMAIN-SUFFIX,lcdn-locator.apple.com
DOMAIN-SUFFIX,lcdn-registration.apple.com
DOMAIN-SUFFIX,ls.apple.com
PROCESS-NAME,trustd
```

> 这些规则可能会随着 Surge 的更新而更新。请参阅软件中的说明以获取最新的子规则。

### LAN

`RULE-SET,LAN,DIRECT`

包含针对局域网 IP 地址和 `.local` 后缀的规则。请注意，此规则集将触发 DNS 查询。

```ini
DOMAIN-SUFFIX,local
IP-CIDR,192.168.0.0/16
IP-CIDR,10.0.0.0/8
IP-CIDR,172.16.0.0/12
IP-CIDR,127.0.0.0/8
IP-CIDR,100.64.0.0/10
IP-CIDR6,fe80::/10
```

## 外部规则集 (External Ruleset)

来自 URL 或本地文件的规则集。规则集文件应为文本文件。每行包含一个不带策略的规则声明。

示例：

```ini
DOMAIN,exampleA.com
DOMAIN,exampleB.com
```

`[Rule]` 中的 `RULE-SET` 规则行接受应用于每个子规则的可选参数：

*   `no-resolve`: 在匹配该集合时跳过 DNS 解析。
*   `extended-matching`: 允许集合内的域名规则同时匹配 SNI 和 HTTP Host 请求头。

示例：

```ini
RULE-SET,https://example.com/social.list,Proxy,no-resolve,extended-matching
```

## 内联规则集 (Inline Ruleset) Mac 5.3.1+

你无需将列表托管在外部，而是可以将规则直接嵌入配置内部。

```ini
[Ruleset Streaming]
DOMAIN-SUFFIX,netflix.com
DOMAIN-SUFFIX,netflix.net
DOMAIN,netflixdnstest0.com

[Rule]
RULE-SET,Streaming,StreamingProxy
```

内联规则集与独立文件共享相同的语法，并受益于相同的预处理/索引优化。
