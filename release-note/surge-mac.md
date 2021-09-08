# Surge Mac Release Note

### **Version 4.1.0**

#### **脚本**
1. 你现在可以通过 UI 界面配置脚本了。

#### **配置文件**
1. 你现在可以把配置文件中的部分段落用单独的一个文件写。详情参考：https://surge.mitsea.com/overview/configuration。

#### **HTTP API**
1. 添加了关于配置文件的 HTTP API，包括 GET /profiles 和 POST /profiles/check。
2. 添加了关于设备管理的 HTTP API，包括 GET /devices、POST /devices 和 GET /devices/icon。
3. HTTP API、代理服务、外部控制器（external controller）现在都支持监听 IPv6 地址了（UI 界面尚无，需要手动在配置文件里配置）。
4. 你可以用 http-api-tls=true 来为 HTTP API 启用 TLS（HTTPS-API）。

#### **自动化改进**
1. 外部资源在 Surge 启动时就会更新，并且当应用一个配置文件时会自动下载其中引用的外部资源。

#### **其他改进**
1. 新的规则类型：SUBNET，可以用通配符匹配 SSID、BSSID 和 路由IP地址。
2. 当处理大量请求的时候，Dashboard 性能有显著提升。

#### **下载地址**
https://dl.nssurge.com/mac/v4/Surge-4.1.0-1298-f07b1b8713b2397518f4b252b5786452.zip

### **Version 4.0.5**

### **Policy Group**

In this release, we completely refactored the policy group functionality, bringing the following changes:

1. The url-test/fallback/load-balance policy group can no longer be configured with a specific testing URL but with a global testing URL or a policy-configured testing URL. The policy's test results can be used directly in all policy group decisions, eliminating the need to retest each policy group individually.
2. All types of policy groups support mixed nesting. The only requirement is that no circular references can be used.
3. When a group policy is used as a sub-policy of the url-test/fallback/load-balance group.
    - The latency of the select/url-test/fallback/ssid group is the latency of the selected policy.
    - The latency of the load-balance group is the average of the latencies of all available policies.
4. The timeout parameter of a policy group marks policies with latency exceeding this parameter as unavailable when making decisions for the group. But the maximum time taken to test the policy group is controlled by the global test-timeout parameter. (Default is 5s)
5. When testing a group due to decision making, all sub-policies that the group may use are tested, including sub-policies of the sub-policy group.
6. You may use no-alert=true parameter to suppress notifications for particular groups.

### **Cloud Notification**

You can receive the notifications on iOS devices. Enable this option first and then configure it on Surge iOS. The two device must use a same iCloud account.

### **Minor Changes**

- Bug fixes.

