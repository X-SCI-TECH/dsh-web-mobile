# 摘抄清单（backlog）

> 状态流转：待摘 → 进行中 → 已摘 ✅ / 跳过 ❌。每项摘抄完成时：更新状态 + 在 `log.md` 追加条目 + 跑 `pnpm verify && pnpm test:core && pnpm build`。
> 行内「fork 位置」给出 fork 提交 hash，动手前 `git show <hash> -- <path>` 取最新实现（fork 独立推进，hash 内容以 fetch 后的实际为准）。

## 🥇 直接摘（小而稳）

| 项 | fork 位置 | 决策 | 状态 | 注意点 |
|---|---|---|---|---|
| compress.ts 大小写修复（`headerValue`） | round 3 merge（`20d2ce2` 带入），`src/compress.ts` | 摘 | 已摘 ✅ d25d7d3 | 上游 `10f55da` 移植时漏带；对原始 `writeHead` 参数大小写不敏感查找（content-type / content-encoding / vary / content-length）。tests/compress.test.ts 3 用例覆盖。 |
| 消息操作 tooltip 残留修复 | `7e58824`，`src/client/styles/layout.css.ts` | 摘 | 已摘 ✅ 190fbbf | 门控 `(hover: none), (pointer: coarse)`。**摘抄时放宽了作用域**：fork 的 `[data-phase] [class*="_actions"] :is(...)` 在我们宿主（0.1.1-rc.2）不命中真实 bubble（实测 bubble 挂在 `gdEzaW_userRow` 内而非 actions 行），已去掉 actions 祖先限定；CDP 实测 8/8（含真实宿主 10 个 tooltip 元素全压制、桌面不生效）。 |
| 三处裸 textarea 锚点改 `textarea, [data-composer-input]` | `c246feb`：`git-chip-reparent.ts` / `stats-line.ts` / `debug.ts` | 摘 | 已摘 ✅ 3fc6c17 | 上游这三处是漏网的；宿主升 0.1.2（Lexical contentEditable）后裸 textarea 全部失效。机械替换，向后兼容（textarea 分支保留）。CDP 冒烟：0.1.1 上 stats 标记 + badge composer 字段均正常。 |

## 🥈 对账合并摘（不能整块抄）

| 项 | fork 位置 | 决策 | 状态 | 注意点 |
|---|---|---|---|---|
| composer 集群 `:has(textarea)` 正向加 `[data-composer-input]` | `7f47f9e`，`layout.css.ts` / `misc.css.ts` | 合并 | 已摘 ✅ e1ea61d | **对账结论：与 PR #47 不冲突**——#47 的 `:not(:has(...))` 用在 `_scroll` 内容规则（排除 composer 内部内容），fork 的正向 `:has(...)` 用在 `_card` 识别规则（找卡片本身），作用对象不同可共存。layout 24 处 + misc 2 处机械替换，另摘 misc 的 `[data-composer-placeholder]` 门控 2 条（0.1.2 空状态折叠）。CDP 11 断言全绿。 |
| dsh-file-viewer 移动端适配全套 | `2ff7976`：新 `file-viewer-compat.ts`（35 行）+ compat.css 106 行 + misc.css 12 行 + sidebar-swipe takeover 3 行 + phone-chrome 注册 2 行 | 按需 | 待摘 | 质量高（marker 门控、media 内、reduced-motion 保留），与 taskboard/ssh 的 `takeoverActive()` 同构。确认有用户用 dsh-file-viewer 再收；`dsfv-*` 前缀升级后对账。 |

## 🥉 参考不摘

| 项 | fork 位置 | 决策 | 状态 | 说明 |
|---|---|---|---|---|
| 会话删除（行菜单 + `/api/mobile-nav.session.delete`） | v2.1.0 系列（`3d877b7` 起）+ round 4 修复 | 不摘，参考 | ❌ 跳过 | 宿主侧 `as unknown as` 戳 `agents.store/detachEntered`、`sessions.store/detach` 私有内部（宿主升级即碎）；客户端克隆宿主 hashed class 注入菜单项、按标题文本反查 sessionId（重名按行位置消歧）；针对 0.1.2-alpha.1 服务形状。参考价值：错误码分级、`withTimeout(agent.whenIdle())` 停止顺序、jsonl backend 检查。 |

## 备选（方向性，未排期）

| 项 | 说明 | 状态 |
|---|---|---|
| 反向帮 fork 同步上游 15 commits（round 9） | PR #46/#47、#43/#44、#41、settings 工具栏锚定。fork 是独立发版线，同步需 fork 作者配合或走 PR。 | 未排期 |
