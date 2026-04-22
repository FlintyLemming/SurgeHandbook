# 模块 (Module)

模块是一组用于覆盖当前配置的设置。你可以使用模块来：

*   调整不可编辑配置（如托管配置和企业配置）中的设置。
*   一键更改部分设置。例如，你可以使用模块为所有主机名开启 MitM，并临时调整过滤器。
*   使用别人编写的模块来完成特定任务。例如，你的同事可能与你分享了一个将 API 请求重写为测试服务器的模块。
*   当你在多台设备之间共享同一个配置时，针对不同的场景可能需要修改某些设置。模块的启用状态不会同步到其他设备，因此你可以使用模块来满足这一需求。

### 基本概念

模块就像是对当前配置的补丁 (patch)。模块中的设置优先级高于配置中的设置。

共有 3 种类型的模块：

*   内部模块 (Internal Modules)：由 Surge 本身提供。
*   本地模块 (Local Modules)：放置在配置目录中的 `.sgmodule` 文件。
*   已安装模块 (Installed Modules)：通过 URL 安装的模块。

### 编写模块

模块的语法与配置相同。允许你覆盖以下段落：

*   `[General]`, `[MITM]`

    *   覆盖：`key = value`
    *   追加至原始值：`key = %APPEND% value`
    *   在原始值最前面插入：`key = %INSERT% value`

        你只能在 `[MITM]` 段落中操作 `hostname`、`skip-server-cert-verify` 和 `tcp-connection` 字段。

        > 用于 HTTP 抓包功能的旧版 `[Replica]` 段落在 Surge Mac 5.4.0 中已被移除，因此模块不再需要对其进行补丁。

*   `[WireGuard *]` 段落

    WireGuard 策略存在于名称以 `WireGuard` 开头的段落中。模块可以像覆盖主配置一样，覆盖或追加这些段落内部的键值。

*   `[Ruleset *]` 段落

    当你定义内联规则集 (inline rulesets) 时，模块现在也可以对它们进行补丁，这对于提供内联列表的托管配置非常有用。

*   Rule, Script, URL Rewrite, Header Rewrite, Host

    新行将插入到原始内容的顶部。

    模块中的规则只能使用内部策略：`DIRECT`、`REJECT` 和 `REJECT-TINYGIF`。

*   Metadata (元数据)

    你可以在模块文件中添加元数据：

    ```ini
      #!name=模块名称
      #!desc=模块的描述
    ```

    你可以将模块限制为仅在指定平台生效。（可选）

    ```ini
      #!system=mac
    ```

### 示例：

```ini
#!name=MitM All Hostnames
#!desc=对所有 443 端口的主机名执行 MitM，排除苹果和其他无法被检查的常见站点。你仍然需要配置 CA 证书并打开 MitM 的总开关。

[MITM]
hostname = -*.apple.com, -*.icloud.com, -*.mzstatic.com, -*.crashlytics.com, -*.facebook.com, -*.instagram.com, *
```
```ini
#!name=Game Console SNAT
#!desc=让 Surge 正确处理 PlayStation、Xbox 和 Nintendo Switch 的 SNAT 对话。仅当 Surge Mac 作为这些设备的路由器时才有用。
#!system=mac
[General]
always-real-ip = %APPEND% *.srv.nintendo.net, *.stun.playstation.net, xbox.*.microsoft.com, *.xboxlive.com
```

### 参数表 Mac 5.5.0+

使用 `#!arguments` 元数据来声明用户在启用模块时可以自定义的参数。其语法遵循标准的查询字符串 (query-string)：

```ini
#!arguments=hostname=example.com&enable_mitm=true
```

每个键 (key) 会成为可用的占位符，如 `%hostname%`、`%enable_mitm%` 等，Surge 会在应用模块之前进行简单的文本替换。在 `#!arguments` 中定义的默认值会在 UI 中显示，并与该模块实例一起保存。

> 请保持占位符为字母数字组合（例如，`%SERVER_HOST%`），以避免与配置语法发生冲突。删除参数时请移除未使用的占位符。

### 运行环境要求 (Requirements) iOS 5.10.0+ Mac 5.6.0+

模块添加了 `#!requirement=` 声明，以支持更复杂的使用条件限制。例如，如果模块使用了新加的 Body Rewrite 功能，它需要限制 Surge 核心版本。

`#!requirement=CORE_VERSION>=20`

它也支持逻辑表达式，例如：`CORE_VERSION>=20 && (SYSTEM = 'iOS' || SYSTEM = 'tvOS')`。

可用于判断的变量如下：

*   CORE\_VERSION: 数字，例如 `20`
*   SYSTEM: 字符串，例如 `macOS`、`iOS`、`tvOS`
*   SYSTEM\_VERSION: 字符串，例如 `Version 17.4.1 (Build 21E236)`
*   DEVICE\_MODEL: 字符串，例如 `Mac15,8`
*   LANGUAGE: 字符串，例如 `zh-Hans`
