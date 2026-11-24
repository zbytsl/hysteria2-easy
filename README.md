# Hysteria 2 Easy Installer

一个面向 systemd Linux VPS 的 Hysteria 2 安装与管理脚本。默认以 Debian 12 为主要测试目标。

## 功能

- 调用 Hysteria 官方安装器安装/更新内核
- 生成 ECDSA P-256 自签 TLS 证书（含 SAN）
- 支持 Hysteria 2 Linux 内置 UDP 端口跳跃
- 为 systemd 服务配置 `CAP_NET_ADMIN`
- 尝试管理 UFW / firewalld 本机 UDP 规则
- 输出 Hysteria 分享链接、官方客户端配置和 mihomo 配置
- 交互菜单及命令行管理
- 卸载时默认保留 `/etc/hysteria`，避免误删配置

## 系统要求

- Debian 12+ / Ubuntu 22.04+（优先）
- systemd
- root 权限
- 可访问 `get.hy2.sh`
- 云厂商安全组允许对应 UDP 端口

## 快速安装

```bash
bash <(curl -fsSL https://raw.githubusercontent.com/你的用户名/hysteria2-easy/main/hy2.sh)
```

或者：

```bash
wget -O hy2.sh https://raw.githubusercontent.com/你的用户名/hysteria2-easy/main/hy2.sh
chmod +x hy2.sh
sudo ./hy2.sh
```

## 命令

```bash
bash hy2.sh install
bash hy2.sh config
bash hy2.sh info
bash hy2.sh cert
bash hy2.sh update
bash hy2.sh uninstall
bash hy2.sh start
bash hy2.sh stop
bash hy2.sh restart
bash hy2.sh status
bash hy2.sh log
```

安装成功后，如果脚本以普通文件方式运行，会尝试安装快捷命令 `hy2`。

## 非交互安装

```bash
HY2_YES=1 \
HY2_DOMAIN=hy.example.com \
HY2_HOP=20000-50000 \
HY2_PASSWORD='ChangeThisToALongRandomPassword' \
HY2_MASQ_URL='https://news.ycombinator.com/' \
bash hy2.sh install
```

使用 IP 部署时，不设置 `HY2_DOMAIN`，并设置 `HY2_SNI`：

```bash
HY2_YES=1 \
HY2_SNI=www.bing.com \
HY2_HOP=20000-50000 \
HY2_PASSWORD='ChangeThisToALongRandomPassword' \
bash hy2.sh install
```

## 重要说明

1. 域名应解析到 VPS 公网地址。使用 Cloudflare 时，Hysteria 2 的 UDP 流量不能通过普通橙云 HTTP 代理；请按实际网络方案配置 DNS only。
2. 必须在云厂商安全组/上游防火墙放行 UDP 端口范围。脚本只能管理 VPS 内部识别到的 UFW 或 firewalld。
3. 端口跳跃依赖 Linux 内置功能以及 nftables 或 iptables。脚本为 systemd 服务增加 `CAP_NET_ADMIN`；不同发行版的 systemd 安全策略可能不同，部署后应检查日志和实际连通性。
4. 自签证书采用 `insecure=true` 配合 SHA-256 指纹 pinning。请勿单独使用 `insecure=true`。
5. 重新生成证书后，所有客户端都必须更新证书指纹。
6. 卸载默认保留 `/etc/hysteria`。确认删除前请先备份密码、证书和配置。
7. 请先在测试 VPS 上验证。脚本不会自动配置云厂商安全组。

## 安全提示

- 不要把 `/etc/hysteria/hy2.env`、`server.key`、节点链接或密码提交到 GitHub。
- 建议使用强随机密码。
- 分享链接包含认证密码，应按敏感凭据处理。
- 发布前请审查脚本并在自己的环境中测试。

## 官方文档

- https://v2.hysteria.network/zh/
- https://v2.hysteria.network/zh/docs/advanced/Port-Hopping/
- https://v2.hysteria.network/zh/docs/getting-started/Server-Installation-Script/

## License

MIT
