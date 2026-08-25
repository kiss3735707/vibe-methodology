---
name: vibe-handoff
description: >-
  Writes and restores VCM session handoff blocks in PROGRESS.md so work survives
  context loss. Use when context is tight, the user says 交接, 换会话, 新开, 从 PROGRESS
  恢复, ending a long session, or resuming in a new chat. Do not use for routine
  small edits that already fit in the current session, project bootstrap, or a
  full milestone implementation.
---

# VCM Handoff

PROGRESS.md 是唯一进度真相源。本 skill 只处理会话边界：结束前把状态写进文件，开始时从文件恢复。不要在这里实现功能（用 `vibe-milestone`）。

同一时刻只允许一个会话**写** PROGRESS.md。并行 agent 只读 ARCHITECTURE 与源码。

## 何时必须交接

满足一条就先写交接块，再继续或结束：

- 用户说「交接 / 换会话 / 新开 / 先交接再走」
- 接下来的改动会跨 3 个子系统
- 同一问题连续失败 2 次
- 会话已很长且还要做大改
- 你即将停手，且当前里程碑未达 DoD

**先交接，再结束。未写交接块的会话 = 未完成。** 不要在上下文将尽时硬写代码。

## 写交接块

更新 `PROGRESS.md` 的「会话交接块」，覆盖写之前把未完成动作并入「下一步行动」。

```
## 会话交接块
- 当前状态：{做到哪了，精确到文件/函数；测试最后一次命令和结果}
- 下一步精确动作：{第 1 步；第 2 步。不要写「继续完成 M3」这种空话}
- 未决问题：{需要用户或下一会话回答的；无则写「无」}
```

同时核对：

- 「项目状态」当前里程碑仍正确
- 「下一步行动」与交接块不打架
- 已知问题里该记的都记了
- **不**把密钥、路径里的隐私、`.env` 内容写入交接块

写完告诉用户：可以开新会话，说「从 PROGRESS 恢复」。未经同意不要借交接之机 `git commit`；若工作树很脏，可**请示**一个 `checkpoint:` 提交。

## 恢复（新会话第一件事）

按序，不要一上来读全库：

1. `PROGRESS.md`（必读）：恢复流程 → 交接块 → 下一步行动 → 工作协议
2. `ARCHITECTURE.md`（需要改代码时）
3. 交接块点名的文件（只读要动的）
4. `DESIGN.md`（仅当下一步涉及用户可见行为）

然后用一句话向用户确认：「从 {交接块的下一步第 1 步} 继续，对吗？」确认后再改代码。

交接块已消费且里程碑仍进行中：把交接块清成模板空壳，避免下一轮误读过期状态；内容已体现在「下一步行动」则即可。

## 并行会话撞车

发现 PROGRESS 被另一会话改过（内容与对话记忆冲突）：

1. 停写代码
2. 以文件为准，不要用对话覆盖文件
3. 把两边未完成动作合并进「下一步行动」和新交接块
4. 问用户谁是主会话

## 停止条件

- 用户只是小改且上下文健康 → 不写空交接块
- PROGRESS.md 不存在 → 提示先 `vibe-bootstrap`，不要发明第二个进度文件
