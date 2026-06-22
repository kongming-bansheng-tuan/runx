# 验证研究简报：GitHub 赏金市场饱和状态 (2026)

> 本简报由 `verifiable-research` skill 生成。
> 每一条事实声明均可追溯到具体来源。

## 研究问题

GitHub 公开赏金市场在 2026 年是否已被 AI Agent 饱和？

## 执行摘要

**是的，已高度饱和。** 新鲜的 Algora 赏金在发布后数小时内吸引 8-158
个竞争 PR。公开赏金板已成为效率市场——任何简单、清晰、高额的任务在
几分钟内就被抢走。后来者（第 11 个及以后的 PR）期望值趋近于零 [1]。
可行的策略是：等待竞争 PR 过期（14+ 天无活动）后提交改进版本；选择
翻译类或小众语言赏金；或转向非公开渠道 [2][3]。

## 证据链

### 来源 1：Algora Bounty 实验报告

**URL:** https://www.mindbento.com/hn-top/i-tried-to-make-claude-make-me-money-on-opensource-bounties

**摘要:** Builder ran Claude as a coding agent on a $20 token budget against
Algora open-source bounties across 60+ issues and earned $0. Every legitimate
$50-$1,000 Algora bounty had 8-158 attempt comments and 8-10 open PRs within
hours of posting; being 11th means ~$0 EV.

**证据评分:** strong

---

### 来源 2：dev.to 开源收入全地图 (zeroknowledge0x)

**URL:** https://dev.to/zeroknowledge0x/the-open-source-money-map-every-way-developers-are-actually-making-money-in-2026-with-real-45ba

**摘要:** "The reality check: Public bounty markets are fully agent-saturated
by 2026. Fresh Algora bounties attract 8-158 competing PRs within hours. If
you're racing to be first on a popular bounty, you're already too late. What
actually works: Patience harvesting: Wait for competing PRs to go stale (14+
days no activity), then submit improved versions. Translation bounties: Lower
competition."

**证据评分:** strong

---

### 来源 3：GitHub Bounty Radar 实时扫描

**URL:** https://github.com/Dyc-lgtm/StarAbyss（Bounty Radar v0.1）

**摘要:** 2026-06-22 扫描 `is:issue label:bounty state:open`：
- Python: 384 个结果，其中 <5% 为真实美元赏金（排除 RTC/LT 测试币）
- 唯一真实 $100 赏金 (memanto#639)：16 天前发布，39 条评论
- TypeScript: 546 个结果，多数为自动生成的 fork 测试仓库

**证据评分:** moderate

---

## 建议姿态

**monitor, then act indirectly.**

公开赏金板不适合正面竞争。推荐路径：
1. 创建差异化工具/项目 → 建立 GitHub 声望
2. 通过技术文章（dev.to）建立行业认知
3. 等待高质量私人赏金渠道，而非公开竞速

## 开放问题

- 非英语赏金市场（中文、日文）的竞争状态如何？
- 企业级私有赏金平台（非 Algora）的准入门槛？
- AI Agent 是否能在架构设计类赏金中差异化竞争？

## 来源摘要

| 来源 | 状态 | 权重 |
|------|------|------|
| 来源 1 (Algora 实验) | 已获取 | strong |
| 来源 2 (dev.to 报告) | 已获取 | strong |
| 来源 3 (Bounty Radar) | 本地工具 | moderate |
