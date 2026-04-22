# 进程规则 (Process Rule)

你可以为指定的进程分配策略。进程规则仅适用于 Surge Mac，Surge iOS 会忽略这些规则。

#### PROCESS-NAME (仅限 Mac)

```ini
PROCESS-NAME,Telegram,Proxy
```

如果请求的进程名称匹配，则触发该规则。支持通配符 `*` 和 `?`。

> 你可以指定可执行文件的文件名或完整路径。对于 macOS 应用程序包，它位于 `.app/Contents/MacOS` 路径下。
