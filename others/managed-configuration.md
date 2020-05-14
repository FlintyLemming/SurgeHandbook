# 托管配置

如果配置文件以下面内容作为文件的开始，则 Surge 会自动的从 URL 更新配置文件。

```text
#!MANAGED-CONFIG http://test.com/surge.conf interval=60 strict=true
```

配置文件仅在 Surge 主程序运行时更新。

> 注意：新版的托管配置一定包含`#!MANAGED-CONFIG`，否则会视为普通配置文件。

## 参数

* interval: 可选，单位秒（默认值：86400s）  

  更新配置文件的间隔时间。

* strict: 可选值 true 或 false （默认值：false）  

  当设置为 true 时，则会在间隔时间到达后，强制更新。如果更新失败用户则会使用过时的配置文件。

  > 注意: 即使 strict 设置为 true，用户仍可以通过小组件和 VPN 开关启动 Surge。

