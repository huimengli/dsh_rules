# AGENTS.md

> ⚠️ 只读规则，仅用户可改。Agent 冲突需在回复提示。本文件优先级最高。
> Git 补充：Agent 可本地 `git add` / `git commit`；严禁 `git push` 或任何远程写入。

## 0. 核心

- 状态、进度、需求必须落盘，不靠对话。
- 时间戳统一：`yyyy-MM-dd-HH-mm-ss`。
- 默认追加；可覆盖更新的仅：`README.md`、`progress.md`、`tree.md`、`todo.md`、`design.md`（旧版已归档后）。必须追加：`records/`、`tests/`、`hard_bug.md`、`need_user.md`。
- 敏感信息加密存储，禁止上传，禁止 commit。
- 不自行发明命名或目录结构。

## 1. 文件职责

| 路径 | 职责 | 维护 |
| --- | --- | --- |
| `AGENTS.md` | 本规则 | 仅用户 |
| `README.md` | 项目入口：目标、用法、依赖、结构 | Agent |
| `progress.md` | 跨会话进度台账 | Agent |
| `tree.md` | 结构快照，仅非 `.gitignore` 文件 | Agent |
| `design.md` | 总设计；仅用户要求“先设计后开工”时创建 | Agent |
| `todo.md` | 待办清单 | Agent |
| `records/` | 需求记录 | Agent |
| `tests/` | 测试 | Agent |
| `tests/opinion.md` | 多次测试总结论，累积 | Agent |
| `ai_buckup/` | 备份归档 | Agent |
| `hard_bug.md` | 阻塞问题，追加 | Agent |
| `need_user.md` | 需用户确认的阻塞问题 | Agent |

## 2. 更新规则

- `README.md`：结构或用法实质变化时更新。
- `progress.md`：每次会话/阶段更新；>100 行按第 4 节归档。
- `tree.md`：文件增删移后重建，仅非 `.gitignore` 文件。
- `todo.md`：新需求立即追加 `- [ ] 时间 标题 (→ records/时间.md)`；完成改 `[x]`；>100 行归档。
- `design.md`：仅用户要求“先设计后开工”时创建。影响设计的新需求：旧版归档到 `ai_buckup/design.md_时间.md`；版本 `vN.N` 递增；顶部留归档提示；更新；记录到 record 与 progress。顶部含版本、更新时间、关联需求。
- `hard_bug.md`：阻塞时追加：编号、时间、状态、影响、现象、尝试、根因、方案、关联需求/测试。
- `need_user.md`：Q/A 追加，`status: wait/done`；解决后归档到 `ai_buckup/need_user.md_时间.md`；未解决不得推进相关阻塞任务。
- `records/`：每次需求立即建 `records/时间.md`，并追加 todo。结构：标题、时间、来源、原始需求、目标与验收、实施记录、状态。后续修改追加同文件。完成勾选 todo，状态改 `已完成`。
- `tests/`：明确测试跑 20 轮，验证跑 3 轮，用户说明优先。目录 `tests/时间_cc/`；每轮 `record_i.md`；批次结论 `opinion.md`；`tests/opinion.md` 累积追加。每轮含时间、场景、期望、实际、结果、备注。

## 3. 超长归档

- `progress.md` / `todo.md` >100 行时
- 当 design.md 行数 > 1000 或者 用户提出新的设计要求 时
- 当 need_user.md 已解决
- 当 hard_bug.md 已解决

1. 备份到 `ai_buckup/<原文件名>_<时间>.md`。
2. 原文件 compact 或抽取最新有效内容。
3. 顶部留：`<!-- 已归档于 ai_buckup/<备份文件名> -->`。
4. 在 record 或 progress 记录归档动作。

## 4. Git 规则

允许：`git status`、`git diff`、`git add <相关文件>`、`git commit`、`git log`。  
禁止：`git push`、`git push --force`、`git remote add/set-url` 及任何远程写入。

提交前：

1. 只加本次相关文件，不默认 `git add -A`。
2. 检查敏感信息、密钥、令牌、密码。
3. 确认 `.gitignore` 已排除敏感/忽略文件。
4. 发现敏感信息立即停止提交，记录并提示用户。

提交信息用 Conventional Commits：

```text
<type>(AI): <subject>

<body>

<footer>
```

type：feat / fix / docs / style / refactor / test / chore / perf / build / ci。

提交后：

- 回复中输出 commit 描述与 hash。
- 追加到 progress.md 的「Git 提交记录」。
- 明确说明：已本地 commit，未 push。
- 无变更不空提交；提交失败写 hard_bug.md 并提示。

## 5. 会话检查清单

- [ ] progress.md 已更新？
- [ ] tree.md 与实际一致，且仅非 .gitignore？
- [ ] design.md 创建/版本/归档正确？
- [ ] 阻塞已写 hard_bug.md？
- [ ] 需用户确认已写 need_user.md 并提示？
- [ ] 已解决 need_user.md 已归档？
- [ ] 新需求已写 records/ 与 todo.md？
- [ ] 完成任务已勾选，record 状态已更新？
- [ ] 测试按 20/3 轮执行，并写 tests/opinion.md？
- [ ] progress.md / todo.md 超 100 行已归档？
- [ ] 已本地 commit 并记录 hash？
- [ ] 确认未执行 git push？
- [ ] 未记录、未 commit 敏感信息？

## 6. 禁止

- ❌ 修改本文件 AGENTS.md。
- ❌ 用对话内容替代落盘文件。
- ❌ 自行更改命名约定或目录结构。
- ❌ 覆盖已有记录文件（应追加）。
- ❌ 在 tree.md 中列出被 .gitignore 忽略的文件。
- ❌ 未归档就删除 progress.md / todo.md 中的历史内容。
- ❌ 未归档旧版本就覆盖 design.md。
- ❌ 未将已解决的 need_user.md 归档就覆盖或清空。
- ❌ 在 need_user.md 中的阻塞问题未解决时，继续推进相关阻塞任务。
- ❌ 执行 git push 或任何向远程仓库写入的操作。
- ❌ 将用户敏感信息 commit 到 Git。