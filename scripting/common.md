*脚本需要 Surge iOS 4 或 Surge Mac 3.3.0 以上版本*

# 脚本

使用 JavaScripe 随心所欲的拓展 Surge 功能。

## 脚本字段

```ini
[Script]
http-response ^http://www.example.com/test script-path=test.js,max-size=16384,debug=true
cron "* * * * *" script-path=fired.js
policy-group worst script-path=worst.js,debug=true
http-request ^http://httpbin.org script-path=http-request.js,max-size=16384,debug=true,requires-body=true
dns local script-path=dns.js,debug=true
```

每一行包含三个部分：type、value、parameters。每一种 type 对应不一样的 value。

公共参数：

- script-path：脚本的路径，可以是配置文件目录的相对路径也可以是绝对路径或 URL。
- script-update-interval：脚本的更新间隔，使用 URL 作为脚本路径时可以，单位秒。
- debug：启用调试模式。在每一次执行脚本时会从文件系统加载脚本。
- timeout：脚本运行的最常时间。默认值是 10 秒。

http-request/http-response 可用的参数：

- requires-body：允许脚本修改请求和响应的正文部分。默认值为 false。这个行为过多的资源，请在必要时选择。
- max-size: 请求和响应的正文部分允许的最大值。默认值为 131072 字节（128KB）。

脚本需要将响应的正文部分整体载入到内存。一个巨大的相应体正文可能会引发 Surge iOS 的崩溃，因为 iOS 系统限制了 Network Extension 可以占用的最大内存。

请为必要的网址使用脚本功能。

如果请求的响应的正文部分超过了 max-size 的值，Surge 会回退到 passthrough 模式并跳过这个脚本。

## 基本约束

脚本允许进行异步的操作。即使不需要结果的脚本，也需要调用`$done(value)`来表明已经结束。否则，脚本会因为超时而触发警告。

## 性能

你不需要太担心性能问题。JavaScript Core 非常的高效。

## 公共API

- `console.log(message<String>)`  
  输出日志到Surge的日志文件。
- `setTimeout(function[, delay])`  
  与浏览器中的setTimeout行为相同。
- `$httpClient.post(URL<String> or options<Object>, callback<Function>)`  
  发起一个 HTTP 的 POST 请求。第一个参数可以是一个 URL 或对象。下面是对象的示例。
  ```json
  {
    url: "http://www.example.com/",
      headers: {
        Content-Type: "application/json"
      },
    body: "{}"
  }
  ```
  使用对象作为选项时，`url` 是必选的，如果 `header` 存在，他会覆盖现有的 Header字段。`body` 可以是字符串或对象。当传入对象时，会被编码为JSON字符串，并将 'Content-Type' 设置为 'application/json'。  
  `callback: callback(error, response, data)`  
  运行成功时, `error` 传入 `null`，`response` 包含 'status' 和 'headers'。  
  类似的函数有：`$httpClient.get`，`$httpClient.put`，`$httpClient.delete`，`$httpClient.head`，`$httpClient.options`，`$httpClient.patch`。

- `$notification.post(title<String>, subtitle<String>, body<String>)`  
  发送一个通知。

- `$utils.geoip(ip<String>)`  
  执行 GeoIP 查找。返回 ISO 3166 格式的代码。

- `$surge.setSelectGroupPolicy(groupName<String>, policyName<String>)`  
  设置选择策略组的策略。成功返回 true。

- `$surge.selectGroupDetails()`  
  获取所有选择策略组的策略的细节。

- `$surge.setOutboundMode(mode<String>)`  
  设置出站模式，可选值 'direct'，'global-proxy' 或 'rule'。成功返回 true。

- `$surge.setHTTPCaptureEnabled(enabled<Boolean>)`
- `$surge.setCellularModeEnabled(enabled<Boolean>)`
- `$surge.setRewriteEnabled(enabled<Boolean>)`
- `$surge.setEnhancedModeEnabled(enabled<Boolean>)` Surge Mac 可用。  
  控制 Surge 功能的状态。

- `$network`  
  这个对象包含了网络环境的信息。

- `$script.name<String>`  
  正在执行的脚本名。

- `$script.startTime<Date>`  
  当前脚本的启动时间。

- `$persistentStore.write(data<String>, [key<String>])`  
  写入持久化数据。只允许传入字符串，成功返回 true。

- `$persistentStore.read([key<String>])`  
  后去保存的数据，返回字符串或空值。  
  如果相同路径的脚本数据存储池中未定义该键，会从其他的脚本中获取相同键的数据。即可以使用键在不同的脚本之间共享数据。
