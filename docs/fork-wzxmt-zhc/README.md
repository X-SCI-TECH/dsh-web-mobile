# fork wzxmt-zhc/dsh-web-mobile 对账与摘抄

> 本目录是对社区 fork [wzxmt-zhc/dsh-web-mobile](https://github.com/wzxmt-zhc/dsh-web-mobile) 的专项工作文档：记录对账快照、摘抄决策与推进日志。**接手这个专项时先读本文件恢复上下文，再读 `backlog.md` 挑任务，最后看 `log.md` 看做过什么。**

## 接手协议

1. **读本文件**（对账快照）→ 恢复「fork 现状、差距、坑」的上下文。
2. **读 `backlog.md`** → 看摘抄清单的状态，挑「待摘」条目。
3. **recall 记忆**：开工前 recall 「dsh-web-mobile fork」相关教训（评估结论沉淀在 mnemopi `f4ae9279861fc778` 等条目里；记忆存教训/结论，本文档存进度/决策，两边不重复）。
4. **动手前重新对账**（fork 在独立推进，快照会过时）：

   ```sh
   git fetch https://github.com/wzxmt-zhc/dsh-web-mobile.git main:refs/remotes/fork/main
   git log --oneline origin/main..fork/main          # fork 独有
   git log --oneline fork/main..origin/main          # fork 缺失
   ```

   fork **没有配置为 remote**（防误推），`refs/remotes/fork/main` 是临时引用，重开工作区后需重新 fetch。
5. **每完成一步**：在 `log.md` 追加条目（日期 + 做了什么 + 结果），并同步更新 `backlog.md` 对应行的状态。完成一项摘抄后按项目惯例跑 `pnpm verify && pnpm test:core && pnpm build`。

## 对账快照（2026-09-05）

| 项 | 值 |
|---|---|
| fork 分支 | 仅 `main` |
| fork head | `2ff7976` · v2.5.9（feat: 适配 dsh-file-viewer 移动端） |
| 上游 head（对账时） | `75d2311`（package.json 仍标 2.3.0，PR #47 之后未发版） |
| merge-base | `ebbd18b`（round 8：触屏断点守卫批次，v2.5.5） |
| fork 落后上游 | 15 commits（PR #46 整批、#43/#44、#41、settings 工具栏结构锚定、PR #47） |
| 宿主目标差异 | fork 针对 **DSH 0.1.2-alpha.1** 开发；本仓库实测基线 0.1.1-rc.2 / 0.1.2-rc.1 |

### fork 独有功能（上游没有）

| 功能 | fork 版本 | 摘抄评估 |
|---|---|---|
| 会话删除（行菜单 + 宿主 `/api/mobile-nav.session.delete`） | v2.1.0 | 🥉 参考不摘（依赖宿主私有内部 + 按标题反查 sessionId，见 backlog） |
| 压缩 header 大小写修复（`headerValue`） | v2.5.1 后 | 🥇 直接摘 |
| composer 选择器适配 contentEditable（alpha.1） | v2.5.6/7 | 🥈 对账合并（与 PR #47 的 `data-composer-input` 用法方向不同） |
| 消息操作 tooltip 残留修复 | v2.5.8 | 🥇 直接摘 |
| dsh-file-viewer 移动端适配 | v2.5.9 | 🥈 按需摘 |

### 已知坑

- **fork 提交作者统一为 `dev@dsh-web-mobile.local`**（历史被重写/压缩过），cherry-pick 时用 `--author` 处理。
- **composer 选择器两边各自改过**（fork 对 alpha.1、上游对 rc.1），合并 CSS 会冲突在生成文件 `lib/types/client/styles/*.css.d.ts`——按惯例 `pnpm build` 重建 lib 再 `git add`，不手改 d.ts。
- fork 的 alpha.1 适配不能整块抄：本仓库 PR #47 的 `data-composer-input` 是 `:not(:has(...))` 排除型，fork 是正向扩展型，两套需对账合并。
- 摘抄项落地后，若新增 `data-mobile-nav` 标记或新选择器，记得同步 misc.css.ts 桌面隐藏块清单与 README changelog（项目惯例）。

## 文档索引

- `backlog.md` — 摘抄清单：每项的 fork 位置、决策、状态、注意点。
- `log.md` — 按时间的推进日志。
