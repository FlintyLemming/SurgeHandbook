# SSH

你可以使用 SSH 协议作为代理策略，相当于 `ssh -D`。

配置文件语法：

*   密码认证

```ini
[Proxy]
proxy = ssh, 1.2.3.4, 22, username=root, password=pw
```

*   公钥认证

```ini
[Proxy]
proxy = ssh, 1.2.3.4, 22, username=root, private-key=key1

[Keystore]
key1 = type=openssh-private-key, base64=[私钥文件的 Base64 编码内容]
```

*   请注意，你必须对整个私钥文件再次进行 Base64 编码，即使私钥文件本身就是 Base64 编码的。
*   支持全部四种类型的私钥：RSA/ECDSA/ED25519/DSA。
*   Surge 仅支持 `curve25519-sha256` 作为密钥交换 (kex) 算法，并仅支持 `aes128-gcm` 作为加密算法。这意味着 SSH 服务器必须使用 OpenSSH v7.3 或更高版本。（这应该不是问题，因为 OpenSSH 7.3 已于 2016-08-01 发布。）
*   你现在可以指定空闲超时参数。默认值为 180 秒。

```ini
[Proxy]
proxy = ssh, 1.2.3.4, 22, username=root, password=pw, idle-timeout=180
```

## 服务器指纹 (Server Fingerprint)

为了防范 MITM 攻击，你可以使用 `server-fingerprint` 参数指定服务器的公钥指纹，这能确保只连接到合法的服务器。

```ini
[Proxy]
proxy = ssh, 1.2.3.4, 22, username=root, password=pw, idle-timeout=180, server-fingerprint = "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBk2No6KBq2m9VTCcHXXJBX4/A3RNr+L+yDBl5+TF9qz"
```

由于一台服务器可能拥有多个公钥，`server-fingerprint` 参数支持配置多个指纹。

```ini
server-fingerprint = "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBk2No6KBq2m9VTCcHXXJBX4/A3RNr+L+yDBl5+TF9qz,ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD7aoFCymj8NJL+xMqYzRLGpIfVd2sebgtnD3cplG7/lrvPGYIpRAOkKqdUBOkRd2x68JFe0u+gBHQxFkv8o81Saqr6qxcrq4mPiyqxOTRkvDMtYrjJ4AJZE26nCzHRCC7Ji6Mq2OtepTJcC9uk2LLcRrF3G05qu6ToeK1LgXgqc+b2RLOQJ1AXEeNgn0NIXWlBv4AhQRJ6fFQi4HO/jkxpFNfzKY+dPDx6P3VAazYa2nl8wpLbXt+tq6SBv8RctwDuYszAbjSCPPJq7ToX/Svqqbl82qtOLOofcQ8/f8809i4RQ0yuEpVLnVVWd7cZx5h45vt+/I1Ifr2pS7BqhLL/,ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLdhR3D2BvyD7FTXfx0CrjZF2tVgoVRFi1poGKoX0eXc9OlpiaqNos4niiN0GWyoT4mL724cgvaL+vHW8sTZE5A="
```

如果服务器的 sshd 支持 ed25519，则只需要 ssh-ed25519 的指纹。

你可以从 `~/.ssh/known_hosts` 文件中获取服务器指纹。或者你可以在受信任的网络环境中使用 `ssh-keyscan example.com` 命令来获取。在将其复制到 Surge 之前，请移除行首的主机名。
