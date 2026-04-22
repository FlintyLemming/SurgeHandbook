# 子网组 (Subnet Group)

使用子网策略组，你可以根据当前的网络环境自动选择一个策略。你可以使用[子网表达式 (subnet expression)](../rule/subnet.md)作为条件。

`Subnet Group = subnet, default = ProxyHTTP, TYPE:WIFI = ProxyHTTP, SSID:MyHome = ProxySOCKS5`

从 Surge iOS 4.12.0 & Surge Mac 4.5.0 开始，SSID 组现更名为子网组 (Subnet Group)。依然支持 SSID 组的旧语法。你可以使用组类型关键字 `subnet` 或 `ssid` 以保持兼容。

### 参数

#### `default`: 必填

当没有匹配任何子网表达式时的策略。

#### `cellular`: 可选 (已弃用，请改用 `TYPE:CELLULAR`)

蜂窝网络的策略。如果不提供，则将使用默认策略。
