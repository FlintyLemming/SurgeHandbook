# HTTP 返回（HTTP Response）

💡**本页文档来自[官方社区](https://community.nssurge.com/d/33-scripting)**

用于修改 HTTP 返回体，该类型下第二参数为匹配 URL 的正则表达式，被匹配到的请求会被执行脚本。

传入参数为 $request 和 $response，字段为

- $request.url<String>：请求的 URL
- $request.method<String>：请求的 HTTP 方法
- $response.status<Number>: 响应的 HTTP 状态码
- $response.headers<Object>: 响应的 HTTP Headers
- $response.body<String>: 响应的 HTTP Body，以 UTF-8 解码后的字符串，仅当 requires-body = true 时有效
- $request.rawBody<Uint8Array>：响应的原始二进制 Body，仅当 requires-body = true 时有效，请注意 JS 的 Uint8Array 没有只读限制，但是请勿修改该对象，如果要修改应在复制后修改再返回。

应执行 $done 返回一个对象，可选包含三个字段：

- body<String>：使用该 body 覆盖原来的响应 body，仅当 requires-body = true 时有效。
- rawBody<Uint8Array>：使用该 raw body 覆盖原来的请求 body，仅当 requires-body = true 时有效。该属性优先级高于 body。
- headers<Object>：使用该 headers 词典完全覆盖原来的 headers，注意部分 HTTP 特殊字段不可被修改，如 Content-Length
- status<Number>：覆盖原来的 HTTP 状态码

使用 $done(); 表示终止该请求，使用 $done({}); 表示不对该请求进行修改。

一个简单样例

```
let headers = $response.headers;
headers['X-Modified-By'] = 'Surge';

$done({headers});
```

注意样例使用了 JS ES6 语法。