# Shairport Sync addon for Home Assistant

[![GitHub Release][releases-shield]][releases]
[![License][license-shield]](LICENSE)

使用 [Shairport Sync](https://github.com/mikebrady/shairport-sync) 将 Home Assistant 设备变为 AirPlay 音频接收器。

## 关于

此 addon 会在你的 Home Assistant 实例上运行 Shairport Sync，使其可以作为 AirPlay 音频接收器。你可以从 iPhone、iPad 或 Mac 直接向 Home Assistant 串流音乐。

## 支持的架构

![Supports aarch64 Architecture][aarch64-shield]
![Supports amd64 Architecture][amd64-shield]

## 安装

1. 在 Home Assistant 中进入 **设置** → **加载项** → **加载项商店**
2. 点击右上角的三个点 → **仓库**
3. 添加此仓库 URL：`https://github.com/WangChuDi/addon-shairport-sync`
4. 查找并安装 "Shairport Sync" addon
5. 启动 addon

## 配置

```yaml
log_level: info
avahi_interfaces: ""
avahi_hostname: ""
avahi_domainname: local
airplay_name: Home Assistant
enable_ipv6: false
```

### 选项说明

| 选项 | 说明 |
|---|---|
| `log_level` | 日志级别，可选：`trace`, `debug`, `info`, `notice`, `warning`, `error`, `fatal` |
| `avahi_interfaces` | Avahi 使用的网络接口，留空自动检测 |
| `avahi_hostname` | Avahi 主机名，留空自动检测 |
| `avahi_domainname` | Avahi 域名，默认 `local` |
| `airplay_name` | AirPlay 设备显示名称 |
| `enable_ipv6` | 是否启用 IPv6 |

## 技术信息

- 基于 [Shairport Sync](https://github.com/mikebrady/shairport-sync) 最新版本（从源码编译）
- 使用经典 AirPlay 协议
- 基础镜像：`ghcr.io/hassio-addons/base:20.0.1`
- s6-overlay v3 服务管理

[aarch64-shield]: https://img.shields.io/badge/architecture-aarch64-blue.svg
[amd64-shield]: https://img.shields.io/badge/architecture-amd64-blue.svg
[license-shield]: https://img.shields.io/github/license/WangChuDi/addon-shairport-sync.svg
[releases-shield]: https://img.shields.io/github/release/WangChuDi/addon-shairport-sync.svg
[releases]: https://github.com/WangChuDi/addon-shairport-sync/releases
