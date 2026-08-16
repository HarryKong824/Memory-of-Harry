---
title: "Docker 在 Windows 沙箱的正确驱动方式"
module: tech
type: pattern
tags: [docker, windows, powershell, sandbox]
created: 2026-08-16
updated: 2026-08-16
status: active
source: "本机环境验证 + 用户记忆"
related: []
---

# Docker 在 Windows 沙箱的正确驱动方式

## 上下文
在本机（Windows + WorkBuddy 沙箱）跑 docker compose 全栈时，内置 `docker` 是损坏 shim，且 daemon 需先启动。

## 要点
- Bash 内置 `docker` 报 `/usr/bin/env: 'sh'` 损坏 → 必须用 PowerShell 调全路径 `C:\Program Files\Docker\Docker\resources\bin\docker.exe`。
- daemon 未起时 `docker info` 报 `npipe://... not found` → 先 `Start-Process "C:\Program Files\Docker\Docker\Docker Desktop.exe"`（约 20s up）。
- compose 中 `postgres` 服务常无 `restart: unless-stopped`，机器/Docker 重启后需手动再起。

## 可复用命令
```
# PowerShell
Start-Process "C:\Program Files\Docker\Docker\Docker Desktop.exe"
& "C:\Program Files\Docker\Docker\resources\bin\docker.exe" compose -f <项目>/docker-compose.yml up -d postgres
```

## 遗留疑问
- 是否有非 Desktop 的 daemon 启动方式以避免 GUI 依赖？
