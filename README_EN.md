# Complete Clash Advanced Configuration Guide 2026

> This repository provides advanced Clash configuration tutorials, including rule-based routing, subscription management, TUN mode, and more.

## Table of Contents

- [Introduction](#introduction)
- [Recommended Providers](#recommended-providers)
- [Basic Configuration](#basic-configuration)
- [Rule-Based Routing](#rule-based-routing)
- [Subscription Management](#subscription-management)
- [TUN Mode](#tun-mode)
- [FAQ](#faq)

## Introduction

This guide is for users who already have Clash basics and want to learn advanced configuration.

## Recommended Providers

| Provider | URL | Features |
|----------|-----|----------|
| ClashVIP | https://clashvip.net | Netflix/Disney+ unblocked, low latency |
| Navigation | https://nav.clashvip.net | Provider directory |
| Community | https://bbs.clashhub.net | User discussion |

## Basic Configuration

### Config Structure

```yaml
port: 7890
socks-port: 7891
allow-lan: false
mode: rule
log-level: info
external-controller: 127.0.0.1:9090

dns:
  enable: true
  listen: 0.0.0.0:53
  enhanced-mode: fake-ip
  nameserver:
    - 223.5.5.5
    - 119.29.29.29

proxy-groups:
  - name: "Proxy"
    type: url-test
    proxies:
      - 香港节点
    url: "http://www.gstatic.com/generate_204"
    interval: 300
```

## Rule-Based Routing

### Rule Types

| Type | Example | Description |
|------|---------|-------------|
| DOMAIN | `DOMAIN,google.com` | Exact match |
| DOMAIN-SUFFIX | `DOMAIN-SUFFIX,youtube.com` | Suffix match |
| DOMAIN-KEYWORD | `DOMAIN-KEYWORD,google` | Keyword match |
| GEOIP | `GEOIP,CN` | IP geolocation |
| RULE-SET | `RULE-SET,applications` | Rule set |

### Recommended Rules

```yaml
rules:
  # Google
  - DOMAIN-SUFFIX,google.com,Proxy
  - DOMAIN-SUFFIX,youtube.com,Proxy
  
  # Streaming
  - DOMAIN-SUFFIX,netflix.com,Proxy
  - DOMAIN-SUFFIX,disneyplus.com,Proxy
  
  # China Direct
  - GEOIP,CN,DIRECT
  - DOMAIN-SUFFIX,baidu.com,DIRECT
  
  - MATCH,Proxy
```

## TUN Mode

### What is TUN Mode?

TUN mode creates a virtual network interface, routing all traffic through Clash.

### Enable TUN (Windows)

1. Open Clash for Windows
2. Settings → Enable TUN Mode
3. Install TUN driver (first time)
4. Configure TUN settings

## FAQ

### Q: High latency?

1. Use url-test for auto selection
2. Manually test nodes
3. Contact provider support

### Q: Subscription expired?

1. Get new subscription from provider
2. Check expiration date
3. Update config

## Resources

- [Clash for Windows](https://github.com/Fndroid/clash_for_windows_pkg)
- [Clash Docs](https://docs.cfw.lbyczf.com)
- [Recommended](https://clashvip.net)

---
Last Updated: 2026-03-27
