# 本地 DNS 映射

Surge 支持本地自定义 DNS 映射。这跟你编辑系统的 /etc/hosts 文件是一样的，但功能更强大。

```text
[Host]
abc.com = 1.2.3.4
*.dev = 6.7.8.9
foo.com = bar.com
bar.com = server:8.8.8.8
```

## 通配符

你可以像下面这样使用像 \* 这样的通配符匹配整个域名。但这个 \* 能匹配一整个不被 . 分隔的字符串，所以使用时请注意。例如，\*google.com 可以匹配 google.com、foo.google.com 和 bargoogle.com。而 \*.google.com 则**不能**匹配 google.com。

```text
[Host]
*.dev = 6.7.8.9
```

## 同义名

这类似于 CNAME 类型的解析。

```text
[Host]
foo.com = bar.com
```

## 关联 **DNS Server**

你可以给一个或多个域名指定想要的 DNS 服务器。

```text
[Host]
bar.com = server:8.8.8.8
```

由于 Surge 有自己的 DNS 客户端实现方式，所以一些主机名可能无法解析。您可以使用 server:system 让系统解析。

```text
[Host]
Macbook = server:system
```

默认情况下 .local 的域名都是由系统自己解析的。

## 组合使用

上面说的几个使用方法可以组合使用。

```text
[Host]
*.dev = foo.com
*.bar.com = server:system
```

