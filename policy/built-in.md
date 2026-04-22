# 内置策略 (Built-in Policy)

Surge 包含几种内置策略，其中最重要的是 `DIRECT` 和 `REJECT`。`DIRECT` 表示请求应当直接发送给主机，而 `REJECT` 表示该请求应当被拒绝。

### 内置策略

#### DIRECT

将请求直接发送给主机。

#### CELLULAR 仅限 iOS

优先使用蜂窝网络而非 Wi-Fi 网络。

#### CELLULAR-ONLY 仅限 iOS

仅使用蜂窝网络。如果蜂窝网络不可用，则连接失败。

#### HYBRID 仅限 iOS

尝试同时通过 Wi-Fi 和蜂窝网络建立连接。仅在未开启“All Hybrid”选项时有意义。

#### NO-HYBRID 仅限 iOS

如果 Wi-Fi 可用，则永远不尝试通过蜂窝网络建立连接。仅在开启了“All Hybrid”或“Wi-Fi Assist”选项时有意义。

关于 REJECT/REJECT-DROP/REJECT-NO-DROP，请查看 [REJECT 策略](reject.md) 页面。

### 别名 (Alias)

内置策略可以直接在规则和策略组中使用。你也可以在 `[Proxy]` 段落中定义别名。

```ini
[Proxy]
On = direct
Off = reject
```

然后，你可以在规则和策略组中使用 `On` 和 `Off` 作为策略名称。
