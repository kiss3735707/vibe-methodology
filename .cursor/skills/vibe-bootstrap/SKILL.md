---
name: vibe-bootstrap
description: >-
  Bootstraps Vibe Coding Methodology (VCM) onto a repo: DESIGN/PROGRESS/ARCHITECTURE,
  milestone split with M0 test command, AGENTS.md merge without overwrite. Use when
  starting a new project, initializing VCM, 脚手架, 新项目, 接入方法论, brownfield, or adopting
  VCM into an existing codebase. Do not use for ordinary feature work, bug fixes, Q&A,
  or session handoff.
---

# VCM Bootstrap

把目标仓库接到 VCM。模板 SSOT 在方法论仓库 `templates/`。本 skill 只负责启动，启动完成后用 `vibe-milestone` 做功能。

## 模板位置

按序找 `DESIGN.md` / `PROGRESS.md` / `ARCHITECTURE.md`：

1. 当前工作区 `templates/`
2. 本 skill 的 `../../../templates/`（skill 在 `vibe-methodology/.cursor/skills/vibe-bootstrap/` 时）
3. 都没有：按 `templates/` 骨架现场生成，并告知用户去方法论仓库取完整模板

核心规则文件 `CORE.md` 与模板同级（工作区根或 `../../../CORE.md`）。复制为项目根 `AGENTS.md`（Cursor / Codex）或 `CLAUDE.md`（Claude Code）。

## 先探测

列出是否已有：`AGENTS.md`、`CLAUDE.md`、`README.md`、测试命令、`DESIGN.md` / `PROGRESS.md` / `ARCHITECTURE.md`、`.git`。

有 `.git` 不等于已接入 VCM。

## 绿场（空目录或刚 git init）

1. 复制三件套到项目根；复制 `CORE.md` → `AGENTS.md` 或 `CLAUDE.md`
2. 请用户填（可代拟草稿，但范围 / 明确不做必须等人认）：一句话描述、做 / 不做、技术栈
3. 拆 5–8 个里程碑；**M0 固定为「`{test_cmd}` 能红能绿」**。未定栈就问，禁止把 node 示例当默认
4. 把 `{run_cmd}` / `{test_cmd}` 写入 PROGRESS「运行与验证」和 ARCHITECTURE「测试入口」
5. 轻量项目：用户同意后可把 DESIGN/ARCHITECTURE 并进 PROGRESS，里程碑 3 个（含 M0）
6. **拟定** `init: 脚手架 + 设计 + 里程碑拆分`，询问是否现在提交。未经同意不 `git commit`

## 棕场（已有代码）

禁止铺平行真相源、禁止借机重构。

1. **规则文件**：已有 `AGENTS.md` / `CLAUDE.md` → 追加 `## VCM` 节（或请用户选合并方式），**不覆盖**。没有则复制 CORE
2. **PROGRESS**：没有就从模板建；有则只补缺失节（恢复流程 / 工作协议 / 里程碑表），不删已有内容
3. **DESIGN**：没有则从 README 蒸馏一页（目标 / 范围 / 明确不做）；已有产品文档则链过去，不另写一份打架的规格
4. **`{test_cmd}`**：沿用仓库已有测试入口。没有则 M0 = 补一条最小、可重复的验证命令（写入 SSOT），不要用完即扔的脚本
5. **ARCHITECTURE**：按现有目录填模块表，标纯逻辑 vs I/O。不改代码结构
6. 第一功能里程碑之前，M0 必须能跑（或诚实记录「无测试，M0 进行中」）
7. 拟定接入提交（`docs: 接入 VCM`），询问是否提交

## 停止条件

- 用户只是问 VCM 是什么 → 指向 `README.md`，不改目标仓库
- 目标仓库已有完整三件套且 M0 已绿 → 告诉用户改用 `vibe-milestone`
- 发现密钥 / `.env` 将被加入 git → 停下并警告

## 完成时交给用户

- 里程碑表（含 M0）和下一步行动已写入 PROGRESS
- `{test_cmd}` 已填或明确列为 M0
- 提交已请示，未擅自 commit
