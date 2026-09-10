# 面向 SRE 的 CI/CD 课程目录

> 调研对象：优点知识（youdianzhishi.com）+ 小乙运维（小乙运维杂货铺）
> 调研时间：2026-09-03
> 产出：①调研摘要　②SRE 视角的 CI/CD 课程目录

---

## 一、调研摘要

### 1.1 平台与讲师

- **优点知识（youdianzhishi.com）**：专注云原生实战的视频课程平台，官方文档库为 `docs.youdianzhishi.com`。课程线覆盖 Kubernetes、Prometheus、Go 运维开发、DevOps、CI/CD 等方向。
- **小乙运维（小乙运维杂货铺）**：优点知识主力讲师/内容作者。定位为 K8s / Prometheus / CI/CD / Golang 运维开发专家，现为北京鉴智科技运维负责人，负责 400+ 台 GPU 节点集群、AIOnK8s / Volcano 离线训练平台。

### 1.2 代表课程（与 SRE / CI/CD 相关）

| 课程 | 技术栈/重点 | 规模/备注 |
|------|-------------|-----------|
| 7 模块大运维平台开发实战 | Go + Vue3 + K8s + GitLab CI/CD + 服务树 + Prometheus 监控 | 约 1587 节，88GB，含蓝绿/金丝雀/灰度发布 |
| Go 运维开发训练营（1+2 期） | Go 运维开发、并发、网络、可观测性、Operator | 完结，五阶段路径 |
| Kubernetes 进阶训练营（1-4 期） | K8s 部署/调度/网络/排障 | 入门到进阶手册 |
| Kubernetes 开发课 | Client-go、Operator、准入控制器、调度器源码 | 源码级二次开发 |
| Kubernetes 网络训练营 | CNI 插件原理与实战 | 网络难点专项 |
| Prometheus 入门到实战 | 指标采集、TSDB、告警 | 监控体系 |
| DevOps 训练营（2023 版） | Jenkins / Terraform / 端到端工具链 | 17.4h+，更新中 |
| 2 天搞定微服务 CI/CD 实践 | 微服务全链路 CI/CD | 入门级实战 |
| Spinnaker 实践 | 持续交付 / 多云部署 | 发布平台 |

### 1.3 小乙运维 CI/CD 技术栈要点（可用于课程设计锚点）

- **CI 侧**：GitLab CI + Harbor（镜像仓库）+ Kaniko（无特权构建）。
- **CD 侧**：Tekton、Argo CD、Kruise Rollout；多环境多泳道发布流程。
- **发布策略**：蓝绿、金丝雀、分阶段灰度、滚动发布。
- **配套能力**：服务树 + CMDB、RBAC/审计、Prometheus 监控告警、值班工单。

### 1.4 社区活跃度

- **无官方量化指标**（平台未公开课程学习人数、发帖量等统计数据）。
- 可观测活跃信号：
  - B 站频道「小乙运维杂货铺」持续更新（截至 2025 年前后仍在发「5 周年回顾」「AI/GPU 运维」等视频）。
  - Go 语言中文网（studygolang）发布「7 模块大运维平台」项目介绍；Gitee 维护 `devops-guidebook`（运维进阶宝典）开源资料。
  - 2025 年前后 SegmentFault 出现「Go 运维开发训练营第 2 期」复盘文章。
  - 课程持续迭代：K8s 训练营已到第 4 期，大运维平台课程已推进到「第 8 模块」。
- **结论**：社区以「课程 + B 站 + 开源资料 + 答疑群」的闭环形式运转，活跃度中等偏上但偏垂直、无公开数字可考；信息主要散落在第三方资源站与自媒体，官方站本身依赖 JS 渲染、课程章节未公开抓取。

---

## 二、面向 SRE 的 CI/CD 课程目录（设计稿）

> 设计取向：以「变更的可靠交付」为主线，把 CI/CD 从「能跑通流水线」提升到「可观测、可回滚、可审计、可规模化」的 SRE 工程能力。

### 课程定位

- **目标人群**：具备基础 Linux / 容器 / K8s 概念的运维工程师、DevOps 工程师，向 SRE 进阶者。
- **前置条件**：Linux 基础、Docker 基础、K8s 基础概念、Shell（Go/脚本二选一）。
- **产出**：一套生产可用的多环境、多泳道、GitOps 化、带灰度与可观测性的持续交付体系（含 Istio 服务网格流量治理与渐进式交付）。

