## URL Scheme（仅 Surge iOS）

Surge iOS 支持 4 种操作和 1 种选项。

操作：

- `surge:///start`

    启动当前选中的配置文件。

- `surge:///stop`

    停止当前会话。

- `surge:///toggle`

    启动或停止当前选中的配置文件。

- `surge:///install-config?url=x`

    从某个 URL 安装配置文件。该 URL 需先通过百分号编码处理。

选项：

- `autoclose=true`

    操作完成时自动关闭 Surge。（不能与 `install-config` 一同使用）

    样例：`surge:///toggle?autoclose=true`

## x-callback-url

Surge 从 3.4 版本开始支持 x-callback-url 规范。URL 的 scheme 为 `surge`，可用的操作有 `start`、`stop` 和 `toggle`。