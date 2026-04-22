### http-request

使用脚本修改 HTTP 请求。value 字段是一个正则表达式，用于匹配请求的 URL。

传入的参数包含在 `$request` 中：

*   `$request.url<String>`: 请求的 URL。
*   `$request.method<String>`：请求的 HTTP 方法。
*   `$request.headers<Object>`：请求的 HTTP 标头。
*   `$request.body<String or Uint8Array>`：请求的 body。仅在 `requires-body = true` 时有效。
*   `$request.id<String>`: 用于在不同脚本间保持连续性的唯一 ID。

脚本必须调用 `$done()` 并传入一个对象。该对象可以包含：

*   `url<String>`: 使用新的 URL 覆盖旧的 URL。请注意，它不会像 URL Rewrite 那样更新请求头中的 'Host' 字段。如果需要，您必须通过返回修改后的 header 对象来手动更改它。
*   `headers<Object>`: 使用新的 headers 覆盖所有旧的 headers。请注意，某些字段（例如 'Content-Length'）可能无法修改。
*   `body<String or Uint8Array>`: 使用新的 body 覆盖旧的 body。仅在 `requires-body = true` 时有效。

*   `response<Object>`：如果该对象存在，Surge 将直接返回 HTTP 响应，而无需进行真正的网络操作。该对象可以包含：

    *   `status<Number>`: 响应的 HTTP 状态码。（可选。默认值：200）
    *   `headers<Object>`: 响应的 HTTP 标头。（可选）
    *   `body<String or Uint8Array>`: 响应的 HTTP Body。（可选）

您可以调用 `$done();` 来中止请求而不返回任何内容。或者使用 `$done({});` 保持请求不变。

当前版本的一些限制：

*   使用 chunked 编码时，请求的 body 可能无法被覆盖。
*   当请求头中存在 'Expect: 100-continue' 时，请求的 body 可能无法被覆盖。

一个简单的示例：

```javascript
let headers = $request.headers;
headers['X-Modified-By'] = 'Surge';

$done({headers});
```
