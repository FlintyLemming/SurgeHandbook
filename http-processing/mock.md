# 模拟 (Mock)

你可以模拟 HTTP 服务器并返回静态响应。此功能也可以被称为 Map Local 或 API Mocking。如果你想动态返回响应，请尝试使用脚本 (scripting)。

示例：

```ini
[Map Local]
^http://surgetest\.com/json data-type=text data="{}" status-code=500
^http://surgetest\.com/gif data-type=tiny-gif status-code=200
^http://surgetest\.com/file data-type=file data="data/map-local.json" header="a:b|foo:bar"
^http://surgetest\.com/base64 data="dGVzdA==" data-type=base64
```

### 参数

#### URL 匹配模式 (URL Pattern)

每行通过以空格分隔的多个参数进行定义，其中第一个参数是用于匹配 URL 的正则表达式。如果某个 HTTP 请求（或解密后的 HTTPS 请求）与此表达式匹配，则应用该规则。

#### `data-type`

Surge 目前支持四种数据类型：

*   `file`: 返回特定文件或 URL 的内容。
*   `text`: 返回数据字段的文本，使用 UTF-8 编码。iOS 5.9.1+ Mac 5.5.1+
*   `tiny-gif`: 返回一个 1 像素的 GIF。iOS 5.9.1+ Mac 5.5.1+
*   `base64`: 返回使用 Base64 编码的二进制数据。iOS 5.9.1+ Mac 5.5.1+

#### `data`

*   对于 `file` 类型，此字段应为数据文件的路径，相对路径是相对于配置文件的目录。在 macOS 中，也可以使用绝对路径。
*   对于 `text` 类型，此字段即内容本身。
*   对于 `tiny-gif` 类型，此字段没有意义。
*   对于 `base64` 类型，此字段应包含有效的 Base64 数据。

你可以使用 `data-type=text data=""` 来返回空结果。

#### `header`

此参数允许你自定义返回结果的 HTTP 请求头。使用 `|` 来分隔多个键值对。

### 关于 Content-Type

你可以使用 `header` 字段来控制返回结果的 Content-Type。如果未提供，Surge 将会尽可能地补全。

*   对于 `file` 类型，它将尝试把文件扩展名转换为 MIME 类型。如果失败，将使用 `application/octet-stream`。
*   对于 `text` 类型，默认使用 `plain\text`。
*   对于 `tiny-gif` 类型，默认使用 `image/gif`。
*   对于 `base64` 类型，默认使用 `application/octet-stream`。
