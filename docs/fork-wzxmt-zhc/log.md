# 推进日志

> 按时间倒序（最新在上）。每条：日期 + 做了什么 + 结果/遗留。做完一步就记，别攒到结尾。

## 2026-09-05

- **🥇-2 已摘：tooltip 残留修复**（提交 190fbbf）：`@media (hover:none),(pointer:coarse)` 内压制 `[data-phase] :is([class*="_bubble"],[role="tooltip"])`。**摘抄时放宽 fork 的作用域**——CDP 实测 fork 原选择器（`_actions` 祖先限定）在我们宿主 0.1.1-rc.2 不命中真实 bubble（挂 `gdEzaW_userRow` 内），放宽后 10 个真实宿主 tooltip 全压制、桌面 1440 不受影响（探针 `.local-tests/tooltip-probe.mjs` 8/8）。
- **发现宿主 session 恢复的探针坑**：0.1.1-rc.2 的 persist store（dsh-client-runtime `createSnapshotStore` persist=localStorage key `dsh.sessions.current`）在 boot 早期把内存空快照写回 localStorage，**会覆盖 addScriptToEvaluateOnNewDocument 的预注入**——正确做法是 boot 完成后再写 localStorage 并 reload，然后轮询 `data-phase="active"`（等异步恢复，1500ms sleep 不够，实测需轮询）。恢复成功条件：sessionId 必须属于 dsh web 进程 cwd 的 sessions 目录（本机 3080 的 cwd 是 home，不是 dsh-mobile-nav）。
- **🥇-1 已摘：compress.ts headerValue 大小写修复**（提交 d25d7d3）：headerValue 纯函数导出 + isDeferrable / varyWithAcceptEncoding / content-length 删除改大小写不敏感；新增 tests/compress.test.ts 3 用例（全量 60/60 绿）；verify + build + diff --check 通过。
- **建立本框架**：`docs/fork-wzxmt-zhc/`（README 对账快照 + backlog 摘抄清单 + 本日志），确立接手协议：恢复上下文读 README → 挑任务读 backlog → 推进记 log。
- **完成首次对账**（结论已写入 README/backlog）：
  - fork 仅 `main` 分支，head `2ff7976` v2.5.9；merge-base `ebbd18b`（round 8）；落后上游 15 commits；fork 针对宿主 0.1.2-alpha.1，本仓库基线 rc.1/rc.2。
  - fork 独有：会话删除、压缩 headerValue 大小写修复、alpha.1 composer 适配、tooltip 残留修复、file-viewer 适配。
  - 摘抄三档评估：🥇 直接摘（headerValue / tooltip / 三处 textarea 锚点）；🥈 对账合并（composer 集群选择器、file-viewer 全套）；🥉 参考不摘（会话删除）。
- **评估结论已入记忆**：mnemopi `f4ae9279861fc778`（三档摘抄结论）、`2bef97326b58f9ed`（fork 独立发版线概况）。
- 本地已有临时引用 `refs/remotes/fork/main`（重开工作区后需重新 fetch，命令见 README 接手协议）。
- **未动手**：所有摘抄条目均为「待摘」，用户当前指示是只看不动。
