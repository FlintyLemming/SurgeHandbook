# 端口转发 (Port Forwarding) iOS 5.14.3+ Mac 5.10.0+

Surge 可以监听特定的本地端口，并将来自该端口的 TCP 请求转发到特定的主机。当 Surge 的请求处理（系统代理或增强模式）未启用时，此功能可以独立使用。

配置示例：

```ini
[Port Forwarding]
0.0.0.0:6841 localhost:3306 policy=SQL-Server-Proxy
```

`policy` 参数是可选的；如果未指定，将使用标准的代理匹配来决定策略。

此功能通常用于开发和调试场景，例如使用 SSH 连接到像 MariaDB 这样的服务器。
