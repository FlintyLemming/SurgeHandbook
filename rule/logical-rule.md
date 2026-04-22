# 逻辑规则 (Logical Rule)

使用逻辑操作符规则将多个规则组合起来以应对复杂场景。你可以将逻辑规则嵌套在另一个逻辑规则中。

#### AND 规则

如果所有子规则都匹配，则触发该规则。

```ini
AND,((#Rule1), (#Rule2), (#Rule3)...),Policy
```

示例：

```ini
AND,((SRC-IP,192.168.1.110), (DOMAIN, example.com)),DIRECT
```

#### OR 规则

如果任意子规则匹配，则触发该规则。

`OR,((#Rule1), (#Rule2), (#Rule3)...),Policy`

示例：

```ini
OR,((SRC-IP,192.168.1.110), (SRC-IP,192.168.1.111)),DIRECT
```

#### NOT 规则

反转原始规则的评估结果。

```ini
NOT,((#Rule1)),Policy
```

示例：

```ini
AND,((NOT,((SRC-IP,192.168.1.110))),(DOMAIN, example.com)),DIRECT
```
