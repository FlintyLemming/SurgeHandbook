*脚本功能需要 Surge iOS 4 或 Surge Mac 3.3.0*

# 脚本 (Scripting)

你可以使用 JavaScript 按照你的意愿扩展 Surge 的能力。

## 脚本段落 (Script Section)

```ini
[Script]
script1 = type=http-response,pattern=^http://www.example.com/test,script-path=test.js,max-size=16384,debug=true
script2 = type=cron,cronexp="* * * * *",script-path=fired.js
script3 = type=http-request,pattern=^http://httpbin.org,script-path=http-request.js,max-size=16384,debug=true,requires-body=true
script4 = type=dns,script-path=dns.js,debug=true
```

每一行包含两个组成部分：脚本名称和参数。公共参数：

*   `type`: 脚本的类型：`http-request`、`http-response`、`cron`、`event`、`dns`、`rule`、`generic`。
*   `script-path`: 脚本的路径，可以是相对于配置文件的相对路径、绝对路径或 URL。
*   `script-update-interval`: 当为 script-path 使用 URL 时的更新间隔，单位为秒。
*   `debug`: 开启调试模式，具有以下效果：
    1.  每次执行脚本前，都会从文件系统重新加载脚本。（仅限 Surge Mac）
    2.  对于 `http-request` 和 `http-response` 脚本，当你使用 `console.log()` 输出日志时，这些消息也会显示在请求的注释 (notes) 中。
*   `timeout`: 脚本的最长运行时间。默认值为 5 秒。
*   `argument`: 脚本可以通过 `$argument` 获取该值。
*   `engine`: 参见本文后半部分。

`http-request` 和 `http-response` 的参数：

*   `pattern`: 用于匹配 URL 的正则表达式。
*   `requires-body`: 允许脚本修改请求/响应体。默认值为 false。此行为开销很大。仅在必要时开启。
*   `max-size`: 允许的请求/响应体的最大大小。默认值为 131072 (128KB)。
*   `binary-body-mode`: 仅在 iOS 15 和 macOS 中可用。原始二进制数据将以 `Uint8Array` 而不是字符串值的形式传递给脚本。

脚本功能要求 Surge 将整个响应体数据加载到内存中。巨大的响应体可能会导致 Surge iOS 崩溃，因为 iOS 系统限制了 Network Extension 可以占用的最大内存量。

请仅为必要的 URL 启用脚本。

如果响应体大小超过 `max-size` 的值，Surge 将回退到直通 (passthrough) 模式，并跳过对该请求的脚本处理。

## 基本限制 (Basic Constraints)

脚本允许异步操作。应该调用 `$done(value)` 来表示完成，即使对于不需要结果的脚本也是如此。否则，脚本会因为超时而收到警告。

## 性能 (Performances)

你无需担心脚本的性能。JavaScript 核心非常高效。

## 公共 API (Public API)

### 基本信息

*   **`$network`**

该对象包含网络环境的详细信息。

*   **`$script`**

    *   `$script.name<String>`: 正在执行的脚本名称。
    *   `$script.startTime<Date>`: 当前脚本开始的时间。
    *   `$script.type<String>`: 当前脚本的类型。
*   **`$environment`**

    *   `$environment.system<String>`: iOS 或 macOS。
    *   `$environment.surge-build<String>`: Surge 的构建号。
    *   `$environment.surge-version<String>`: Surge 的简短版本号。
    *   `$environment.language<String>`: 当前 Surge 的 UI 语言。
    *   `$environment.device-model<String>`: 当前设备型号。iOS 5.9.0+ Mac 5.5.0+

### 持久化存储 (Persistent Store)

*   **`$persistentStore.write(data<String>, [key<String>])`**

永久保存数据。只允许字符串。如果成功则返回 true。

*   **`$persistentStore.read([key<String>])`**

获取保存的数据。返回字符串或 Null。

如果 key 为 undefined，具有相同 script-path 的脚本将共享存储池。使用 key 可以在不同脚本之间共享数据。

提示：Surge Mac 将 `$persistentStore` 数据写入目录 `~/Library/Application Support/com.nssurge.surge-mac/SGJSVMPersistentStore/`。你可以直接在此处编辑文件以进行调试。

### 控制 Surge

*   **`$httpAPI(method<String>, path<String>, body<Object>, callback<Function>(result<Object>))`**

你可以使用 `$httpAPI` 调用所有的 HTTP API 来控制 Surge 的功能。不需要身份验证参数。请参阅 HTTP API 部分以了解可用功能。

### $httpClient

*   **`$httpClient.post(URL<String> 或 options<Object>, callback<Function>)`**

发起 HTTP POST 请求。第一个参数可以是 URL 或选项对象。选项对象示例如下：

```json
{
  url: "http://www.example.com/",
  headers: {
    Content-Type: "application/json"
    },
  body: "{}",
  timeout: 5
}
```

当使用对象作为选项列表时，`url` 是必需的。如果存在 `headers` 字段，它将覆盖所有现有的标头字段。`body` 可以是字符串或对象。如果提供的是对象，它将被编码为 JSON 字符串，并且 'Content-Type' 将设置为 `application/json`。

#### callback

callback: `callback(error, response, data)`

成功时，error 为 null，response 对象包含 `status` 和 `headers` 属性。

