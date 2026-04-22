# HTTP API iOS 4.4.0+ Mac 4.0.0+

你可以使用 HTTP API 来控制 Surge。

## 配置

```ini
[General]
http-api = examplekey@0.0.0.0:6171
http-api-tls = false
```

## 身份验证

所有请求的 'X-Key' 请求头中必须填入 API 密钥。

```http
GET /v1/events
X-Key: examplekey
Accept: */*
```

在某些特定情况下，如果不方便设置请求头，你也可以通过 URL 参数进行传递。例如，直接通过浏览器下载 CA 证书。

```
http://127.0.0.1:6171/v1/mitm/ca?x-key=examplekey
```

## HTTPS(TLS)

设置 `http-api-tls = true` 可以为 HTTP API 服务启用 HTTPS 支持。Surge 将使用 MITM 的 CA 证书为相应的访问地址生成服务器证书。

## 基本限制

目前仅支持不带 TLS 的 HTTP。Surge 仅使用 GET 和 POST 方法。

*   对于 GET 方法，你应该使用 URL 参数发送参数。
*   对于 POST 方法，你应该使用 JSON 请求体发送参数。

Surge 将始终返回一个 JSON 请求体作为响应。

## 路径 (Paths)

### 切换功能状态 (Toggle capabilities)

*   GET /v1/features/mitm
*   POST /v1/features/mitm
*   GET /v1/features/capture
*   POST /v1/features/capture
*   GET /v1/features/rewrite
*   POST /v1/features/rewrite
*   GET /v1/features/scripting
*   POST /v1/features/scripting
*   GET /v1/features/system\_proxy (仅限 Surge Mac)
*   POST /v1/features/system\_proxy (仅限 Surge Mac)
*   GET /v1/features/enhanced\_mode (仅限 Surge Mac)
*   POST /v1/features/enhanced\_mode (仅限 Surge Mac)

使用 GET 方法获取某个功能的状态。

GET 响应示例：

```json
{"enabled":true}
```

使用 POST 方法调整某个功能的状态。

POST 请求示例：

```json
{"enabled":true}
```

### 出站模式 (Outbound Mode)

*   GET /v1/outbound
*   POST /v1/outbound

使用 GET 获取出站模式，使用 POST 更改它。

GET 响应示例：

```json
{"mode":"rule"}
```

POST 请求示例：

```json
{"mode":"rule"}
```

可能的模式：direct, proxy, rule

*   GET /v1/outbound/global
*   POST /v1/outbound/global

获取或更改全局出站模式的默认策略。

GET 响应示例：

```json
{"policy":"ProxyA"}
```

POST 请求示例：

```json
{"policy":"ProxyB"}
```

### 代理策略 (Proxy Policy)

*   GET /v1/policies

列出所有策略。

*   GET /v1/policies/detail?policy\_name=ProxyNameHere

获取策略的详细信息。

*   POST /v1/policies/test

使用 URL 测试策略。

请求示例：

```json
{"policy_names": ["ProxyA", "ProxyB"], "url": "http://bing.com"}
```

*   GET /v1/policy\_groups

列出所有策略组及其选项。

*   GET /v1/policy\_groups/test\_results

获取 url-test/fallback/load-balance 组的测试结果。

*   GET /v1/policy\_groups/select?group\_name=GroupNameHere

获取手动选择组 (select group) 的选项。

响应示例：

```json
{"policy": "ProxyA"}
```

*   POST /v1/policy\_groups/select

更改手动选择组 (select group) 的选项。

请求示例：

```json
{"group_name": "GroupA", "policy": "ProxyA"}
```

*   POST /v1/policy\_groups/test

立即测试一个策略组。

请求示例：

```json
{"group_name": "GroupA"}
```

响应示例：

```json
{
    "available": [
        "ProxyA",
        "ProxyB"
    ]
}
```

### 请求 (Requests)

*   GET /v1/requests/recent

列出最近的请求。

*   GET /v1/requests/active

列出所有活动的请求。

*   POST /v1/requests/kill

终止一个活动的请求。

请求示例：

```json
{"id": 100}
```

### 配置 (Profiles)

*   GET /v1/profiles/current?sensitive=0

获取当前配置的文本内容。如果 'sensitive' 为 false，则所有密码字段都将被掩码隐藏。

*   POST /v1/profiles/reload

立即执行配置重新加载。

*   POST /v1/profiles/switch (仅限 Surge Mac)

请求示例：

```json
{"name": "Profile2"}
```

切换到另一个配置。

*   GET /v1/profiles (仅限 Mac 4.0.6+)

获取所有可用的配置名称。

*   POST /v1/profiles/check (仅限 Mac 4.0.6+)

请求示例：

```json
{"name": "Profile2"}
```

检查配置。如果配置无效，将返回一个错误。否则 "error" 字段将为 null。

### DNS

*   POST /v1/dns/flush

清除 DNS 缓存。

*   GET /v1/dns

获取当前的 DNS 缓存内容。

*   POST /v1/test/dns\_delay

测试 DNS 延迟。

## 模块 (Modules)

*   GET /v1/modules

列出可用和启用的模块。

响应示例：

```json
{
    "enabled": [
        "router.com"
    ],
    "available": [
        "Game Console SNAT",
        "Google Home Devices",
        "router.com",
        "MitM All Hostnames"
    ]
}
```

*   POST /v1/modules

启用或禁用模块。

请求示例：

```json
{
    "router.com": false,
    "Google Home Devices": true
}
```

### 脚本 (Scripting)

*   GET /v1/scripting

列出所有脚本。

*   POST /v1/scripting/evaluate

在模拟环境中执行脚本。

请求示例：

```json
{
    "script_text": "The content of JS script",
    "mock_type": "cron",
    "timeout": 5
}
```

*   POST /v1/scripting/cron/evaluate

立即执行一个 cron 脚本。

请求示例：

```json
{
    "script_name": "script1"
}
```

### 设备管理 (Device Management) (仅限 Mac 4.0.6+)

*   GET /v1/devices

获取当前活动的和已保存的设备列表。

*   GET /v1/devices/icon?id={iconID}

获取设备的图标。你可以从 `device.dhcpDevice.icon` 中获取 iconID。

*   POST /v1/devices

更改设备属性。`physicalAddress` 字段为必填。你可以调整 `name`、`address` 和 `shouldHandledBySurge` 中的一个或多个属性。

请求示例：

```json
{
    "physicalAddress":"F0:9F:C2:00:00:00",
    "name": "Computer",
    "address": "192.168.1.200",
    "shouldHandledBySurge": true
}
```

### 杂项 (Misc)

*   POST /v1/stop

关闭 Surge 引擎。如果在 Surge iOS 上启用了 Always On，Surge 引擎将会重新启动。

*   GET /v1/events

获取事件中心的内容。

*   GET /v1/rules

获取规则列表。

*   GET /v1/traffic

获取流量信息。

*   POST /v1/log/level

更改当前会话的日志级别。

请求示例：

```json
{"level": "verbose"}
```

*   GET /v1/mitm/ca

获取 MITM 的 CA 证书，采用 DER 二进制格式。（仅包含证书，不包含私钥）