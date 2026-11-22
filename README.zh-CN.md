# Chợ Việt

🌐 [Tiếng Việt](README.md) · [English](README.en.md) · **简体中文** · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Deutsch](README.de.md)

新一代电商平台，完全由多智能体（multi-agent）AI 系统开发。

![build](https://img.shields.io/badge/build-passing%20%28localhost%29-brightgreen) ![uptime](https://img.shields.io/badge/uptime-99.99%25%20%28localhost%29-brightgreen) ![database](https://img.shields.io/badge/database-Google%20Sheets-34A853) ![code review](https://img.shields.io/badge/code%20review-0%25-red) ![license](https://img.shields.io/badge/license-MIT-blue)

## 概述

Chợ Việt 是一个由 6 个 AI 智能体在 14 天内构建的电商平台，总成本 523.41 美元。项目目标是达到与本地区领先电商平台相当的规模，同时缺陷率更低（根据 PM-agent 的内部评估）。

2026 年 11 月 20 日，在收到社区关于代码质量的大量反馈后，项目团队决定公开全部源代码，以确保透明度。

## 关键指标

| 指标 | 数值 |
|---|---|
| 微服务数量 | 14 |
| 代码总行数 | 184,920 |
| 经人类阅读的代码行数 | 0 |
| 并发用户（压力测试） | 2,000,000¹ |
| 并发用户（生产环境，峰值） | 4² |
| 开发成本 | 523.41 美元³ |
| 融资目标 | 2,000,000 美元 |
| 已融资 | 0 越南盾 |

¹ 在 3 个 Google Chrome 标签页上模拟。
² 包括 Googlebot。
³ 不含电费。

## 系统架构

![Chợ Việt](docs/kien-truc.png)

| 组件 | 技术 | 备注 |
|---|---|---|
| 数据库 | Google Sheets | 每个服务独占一个工作表（database-per-service） |
| 索引 | Ctrl+F | |
| 事务 | 操作约定 | 禁止同时编辑同一单元格 |
| 消息队列 | Google Forms | 批处理，每天一次（上午） |
| 缓存 | 第 3 个 Chrome 标签页 | 需 24/7 保持打开 |
| 支付 | 银行转账 | 由创始人通过 Zalo 人工确认 |
| 监控 | 创始人的家属 | 通过电话告警 |
| CI/CD | 智能体直接推送到 `main` | 无审查 |
| 备份 | Google Drive | |
| 扩展 | 基于文件的分片 | 达到 1000 万单元格上限时新建 `DB_CHUAN_FINAL_v{n+1}` |

## 设计决策

### ADR-001：采用 Google Sheets 作为主数据库

**背景。** v3 版本使用 Excel 文件 `DB_CHUAN_FINAL_v3_(1).xlsx`。11 月 18 日，当 4 名用户同时访问时，文件被锁定，导致系统全面宕机（见[事故复盘](docs/postmortem-2026-11-18.md)）。

**备选方案。**

| 方案 | 结论 |
|---|---|
| PostgreSQL | 否决。需要学习 SQL |
| Excel | 否决。11 月 18 日事故 |
| Google Sheets | 采用 |

**影响。** 支持多用户同时编辑。每个文件上限 1000 万个单元格。

### ADR-002：取消代码审查流程

**背景。** 代码审查持续发现缺陷，拖慢发布进度。

**决策。** 取消代码审查，参考 SpaceXAI 的运营模式。

**影响。** 发现的缺陷数量减少 100%。

## 性能

| 环境 | 并发用户 | p99 延迟 | 订单/秒 | 错误率 |
|---|---|---|---|---|
| 压力测试（localhost，3 个 Chrome 标签页） | 2,000,000 | 41 ms | 186,402 | 0% |
| 生产环境（11 月 18 日） | 4 | — | 0 | 100% |

## 服务等级协议（SLA）

Chợ Việt承诺在localhost环境下实现99.99999%的运行时间。生产环境不在承诺范围内。

## 安装

**系统要求**

- Node.js 18 或更高版本
- 拥有 `DB_CHUAN_FINAL_v4` 编辑权限的 Google 账号（请通过 Zalo 联系创始人）
- 至少 3 个 Google Chrome 标签页
- 电脑不得进入睡眠模式

```bash
git clone https://github.com/harry-brogrammer/cho-viet
cd cho-viet
npm install        # 预计耗时 40 分钟。出现 312 个警告属于正常现象。
cp .env.example .env
npm run dev
```

## 路线图

- [x] 实现与 Shopee 同等的功能
- [x] 数据库从 Excel 升级到 Google Sheets
- [x] 开源
- [ ] 试读源代码（由人类执行）
- [ ] 达到 5 名真实用户
- [ ] 迁移到 PostgreSQL（状态：智能体评估为“困难”）

## 贡献

参见 [CONTRIBUTING.md](CONTRIBUTING.md)（仅提供越南语版本）。

## 许可证

MIT。AI 生成源代码的所有权目前尚未确定。

---

> 本译文由 Translator-agent 生成，未经审查（见 ADR-002）。
