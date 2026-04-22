# 配置文件

Surge 的核心功能由配置文件控制。基本上，配置文件的所有内容都可以通过用户界面进行调整。但部分实验性功能可能尚未提供配置视图。当你遇到一些特殊需求时，可能需要手动编辑配置文件来实现。

### 配置文件内容

配置文件的格式遵循 INI 文件的格式，带有 `[Section]` 段落，用于划分不同的段落并分隔设置。

每个段落的配置行都有其特定的语法，例如 `[General]` 和 `[MITM]` 段落仅仅是 `key = value` 的形式：

```ini
[General]
key = value
```

在这些段落中，配置行的顺序没有任何影响。但是，在诸如 `[Rule]` 这样的段落中，配置行的顺序非常重要。

### 配置文件分类

配置文件分为三类：

1.  普通配置文件：手动创建或默认使用的配置。
2.  托管配置文件：通常由企业管理员或服务提供商提供。托管配置文件无法在本地修改，因为它可以被远程更新。如果你想进行更改，应先创建一个副本，将其转换为普通配置。
3.  企业配置文件：仅限企业版，不能被修改或查看，也不能被复制。

### 分离的配置段落 (Detached Profile Section)

为了满足各种复杂的使用场景，Surge 支持将配置文件中的某个段落分离到另一个文件中。

例如：

Main.conf

```ini
[Proxy]
#!include Proxy.dconf
```

其中引用的另一个文件必须包含对应段落的 `[]` 声明。因此，该文件可以是仅包含部分段落（一个或多个）的文件，也可以是一个完整的配置文件。

Proxy.dconf

```ini
[Proxy]
ProxyA = http, 1.2.3.4, 80
```

使用此功能，你可以：

1.  引用托管配置的 `[Proxy]`、`[Proxy Group]`、`[Rule]` 段落，并自行编写其他段落。这使你可以享受托管配置中代理相关内容的更新，而不会影响通过 UI 调整的其他功能。
2.  在多个配置之间共享特定段落的内容。例如，在 iOS 和 macOS 上同时使用 Surge 时，`[Proxy]`、`[Proxy Group]`、`[Rule]` 等段落的内容往往是相同的，但 `[General]` 的内容可能差异很大。你可以创建两个配置 iOS.conf 和 macOS.conf，并将重复的段落放在另一个文件中。

```ini
[Proxy]
#!include Forwarding.dconf

[Proxy Group]
#!include Forwarding.dconf

[Rule]
#!include Forwarding.dconf
```

这样，在 iOS 上调整 `[General]` 段落时，就不会影响 macOS，并避免了维护两套代理配置的麻烦。这也完全不会干扰使用 UI 的配置过程。

一些额外说明：

*   通过 UI 修改配置后，配置会根据 include 语句写入到对应的分离配置文件中。如果文件中包含未被引用的其他段落，写入操作将只会修改被引用的段落。
*   如果引用的是托管配置文件，则该段落相关的配置无法被编辑，但不影响调整其他段落。
*   文件名的后缀没有硬性要求，如果它是一个完整的配置文件，你可以继续使用 conf 后缀；如果不是完整的配置文件，建议使用其他后缀以避免在配置列表中显示。
*   从 Surge iOS 4.12.0 和 Surge Mac 4.5.0 开始，你可以在一个段落中包含多个分离的配置文件。但该段落会被标记为只读，无法使用 UI 编辑。

    ```ini
    [Proxy]
    #!include A.dconf, B.dconf
    ```

#### 链接配置 (Linked Profiles) Mac 6.0.0+

`#!include` 也可以直接引用远程托管配置文件（一个 URL）。这让你可以构建一个本地的“覆盖 (overlay)”配置，该配置将保持追踪上游配置的更新。

```ini
[Rule]
#!include https://example.com/managed.conf
```

当引用的内容是只读的时，如果你尝试编辑这些段落，Surge 会提示创建一个链接层 (linked layer)。本地层仅存储你的覆盖设置，而远程托管配置文件则自动接收更新。

### 模块 (Modules)

分离的配置段落功能用于将单个配置文件拆分为多个文件，而模块是配置文件的补丁，每个模块文件用于调整配置文件的各个部分，以实现特定任务。

模块可以：

*   灵活地开启和关闭。
*   调整同一文件中的多个段落。
*   通过 URL 安装并保持更新。

但是：

*   模块不能调整 `[Proxy]`、`[Proxy Group]`、`[Rule]` 段落的内容。
*   模块不能调整 MITM 的 CA 证书。
*   模块的设置会覆盖主配置，因此无法通过 UI 进行调整。

模块的详细说明见：[https://manual.nssurge.com/others/module.html](../others/module.md)

### 注释 (Comment)

Surge 配置文件支持注释行，以 `#`、`;` 和 `//` 开头。也支持行内注释。

```ini
# 这是一个注释行
; 这是一个注释行
// 这是一个注释行
```

```ini
dns-server = 8.8.8.8 // 这是一个行内注释
dns-server = 8.8.8.8 # 这是一个行内注释
dns-server = 8.8.8.8 ; 这是一个行内注释
```

请注意，使用行内注释时，分隔符前必须至少有一个空格。

### 行条件 (Line Requirement) iOS 5.11.0+ Mac 5.7.0+

你可以设置约束，使得某一行配置仅在满足特定条件时生效。

```ini
Group = url, policyA, policyB #!REQUIREMENT CORE_VERSION<22
```

对于原生支持开启/关闭状态配置的项，不满足条件的配置行将等同于处于禁用状态。对于其他行，它们将等同于行注释。

`#!REQUIREMENT` 表达式也可以在行首使用。由于以前的版本不支持此表达式，因此提供了行首和行尾两种格式。你可以灵活利用这一机制来兼容旧版本。例如，如果希望支持智能组 (Smart groups) 的客户端使用智能组，你可以这样写：

```ini
#!REQUIREMENT CORE_VERSION>=22 Group = smart, policyA, policyB
Group = url, policyA, policyB //!REQUIREMENT CORE_VERSION<22
```

由于在旧版本中第一行将被视为简单的注释，因此不会有任何效果，而第二行的行尾注释也只会作为常规注释处理。

#### REQUIREMENT 表达式

可用于判断的变量包括 `CORE_VERSION`、`SYSTEM`、`SYSTEM_VERSION`、`DEVICE_MODEL`、`LANGUAGE`。

可以使用的运算符有 `=,==,>=,=>,<=,=<,>,<,!=,<>,AND,&&,OR,||,NOT,!,BEGINSWITH,CONTAINS,ENDSWITH,LIKE,MATCHES`。

变量值的典型示例：

```
CORE_VERSION: 22
SYSTEM: iOS
SYSTEM_VERSION: System Version 17.4.1 (Build 21E236)
DEVICE_MODEL: iPhone16,1
LANGUAGE: en-US
```

表达式中的字符串应使用 `''` 括起来，例如 `#!REQUIREMENT SYSTEM=='macOS'`

##### 简写标记 iOS 5.14.3+ Mac 5.10.0+

为了方便，提供了三种简写标记：`#!IOS-ONLY`、`#!MACOS-ONLY`、`#!TVOS-ONLY`。

例如：

```ini
DOMAIN,reject.com,REJECT #!MACOS-ONLY
```
