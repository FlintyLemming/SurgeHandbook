# 子网设置 (Subnet Settings)

你可以使用[子网表达式 (subnet expression)](../rule/subnet.md)来匹配指定的网络并应用特定设置。

> 出于兼容性原因，子网设置在配置中被命名为 `[SSID Setting]`。

### 挂起 (Suspend)

在指定的网络下临时挂起 Surge。

```ini
[SSID Setting]
SSID:MyHome suspend=true
```

### 蜂窝网络回退 (Cellular Fallback) (仅限 iOS)

控制指定 Wi-Fi 网络的 Wi-Fi 助理 (Wi-Fi assist) 和混合网络 (Hybrid Network) 行为。

```ini
[SSID Setting]
SSID:MyHome cellular-fallback=off
```

*   `cellular-fallback=default` 使用全局的 Wi-Fi 助理和混合网络设置。
*   `cellular-fallback=off` 为该网络关闭 Wi-Fi 助理和混合网络。
*   `cellular-fallback=hybrid` 为该网络开启混合网络。
*   `cellular-fallback=wifi-assist` 为该网络开启 Wi-Fi 助理。

### TCP Fast Open 行为 (TCP Fast Open Behaviour)

```ini
[SSID Setting]
SSID:MyHome tfo-behaviour=force-enabled
```

*   `tfo-behaviour=auto` 使用默认的 TFO 行为。
*   `tfo-behaviour=force-disabled` 完全为该网络禁用 TFO。
*   `tfo-behaviour=force-enabled` 强制为该网络开启 TFO。此选项会使 Surge 忽略系统的 TFO 黑洞检测机制。

### DNS 覆盖 (DNS Override)

覆盖指定网络的 DNS 设置。

```ini
[SSID Setting]
SSID:MyHome dns-server=8.8.8.8,encrypted-dns-server=https://1.1.1.1/
```

如果加密 DNS 是在全局 DNS 设置中配置的，你必须在下面显式输入关键字 `off` 以使用传统 DNS。

```ini
[SSID Setting]
SSID:MyHome dns-server=8.8.8.8,encrypted-dns-server=off
```
