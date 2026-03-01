<h1 align="center">
  <img src="./docs/logo.png" alt="Clash" width="200">
  <br>Clash<br>
</h1>

<p align="center">
  <a href="./README.md">English</a> | <a href="./README.zh.md">简体中文</a>
</p>

<h4 align="center">基于 Go 语言的规则隧道代理。</h4>

<p align="center">
  <a href="https://github.com/nocturix/clash/actions">
    <img src="https://img.shields.io/github/actions/workflow/status/nocturix/clash/release.yml?branch=master&style=flat-square" alt="Github Actions">
  </a>
  <a href="https://goreportcard.com/report/github.com/nocturix/clash">
    <img src="https://goreportcard.com/badge/github.com/nocturix/clash?style=flat-square">
  </a>
  <img src="https://img.shields.io/github/go-mod/go-version/nocturix/clash?style=flat-square">
  <a href="https://github.com/nocturix/clash/releases">
    <img src="https://img.shields.io/github/release/nocturix/clash/all.svg?style=flat-square">
  </a>
  <a href="https://github.com/nocturix/clash/releases/tag/premium">
    <img src="https://img.shields.io/badge/release-Premium-00b4f0?style=flat-square">
  </a>
</p>

## 功能特性

以下是 Clash 的功能概览。

- 入站协议：HTTP、HTTPS、SOCKS5 服务器、TUN 设备
- 出站协议：Shadowsocks(R)、VMess、Trojan、Snell、SOCKS5、HTTP(S)、WireGuard
- 规则路由：动态脚本、域名、IP 地址、进程名称等
- Fake-IP DNS：最小化 DNS 污染的影响，提升网络性能
- 透明代理：Redirect TCP 和 TProxy TCP/UDP，支持自动路由表/规则管理
- 代理组：自动故障转移、负载均衡或延迟测试
- 远程提供者：动态加载远程代理列表
- RESTful API：通过全面的 API 就地更新配置

*部分功能可能仅在 [Premium 核心](https://dreamacro.github.io/clash/premium/introduction.html)中可用。*

## 文档

您可以在 [https://nocturix.github.io/clash/](https://nocturix.github.io/clash/) 查看最新文档。

## 致谢

- [riobard/go-shadowsocks2](https://github.com/riobard/go-shadowsocks2)
- [v2ray/v2ray-core](https://github.com/v2ray/v2ray-core)
- [WireGuard/wireguard-go](https://github.com/WireGuard/wireguard-go)

## 许可证

本软件采用 GPL-3.0 许可证发布。

[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2FDreamacro%2Fclash.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2FDreamacro%2Fclash?ref=badge_large)
