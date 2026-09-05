# 推进日志

> 按时间倒序（最新在上）。每条：日期 + 做了什么 + 结果/遗留。做完一步就记，别攒到结尾。

## 2026-09-05

- **建立本框架**：`docs/fork-wzxmt-zhc/`（README 对账快照 + backlog 摘抄清单 + 本日志），确立接手协议：恢复上下文读 README → 挑任务读 backlog → 推进记 log。
- **完成首次对账**（结论已写入 README/backlog）：
  - fork 仅 `main` 分支，head `2ff7976` v2.5.9；merge-base `ebbd18b`（round 8）；落后上游 15 commits；fork 针对宿主 0.1.2-alpha.1，本仓库基线 rc.1/rc.2。
  - fork 独有：会话删除、压缩 headerValue 大小写修复、alpha.1 composer 适配、tooltip 残留修复、file-viewer 适配。
  - 摘抄三档评估：🥇 直接摘（headerValue / tooltip / 三处 textarea 锚点）；🥈 对账合并（composer 集群选择器、file-viewer 全套）；🥉 参考不摘（会话删除）。
- **评估结论已入记忆**：mnemopi `f4ae9279861fc778`（三档摘抄结论）、`2bef97326b58f9ed`（fork 独立发版线概况）。
- 本地已有临时引用 `refs/remotes/fork/main`（重开工作区后需重新 fetch，命令见 README 接手协议）。
- **未动手**：所有摘抄条目均为「待摘」，用户当前指示是只看不动。
