# HTTP API


* 仅Surge iOS 4.4.0 和 Surge Mac 4.0.0 以上版本支持通过 HTTP API 控制 Surge。

## 配置

```conf
[General]
http-api = examplekey@0.0.0.0:6171
```

## 身份验证

API key 必须填写在所有请求的 'X-Key' 的 Header 中。

```
GET /v1/events
X-Key: examplekey
Accept: */*
```

## 基本约束

当前版本仅支持未经 TLS 加密的 HTTP 请求。Surge 仅适用 GET 和 POST 方法。

* 对于 GET 请求，应适用 URL 发送参数。
* 对于 POST 请求，应使用 JSON Body 发送参数。

Surge 总会返回 JSON Body 格式的响应。

## 路径

### 切换开关功能

* GET /v1/features/mitm
* POST /v1/features/mitm
* GET /v1/features/capture
* POST /v1/features/capture
* GET /v1/features/rewrite
* POST /v1/features/rewrite
* GET /v1/features/scripting
* POST /v1/features/scripting
* GET /v1/features/system_proxy （仅 Surge Mac 可用）
* POST /v1/features/system_proxy （仅 Surge Mac 可用）
* GET /v1/features/enhanced_mode （仅 Surge Mac 可用）
* POST /v1/features/enhanced_mode （仅 Surge Mac 可用）

使用 GET 方法获取开关的状态。

GET 方法响应示例:

```json
{"enabled":true}
```

适用 POST 请求调整开关状态。

POST 方法请求示例:

```json
{"enabled":true}
```

## 出站模式

* GET /v1/outbound
* POST /v1/outbound

适用 GET 请求 请求获取状态，适用 POST 请求修改。

GET 方法响应示例:

```
{"mode":"rule"}
```

适用 POST 请求调整开关状态。

POST 方法请求示例:

```json
{"mode":"rule"}
```

模式的值可能为：direct, proxy, rule

* GET /v1/outbound/global
* POST /v1/outbound/global

获取或更改全局出站模式的默认策略。

GET 方法响应示例:

```json
{"policy":"ProxyA"}
```

适用 POST 请求调整开关状态。

POST 方法请求示例:

```json
{"policy":"ProxyB"}
```

## 代理策略组

* GET /v1/policies

获取所有策略组。

* GET /v1/policies/detail?policy_name=ProxyNameHere

获取代理的详细信息。

* POST /v1/policies/test

对策略组进行测速。

请求示例：

```json
{"policy_names": ["ProxyA", "ProxyB"], "url": "http://bing.com"}
```

* GET /v1/policy_groups

列出所有的策略组和他们的选项。

* GET /v1/policy_groups/test_results

获取 url-test/fallback/load-balance 组的测速结果。

* GET /v1/policy_groups/select?group_name=GroupNameHere

获取 select 组的选项。

请求示例：

```json
{"policy": "ProxyA"}
```

* POST /v1/policy_groups/select

改变 select 策略组的选项。

请求示例：

```json
{"group_name": "GroupA", "policy": "ProxyA"}
```

* POST /v1/policy_groups/test

立即测试一个策略组。

请求示例：

```json
{"group_name": "GroupA"}
```

响应示例：

```json
{
    "results": [
        {
            "data": {
                "ProxyA": {
                    "tcp": 48,
                    "rtt": 45,
                    "receive": 273,
                    "available": 213
                },
                "ProxyB": {
                    "tcp": 48,
                    "tfo": false,
                    "receive": 164,
                    "rtt": 51,
                    "available": 48
                },
                "ProxyC": {}
            },
            "time": 1595510609.6405091
        }
    ],
    "time": 1595510609.6405091,
    "winner": "ProxyA"
}
```

## 请求

* GET /v1/requests/recent

获取最近请求。

* GET /v1/requests/active

获取所有活动中的请求。

* POST /v1/requests/kill

杀死一个请求。

请求示例：

```json
{"id": 100}
```

## Profiles

* GET /v1/profiles/current?sensitive=0

获取当前配置文件的内容。 如果"sensitive"为假，则所有密码字段都将被屏蔽。

* POST /v1/profiles/reload

立即重新载入配置文件。

* POST /v1/profiles/switch （仅 Surge Mac 可用）

请求示例：

```json
{"name": "Profile2"}
```

切换到另一个配置。

* GET /v1/profiles （仅 Surge Mac 4.0.6 及以上版本可用）

获取可用的配置文件名。

* POST /v1/profiles/check （仅 Surge Mac 4.0.6 及以上版本可用）

请求示例：

```json
{"name": "Profile2"}
```

检查配置。如果配置文件无效返回错误，否则 "error" 字段为空。
## DNS
* POST /v1/dns/flush

刷新 DNS 缓存。

* GET /v1/dns

获取当前 DNS 缓存内容。

* POST /v1/test/dns_delay

测试 DNS 延时。

## 模块

* GET /v1/modules

获取可用和激活的模块。

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

* POST /v1/modules

启动或关闭模块

请求示例：

```json
{
    "router.com": false,
    "Google Home Devices": true
}
```

## 脚本

* GET /v1/scripting

获取脚本列表。

POST /v1/scripting/evaluate

适用 Mock 环境执行脚本。

请求示例：

```json
{
    "script_text": "The content of JS script",
    "mock_type": "cron",
    "timeout": 5
}
```

* POST /v1/scripting/cron/evaluate

立即执行一个定时脚本。

请求示例：

```json
{
    "script_name": "script1",
}
```

## 设备管理（仅 Mac 4.0.6 以上版本可用）

* GET /v1/devices

获取当前激活和保存的设备列表。

* GET /v1/devices/icon?id={iconID}

获取设备的图标。你可以从 "device.dhcpDevice.icon" 中获取图标 ID。

* POST /v1/devices

改变设备的属性。`physicalAddress` 字段是必须的，可以从`name`、`address`、`shouldHandledBySurge` 中调整一个或多个属性。

请求示例：

```json
{
    "physicalAddress":"F0:9F:C2:00:00:00",
    "name": "Computer",
    "address": "192.168.1.200",
    "shouldHandledBySurge": true
}
```
## 杂项

* POST /v1/stop

停止 Surge。若在 Surge iOS 上开启了 Always On 则会重启。

* GET /v1/events

获取事件中心的内容。

* GET /v1/rules

获取规则列表。

* GET /v1/traffic

获取流量信息。

* POST /v1/log/level

修改当前会话的日志等级。

请求示例：

```json
{"level": "verbose"}
```