[https://dl.nssurge.com/mac/v4/Surge-4.0.5-1262-db70f680cd0f15236c8415ec7b804c3a.zip](https://dl.nssurge.com/mac/v4/Surge-4.0.5-1262-db70f680cd0f15236c8415ec7b804c3a.zip)

### **Version 4.0.4**

- Bug fixes.

[https://dl.nssurge.com/mac/v4/Surge-4.0.4-1227-9acb8b9e3f39e9048fc82e427184a4af.zip](https://dl.nssurge.com/mac/v4/Surge-4.0.4-1227-9acb8b9e3f39e9048fc82e427184a4af.zip)

### **Version 4.0.3**

- You may override the testing URL of a policy for network diagnostics and activity cards.
- The GeoIP database can be updated automatically in the background.
- Bug fixes.

[https://dl.nssurge.com/mac/v4/Surge-4.0.3-1224-4ef8ae10c8a74c395bb4b6c3f6af6af6.zip](https://dl.nssurge.com/mac/v4/Surge-4.0.3-1224-4ef8ae10c8a74c395bb4b6c3f6af6af6.zip)

### **Version 4.0.2**

- You may now customize the GeoIP database updating URL.
- tun-excluded-routes and tun-included-routes are now available for Surge Mac.
- Bug fixes.

[https://dl.nssurge.com/mac/v4/Surge-4.0.2-1219-dbd08724b90aa8b444cd6d0679a245b5.zip](https://dl.nssurge.com/mac/v4/Surge-4.0.2-1219-dbd08724b90aa8b444cd6d0679a245b5.zip)

### **Version 4.0.1**

- You may configure the proxy chain with the UI now.
- Fixed some visual inconsistency under reducing transparency mode.
- Bug fixes.

[https://dl.nssurge.com/mac/v4/Surge-4.0.1-1207-ee7bea1b244950c82a6f90e060fa2d89.zip](https://dl.nssurge.com/mac/v4/Surge-4.0.1-1207-ee7bea1b244950c82a6f90e060fa2d89.zip)

### **Version 4.0.0**

- The first version of 4.0.0.

[https://dl.nssurge.com/mac/v4/Surge-4.0.0-1191-d8140b0084223fd3fc4335e4414c0884.zip](https://dl.nssurge.com/mac/v4/Surge-4.0.0-1191-d8140b0084223fd3fc4335e4414c0884.zip)

## **Surge Mac V3**

### **Version 3.5.8**

- Bug fixes

[https://dl.nssurge.com/mac/v3/Surge-3.5.8-1130.zip](https://dl.nssurge.com/mac/v3/Surge-3.5.8-1130.zip)

### **Version 3.5.7**

- Bug fixes

[https://dl.nssurge.com/mac/v3/Surge-3.5.7-1129.zip](https://dl.nssurge.com/mac/v3/Surge-3.5.7-1129.zip)

### **Version 3.5.5**

### **Minor Changes**

- All URL resources now support URLs with a username and password (e.g. [https://user:pass@example.com](https://user:pass@example.com/)), including managed profile, external resources, and importing profile form URL.
- You may switch among the main views with shortcut keys.
- Bug fixes.

[https://dl.nssurge.com/mac/v3/Surge-3.5.5-1123.zip](https://dl.nssurge.com/mac/v3/Surge-3.5.5-1123.zip)

### **Version 3.5.4**

### **Changes in Policy Group**

- New parameter: policy-regex-filter. If the parameter is configured, only matched policy line will be used.

### **Minor Changes**

- Provides more details for the TLS handshake error.
- Increases the file description limitation alert threshold.

[https://dl.nssurge.com/mac/v3/Surge-3.5.4-1119.zip](https://dl.nssurge.com/mac/v3/Surge-3.5.4-1119.zip)

### **Version 3.5.3**

### **New Parameter: use-local-host-item-for-proxy**

`[General]`

`use-local-host-item-for-proxy = true`

If use-local-host-item-for-proxy is true, Surge sends the proxy request with the IP address defined in the [Host] section, instead of the original domain.

### **Changes in Load Balance Group**

- load-balance group now supports connectivity testing before being used. Add 'url' parameter to enable it.
- Parameters 'timeout', 'interval' and 'evaluate-before-use' are also available.

### **Minor Changes**

- Surge will send an ICMP port unreachable message if UDP forwarding fails.
- Eliminate unnecessary local DNS lookup while forwarding UDP traffic to a proxy server.
- Fixed a bug that connecting to Surge iOS via USB is not working in Surge Dashboard.

[https://dl.nssurge.com/mac/v3/Surge-3.5.3-1094.zip](https://dl.nssurge.com/mac/v3/Surge-3.5.3-1094.zip)

### **Version 3.5.2**

### **SSID Suspend**

- Surge Mac supports SSID suspend now. The system proxy and enhanced mode will be temporarily suspended under specified SSIDs.
- The name of WiFi can be an SSID, a BSSID, or a gateway IP address.
- No UI configuration in the current version.

### **REJECT-DROP**

- REJECT-DROP policy is now effective to proxy connections. The connections matched with a REJECT-DROP policy will be closed in 60-120s later without any data returned.

### **Global Proxy**

- You may now select and view sub-policy for policy groups while using the global proxy mode.

[https://dl.nssurge.com/mac/v3/Surge-3.5.2-1082.zip](https://dl.nssurge.com/mac/v3/Surge-3.5.2-1082.zip)

### **Version 3.5.1**

### **New rule type: DOMAIN-SET**

- DOMAIN-SET is just like RULE-SET. But it is designed a large number of rules and highly efficient.
- Unlike RULE-SET, you can only write hostnames (domain or IP address) in it. One hostname per line.
- You may use "." prefix to include all sub-domains.

### **Changes in SRC-IP**

- SRC-IP rule now supports IP-CIDR for both IPv4 and IPv6.

### **Changes in DNS over HTTPS**

- From this version, if DNS-over-HTTPS is configured, the traditional DNS will only be used to test the connectivity and resolve the domain in the DOH URL.
- The DNS over HTTPS now has a separate parameter: doh-server. The DOH servers in 'dns-server' will be moved to the new parameter after saving.
- The legacy DNS is always required now.
- DOH can be matched with rule 'PROTOCOL,DOH' now.
- Added a new parameter 'doh-follow-outbound-mode'. In the previous version, the DOH client follows the system proxy settings. From this version, all DOH requests will use DIRECT policy by default. If 'doh-follow-outbound-mode' is set, the DOH requests will follow the outbound mode settings regardless of the system proxy settings.
- We are refactoring the HTTP client for DOH and scripting. Please feedback if you encounter any issue.

### **Changes in Scripting**

- Added a simple view to test the script. You may find it in the Window menu.

### **Minor Changes**

- Fixed a crash in Dashboard while using search.
- Bug fixes.

### **Known Issues**

- You may not configure DOH with UI in this version temporarily.

[https://dl.nssurge.com/mac/v3/Surge-3.5.1-1069.zip](https://dl.nssurge.com/mac/v3/Surge-3.5.1-1069.zip)

### **Version 3.5.0**

- New feature: Module, which can override the current profile with a set of settings. Highly flexible for diverse purposes. See the post in the community for more information: [https://community.nssurge.com/d/225-module](https://community.nssurge.com/d/225-module).
- You may enable modules in the menu now.
- You may view the detail of a module by double clicking.
- Supports pattern filter for Dashboard requests.
- Added a new rule type: PROTOCOL. The possible values are HTTP, HTTPS, SOCKS, SNELL, TCP, UDP.
- You may now use UI to add and edit load-balance group.
    - DNS over HTTP (DoH) now uses DNS wireformat by default. You may configure doh-format=json in [General] to continue using JSON format.
    - TCP connection setup optimizations.
    - Bug fixes.

[https://dl.nssurge.com/mac/v3/Surge-3.5.0-1039.zip](https://dl.nssurge.com/mac/v3/Surge-3.5.0-1039.zip)

### **Version 3.4.0**

- Snell protocol now upgrade to version 2, supporting to reuse TCP connections to improve performance. [https://github.com/surge-networks/snell/releases](https://github.com/surge-networks/snell/releases)
- Supports a new proxy protocol: Trojan.
- Remote Dashboard now upgraded to Remote Controller. You may use Surge iOS to select policy group, toggle HTTP capture/MitM, and switch outbound mode remotely.
- The comment lines in the text config won't lost after editing with UI.
- You may open the new connection window of Dashboard by holding the Option key while clicking the Dashboard item in the main menu.
- Supports to use OpenSSL as TLS provider. See the post in the community for more information: [https://community.nssurge.com/d/196-surge-ios-mac-tls-provider](https://community.nssurge.com/d/196-surge-ios-mac-tls-provider).
- Fixed a bug that Surge may not be able to process DNS answer packets which is longer than 512 bytes.

[https://dl.nssurge.com/mac/v3/Surge-3.4.0-989.zip](https://dl.nssurge.com/mac/v3/Surge-3.4.0-989.zip)

### **Version 3.3.3**

- Fixed a bug which causes TFO failed.
- You may use a profile which stores in a subdirectory of the profile directory.
- Added Traditional Chinese localizations.
- Fixed a bug that the menu might be unresponsive.
- Fixed crashs on macOS 10.11.

[https://dl.nssurge.com/mac/v3/Surge-3.3.3-939.zip](https://dl.nssurge.com/mac/v3/Surge-3.3.3-939.zip)

### **Version 3.3.2**

- Supports MITM on non-standard port for TCP mode.
- Proxy editing view now supports VMess protocol and all misc options.
- A new option 'persistent' has been added to the load-balance group. (aka PCC, per connection classifier) When 'persistent=true' is set, a same hostname will always get the same policy.
- Bug fixes.

[https://dl.nssurge.com/mac/v3/Surge-3.3.2-925.zip](https://dl.nssurge.com/mac/v3/Surge-3.3.2-925.zip)

### **Version 3.3.1**

- Supports VMess proxy protocol.
    - vmess-proxy= vmess, example.com, 443, username = 12345678-abcd-1234-1234-47ffca0ce229, ws=true, tls=true, ws-path=/v2, ws-headers=X-Header-1:value|X-Header-2:value
    - All proxy options for TLS proxy are available.
    - Web-socket and TLS options would degrade performance. Only enable when necessary.
    - Surge only supports chacha20-poly1305 encryption algorithm. Please make sure the server supports it. We have no plan to implement other ciphers.

[https://dl.nssurge.com/mac/v3/Surge-3.3.1-906.zip](https://dl.nssurge.com/mac/v3/Surge-3.3.1-906.zip)

### **Version 3.3.0**

- The scripting has been rewritten totally. The old scripts are not compatible with this version. See [https://community.nssurge.com/d/33-scripting/3](https://community.nssurge.com/d/33-scripting/3)
- Added support for TLS 1.3. Append 'tls13=true' to the proxy line to enable it. (Requires macOS 10.14 or above)
- Supports to use 'X-Surge-Policy' to force policy for HTTP/HTTPS requests.
- SSID group now supports to use the IP address of default router as an identifier.
- New policy group type: load-balance, which will use a random sub-policy for every request.
- Supports DNS over HTTPS. More information in community: [https://community.nssurge.com/d/48-dns-over-http](https://community.nssurge.com/d/48-dns-over-http)
- IN-PORT and DEST-PORT rule now supports port range expression: `DEST-PORT,8000-8999,DIRECT`
- Provides compatibility for Surge iOS 4.
- Surge Mac software package is now notarized by Apple.
- A new standalone view to manage all external resources.

[https://dl.nssurge.com/mac/v3/Surge-3.3.0-893.zip](https://dl.nssurge.com/mac/v3/Surge-3.3.0-893.zip)

### **Version 3.2.1**

- Fixed a bug that Handoff doesn't work between Surge iOS and Dashboard.
- Fixed a bug that 'Update All Remote Resources' may not work.

[https://dl.nssurge.com/mac/v3/Surge-3.2.1-863.zip](https://dl.nssurge.com/mac/v3/Surge-3.2.1-863.zip)

### **Version 3.2.0**

**Scripting**

- New major feature: scripting. You may use JavaScript to modify the response as you wish. See the manual for more information: [https://manual.nssurge.com/http-processing/scripting.html](https://manual.nssurge.com/http-processing/scripting.html)
- You can now use a script to modify the response headers and status code.

**Dashboard**

- USB module has been refactored to improve stability. Also, you may choose the device from multiple USB devices now.

**MitM**

- HTTP and MitM engine has been refactored. Please report if you encounter any issues.
- You can now use URL-REGEX rule for MitM connections.
- You may use prefix '-' to exclude domains for MitM. Example:

    ```
    [MITM]
    hostname = -*.apple.com, -*.icloud.com, *

    ```

- MitM hostname list now supports port number. By default only the connections to port 443 will be decrypted. Use suffix :port to enable MitM for other ports. Use suffix :0 to enable MitM for all ports on the hostname.
- URL rewrite type 'header' is now available for MitM connections. You may also use it to rewrite a plain HTTP request to an HTTPS request.

**Misc**

- You can now enable/disable a rule.
- Added a small indicator in the menu icon for Metered Network Mode.</lo>
- Added main switches for rewrite and scripting.
- Supports TCP SACKs for Surge VIF.
- New general option: force-http-engine-hosts. You can force Surge to treat a raw TCP connection as an HTTP connection, to enable high-level functions such as URL-REGEX rules, rewrite and scripting. This option uses the same format as [MITM] hostname option.
- New option for url-test/fallback group: evaluate-before-use. By default, the requests before a connection evaluation will use the first policy in the list and trigger the evaluate. Enable the option to delay the requests until the evaluation completed.

[https://dl.nssurge.com/mac/v3/Surge-3.2.0-860.zip](https://dl.nssurge.com/mac/v3/Surge-3.2.0-860.zip)

### **Version 3.1.1**

- Bug fixes.

[https://dl.nssurge.com/mac/v3/Surge-3.1.1-811.zip](https://dl.nssurge.com/mac/v3/Surge-3.1.1-811.zip)

### **Version 3.1.0**

- Added more feature to the main menu.
- Dashboard now supports to export all requests to an archive file for opening later or sharing.
- Supports a new proxy protocol: Snell. ([https://github.com/surge-networks/snell](https://github.com/surge-networks/snell))
- Surge Mac can work as a Snell proxy server now. See [https://manual.nssurge.com/others/snell-server.html](https://manual.nssurge.com/others/snell-server.html) for more information.
- A new option to automatically reload if the profile was modified externally/remotely.
- Fixed a compatibility issue with some FTP clients.
- Added a new option to disable automatically notification dismissing.
- The update notification is now shown as a banner instead of an alert window.
- Bug fixes.

[https://dl.nssurge.com/mac/v3/Surge-3.1.0-807.zip](https://dl.nssurge.com/mac/v3/Surge-3.1.0-807.zip)

### **Version 3.0.6**

- Optimizations for no network error handling.
- Reduces CPU usage on idle.
- Fixed a bug while enabling MitM with a new certificate.
- Fixed crashes on macOS 10.11.

[https://dl.nssurge.com/mac/v3/Surge-3.0.6-781.zip](https://dl.nssurge.com/mac/v3/Surge-3.0.6-781.zip)

### **Version 3.0.5**

- CPU usage optimizations (50% reduced for high throughout).
- Enabled Hardened Runtime to get enhanced security protections in macOS Mojave.
- Add more notes for rule evaluating stage.
- WeChat.app may flood ping when network is unstable, which causes a high CPU usage of Surge. We added a mechanism to limit ICMP throughput in this version.

[https://dl.nssurge.com/mac/v3/Surge-3.0.5-773.zip](https://dl.nssurge.com/mac/v3/Surge-3.0.5-773.zip)

### **Version 3.0.4**

- Added a new option 'hijack-dns' to hijack DNS queries to other DNS servers with fake IP addresses. See manual for more information: [https://manual.nssurge.com/others/misc-options.html](https://manual.nssurge.com/others/misc-options.html).
- Bug fixes

[https://dl.nssurge.com/mac/v3/Surge-3.0.4-759.zip](https://dl.nssurge.com/mac/v3/Surge-3.0.4-759.zip)

### **Version 3.0.3**

- Supports new iCloud container for Surge iOS migration.
- The MitM feature is now compatible with Android system. Please regenerate an new CA certificate before using with Android.
- Fixed some UI issues in Dashboard.
- Fixed a bug that MitM may refuse to enable after modifying settings.
- Fixed a bug that br decompress may fail.
- Fixed a bug that the menu item may use a wrong color if the accent color of system isn't blue.
- Fixed the JSON viewer color issue in the Dark Mode.
- Minor bug fixes.

[https://dl.nssurge.com/mac/v3/Surge-3.0.3-754.zip](https://dl.nssurge.com/mac/v3/Surge-3.0.3-754.zip)

### **Version 3.0.2**

- Allows import the profile from a URL.
- Fixed an issue that the HTTP capture button may show wrong state in Dashboard.
- Fixed an issue that Dashboard doesn't show User-Agent as the process name while connecting to iOS device.
- Fixed an issue that the bandwidth of processes may be inaccurate.
- Fixed an issue that the DEST-PORT rule may not be parsed.
- Fixed an issue that ruleset can't be used with logical type rule.

[https://dl.nssurge.com/mac/v3/Surge-3.0.2-736.zip](https://dl.nssurge.com/mac/v3/Surge-3.0.2-736.zip)

### **Version 3.0.1**

- Fixed an issue that TFO option will not be saved.
- Fixed an issue that UDP relay option shows wrong state.
- Fixed some i18n issues.
- Fixed crashs on macOS 10.11.
- Save proxy declarations with legacy style (custom) if the proxy is written in legacy style in the text file.
- Other minor bug fixes.

[https://dl.nssurge.com/mac/v3/Surge-3.0.1-711.zip](https://dl.nssurge.com/mac/v3/Surge-3.0.1-711.zip)

### **Version 3.0.0**

[https://dl.nssurge.com/mac/v3/Surge-3.0.0-702.zip](https://dl.nssurge.com/mac/v3/Surge-3.0.0-702.zip)

## **Surge Mac V2**

### **Version 2.6.7**

- Fixed a compatibility issue with 304 response.
- Fixed a Dashboard crash.

[https://dl.nssurge.com/mac/Surge-2.6.7-656.zip](https://dl.nssurge.com/mac/Surge-2.6.7-656.zip)

### **Version 2.6.6**

- Fixed a compatibility issue with 304 response.
- Fixed an issue that Dashboard may not use the correct encoding to decode text body.

[https://dl.nssurge.com/mac/Surge-2.6.6-654.zip](https://dl.nssurge.com/mac/Surge-2.6.6-654.zip)

### **Version 2.6.5**

- Bug fixes.

[https://nssurge.com/mac/Surge-2.6.5-652.zip](https://nssurge.com/mac/Surge-2.6.5-652.zip)

### **Version 2.6.4**

- Bug fixes.

[https://nssurge.com/mac/Surge-2.6.4-647.zip](https://nssurge.com/mac/Surge-2.6.4-647.zip)

### **Version 2.6.3**

- New Features: External Proxy Provider. See [https://medium.com/@Blankwonder/surge-mac-new-features-external-proxy-provider-375e0e9ea660](https://medium.com/@Blankwonder/surge-mac-new-features-external-proxy-provider-375e0e9ea660) for more information.
- Surge will automatically track system proxy settings now. When Surge is no longer the default proxy, the status icon will turn grey and a notification will raise.
- Fixed a compatibility issue with Docker.

[https://nssurge.com/mac/Surge-2.6.3-637.zip](https://nssurge.com/mac/Surge-2.6.3-637.zip)

### **Version 2.6.2**

- Fixed an issue that the UDP mode with AEAD ciphers doesn't work.
- Bug fixes.

[https://nssurge.com/mac/Surge-2.6.2-618.zip](https://nssurge.com/mac/Surge-2.6.2-618.zip)

### **Version 2.6.1**

- Surge now allows expired DNS answers for performance reasons. See 'Optimistic DNS' section in [https://developer.apple.com/videos/play/wwdc2018/714/](https://developer.apple.com/videos/play/wwdc2018/714/) for more information.
- Performance improvements.
- Fixed an issue that UDP traffics are not included in the real-time speed.
- Supports hardware acceleration for AES-GCM encryption.
- Supports NAT64 in a pure IPv6 network. (Previous versions already supported DNS64)

[https://nssurge.com/mac/Surge-2.6.1-612.zip](https://nssurge.com/mac/Surge-2.6.1-612.zip)

### **Version 2.6.0**

- Supports using Surge Mac as a gateway.
- A new setup guide view.
- A new config panel for traffic capture options.
- Fixed an issue which Dashboard may disconnect unexpectedly under huge pressure.
- The status bar icon will be red while traffic capture is enabled.
- Improved TUN interface performance.
- Enabling TCP Fast Open in macOS 10.14.

[https://nssurge.com/mac/Surge-2.6.0-596.zip](https://nssurge.com/mac/Surge-2.6.0-596.zip)

### **Version 2.5.3**

- Supports UDP relay for shadowsocks protocol. A brief introduction in Chinese: [https://trello.com/c/ugOMxD3u](https://trello.com/c/ugOMxD3u).
- You may use Dashboard to view UDP conversations.
- Dashboard now can save multiple remote machine profiles.
- Improved the JSON viewer in Dashboard.
- Added an UI switch for the dns-failed option in FINAL rule.
- Bug fixes.

[https://nssurge.com/mac/Surge-2.5.3-563.zip](https://nssurge.com/mac/Surge-2.5.3-563.zip)

### **Version 2.5.2**

- You may toggle the hidden state of columns in Dashboard now.
- Supports to export selected rows to csv file.
- Added a connection duration column in Dashboard.
- Supports obfs-uri parameter.
- Improved the benchmark view.
- Fixed a serious bug in the SOCKS5 proxy implementation.
- Bug fixes.

[https://nssurge.com/mac/Surge-2.5.2-544.zip](https://nssurge.com/mac/Surge-2.5.2-544.zip)

### **Version 2.5.1**

- The MitM enabling switch has been moved to the main menu and isolated from profile.
- Bug fixes.

[http://dl.nssurge.com/mac/Surge-2.5.1-528.zip](http://dl.nssurge.com/mac/Surge-2.5.1-528.zip)

### **Version 2.5.0**

- Added Outbound Mode options: Direct Outbound, Global Proxy and By Rule.
- Added options for all policy to specify outgoing interface: 'interface' and 'allow-other-interface'.
- Added all_proxy environment variable for 'Copy Shell Export Command'
- Supports client-side SSL/TLS certificate validation for HTTPS and SOCKS5-TLS proxy. A config example is here: [https://gist.github.com/Blankwonder/cd9fa1987e41cf1a1f1df50583ba1d9c](https://gist.github.com/Blankwonder/cd9fa1987e41cf1a1f1df50583ba1d9c) (DO NOT support editing with UI in this version.)
- Refined MitM.
- Concurrently setup connection to host with Round-robin DNS to boost performance.
- Bug fixes.

[http://dl.nssurge.com/mac/Surge-2.5.0-520.zip](http://dl.nssurge.com/mac/Surge-2.5.0-520.zip)

### **Version 2.4.6**

- Supports xchacha20-ietf-poly1305.
- Bug fixes.
- HTTP request header and response header can be extracted from TCP connection now. (SOCKS5 and TUN)
- Enhanced mode can handle all connections now, even for connections initialized with IP address directly.
- Surge TUN now supports forwarding ICMP packets.

**From this version, the minimum system version requirement was raised to macOS 10.11. If you are still using macOS 10.10, please use version 2.4.5.**

[http://dl.nssurge.com/mac/Surge-2.4.6-490.zip](http://dl.nssurge.com/mac/Surge-2.4.6-490.zip)

### **Version 2.4.5**

- Bug fixes.
- Improved performance for high concurrency.
- TCP fast open has been disabled temporarily since there is a serious problem in macOS/iOS kernel.
- Dashboard will display decoded URL query now.

[http://dl.nssurge.com/mac/Surge-2.4.5-468.zip](http://dl.nssurge.com/mac/Surge-2.4.5-468.zip)

### **Version 2.4.4**

- Supports obfs=tls for shadowsocks protocol.
- Refined the proxy edit panel.
- Added Simplified Chinese language.

[http://dl.nssurge.com/mac/Surge-2.4.4-459.zip](http://dl.nssurge.com/mac/Surge-2.4.4-459.zip)

### **Version 2.4.3**

- Supports obfs=tls for shadowsocks protocol.
- Refined the proxy edit panel.
- Added Simplified Chinese language.

[http://dl.nssurge.com/mac/Surge-2.4.3-457.zip](http://dl.nssurge.com/mac/Surge-2.4.3-457.zip)

### **Version 2.4.2**

- Fixed an issue that enhanced mode may not be closed properly when switching to a profile without dns-server.
- Fixed an issue that managed profile updating and license info are unavailable while enhanced mode enabled.
- When the necessary port is used by another process, the error alert will show which process is using the port.
- Fixed an issue that map local items can't be edited with UI.
- Fixed an issue that system proxy settings may not be reset properly.
- Auto URL test group will execute a retest immediately after the selected policy has failed.

[http://dl.nssurge.com/mac/Surge-2.4.2-445.zip](http://dl.nssurge.com/mac/Surge-2.4.2-445.zip)

### **Version 2.4.1**

- Bug fixes.

[http://dl.nssurge.com/mac/Surge-2.4.1-439.zip](http://dl.nssurge.com/mac/Surge-2.4.1-439.zip)

### **Version 2.4.0**

- Supports enterprise license and profile management.
- Fixed a bug that some fields are unavailable in the configuration panel in some cases.
- Fixed a bug that the FINAL rule can't be edited.
- Fixed a bug that you may not be able to use custom storage path for profiles.
- The interface related options are no longer controlled by profile. Sorry for the repetitive changes.
- You may use $1, $2 to use the matched string in the value while using header rewrite.
- Added an option for HTTP/HTTPS proxy: always-use-connect. When it is true, Surge will use CONNECT method for plain HTTP requests.

[http://dl.nssurge.com/mac/Surge-2.4.0-429.zip](http://dl.nssurge.com/mac/Surge-2.4.0-429.zip)

### **Version 2.3.2**

- Added a option to control whether show proxy error notification.
- Fixed a problem that Dashboard show data doesn't exist error.

[http://dl.nssurge.com/mac/Surge-2.3.2-421.zip](http://dl.nssurge.com/mac/Surge-2.3.2-421.zip)

### **Version 2.3.1**

- Added a wizard to install CA’s root certificate for iOS simulator.
- Connectivity quality is now an option. (Not show by default)
- Line comments in [Rule] section in profile file is now presented in UI.
- You may add proxy rule with Dashboard by right-clicking the request or process.
- Dashboard will always open a new window for local machine, instead of asking. You may use "File" menu to connect to a remote machine.
- Added a patch mechanism for adjusting settings for managed config. See manual for more information: [https://manual.nssurge.com/others/managed-configuration.html](https://manual.nssurge.com/others/managed-configuration.html)

[http://dl.nssurge.com/mac/Surge-2.3.1-420.zip](http://dl.nssurge.com/mac/Surge-2.3.1-420.zip)

### **Version 2.3.0**

- Completely redesign the configure interface. You may configure every function with UI now.
- Proxy benchmark is now moved to main application from Dashboard.
- New feature: Header rewrite. See manual for more information: [https://manual.nssurge.com/header-rewrite.html](https://manual.nssurge.com/header-rewrite.html).
- You may switch profile with command line now: surge-cli switch-profile profilename.

[http://dl.nssurge.com/mac/Surge-2.3.0-416.zip](http://dl.nssurge.com/mac/Surge-2.3.0-416.zip)

### **Version 2.2.4**

- Notifications presented by Surge will be removed from Notification Center automatically.
- The interval of attempts to refresh managed config changes to one hour from one minute. (After config expired)
- Supports new encryption methods for shadowsocks-libev 3.0.
- Optimized Dashboard performance.
- Supports TCP Fast Open for shadowsocks proxy. You need add "tfo=true" flag in [Proxy] section to enable the feature. You may use benchmark to confirm TFO is working.
- You can sort benchmark results now.
- You may choose to reload config after managed config updated.

[http://dl.nssurge.com/mac/Surge-2.2.4-394.zip](http://dl.nssurge.com/mac/Surge-2.2.4-394.zip)

### **Version 2.2.2**

- Fixed a bug when using SOCKS5 without authorization.

[http://dl.nssurge.com/mac/Surge-2.2.2-375.zip](http://dl.nssurge.com/mac/Surge-2.2.2-375.zip)

### **Version 2.2.1**

- You may use Dashboard to benchmark proxies now.
- Fixed "Too many open files" error by raising limit to 2048.
- Fixed a bug in SOCKS5 with authorization.
- Fixed a bug that managed config may refresh continuously.

[http://dl.nssurge.com/mac/Surge-2.2.1-374.zip](http://dl.nssurge.com/mac/Surge-2.2.1-374.zip)

### **Version 2.2.0**

- Map local function is now available.
- Adds notifications when proxy encounters errors.
- Network changed notification will show service name instead of BSD name now.
- Fixed a bug that Dashboard may show the incorrect state of body dump.
- Changes for HTTPS and SOCK5-TLS proxy:
    - Option 'skip-common-name-verify' is deprecated.
    - Add a new option 'skip-cert-verify' to skip certificate verify completely.
    - Add a new option 'sni' to customize SNI field while handshaking. You may use 'sni=off' to disable SNI.
- New rule type: PROCESS-NAME, USER-AGENT and URL-REGEX.
- You can use simple wildcard matching (? and *) for PROCESS-NAME rule, local DNS mapping and MitM hosts.
- Dashboard supports display POST form data in a table view.
- You may let Surge reload config by sending SIGHUP. You can use command 'killall -HUP Surge' or 'surge-cli reload'.
- Managed configuration is supported now.
- Add a new option 'skip-server-cert-verify' for MitM.

[http://dl.nssurge.com/mac/Surge-2.2.0-368.zip](http://dl.nssurge.com/mac/Surge-2.2.0-368.zip)

### **Version 2.1.4**

- Fixed a bug that helper may crash on macOS 10.10.
- Add a option to remove Surge helper for troubleshooting.
- Bug fixes.

[http://dl.nssurge.com/mac/Surge-2.1.4-362.zip](http://dl.nssurge.com/mac/Surge-2.1.4-362.zip)

### **Version 2.1.3**

- Fixed a bug that helper may crash on macOS 10.10.
- Add a option to remove Surge helper for troubleshooting.
- Bug fixes.

[http://dl.nssurge.com/mac/Surge-2.1.3-337.zip](http://dl.nssurge.com/mac/Surge-2.1.3-337.zip)

### **Version 2.1.2**

- New option: Collapse policy group items in menu
- Fixed a bug that enhanced mode DNS settings may not be reverted.
- Hold option key to click 'Copy Shell Export Command' to get a command with primary interface IP instead of 127.0.0.1.
- Bug fixes.

[http://dl.nssurge.com/mac/Surge-2.1.2-327.zip](http://dl.nssurge.com/mac/Surge-2.1.2-327.zip)

### **Version 2.1.0**

- New feature: Enhanced Mode

    Some applications may not obey the system proxy settings. Using enhanced mode can make all applications handled by Surge.

- New rule type: IP-CIDR6

    Example: IP-CIDR6,2005::/16,DIRECT,no-resolve

- The /etc/hosts file will be reloaded automatically if it has changes.

[http://dl.nssurge.com/mac/Surge-2.1.0-318.zip](http://dl.nssurge.com/mac/Surge-2.1.0-318.zip)

### **Version 2.0.13**

- Dashboard supports to use ⌘ + 1,2,3,4 to switch panel.
- Dashboard Supports handoff with Surge iOS.
- Fixed a bug that Dashboard may show incorrect process name.

[http://dl.nssurge.com/mac/Surge-2.0.13-304.zip](http://dl.nssurge.com/mac/Surge-2.0.13-304.zip)

### **Version 2.0.12**

- Bug fixes.
- Supported SNI while performing MitM.
- The original certificate will be resigned and used while performing MitM, instead of generating a new certificate.

[http://dl.nssurge.com/mac/Surge-2.0.12-295.zip](http://dl.nssurge.com/mac/Surge-2.0.12-295.zip)

### **Version 2.0.11**

- Rule test cache will be flushed after network switching now.
- Added a option 'Grey icon if set as system proxy is disabled'.
- Bug fixes and performance improvements.

[http://dl.nssurge.com/mac/Surge-2.0.11-289.zip](http://dl.nssurge.com/mac/Surge-2.0.11-289.zip)

### **Version 2.0.10**

- Surge talks to HTTP proxies with a plain HTTP method for non-HTTPS requests now, instead of CONNECT.
- Improved compatibility with some HTTP server.
- Improved compatibility with some DNS server.

[http://dl.nssurge.com/mac/Surge-2.0.10-280.zip](http://dl.nssurge.com/mac/Surge-2.0.10-280.zip)

### **Version 2.0.9**

- Dashborad: The height of the detail panel will not change now while switching pages.
- A notification will show when proxy client access from other machine.
- Used SF Mono as monospaced font for header and body data display.
- Supported TCP half-open mechanism.

[http://dl.nssurge.com/mac/Surge-2.0.9-273.zip](http://dl.nssurge.com/mac/Surge-2.0.9-273.zip)

### **Version 2.0.8**

- Add a new option 'exclude-simple-hostnames' in the gereral section.
- Dashborad: Selected row will not be lost while the filter or sort column changed.
- Dashborad: Fixes some issues in the active panel.

[http://dl.nssurge.com/mac/Surge-2.0.8-260.zip](http://dl.nssurge.com/mac/Surge-2.0.8-260.zip)

### **Version 2.0.5**

- Bug fixes.

[http://dl.nssurge.com/mac/Surge-2.0.5-255.zip](http://dl.nssurge.com/mac/Surge-2.0.5-255.zip)

### **Version 2.0.3**

- New feature: Show connectivity quality in menu.

    Surge will send a DNS question to all DNS servers concurrently to test physical network connectivity while opening the menu.

- Fixes a problem that Surge may freeze while opening the menu.
- Fixes a problem that if a policy group contains duplicate policies, Surge may crash.

[http://dl.nssurge.com/mac/Surge-2.0.3-250.zip](http://dl.nssurge.com/mac/Surge-2.0.3-250.zip)

### **Version 2.0.2**

- Dashboard will no longer display process icon in remote mode.
- Fixes a bug: "Set as System Proxy" option does not work properly if only SOCKS service is enabled.
- Fixes a bug: Dashboard can't add a rule with no-resolve option on and comment not empty.
- Minor bug fixes.

### **Version 2.0.1**

- Bug fixes