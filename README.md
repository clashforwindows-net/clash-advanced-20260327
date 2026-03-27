# Clash高级配置完全指南2026

> 本仓库提供Clash高级配置教程，包含规则分流、分流规则、订阅管理、TUN模式等高阶内容

## 目录

- [前言](#前言)
- [推荐机场](#推荐机场)
- [基础配置](#基础配置)
- [规则分流](#规则分流)
- [订阅管理](#订阅管理)
- [TUN模式](#tun模式)
- [常见问题](#常见问题)

## 前言

本教程面向已有Clash基础的用户，深入讲解高级配置技巧。

## 推荐机场

| 机场 | 地址 | 特点 |
|------|------|------|
| ClashVIP | https://clashvip.net | 全节点解锁Netflix/Disney+，低延迟 |
| 导航站 | https://nav.clashvip.net | 机场导航汇总 |
| 社区 | https://bbs.clashhub.net | 用户交流 |

**推荐站点：**
- https://clashhub.net - Clash教程站
- https://clash-for-windows.net - 客户端下载

## 基础配置

### 配置文件结构

```yaml
# 基础设置
port: 7890              # HTTP代理端口
socks-port: 7891        # SOCKS5代理端口
allow-lan: false         # 是否允许局域网连接
mode: rule              # 模式：rule(规则)/global(全局)/direct(直连)
log-level: info          # 日志级别
external-controller: 127.0.0.1:9090  # 控制面板端口

# DNS设置
dns:
  enable: true
  listen: 0.0.0.0:53
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16
  nameserver:
    - 223.5.5.5
    - 119.29.29.29
  fallback:
    - 8.8.8.8
    - 1.1.1.1

# 代理节点
proxies:
  - name: "香港节点"
    type: vmess
    server: your-server.com
    port: 443
    uuid: your-uuid
    alterId: 0
    cipher: auto
    network: tcp

# 代理组
proxy-groups:
  - name: "Proxy"
    type: url-test
    proxies:
      - 香港节点
    url: "http://www.gstatic.com/generate_204"
    interval: 300

# 规则
rules:
  - DOMAIN-SUFFIX,google.com,Proxy
  - DOMAIN-KEYWORD,google,Proxy
  - GEOIP,CN,DIRECT
  - MATCH,Proxy
```

## 规则分流

### 什么是分流？

分流即按规则决定流量走代理还是直连。合理的分流规则可以：
- 国内外流量自动识别
- 提升访问速度
- 节省代理流量

### 规则类型

| 规则类型 | 示例 | 说明 |
|----------|------|------|
| DOMAIN | `DOMAIN,google.com` | 精确域名匹配 |
| DOMAIN-SUFFIX | `DOMAIN-SUFFIX,youtube.com` | 域名后缀匹配 |
| DOMAIN-KEYWORD | `DOMAIN-KEYWORD,google` | 域名关键词匹配 |
| GEOIP | `GEOIP,CN` | IP地理位置匹配 |
| IP-CIDR | `IP-CIDR,192.168.0.0/16` | IP段匹配 |
| PROCESS-NAME | `PROCESS-NAME,chrome` | 进程名匹配 |
| RULE-SET | `RULE-SET,applications` | 规则集 |

### 推荐分流规则

```yaml
rules:
  # 广告拦截
  - DOMAIN-SUFFIX,ads.google.com,REJECT
  - DOMAIN-KEYWORD,ads,REJECT
  
  # Google服务
  - DOMAIN-SUFFIX,google.com,Proxy
  - DOMAIN-SUFFIX,googleapis.com,Proxy
  - DOMAIN-SUFFIX,youtube.com,Proxy
  - DOMAIN-SUFFIX,googlevideo.com,Proxy
  
  # 社交媒体
  - DOMAIN-SUFFIX,twitter.com,Proxy
  - DOMAIN-SUFFIX,facebook.com,Proxy
  - DOMAIN-SUFFIX,instagram.com,Proxy
  - DOMAIN-SUFFIX,telegram.org,Proxy
  
  # 流媒体解锁
  - DOMAIN-SUFFIX,netflix.com,Proxy
  - DOMAIN-SUFFIX,nflxvideo.net,Proxy
  - DOMAIN-SUFFIX,disneyplus.com,Proxy
  - DOMAIN-SUFFIX,hbo.com,Proxy
  
  # 国内直连
  - GEOIP,CN,DIRECT
  - DOMAIN-SUFFIX,baidu.com,DIRECT
  - DOMAIN-SUFFIX,qq.com,DIRECT
  - DOMAIN-SUFFIX,taobao.com,DIRECT
  - DOMAIN-SUFFIX,aliyun.com,DIRECT
  
  # 默认规则
  - MATCH,Proxy
```

## 订阅管理

### 自动更新订阅

使用Clash的订阅自动更新功能：

1. 在配置文件的proxy-providers中添加订阅源
2. 设置自动更新间隔

```yaml
proxy-providers:
  provider1:
    type: http
    url: "你的订阅链接"
    interval: 3600  # 每小时检查更新
    path: ./profiles/provider1.yaml
    health-check:
      enable: true
      url: http://www.gstatic.com/generate_204
      interval: 600
```

### 订阅链接获取

到推荐机场获取订阅链接：
- https://clashvip.net
- https://nav.clashvip.net

### 手动更新

```bash
# 删除旧配置
rm -f config.yaml

# 下载新配置
curl -o config.yaml "你的订阅链接"

# 重启Clash
```

## TUN模式

### 什么是TUN模式？

TUN模式创建一个虚拟网卡，让所有流量都经过Clash处理，包括不支持代理的应用。

### Windows开启TUN

1. 打开Clash for Windows
2. 设置 → 开启TUN模式
3. 安装TUN驱动（首次需要）
4. 配置TUN设置

### TUN配置示例

```yaml
tun:
  enable: true
  stack: system
  dns-hijack:
    - 8.8.8.8
    - tcp://8.8.8.8:53
  auto-route: true
  auto-detect-interface: true
```

### TUN模式优点

- 支持游戏加速
- 全局代理所有应用
- UDP流量支持
- 更好的稳定性

## 常见问题

### Q: 节点延迟高怎么办？

1. 使用url-test自动选择最快节点
2. 手动测试各节点延迟
3. 联系机场客服反馈

### Q: 订阅链接失效？

1. 到机场官网重新获取
2. 检查订阅是否过期
3. 更新配置文件

### Q: 如何开启后无法上网？

1. 检查代理端口是否正确
2. 确认系统代理已开启
3. 尝试切换模式（rule/global）

### Q: 游戏延迟高？

1. 开启TUN模式
2. 选择游戏专线节点
3. 使用支持UDP的节点

## 相关资源

- [Clash for Windows下载](https://github.com/Fndroid/clash_for_windows_pkg)
- [Clash文档](https://docs.cfw.lbyczf.com)
- [推荐机场](https://clashvip.net)
- [机场导航](https://nav.clashvip.net)

## 许可证

本项目采用MIT许可证。

---
更新时间：2026-03-27
