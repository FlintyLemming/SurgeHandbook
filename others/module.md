# 模块
 > 部分翻译来源于：[Surge 官方社区 —— Module](https://community.nssurge.com/d/225-module)

Module（模块）是一系列设置的集合，可用于覆盖当前配置的部分设定，有非常多的使用场景：

- 微调不可编辑的配置的设定，如托管配置和企业配置。
- 快捷的在不同工作环境中切换，比如临时开启对所有域名的 MitM 并调整过滤器。
- 使用他人编写的模块以完成某些特定任务，比如，你的同事可以编写一个模块将应用的 API 请求重定向至测试服务器。
- 如果你在多个设备间使用了同一份配置，有可能需要根据设备的使用场景进行微调。模块的开启状态是保存于当前设备的，可以用于在不同设备间的差异性修改。

## 基本概念

模块相当于给当前配置打补丁，其优先级要高于配置本身的设置。

有三种模块：

- 内置模块：Surge 会预置一些模块，随着 Surge 自身更新。
- 本地模块：放置在配置文件目录中的 .sgmodule 文件。
- 安装的模块：从某个 URL 安装的模块。

你可以同时开启多个模块，模块的开启状态保存于当前设备，不会进行同步。切换配置也不影响模块的开启状态。

## 编写模块

模块的内容和标准配置基本一致，目前支持调整以下段：

- General, Replica, MITM

  - 直接覆盖原始值：`key = value`
  - 在原始值的末尾进行追加：`key = %APPEND% value`
  - 在原始值的开始进行插入：`key = %INSERT% value`

  你只能在 MITM 段中操作 hostname 字段。

- Rule, Script, URL Rewrite, Header Rewrite, Host

  新加入的定义将会追加在原始内容的顶部。

  模块中的规则只可以使用 DIRECT、REJECT、REJECT-TINYGIF 三个内置策略。

- 元数据

  你可以在模块文件里添加元数据：

  ```
  #!name=Name Here
  #!desc=Description Here
  ```

  （可选）你还可以限制模块的使用平台（可取值为 ios 和 mac）：

  ```
  #!system=mac
  ```

## 模块样例：

```
#!name=MitM All Hostnames
#!desc=Perform MitM on all hostnames with port 443, except those to Apple and other common sites which can‘t be inspected. You still need to configure a CA certificate and enable the main switch of MitM.

[MITM]
hostname = -*.apple.com, -*.icloud.com, -*.mzstatic.com, -*.crashlytics.com, -*.facebook.com, -*.instagram.com, *
```

```
#!name=Game Console SNAT
#!desc=Let Surge handle SNAT conversation properly for PlayStation, Xbox, and Nintendo Switch. Only useful if Surge Mac acts the router for these devices.
#!system=mac

[General]
always-real-ip = %APPEND% *.srv.nintendo.net, *.stun.playstation.net, xbox.*.microsoft.com, *.xboxlive.com
```
