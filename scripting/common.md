# 基础

脚本需要 Surge iOS 4 或 Surge Mac 3.3.0 以上版本

## 脚本

使用 JavaScript 随心所欲的拓展 Surge 功能。

### 脚本字段

```ini
[Script]
script1 = type=http-response,pattern=^http://www.example.com/test script-path=test.js,max-size=16384,debug=true
scropt2 = type=cron,cronexp="* * * * *",script-path=fired.js
scropt3 = type=http-request,pattern=^http://httpbin.org script-path=http-request.js,max-size=16384,debug=true,requires-body=true
scropt4 = type=dns,script-path=dns.js,debug=true
```

每一行都有两个组成部分：脚本名称、参数。常见的参数：

- type: 脚本的类型，可选值：http-request, http-response, cron, event, dns, rule.
- script-path：脚本的路径，可以是相对路径、绝对路径或者 URL。
- script-update-interval：当脚本路径为 URL 时的自动更新频率，单位为秒。
- debug：开启 debug 模式，如果是一个本地脚本，那么每次执行脚本时，都会从本地存储重新加载脚本，方便调试。
- timeout：脚本的最长运行时间，默认为 10s。

http-request/http-response 类型脚本的可用参数：

- pattern: 匹配URL的正则。
- requires-body：表示该脚本需要对 body 进行处理，默认为 false。如果只需要修改 URL 或者 Headers 请不要开启该选项，将大幅节约资源。
- max-size: 表示该脚本最大允许处理的 body 大小，若超过则放弃处理，默认值为 131072 （128KB）。

由于进行脚本修改会需要 Surge 先将 response body 完全下载后再进行处理，如果遇到了较大的数据将导致内存占用量暴增，Surge iOS 受系统内存限制在该情况下极易被直接终止。所以请务必仔细配置 URL 匹配规则，仅对需要的 URL 进行处理。

当返回的数据长度超过 max-size 设定值后，Surge 将放弃对该请求执行脚本并回退到 passthrough 模式。

### 基本定义

所有脚本允许异步操作，使用 `$done(value<Object>)` 方法表示完成并返回相应结果。即使是不要求返回结果的脚本类型也应当在完成任务后调用 `$done()` 退出，否则脚本会因为超时而产生警告。

### 性能

JS Script 的执行效率极高，不必担心因使用脚本而带来性能问题（Body 修改类除外，会影响整体逻辑），在我们的测试环境下，一个简单脚本的完整执行仅耗时 0.2ms。

### 公共API

* `console.log(message<String>)`  

  输出到 Surge 日志

* `setTimeout(function[, delay])`  

  与浏览器的 setTimeout 方法一致

* `$httpClient.post(URL<String> or options<Object>, callback<Function>)`  
  发起一个 HTTP POST 请求。第一个参数可以是一个 URL 或参数表，参数表为。

  ```json
  {
    url: "http://www.example.com/",
      headers: {
        Content-Type: "application/json"
      },
    body: "{}"
  }
  ```

  当使用参数表时，`url` 参数必选，其余选填，`header` 字段存在会覆盖默认的所有 Header。`body` 可以是 string 或 object。当为 object 时，将自动进行 JSON 编码，并设置 'Content-Type' 为 'application/json'。

  callback定义为`callback(error<String>, response<Object>, data<String>)`  
  error 为 Null 表示请求成功，response 包含 status 和 headers 两个字段。

  其余类似的方法有：`$httpClient.get`，`$httpClient.put`，`$httpClient.delete`，`$httpClient.head`，`$httpClient.options`，`$httpClient.patch`。

* `$notification.post(title<String>, subtitle<String>, body<String>)` 向通知中心发送通知，Surge iOS 上需开启通知总开关
* `$utils.geoip(ip<String>)` 进行 GeoIP 查询，返回结果为 ISO 3166 的国家编码
* `$surge.setSelectGroupPolicy(groupName<String>, policyName<String>)` 修改 select 策略组的当前选项，返回 bool 值表示是否成功
* `$surge.selectGroupDetails()` 获得当前 select 策略组的信息，包含组名称，子策略，和当前选择的策略
* `$surge.setOutboundMode(mode<String>)` mode 取值可为 "direct", "global-proxy", "rule"，修改 Surge 的 Outbound Mode，返回 bool 值表示是否成功
* `$surge.setHTTPCaptureEnabled(enabled<Boolean>)`
* `$surge.setCellularModeEnabled(enabled<Boolean>)`
* `$surge.setRewriteEnabled(enabled<Boolean>)`
* `$surge.setEnhancedModeEnabled(enabled<Boolean>)` 仅 Surge Mac 可用 以上四项，用于控制 Surge 的各项功能的开启
* `$network` 当前网络状态的总览，包含 IP 和 SSID 等信息
* `$script.name<String>` 当前执行的脚本的文件名
* `$script.startTime<Date>` 当前执行的脚本的开始时间
* `$persistentStore.write(data<String>, [key<String>])` 持久化保存数据，返回 bool 值表示是否成功，仅支持传入 string
* `$persistentStore.read([key<String>])` 读取保存的持久化数据，返回 string 或 Null 不传入 key 时，同一个 script-path 的脚本共享一个存储池。可传入一个固定的 key 以在多个脚本间共享数据。