相似函数：**`$httpClient.get`**, **`$httpClient.put`**, **`$httpClient.delete`**, **`$httpClient.head`**, **`$httpClient.options`**, **`$httpClient.patch`**。

#### 选项 (Options)

*   `timeout`: 默认超时时间为 5 秒。你可以使用此选项覆盖它。
*   `insecure`: 如果将此选项设置为 true，https 请求将不验证服务器证书。iOS 5.9.0+ Mac 5.5.0+
*   `auto-cookie`: 控制是否自动处理 Cookie 相关字段并存储，默认启用。如果关闭，Cookie 标头将作为普通字段传递。iOS 5.9.0+ Mac 5.5.0+
*   `auto-redirect`: 控制当遇到 30x HTTP 状态码时是否自动重定向请求，默认启用。iOS 5.9.0+ Mac 5.5.0+

##### 策略 (Policy)

你可以指定一个策略来执行请求：

*   `policy`: 使用现有的策略名称。
*   `policy-descriptor`: 使用带有完整描述符的临时策略。

#### 二进制数据 (Binary Data) iOS 5.4.1+ Mac 5.0.1+

你可以将 `TypedArray` 对象作为 body 传递。

此外，你可以使用 `binary-mode` 参数让 Surge 以 `TypedArray` 而不是字符串形式返回响应数据。

```json
{
  url: "http://www.example.com/",
  binary-mode: true
}
```

### 实用工具 (Utilities)

*   **`console.log(message<String>)`**

打印到 Surge 日志文件。

*   **`setTimeout(function[, delay])`**

与浏览器中的 setTimeout 相同。

*   **`$utils.geoip(ip<String>)`**

执行 GeoIP 查询。结果采用 ISO 3166 代码。

*   **`$utils.ipasn(ip<String>)`**

查找 IP 地址的 ASN。

*   **`$utils.ipaso(ip<String>)`**

查找 IP 地址的 ASO。

*   **`$utils.ungzip(binary<Uint8Array>)`**

解压 gzip 数据。结果也是一个 `Uint8Array`。

*   **`$notification.post(title<String>, subtitle<String>, body<String>[, options<Object>])`**

发送通知。

可用选项：iOS 5.11.0+ Mac 5.7.0+

*   `action`: 点击通知打开 Surge 后的操作。

    *   `open-url`: 打开一个 URL，特定的 URL 由 `url` 参数提供。
    *   `clipboard`: 复制内容到剪贴板（需要用户确认），内容通过 `text` 参数提供。

*   `media-url`: 为通知提供媒体内容，例如图片。内容应该是一个有效的 URL。
*   `media-base64`: 功能同上，但内容直接通过 base64 提供。需要通过 `media-base64-mime` 参数提供内容的 MIME 类型。
*   `auto-dismiss`: 布尔值，指示是否在一段时间（通常是 10 秒）后自动关闭通知。
*   `sound`: 弹出通知时使用默认推送消息声音。

### 手动触发 (Manually Trigger)

你可以在 Surge iOS 上长按脚本或使用系统的快捷指令 (Shortcuts) 应用来手动触发脚本。

如果你使用快捷指令触发脚本，你可以选择向脚本传递一个参数，并使用 `$intent.parameter` 来获取它。

## 脚本引擎 (Script Engine) iOS 5.9.0+ Mac 5.5.0+

Surge 目前包含两个 JavaScript 脚本执行引擎。

### JavaScriptCore (`engine=jsc`)

*   优点：
    1.  引擎初始化快，调用时开销低（低延迟）。
*   缺点：
    1.  由于 JSC 运行在 NE 进程内部，会导致 Surge NE 进程的内存占用显著增加，可能导致系统因超出内存限制而终止。

### WebView (`engine=webview`)

*   优点：
    1.  由于 WebView 的实际运行环境在另一个独立的进程，脚本的执行对 NE 进程的内存使用几乎没有影响，不会导致 Surge NE 进程因内存占用问题被终止。
    2.  WebView 的 JS 执行环境可以使用 JIT，大大提升了复杂或 CPU 密集型脚本的执行效率。
    3.  可以使用 WebAPI。
*   缺点：
    1.  引擎的初始化时间开销略高。
    2.  当脚本与 Surge 之间需要传输大量数据时，由于跨进程通信，效率较低，这在使用 `binary-body-mode` 处理较大请求时尤为明显。

### 使用建议

1.  对于小型、频繁调用、简单的脚本，如 Rule、DNS 类型的脚本，建议使用 JSC。
2.  对于复杂、高内存消耗的脚本（例如解析 MB 级别 HTTP body 的 JSON），建议使用 WebView。

### 配置方法

在脚本配置行添加参数：`engine`，可配置为 `auto`、`jsc`、`webview`。

*   默认值为 `auto`，在可用的情况下始终使用 WebView。
*   如果脚本中使用了 WebAPI，应显式配置为 `webview`，以便脚本在不支持 WebView 的环境中执行时提示用户。

### 引擎可用性

*   iOS: JSC 和 WebView
*   macOS
    *   macOS 10.15 及以下版本: 仅限 JSC
    *   macOS 11.0 及以上版本: JSC 和 WebView
*   tvOS: 仅限 JSC
