# DNS 服务器

你可以使用这些选项去覆盖系统的 DNS 设置。

```
[General]
dns-server = 8.8.8.8, 8.8.4.4

```

可以用 system 来代表系统本身的 DNS 设置（其中，与后面重复的 DNS 服务器将被忽略）。

```
[General]
dns-server = system, 8.8.8.8, 8.8.4.4
```