---

### 模块 0　导论：SRE 视角下的变更与交付

| 章 | 主题 | 要点 | 实验 |
|----|------|------|------|
| 0.1 | 为什么 SRE 要关心 CI/CD | 变更即风险；发布与 SLO/SLI 的关系；MTTR/变更失败率 | 无 |
| 0.2 | 交付链路全景 | 从 commit 到生产的价值流；交付效率四指标（DORA） | 绘制团队现状价值流图 |
| 0.3 | 课程技术栈与实验环境 | GitLab + Harbor + Kaniko + Argo CD + Istio + Kind/Minikube | 搭建统一实验集群（含 Istio sidecar 注入） |

---

### 模块 1　版本控制与协作规范（Git 深入使用）

> 取向：以「日常工作用得最多、也最容易出错」的操作为主线，聚焦 Git 的使用方法与工程约定，不深入对象模型、指针原理等底层机制。

| 章 | 主题 | 要点 | 实验 |
|----|------|------|------|
| 1.1 | Git 环境与基础操作 | 身份配置（user.name/email）、常用 alias 别名、core.editor、凭证管理（SSH/credential）；init 与 clone；工作区/暂存区/本地仓库三态的日常含义（只讲操作）；status 查看状态 | 配置别名与编辑器，克隆远程仓库并完成首次提交 |
| 1.2 | 日常提交工作流 | add（含 `-p` 分块暂存）、commit（`-m`/`-a`）、diff（工作区 vs 暂存 vs 已提交）、log 常用参数；提交粒度与一条可读的提交信息怎么写 | 用 `git add -p` 精细控制暂存内容，写出规范提交 |
| 1.3 | 撤销、回退与误操作找回 | 撤销工作区改动（`restore`/`checkout --`）、取消暂存（`restore --staged`/`reset`）、改写最近提交（`--amend`）；`reset --soft/--mixed/--hard` 的区别与适用场景；用 `revert` 安全撤销已推送提交；`reflog` 找回丢失的 commit/分支；`cherry-pick` 挑选提交 | 依次演练 reset/revert/amend，并用 reflog 找回一次“误删”的分支 |
| 1.4 | 分支的日常管理 | 创建/切换/删除（branch、switch、checkout）；查看分支（`-a`/`-v`/`--merged`）；本地与远程分支的关联（tracking）；重命名分支；删除本地/远程分支 | 建立 feature 分支体系，练习本地与远程分支的创建、关联与清理 |
| 1.5 | 合并、变基与冲突解决 | merge（fast-forward 与 `--no-ff` 三方合并的区别）；rebase 与交互式 `rebase -i`（合并/调整/删除提交）；冲突定位与解决（`--ours`/`--theirs`、mergetool）；merge 冲突 vs rebase 冲突；何时 merge、何时 rebase 的团队约定 | 制造并解决一次 merge 冲突与一次 rebase 冲突，用 `rebase -i` 整理提交历史 |
| 1.6 | 远程仓库协作 | remote 管理（add/remove/show/rename）；fetch 与 pull 的本质区别；push 与 upstream 跟踪；force push 的风险与 `--force-with-lease` 安全强推；多远端（镜像仓库） | 双人协作模拟：fetch+merge、`pull --rebase`、安全强推演练 |
| 1.7 | 标签与版本发布 | 轻量标签 vs 附注标签（annotated）；打标签/删除/推送标签；语义化版本 semver 与 release 流程；基于 tag 触发制品构建 | 为版本打附注标签并推送，模拟基于 tag 触发一次发布 |
| 1.8 | 暂存 stash 与临时工作 | stash 使用场景（切分支前临时保存未完成工作）；save/pop/apply/list/drop；`-u` 包含未跟踪文件 | 在功能开发中 stash 暂存并切到紧急修复分支，处理完后再恢复 |
| 1.9 | 历史查询与问题定位 | log 高级过滤（作者/时间/文件/关键字、`--graph`/`--oneline`）；blame 定位代码归属；bisect 二分定位引入 bug 的提交；show 查看某次提交内容 | 用 `git bisect` 二分定位一个故意引入的 bug，用 blame 追溯关键代码 |
| 1.10 | 提交规范与工具链 | Conventional Commits（feat/fix/chore/...）；commitlint 校验；Commitizen 交互式提交；自动生成 changelog 与语义化版本联动 | 安装 commitlint + Commitizen，规范提交并自动生成 changelog |
| 1.11 | 分支策略与合并模型 | GitHub Flow / GitLab Flow / GitFlow / trunk-based 的差异与适用场景；PR/MR 流程与 Code Review；受保护分支、强制 review、CI 门禁 | 配置受保护分支 + 强制 review，走完一次 PR/MR 合并 |
| 1.12 | 仓库结构与配置分层 | 应用仓库与部署仓库分离；monorepo vs polyrepo 取舍；多环境配置管理（config 分层、环境目录）；submodule/subtree 的使用场景与坑；Git LFS 大文件 | 拆分 app 与 deploy 仓库，用目录结构/Kustomize 管理多环境配置 |
| 1.13 | Git Hooks 与协作门禁 | 客户端 hooks（pre-commit、commit-msg、pre-push）常见场景；husky / pre-commit 框架；与 CI 门禁的分工；团队内落地钩子 | 编写 pre-commit 钩子做本地格式校验，pre-push 钩子跑快速测试 |

