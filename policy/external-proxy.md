# 外部代理程序 (External Proxy Program) 仅限 Mac

Surge Mac 支持外部代理程序策略，这使得 Surge 能够更容易地与其他代理软件协同工作。

以下是一个 ssh 的例子。

首先，策略的关键字类型为 `external`。

```ini
[Proxy]
external = external, exec = "/usr/bin/ssh", args = "11.22.33.44", args = "-D", args = "127.0.0.1:1080", local-port = 1080, addresses = 11.22.33.44
```

`args` 和 `addresses` 参数是可选的，`exec` 和 `local-port` 是必填的。`args` 和 `addresses` 字段可以重复使用以进行追加。

Surge 将执行以下操作：

1.  当该策略被使用时，Surge 会使用 `exec` 和 `args` 参数启动外部进程，随后将请求转发到 SOCKS5 `127.0.0.1:[local-port]`。
2.  如果外部进程被终止，在下次使用该策略时它将自动重启。
3.  当开启增强模式时，Surge 会自动将 `addresses` 参数中的地址从 VIF 路由中排除。（因此请在此字段中填写代理服务器的 IP 地址。不支持主机名和域名。）
4.  Surge 总是对来自外部进程的请求使用 `DIRECT` 策略。（为了应对类似 obfs-local 的插件程序，外部进程的子进程也同样处理。）
5.  Surge 退出时自动关闭所有外部进程，并在增强模式关闭时自动清理路由表项。

一些注意事项：

1.  上述第 3 点和第 4 点的功能有重叠，请使用 `addresses` 声明来排除 VIF 处理，这可以降低处理开销，第 4 点的功能是作为额外的保护措施。
2.  外部进程的 stdout 和 stderr 被重定向到 `/tmp/Surge-External-xxxxxx.log` 以便于排错。
3.  由于外部进程启动可能需要一点时间。如果在转发到 `127.0.0.1:[local-port]` 时遇到连接拒绝 (connection refused) 错误，Surge 将会在 500ms 后自动重试，每个请求最多重试 6 次。
4.  Surge iOS 将会把 `external` 策略视为 `REJECT`，因为它不支持此功能。
