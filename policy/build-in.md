# 内置策略

有两个内置的策略：DIRECT 和 REJECT。DIRECT 会把流量直接发送到目标服务端，REJECT 则会拒绝请求。

DIRECT 和 REJECT 这两个策略可以直接用。或者你也可以像下面这样，用一个名字代替这两个内置策略。

## 别名

```ini
[Proxy]
On = direct
Off = reject
```

之后你就可以在规则里使用 `On` 和 `Off` 作为策略组的策略名。
## 网络接口选项

直连策略支持指定网络接口，即可以使用”interface“这个参数。

```ini
[Proxy]
Crop-VPN = direct, interface = utun0
WiFi = direct, interface = en2, allow-other-interface=true
```

请确保指定的网络接口到目的地址有正确的路由表。

除此之外，你还可以在 interface 基础上增加 `allow-other-interface` 参数。这样当指定的网络接口不可用时，会使用默认的网络接口，不然则会导致错误。

