# HTTP 处理 (HTTP Processing)

Surge 包含了多个用于修改 HTTP 请求和响应的功能，其处理管线 (pipeline) 的顺序如下：

1.  URL 重写 (URL Rewrite)
2.  请求头重写 (Header Rewrite)
3.  请求体重写 (Body Rewrite)
4.  脚本处理 (Script Processing)

其中，脚本处理只能被一个脚本修改，而如果匹配到多个其他重写规则，它们将按顺序生效。
