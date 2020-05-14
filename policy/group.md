# 策略组

一个策略组可以包含多个策略，然后这个策略组本身可以被当成一个策略来使用，并且也可以被嵌套。

有以下几种类型的策略组：select、url-test 和 ssid。配置文件中，在 \[Proxy Group\] 下定义策略组。

## 手动选择的策略组

可以在用户界面选择使用策略组里的某个策略。

    SelectGroup = select, ProxyHTTP, ProxyHTTPS, DIRECT, REJECT

> 在 iOS 版中，你可以在系统的 Today 页面中的小组件控制第一个 select 类型的策略组。而在 macOS 版中，可以通过点击状态栏中的 Surge 图标来手动切换 select 类型的策略组。

## 自动测试的策略组

这个策略组会测试到包含的所有策略的延迟，自动选择最低的策略。

    AutoTestGroup = url-test, ProxySOCKS5, ProxySOCKS5TLS, url = http://www.google.com/generate_204

### 参数

### url：必须

Surge 会发送一个 HTTP 头请求到目的地址。这个测试只关心是否收到了应答数据，即便返回的是一个 HTTP 错误 的数据。

### interval：可选，单位是秒（默认值：600s）

超过这个时间，就不会再进行测试了，除非重新或者正在使用这个策略组。

### tolerance：可选，单位是毫秒（默认值：100ms）

每次重新测试时，只有当延迟最低的策略比上一次测试的延迟最低的策略快 100ms 时，才会使用新的策略。

### timeout：可选，单位是秒（默认值：5s）

当测速策略组中的某个策略超过该时间没有应答，则会放弃该策略。

## Fallback 策略组

与上面提到的自动测试的策略组原理相同，只不过 Fallback 按照从前到后的顺序选择可用策略，而不是选择延迟最低的策略。

    FallbackGroup = fallback, ProxySOCKS5, ProxySOCKS5TLS, url = http://www.google.com/generate_204

### 参数

### url：必须

Surge 会发送一个 HTTP 头请求到目的地址。这个测试只关心是否收到了应答数据，即便返回的是一个 HTTP 错误 的数据。

### interval：可选，单位是毫秒（默认值：100ms）

超过这个时间，就不会再进行测试了，除非重新或者正在使用这个策略组。

### timeout：可选，单位是秒（默认值：5s）

当测速策略组中的某个策略超过该时间没有应答，则会放弃该策略，使用后面一个策略。

## SSID 策略组

根据当前的 Wi-Fi 名称选择相应的策略。

    SSIDGroup = ssid, default = ProxyHTTP, cellular = ProxyHTTP, SSIDName = ProxySOCKS5

### 参数

### default：必须

如果当前 Wi-Fi 名称不在该策略组里，则使用 default 指定的策略。

### cellular：可选

指定在蜂窝数据下使用的策略。如果不指定，则使用 default 所指定的策略。T

## External 策略组

这个功能在 macOS 3.0 版以及 iOS 3.4 版和之后提供。策略组中包含的策略，可以通过链接或者文件的方式远程引用。

    egroup = select, policy-path=proxies.txt

这个文件包含了一系列的策略，格式如下：

    Proxy-A = https, example1.com, 443
    Proxy-B = https, example2.com, 443

