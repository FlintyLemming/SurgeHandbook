# 规则集

从 Surge macOS 3.0 版本、Surge iOS 3.4 版本之后，你可以通过 URL 或者文件导入一个规则集，这个规则集里可以包含很多条规则。 而 Surge 本身则提供两个内置的规则集，包含了已经写好的预置规则。

## 内置规则集

### **SYSTEM**

    RULE-SET,SYSTEM,DIRECT

这个规则集包含了绝大多数来自 macOS 和 iOS 系统本身所发送的请求。但 App Store、iTunes 和其他虽然是苹果推出，但却是内容提供为主的 App 所发送的请求不包含在内。

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

> 这些规则在今后的版本中可能会发生变化，请留意软件中相关说明。

### **LAN**

    RULE-SET,LAN,DIRECT

包括了 'local' 和其他本地 IP 地址。需要注意的是，这个规则集仍然会触发 DNS 查询。

    DOMAIN-SUFFIX,local
    IP-CIDR,192.168.0.0/16
    IP-CIDR,10.0.0.0/8
    IP-CIDR,172.16.0.0/12
    IP-CIDR,127.0.0.0/8
    IP-CIDR,100.64.0.0/10

## **External Ruleset**

可以通过 URL 或者本地文件的形式导入外置规则集。规则集的内容是一行一条规则，但不写对应的策略。

例子：

    DOMAIN,exampleA.com
    DOMAIN,exampleB.com