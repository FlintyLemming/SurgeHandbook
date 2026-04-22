## Surge iOS 的 URL Scheme

Surge iOS 支持 4 种动作和 1 个选项。

动作 (Action):

*   `surge:///start`

    使用选定的配置启动。

*   `surge:///stop`

    停止当前会话。

*   `surge:///toggle`

    使用选定的配置启动或停止。

*   `surge:///install-config?url=x`

    从 URL 安装配置。该 URL 应使用百分号编码。

选项 (Option):

*   `autoclose=true`

    动作完成后自动关闭 Surge。（不能与 `install-config` 一起使用）

    示例：`surge:///toggle?autoclose=true`

## x-callback-url

Surge 从 v3.4 开始支持 `x-callback-url` 规范。URL Scheme 为 `surge`，可用的动作为 `start`、`stop` 和 `toggle`。
