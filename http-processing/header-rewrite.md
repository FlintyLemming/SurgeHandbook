# 请求头重写 (Header Rewrite)

Surge 可以在客户端发送的请求头或响应头被转发给服务器之前对其进行重写。

示例：

```ini
[Header Rewrite]
http-request ^http://example.com header-add DNT 1
http-request ^http://example.com header-del Cookie
http-request ^http://example.com header-replace User-Agent Unknown
http-response ^http://example.com header-replace-regex Date 2022 2023
```

重写规则由几个部分组成：

1.  HTTP 走向：`http-request` 或 `http-response`。旧版本仅支持修改请求，因此这部分可以省略，如下所示：

    ```ini
     [Header Rewrite]
     ^http://example.com header-add DNT 1
    ```

2.  URL 正则表达式
3.  动作类型
4.  请求头字段名 (Header field)
5.  值 (不适用于 header-del)
6.  替换模板 (仅适用于 header-replace-regex)

### header-add

向请求头中追加一个新的标头行，即使该标头字段已经存在。

示例：

```ini
[Header Rewrite]
http-request ^http://example.com header-add DNT 1

修改前:
GET /index.html HTTP/1.1
Host: example.com
Connection: keep-alive
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.1 Safari/603.1.30
Accept-Language: en-us
Accept-Encoding: gzip, deflate

修改后:
GET /index.html HTTP/1.1
Host: example.com
Connection: keep-alive
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.1 Safari/603.1.30
Accept-Language: en-us
Accept-Encoding: gzip, deflate
DNT: 1
```

### header-del

从请求头中删除某一行。

示例：

```ini
[Header Rewrite]
http-request ^http://example.com header-del DNT

修改前:
GET /index.html HTTP/1.1
Host: example.com
Connection: keep-alive
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.1 Safari/603.1.30
Accept-Language: en-us
Accept-Encoding: gzip, deflate
DNT: 1

修改后:
GET /index.html HTTP/1.1
Host: example.com
Connection: keep-alive
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.1 Safari/603.1.30
Accept-Language: en-us
Accept-Encoding: gzip, deflate
```

### header-replace

替换请求头中的值。如果该标头字段不存在，则不执行任何操作。

示例：

```ini
[Header Rewrite]
http-request ^http://example.com header-replace DNT 1

修改前:
GET /index.html HTTP/1.1
Host: example.com
Connection: keep-alive
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.1 Safari/603.1.30
Accept-Language: en-us
Accept-Encoding: gzip, deflate
DNT: 0

修改后:
GET /index.html HTTP/1.1
Host: example.com
Connection: keep-alive
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.1 Safari/603.1.30
Accept-Language: en-us
Accept-Encoding: gzip, deflate
DNT: 1
```

如果你希望在字段存在时添加或替换一条标头行，你可以同时使用 header-add 和 header-del。

```ini
[Header Rewrite]
^http://example.com header-del DNT
^http://example.com header-add DNT 1
```

### header-replace-regex

使用正则表达式和模板替换请求头中的值。如果该标头字段不存在，则不执行任何操作。

```ini
[Header Rewrite]
http-request ^http://example.com header-replace-regex User-Agent Safari Chrome

修改前:
GET /index.html HTTP/1.1
Host: example.com
Connection: keep-alive
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.1 Safari/603.1.30
Accept-Language: en-us
Accept-Encoding: gzip, deflate
DNT: 0

修改后:
GET /index.html HTTP/1.1
Host: example.com
Connection: keep-alive
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_4) AppleWebKit/603.1.30 (KHTML, like Gecko) Version/10.1 Chrome/603.1.30
Accept-Language: en-us
Accept-Encoding: gzip, deflate
DNT: 0
```