---

### 模块 2　构建与制品管理

| 章 | 主题 | 要点 | 实验 |
|----|------|------|------|
| 2.1 | 镜像构建最佳实践 | 多阶段构建、层缓存、非 root、镜像瘦身 | 优化一个 Go 应用镜像体积 |
| 2.2 | 无特权构建：Kaniko/BuildKit | 为何避免 DinD；Kaniko 原理与落地 | GitLab Runner + Kaniko 构建 |
| 2.3 | 制品仓库 Harbor | 制品治理、漏洞扫描、镜像签名、代理缓存 | 部署 Harbor + 策略 |
| 2.4 | 供应链安全基础 | SBOM（Syft）、签名（Cosign）、签名验证 | 生成并校验 SBOM |

---

### 模块 3　持续集成（CI）

| 章 | 主题 | 要点 | 实验 |
|----|------|------|------|
| 3.1 | GitLab CI 核心模型 | Pipeline/Job/Stage/Artifact/Cache；`.gitlab-ci.yml` | 编写首个多阶段流水线 |
| 3.2 | 质量门禁：测试与静态检查 | 单元/集成测试、覆盖率门禁、lint、安全扫描 | 配置测试 + lint + 覆盖率门槛 |
| 3.3 | CI 效率工程 | 缓存、并行、增量测试、runner 治理 | 优化构建时长 |
| 3.4 | CI 与分支策略联动 | 自动环境、preview、阻断合并 | merge request pipeline |

---

### 模块 4　持续交付与 GitOps（CD）

| 章 | 主题 | 要点 | 实验 |
|----|------|------|------|
| 4.1 | 声明式交付：Helm/Kustomize | 渲染差异、values 分层、环境隔离 | 用 Kustomize 管理多环境 |
| 4.2 | Argo CD 核心 | Application、Sync 策略、差异可视化、自动同步 | 部署 Argo CD 并接管应用 |
| 4.3 | 多环境与多泳道 | 环境拓扑、泳道（lane）隔离、环境变量/配置管理 | 搭建 dev/staging/prod 三级环境 |
| 4.4 | 回滚与漂移治理 | 手动/自动回滚、drift detection、自我修复 | 模拟配置漂移并自动纠正 |

---

### 模块 5　发布策略与 Istio 流量治理

| 章 | 主题 | 要点 | 实验 |
|----|------|------|------|
| 5.1 | 滚动发布与 Pod 就绪 | 就绪探针、maxSurge/maxUnavailable、preStop 优雅下线 | 滚动发布零中断演练 |
| 5.2 | 蓝绿发布 | 双版本切换、流量瞬间切换与快速回滚 | Argo Rollouts 蓝绿 |
| 5.3 | 金丝雀与渐进式交付 | 按权重/按 header 灰度；自动指标判停 | Argo Rollouts 金丝雀 + Analysis |
| 5.4 | 分阶段灰度与特性开关 | 用户分群、Feature Flag、运行时配置 | 集成 Flag 实现灰度 |
| 5.5 | Istio 基础与流量模型 | VirtualService、DestinationRule、Gateway、Envoy sidecar | 部署 Istio 并接管服务流量 |
| 5.6 | Istio 权重/头路由灰度 | 基于 HTTP header、cookie、来源的精准灰度分流 | 按 header 定向灰度到新版本 |
| 5.7 | 流量镜像与故障注入 | 影子流量验证、延迟/熔断注入演练 | 镜像流量 + 故障注入压测 |
| 5.8 | Flagger + Istio 自动化渐进交付 | 指标驱动的自动放量与回滚 | Flagger 自动金丝雀 + 自动回滚 |

