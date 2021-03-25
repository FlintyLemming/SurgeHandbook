# 计划任务

在指定时间执行 cron 脚本。cronexp 应该是一个 cron 表达式，它是一个由五个或六个子表达式（字段）组成的字符串，描述了计划的各个细节。

> 译者按：Surge 兼容五位和六位的表示方法。

一些 cron 表达式的样例如下：

* at 2am daily: `0 2 * * *`
* at 5 AM and 5 PM daily: `0 5,17 * * *`
* on every minutes: `* * * * *`
* on every Sunday at 5 PM: `0 17 * * sun`
* every 10 minutes: `*/10 * * * *`

只有一个传入参数： `$cronexp`。

脚本任务执行完毕后请调用 `$done()` 退出。

一个简单样例：

```javascript
// script = type=cron,cronexp="* * * * *",script-path=cron.js
$surge.setSelectGroupPolicy('Group', 'Proxy');
$done();
```
