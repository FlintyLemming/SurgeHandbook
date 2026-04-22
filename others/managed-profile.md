# 托管配置 (Managed Profile)

Surge 可以从一个 URL 自动更新配置。如果配置文本以如下行开头：

`#!MANAGED-CONFIG http://test.com/surge.conf interval=60 strict=true`

配置只能在 Surge 主应用运行时进行更新。

请确保新的远程配置也包含 `#!MANAGED-CONFIG` 行。如果没有，该配置将恢复为标准配置。

### 参数

#### interval: 可选，单位为秒 (默认值: 86400秒)

设置配置的更新间隔。请注意，这是触发更新的最短时间；Surge 不一定在达到此时间后立即触发更新。

#### strict: true 或 false (默认值: false)

如果 strict 为 true，Surge 将在间隔时间到达后强制要求更新。否则，如果更新失败，用户仍然可以使用过时的配置。

> 注意：即使 strict 为 true，用户仍然可以通过小组件或系统设置中的 VPN 开关启动 Surge。

### REQUIREMENT 语句

`!REQUIREMENT` 语句可以用在配置行的开头或结尾，以限制该行仅在特定环境下生效，例如：

`#!REQUIREMENT CORE_VERSION>=22 Group = smart, policyA, policyB`

或者

`Group = url, policyA, policyB //!REQUIREMENT CORE_VERSION<22`

可用于条件的变量包括 `CORE_VERSION, SYSTEM, SYSTEM_VERSION, DEVICE_MODEL, LANGUAGE`。

可用的运算符有 `=,==,>=,=>,<=,=<,>,<,!,<>,AND,&&,OR,||,NOT,!,BEGINSWITH,CONTAINS,ENDSWITH,LIKE,MATCHES`。

一个典型的变量值示例：

```yaml
CORE_VERSION: 22
SYSTEM: iOS
SYSTEM_VERSION: System Version 17.4.1 (Build 21E236)
DEVICE_MODEL: iPhone16,1
LANGUAGE: en-US
```

#### Core Version

当 Surge 发布新功能时，通常会向配置中引入新语法，相应的 Core Version 也会增加。这个版本可以用来判断某个功能是否可用，版本号及主要变更如下：

*   22: Surge Mac 5.7.0, Surge iOS 5.11.0, Smart Group (智能组)
*   20: Surge Mac 5.6.0, Surge iOS 5.10.0, Body Rewrite (请求体重写), Inline Map Local (内联映射本地)

在表达式中使用字符串时，字符串应用 `'` 包裹。当表达式包含空格时，应使用 `""` 包裹。

`#!REQUIREMENT "CORE_VERSION>=22 AND SYSTEM=='iOS'" Group = smart, policyA, policyB`

在 UI 中修改配置时会丢失表达式，因此此功能主要用于托管配置和企业配置。

由于早于 Surge iOS 5.11.0 和 Mac 5.7.0 的版本不支持此表达式，因此提供了行首和行尾两种注释方法，允许灵活地支持旧版本。例如，如果您想为支持 Smart Group 的客户端使用它，你可以写：

```ini
#!REQUIREMENT CORE_VERSION>=22 Group = smart, policyA, policyB
Group = url-test, policyA, policyB //!REQUIREMENT CORE_VERSION<22
```

由于第一行在旧版本中被视为纯注释，因此不会生效，第二行的行尾注释也将被视为普通注释。只有第二行会生效。

### FORBIDDEN-AUTO-UPGRADE 语句

从 Surge iOS 5.11.0 和 Mac 5.7.0 版本开始，Surge 可以自动为升级优化配置，以避免因托管配置未及时调整而无法使用最新功能。

如果出于某些特殊原因，你不希望配置应用某些自动优化，可以使用 `FORBIDDEN-AUTO-UPGRADE` 表达式，例如：

`#!FORBIDDEN-AUTO-UPGRADE smart-group`

目前可用的优化关键字包括：

*   `smart-group`：自动将 `url-test/load-balance` 组升级为 `smart` 组。
