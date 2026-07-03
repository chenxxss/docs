# 设计院数据资产保护平台 — 文档仓库

本仓库包含设计院数据资产保护平台的完整架构文档、部署手册、运维指南和答辩材料。

## 仓库结构

```
docs/
├── README.md                       # 本文件 — 文档仓库总说明
├── architecture/
│   └── architecture-overview.md    # 五层架构总览
├── 完整架构目录规划.md               # 完整架构目录规划（含五层详细目录、数据流、部署顺序）
└── ...                             # 后续补充（架构图、预算规划、部署手册、答辩PPT大纲等）
```

## 平台五层架构简述

| 层级 | 名称 | 核心组件 |
|------|------|---------|
| Layer0 | 运维监控层 | Jenkins + Prometheus/Grafana + ELK |
| Layer1 | Web 应用层 | Nginx + PHP(Laravel) + Samba |
| Layer2 | 存储层 | ZFS 存储池 + NFS 共享 |
| Layer3 | 数据库层 | MySQL MGR 一主两从 |
| Layer4 | 代码与配置版本控制层 | Git (GitHub) + 制品管理 |

## 相关仓库

本仓库是 `chenxxss/docs`，配套的其他仓库包括：

- `chenxxss/--1`  — LNMP 配置文件（nginx/php-fpm/mysql）
- `chenxxss/asset-platform` — Laravel 管理平台源码（待创建）
- `chenxxss/deploy-scripts` — 自动化部署/备份/巡检脚本（待创建）
