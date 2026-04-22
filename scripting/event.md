### 事件 (event)

在指定事件发生时执行脚本。value 是事件的名称。目前仅支持一个事件：`network-changed`。

请调用 `$done()` 以完成执行。

#### 事件类型 (Event Types)

*   `network-changed`: 当系统网络发生变化时触发。

```javascript
// network-changed = script-path=network-changed.js,type=event,event-name=network-changed

$notification.post('DNS Update', $network.dns.join(', '));

$done();
```

*   `notification`: 当 Surge 显示通知时触发。即使通知所属的分类被关闭，脚本仍然可以获取该消息。

```javascript
// notification = script-path=notification.js,type=event,event-name=notification

console.log($event.data);

$done();
```
