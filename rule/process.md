# 进程规则

你可以写能够匹配软件进程的规则，该规则只在 Surge macOS 版生效，iOS 版会自动忽略这个类型的规则。

## PROCESS-NAME

    PROCESS-NAME,Telegram,Proxy

规则会匹配这个进程名的程序，支持 `*` 和 `?` 两种通配符。

> 你也可以把该进程名的所在目录写清楚。至于如何找到这个名称，对于 macOS 软件包，一般在 .app/Contents/MacOS 位置下

