# Surge Mac 命令行 (Surge Mac CLI)

Surge Mac 提供了一个简单的 CLI 程序，以便于控制。你可以在 `/Applications/Surge.app/Contents/Applications/surge-cli` 找到它。

使用 `--help` 可以获取最新的手册。

```
可用命令：
  reload - 重新加载主配置
  switch-profile <profile-name> - 切换到另一个配置

  stop - 关闭 Surge
  unattended-upgrade - 如果有可用更新，则执行无人值守的 Surge 升级

  dump active - 显示所有活动连接
  dump request - 显示最近的连接
  dump rule - 显示所有生效的规则
  dump policy - 显示所有代理和策略组
  dump dns - 显示 DNS 缓存
  dump profile [original / effective] - 显示原始配置和被模块修改后的有效配置
  dump event - 显示事件

  watch request - 持续跟踪新请求

  environment - 显示环境变量设置
  set <key-path> <value> - 修改环境变量设置

  test-network - 测试网络延迟
  test-policy <policy-name> - 测试一个代理
  test-all-policies - 测试所有代理
  test-group <group-name> - 立即重新测试一个策略组

  kill <connection-id> - 终止一个活动连接
  flush dns - 清除 DNS 缓存
  diagnostics - 运行网络诊断
  set-log-level <log-level> - 更改日志级别而不写入配置文件

  script evaluate <script-js-path> [mock-script-type] [timeout] - 从文件加载脚本并执行

可用参数：
  --raw - 以原始 JSON 格式输出结果
  --remote/-r - 连接到远程 Surge 实例而不是本地。例如：--remote password@192.168.2.2:6170
```
