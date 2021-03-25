# 事件

在发生特定事件时执行脚本，该类型下第二参数为事件名称，目前只有 network-changed 一个事件。

脚本任务执行完毕后请调用 `$done()` 退出

一个简单样例

```javascript
// script = type=event,event-name=network-changed,control-api=1,script-path=event.js
$notification.post('DNS Update', $network.dns.join(', '));
$done();
```
