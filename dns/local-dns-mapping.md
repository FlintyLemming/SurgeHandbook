# 本地 DNS 映射

Surge 支持本地自定义的 DNS 映射。它相当于 `/etc/hosts`，但具有更强大的功能，包括通配符、别名以及分配 DNS 服务器。

```ini
[Host]
abc.com = 1.2.3.4
*.dev = 6.7.8.9
foo.com = bar.com
bar.com = server:8.8.8.8
```

## 通配符

你可以使用 `*` 前缀通配所有子域名。请注意 Surge 使用的是简单的字符串匹配。例如，`*google.com` 会匹配 `google.com`、`foo.google.com` 以及 `bargoogle.com`。而 `*.google.com` **不会**匹配 `google.com`。

```ini
[Host]
*.dev = 6.7.8.9
```

## 别名

就像 CNAME 记录一样。

```ini
[Host]
foo.com = bar.com
```

## 分配 DNS 服务器

你可以为单个或多个域名分配指定的 DNS 服务器。

```ini
[Host]
bar.com = server:8.8.8.8
```

由于 Surge 拥有自己的 DNS 客户端实现，某些主机名可能会解析失败。你可以使用 `server:system` 交由系统进行解析。

```ini
[Host]
Macbook = server:system
```

`server:syslib` 的工作原理类似，但它会将查询保留在 Surge 内部，同时将其转发给 macOS 当前正在使用的任何 DNS 服务器。这在增强模式下特别有用，因为在该模式下传统的系统解析器可能会被绕过。

默认情况下，所有带有 `.local` 后缀的主机名都将由系统解析。

## 引用规则集 Mac 5.10.0+

当你已经维护了大型的规则集或域名集时，在 `[Host]` 中重现相同的列表是很繁琐的。Surge 允许将整个 `DOMAIN-SET` 或 `RULE-SET` 绑定到一个 DNS 映射条目，以便自动共享上游服务器或 IP 映射。

```ini
[Host]
DOMAIN-SET:https://example.com/domains.txt = server:https://doh.example.com/dns-query
RULE-SET:https://example.com/rules.txt = 10.0.0.10
```

`DOMAIN-SET:` 期望一个域名列表，而 `RULE-SET:` 可以包含混合的规则类型（DOMAIN、DOMAIN-SUFFIX、IP 范围等）。该语法遵循与[规则集 (Ruleset)](../rule/ruleset.md)中描述的相同的远程文件格式，并且特别有助于将 DOH/DOQ 的分配与受管列表保持一致。

## 代理请求也使用本地 DNS 记录

```ini
[General]
use-local-host-item-for-proxy=true
```

默认情况下，由于 Surge 总是使用域名发送代理请求，因此 DNS 解析总是在远程代理服务器上进行。

启用该选项后，对于符合本地 DNS 映射记录的请求，Surge 会使用本地的 IP 地址而不是原始域名发送代理请求。

该选项仅对使用 IP 地址的本地 DNS 映射记录有效。
