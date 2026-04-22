### http-response

使用脚本修改 HTTP 响应。value 字段是一个正则表达式，用于匹配请求的 URL。

传入的参数包含 `$request` 和 `$response`：

*   `$request.url<String>`: 请求的 URL。
*   `$request.method<String>`：请求的 HTTP 方法。
*   `$request.id<String>`: 用于在不同脚本间保持连续性的唯一 ID。
*   `$request.headers<Object>`：请求的 HTTP 标头。
*   `$response.status<Number>`: 响应的 HTTP 状态码。
*   `$response.headers<Object>`: 响应的 HTTP 标头。
*   `$response.body<String or Uint8Array>`: 响应的 HTTP body，如果未设置 `binary-mode`，则会使用 UTF-8 解码为字符串。仅在 `requires-body = true` 时存在。

脚本必须调用 `$done()` 并传入一个对象。该对象可以包含：

*   `body<String or Uint8Array>`: 使用新的 body 覆盖旧的 body。仅在 `requires-body = true` 时有效。
*   `headers<Object>`: 使用新的 headers 覆盖所有旧的 headers。请注意，某些字段（例如 'Content-Length'）可能无法修改。
*   `status<Number>`: 使用新的状态码覆盖旧的状态码。

您可以调用 `$done();` 来中止请求而不返回任何内容。或者使用 `$done({});` 保持响应不变。

一个简单的示例：

```javascript
let headers = $response.headers;
headers['X-Modified-By'] = 'Surge';

$done({headers});
```
