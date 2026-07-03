# 设计院数据资产保护平台 — 架构总览

## 总体架构

```
┌──────────────────────────────────────────────────────────────┐
│  Layer0: 运维监控层                                            │
│  Jenkins + Prometheus/Grafana + ELK                           │
├──────────────────────────────────────────────────────────────┤
│  Layer1: Web 应用层                                           │
│  Nginx + PHP(Laravel) + Samba                                 │
├──────────────────────────────────────────────────────────────┤
│  Layer2: 存储层                                               │
│  ZFS 存储池 + NFS 共享                                        │
├──────────────────────────────────────────────────────────────┤
│  Layer3: 数据库层                                             │
│  MySQL MGR 一主两从                                           │
├──────────────────────────────────────────────────────────────┤
│  Layer4: 代码与配置版本控制层                                    │
│  Git (GitHub) + 制品管理                                      │
└──────────────────────────────────────────────────────────────┘
```

## 各层职责
- Layer0: 监控所有节点的运行状态、自动化部署与备份、日志集中审计
- Layer1: 对外提供 Web 管理后台、SMB 文件共享服务
- Layer2: 设计院图纸/BIM/文档的集中存储、冷热分层、快照保护
- Layer3: 业务数据库高可用集群，自动故障切换
- Layer4: 配置管理、版本控制、文档归档

## 技术选型
- 操作系统: openEuler 24.03 (Layer1) + Ubuntu Server 24.04 (Layer2)
- Web 服务器: Nginx
- 后端语言: PHP 8.3
- 应用框架: Laravel
- 文件系统: ZFS
- 文件共享: Samba (SMB)
- 数据库: MySQL 8.0 Group Replication
- CI/CD: Jenkins
- 监控: Prometheus + Grafana
- 日志: Elasticsearch + Logstash + Kibana

## 部署规划

本平台采用 VMware 虚拟化技术部署在一台物理机上，物理机配置为 16GB 内存 / 1TB 磁盘，CPU 为 Intel i7。所有业务层均运行在 VMware 虚拟机之上，实现资源隔离和灵活管理。

Layer1 Web 应用层运行 openEuler 24.03 操作系统，分配 4 个 vCPU、8GB 内存，系统盘 20GB 用于操作系统和 LNMP 环境，额外挂载 200GB 数据盘用于存放 Samba 共享文件和备份数据。Layer2 存储层运行 Ubuntu Server 24.04，分配 2 个 vCPU、4GB 内存，挂载 200GB 专用磁盘构建 ZFS 存储池，实现设计院图纸、BIM 模型和文档资料的集中存储、冷热分层归档和定时快照保护。Layer3 数据库层的 MySQL MGR 三节点采用 Docker 容器方式在同一台 VM 或分散部署模拟，实现一主两从的高可用集群架构，利用 MySQL Group Replication 自动进行故障切换和数据一致性同步。Layer0 运维监控层的 Jenkins、Prometheus/Grafana、ELK Stack 同样采用 Docker 方式部署，充分利用有限的硬件资源。Layer4 版本控制层依托 GitHub 远程仓库，本地各节点仅保留 Git 客户端和工作目录。

整体部署策略为"一台物理机承载全部，VMware 做资源切分，Docker 做服务编排"，既满足设计院数据资产保护平台的功能验证和演示需求，也为未来生产环境水平扩展打下基础。