---

### 模块 6　发布可观测性与变更反馈

| 章 | 主题 | 要点 | 实验 |
|----|------|------|------|
| 6.1 | 发布前中后的指标体系 | SLI/SLO、错误预算、发布窗口指标 | 定义服务的 SLI 与阈值 |
| 6.2 | 变更关联监控与日志 | 发布版本与指标/日志/链路关联 | 发布期间对照指标看板 |
| 6.3 | 自动回滚判定 | 基于 Prometheus 指标的自动 abort/rollback | 指标超阈触发自动回滚 |
| 6.4 | 发布审计与复盘 | 变更单、审计日志、事后无责复盘（blameless） | 生成发布审计报告 |
| 6.5 | Istio 可观测性接入 | Envoy sidecar 指标、访问日志、分布式追踪（Jaeger/Zipkin） | 打通网格内金丝雀与基线版本的对比观测 |

---

### 模块 7　安全与合规

| 章 | 主题 | 要点 | 实验 |
|----|------|------|------|
| 7.1 | 密钥与凭据管理 | Vault / 密钥注入、避免硬编码 | GitLab CI 密钥变量 + Vault |
| 7.2 | 流水线权限与 RBAC | 最小权限、环境保护、审批门禁 | 配置生产环境部署审批 |
| 7.3 | 合规与审计追踪 | 部署审计链、镜像来源可追溯、策略即代码（OPA） | OPA 策略拦截不合规部署 |
| 7.4 | Istio 服务网格安全 | mTLS（PeerAuthentication）、授权策略（AuthorizationPolicy） | 配置命名空间级 mTLS + 精细化授权 |

---

### 模块 8　端到端实战项目：多环境多泳道发布平台

| 章 | 主题 | 要点 | 实验 |
|----|------|------|------|
| 8.1 | 需求与架构设计 | 目标：一套 Go 微服务完成全链路自动化交付 | 架构图 + 组件选型 |
| 8.2 | 打通 CI 全链路 | commit → 构建 → 扫描 → 制品入库 | 完成 CI 阶段 |
| 8.3 | 打通 CD 全链路 | 制品 → GitOps 变更 → Argo CD 同步 | 完成 CD 阶段 |
| 8.4 | Istio 灰度与回滚演练 | Istio 权重/头路由金丝雀 + 指标判定 + 一键回滚 | 端到端故障演练 |
| 8.5 | 平台化封装与复盘 | 工单/审批/审计整合，交付成果答辩 | 提交最终交付体系 |

---

### 建议课时与考核

- **总课时**：约 50–60 小时（含实验），分 8 模块、约 50 章（其中模块 1 Git 深入使用占 13 章）。
- **考核**：每模块配套实验 + 模块 8 综合项目；以「能否完成一次含 Istio 灰度与自动回滚的生产级发布」为通过标准。
- **对标参考**：小乙运维的 GitLab CI + Harbor + Kaniko + Argo CD/Kruise Rollout + Prometheus 技术栈，叠加 Istio 服务网格（Flagger 渐进交付）与多环境多泳道发布方法论。

---

## 三、主要参考来源

- 优点知识官方文档库：https://docs.youdianzhishi.com/
- 优点知识官网：https://youdianzhishi.com/
- 《小乙运维杂货铺：基于 Go+Vue+K8s+CICD 架构的七大模块一体化大运维平台开发实战》：https://www.cbish.com/xue/29542.html
- 小乙运维杂货铺（程序员客栈主页）：https://www.proginn.com/wo/1154290
- 优点知识 DevOps 训练营课程页：https://youdianzhishi.com/web/course/1040
- 优点知识 2 天搞定微服务 CI/CD 实践：https://youdianzhishi.com/web/course/1024
- Go 运维开发训练营第 2 期复盘（SegmentFault）：https://segmentfault.com/a/1190000048096096
- 7 模块大运维平台介绍（Go 语言中文网）：https://studygolang.com/topics/18683
