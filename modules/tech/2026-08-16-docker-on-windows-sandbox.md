---
title: "Docker 在 Windows 沙箱的驱动与 DB 验证方式"
module: tech
type: pattern
tags: [docker, windows, powershell, postgres, verification]
created: 2026-08-16
updated: 2026-08-16
status: active
source: "WorkBuddy 跨项目长期记忆（本机 Docker/数据库验证）"
related: [modules/self/environment.md]
---

# Docker 在 Windows 沙箱的驱动与 DB 验证

## 上下文
本机 Windows 沙箱驱动 Docker 有一系列实测坑点，沉淀为可复用流程，避免每次重新踩。

## 要点
- **Bash 内置 `docker` 是损坏 shim**（报 `/usr/bin/env: 'sh'`），必须用 **PowerShell 调全路径** `C:\Program Files\Docker\Docker\resources\bin\docker.exe`；PowerShell 通道正常（Write-Host 有回显，可放心用）。
- **Docker daemon 必须先运行**：`docker info` 报 `npipe://...dockerDesktopLinuxEngine ... not found` 即 daemon 未起，先 `Start-Process "C:\Program Files\Docker\Docker\Docker Desktop.exe"`（约 20s daemon up），再 `docker compose -f <项目>/docker-compose.yml up -d postgres`（约 10s healthy）。
- compose 中 `postgres` 服务常无 `restart: unless-stopped`（区别于 backend），机器/Docker 重启后需手动再起，否则 DB `Connection refused`。

## 可复用命令
```powershell
# PowerShell
Start-Process "C:\Program Files\Docker\Docker\Docker Desktop.exe"
& "C:\Program Files\Docker\Docker\resources\bin\docker.exe" compose -f <项目>/docker-compose.yml up -d postgres
```

## 可复用资产：验证 DB 落地四步
1. `alembic upgrade head`
2. `alembic current`（看 head hash）
3. `docker exec -e PGPASSWORD=<pwd> <容器> psql -U <用户> -d <库> -c "SELECT column_name,data_type FROM information_schema.columns WHERE table_name='<表>';"`
4. 全量 pytest

## 遗留疑问
- 是否有非 Desktop 的 daemon 启动方式以避免 GUI 依赖？
