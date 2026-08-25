---
name: vibe-milestone
description: >-
  Runs the VCM milestone loop (Definition of Ready, implement, run tests, user
  acceptance, SSOT update, commit request) and the red-green bugfix loop. Use when
  implementing a milestone, adding a feature increment, fixing a bug, claiming work
  is done, or when the user says 按 VCM 做, Ready, 里程碑, 验收. Do not use for read-only
  questions, one-line copy edits, project bootstrap, or session handoff.
---

# VCM Milestone

一次只做一个里程碑。小改动（只读、单行文案、一次性探索）不要用本 skill，只守 `AGENTS.md` 四原则。启动项目用 `vibe-bootstrap`；换会话用 `vibe-handoff`。

动手前读 `PROGRESS.md`，需要定位时读 `ARCHITECTURE.md`，涉及用户可见行为时读 `DESIGN.md`。同一时刻只由本会话写 PROGRESS。

成功标准对照：方法论仓库 `examples/success-criteria.md`。坏 = 形容词；好 = 输入 + 操作 + 可观察输出 / 断言。

## Definition of Ready

未全部满足则只做定义 / 提问，不写功能代码：

- [ ] 成功标准可证伪
- [ ] `{test_cmd}` 已知（见 PROGRESS「运行与验证」）。没有 → 先做 M0，不做功能
- [ ] 范围不跨两个系统
- [ ] 用户可见取舍已问过，或明确是实现细节
- [ ] 大改前的 git 检查点已存在，或已拟定 `checkpoint: ...` 并问用户是否现在提交

范围不清或多种架构 → 先计划（Cursor 用 Plan 模式），再实现。

## 功能里程碑（9 步）

```
① 定义 → ② 决策 → ③ 实现 → ④ 跑验证 → ⑤ 用户验收 → ⑥ 记录 → ⑦ SSOT 自检 → ⑧ 提交请示 → ⑨ 可选互审
```

1. **定义**：做什么 + 成功标准。写进当前里程碑行，不要同时开下一行
2. **决策**：必须问用户的 = 可见行为 / 范围 / 对外接口。实现细节自选并记决策表一条
3. **实现**：外科手术。可测逻辑与 UI/I/O 分开。无关问题记「已知问题」，不顺手修
4. **验证（硬门禁）**：跑 `{test_cmd}`，把退出码和摘要写入 PROGRESS。口头全绿 = 未完成。失败则停在本步
5. **验收**：**禁止代勾**。纯逻辑已被断言覆盖 → 验收列可写「由断言覆盖，待用户点头关闭里程碑」。界面 / 手感 → 列出操作+预期，等用户说「确认」或「没过」
6. **记录**：状态 / 验证 / 决策 / 待调参。未记录 = 未完成
7. **SSOT 自检**：见下方清单
8. **提交请示**：拟定 `M{n}: feat|fix|docs ...`，问「是否现在提交」。未经同意不 `git commit`
9. **互审（可选）**：建议新会话看 diff；问题回到 ③

中途改需求：先停 ③。冻结到下一里程碑 / 改成功标准（可见则改 DESIGN）/ 拆行。不允许边改需求边打 ✅。

## 修 bug（红-绿-记录）

1. 复现（红）：失败测试或可重复步骤。无法自动化 → 记录现象+步骤+根因判断，等用户按步骤确认
2. 最小修复，只修这一个
3. 再跑 `{test_cmd}`（该用例 + 全量）
4. 决策表：「修复：根因 + 改动 + 关联测试」
5. 请示提交

没有复现就修 = 猜测，不允许宣称完成。

## 回滚

改坏了：`git diff` → 指出检查点 → 说明会丢什么 → 请用户选 `revert`（默认）或 `reset --hard`（必须明示）→ 把 PROGRESS 改回诚实状态 → 决策表记一条。不擅自 `--hard`。

## SSOT 自检

- [ ] 里程碑表一行一个，状态仅 ✅ / 🔄 / ⬜
- [ ] 「项目状态」与「下一步行动」无矛盾
- [ ] 决策有 `有效` 或 `~~被 X 替代~~`；不删旧决策
- [ ] 用户验收列未被代勾
- [ ] 无密钥 / `.env` / 隐私进入即将提交的 diff
- [ ] 每 ~3 个里程碑或定位失败时，对照目录扫一眼 ARCHITECTURE 是否漂移

## Definition of Done

- [ ] 最简实现
- [ ] `{test_cmd}` 已跑过且全绿（有摘要）
- [ ] 人验收已由用户确认，或已由断言覆盖并经用户关闭
- [ ] PROGRESS 已更新且自检通过
- [ ] 提交已请示（同意后才 git 闭环）

未满足 DoD 不得把里程碑标 ✅，不得宣称「做完了」。
