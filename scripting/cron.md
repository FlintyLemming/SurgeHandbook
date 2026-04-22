### 计划任务 (cron)

在指定的时间执行脚本。该值应为 cron 表达式，这是一个由五或六个子表达式（字段）组成的字符串，用于描述排程的具体细节。

一些 cron 表达式示例：

*   每天凌晨 2 点: `0 2 * * *`
*   每天早上 5 点和下午 5 点: `0 5,17 * * *`
*   每分钟: `* * * * *`
*   每秒: `* * * * * *`
*   每个星期日下午 5 点: `0 17 * * sun`
*   每 10 分钟: `*/10 * * * *`

只有一个传入参数：`$cronexp`。

请调用 `$done()` 以完成执行。

一个简单的例子：

```javascript
// cron "0 2 * * *" script-path=cron.js
$surge.setSelectGroupPolicy('Group', 'Proxy');
$done();
```

### 附加 API (Additional API)

*   `$cronexp`: cron 表达式字符串。
