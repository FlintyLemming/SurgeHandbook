# 信息面板 (Information Panel) 仅限 iOS 4.9.3+

Surge iOS 4.9.3 增加了一项实验性功能，允许用户自定义一个或多个信息面板以显示相关信息。

要访问此功能，用户需要拥有在 2021 年 9 月 22 日之后到期的有效订阅。如果订阅已过期且配置中仍有 Panel 字段，该面板将不会显示，但不会影响其他功能的正常使用。示例：

```ini
[Panel]
PanelA = title="Panel Title",content="Panel Content\nSecondLine",style=info
```

支持的 `style` 参数有 `good`、`info`、`alert`、`error`。

`PanelA` 是信息面板的名称，此参数将在脚本模式下传递给脚本。

### 静态模式 (Static mode)

上述配置生成的面板是静态的，它可以与托管配置或企业配置结合使用，以便在更新配置时更新面板内容，并为最终用户提供操作指南。

### 动态模式 (Dynamic Mode)

面板的内容可以通过脚本进行更新。

```ini
[Panel]
PanelB = title="Panel Title",content="Panel Content\nSecondLine",style=info,script-name=panel

[Script]
panel = script-path=panel.js,type=generic
```

新版本还引入了 `generic` 类型的脚本。当用户点击刷新按钮时，脚本将使用以下参数执行：

`$input : { purpose: "panel", position: "policy-selection", panelName: "PanelB" }, $trigger: "button" // 或 "auto-interval"`

脚本应在 `$done()` 中返回 `title`、`content` 和 `style` 字段。

在脚本首次执行之前，面板使用定义行中的静态内容。运行后，Surge 将自动缓存最后一次脚本的返回结果，并在执行刷新前始终显示最后一次脚本的结果。

脚本示例：

```javascript
$httpClient.get("https://api.my-ip.io/ip", function(error, response, data){
    $done({
        title: "External IP Address",
        content: data,
    });
});
```

此外，你还可以指定 `update-interval` 参数使面板自动更新。

```ini
[Panel]
PanelB = title="Panel Title",content="Panel Content\nSecondLine",style=info,script-name=panel,update-interval=60
```

自动更新仅在用户切换到策略选择视图时发生。因此，你可以在这里指定一个较小的时间（例如 1），使面板每次都自动更新。

## 更多自定义 (More customization)

*   当不传入 `style` 字段时，卡片将不显示图标，仅显示文本。
*   当不传入 `style` 字段时，可以传入 `icon` 字段以使用任何有效的 SF Symbol Name 自定义图标，例如 `bolt.horizontal.circle.fill`。
*   使用 `icon` 字段时，传入 `icon-color` 字段以控制图标的颜色，该值为颜色的 HEX 代码。
