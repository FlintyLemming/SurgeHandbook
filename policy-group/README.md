# 策略组 (Policy Group)

策略组可以包含多个策略。它可以是代理策略、另一个策略组，或者是内置策略。

策略组的存在是为了在应用代理规则时，能够灵活地调整具体使用的策略，而不需要修改规则本身。

有几种策略组类型：`select`、`url-test`、`fallback`、`load-balance` 和 `subnet`。策略组应在 `[Proxy Group]` 段落中声明。
