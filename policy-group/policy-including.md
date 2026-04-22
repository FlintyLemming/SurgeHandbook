# 包含策略 (Policy Including)

## 包含外部策略

策略组可以导入在外部文件或从 URL 中定义的策略。

`egroup = select, policy-path=proxies.txt`

该文件包含策略列表，就像主配置文件中的定义行一样。

```ini
Proxy-A = https, example1.com, 443
Proxy-B = https, example2.com, 443
```

#### `update-interval`: 可选，单位为秒

更新间隔。仅当路径是 URL 时有意义。

#### `policy-regex-filter`: 可选

仅使用策略名称与正则表达式匹配的策略。

#### `external-policy-modifier`: 可选

你可以使用此参数修改外部策略的参数。

例如，开启 TFO 并更改测试 URL：

```ini
external-policy-modifier="test-url=http://apple.com/,tfo=true"
```

### `external-policy-name-prefix`: 可选

为这个外部策略组中子策略的策略名增加前缀，以便于同时使用多个不同的外部策略组时区分。

## 包含现有策略 iOS 4.12.0+ Mac 4.5.0+

你可以使用 `include-all-proxies` 和 `include-other-group` 来包含所有代理，或者重用来自另一个组的现有定义。

#### `include-all-proxies`

参数 `include-all-proxies=true` 包含 `[Proxy]` 段落中定义的所有代理策略，并可以与 `policy-regex-filter` 参数一起使用来进行过滤。

#### `include-other-group`

参数 `include-other-group="group1,group2"` 包含来自另一个策略组的策略，并且可以包含多个以逗号分隔的策略组。它也可以与 `policy-regex-filter` 参数一起使用来进行过滤。

*   允许在一个策略组中同时使用 `include-all-proxies`、`include-other-group` 和 `policy-path` 参数。`policy-regex-filter` 参数适用于这三者。
*   对于 `include-other-group` 参数引入的策略组之间存在优先级顺序，但在 `include-all-proxies`、`include-other-group` 和 `policy-path` 之间不存在优先级顺序。对于子策略顺序有意义的场景（如 fallback 组），请使用包含 `include-other-group` 的策略组嵌套。
