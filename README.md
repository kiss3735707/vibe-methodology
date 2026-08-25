# Vibe Coding 方法论（VCM v1.1）

一套面向 AI 辅助开发（vibe coding）的工程化管理方法论（[MIT License](LICENSE)）。在 [Karpathy 四原则](https://github.com/multica-ai/andrej-karpathy-skills)（先思考 / 简洁优先 / 外科手术 / 目标驱动）之上，叠加 **SSOT 进度管理 + Git 纪律 + 上下文预算** 三层，让 AI 犯错有代价、进展有记录、上下文可接力。

Cursor 工作流已拆成项目 skill（`.cursor/skills/`）：启动、里程碑、交接。四原则写在 `CORE.md`（本仓库 `AGENTS.md` 是它的符号链接），不再做成第四个 skill。

## 这套方法解决什么

| 痛点 | 解法 |
|------|------|
| AI 带错假设一路跑，不提问不澄清 | 铁律 1：先思考再写码 |
| AI 过度设计、代码膨胀 | 铁律 2：简洁优先 |
| AI 顺手改无关代码/注释 | 铁律 3：外科手术 |
| "看起来能跑"的假完成 | 铁律 4：目标驱动 + 验证硬门禁（必须跑命令；用户验收禁止代勾） |
| 上下文一断全忘，反复问同样的问题 | SSOT：PROGRESS.md + 会话交接块 |
| AI 引入回归，无法回滚 | Git 检查点（提交前先问你） |
| SSOT 自己长出矛盾 | SSOT 卫生：每次更新后一致性自检 |
| 小改动被 9 步循环拖死 | 只读 / 单行 / 探索不走完整循环 |

## 文件地图

| 文件 | 角色 | 谁用 |
|------|------|------|
| `CORE.md` | 核心规则（常驻，唯一正文） | **复制进每个项目根目录**，命名 `AGENTS.md` 或 `CLAUDE.md` |
| `AGENTS.md` | 指向 `CORE.md` 的符号链接 | 本仓库 Cursor 读取；不要另写一份 |
| `METHODOLOGY.md` | 完整手册 | 你自己读，不必给 AI |
| `.cursor/skills/vibe-bootstrap/` | 绿场 / 棕场启动 | Agent 按需加载 |
| `.cursor/skills/vibe-milestone/` | 里程碑循环 / 修 bug / DoD | Agent 按需加载 |
| `.cursor/skills/vibe-handoff/` | 交接与恢复 | Agent 按需加载 |
| `templates/DESIGN.md` | 产品意图模板 | 项目启动时复制 |
| `templates/PROGRESS.md` | 进度 SSOT 模板 | 项目启动时复制 |
| `templates/ARCHITECTURE.md` | 模块地图模板 | 项目启动时复制 |
| `examples/success-criteria.md` | 成功标准对照示例 | 人读；不要抄进模板 |
| `LICENSE` | MIT | 允许复制、修改、再分发（含商用）；保留版权与许可声明即可 |

## 安装 Cursor skills

需要 [Cursor 2.4+](https://cursor.com/changelog/2-4)（Agent Skills）。每个 skill 是「目录 + `SKILL.md`」，不要把三个 `SKILL.md` 平铺拷贝。不要写入 `~/.cursor/skills-cursor/`（Cursor 内置目录）。

```bash
git clone https://github.com/kiss3735707/vibe-methodology.git
cd vibe-methodology

# 项目级：只在某个产品仓库生效（可随仓库共享给同事）
mkdir -p <你的项目>/.cursor/skills
cp -R .cursor/skills/* <你的项目>/.cursor/skills/

# 全局级：对本机所有项目生效
mkdir -p ~/.cursor/skills
cp -R .cursor/skills/* ~/.cursor/skills/
```

装完后**新开一个 Agent 对话**。触发方式：

- 自然语言：说「按 VCM 初始化」「按 VCM 做 M3」「先交接再走」
- 显式点名：`/vibe-bootstrap`、`/vibe-milestone`、`/vibe-handoff`

Skill 只是工作流。新项目仍要复制 `templates/` 和 `CORE.md`（见下方）。全局安装后，`vibe-bootstrap` 会先在当前工作区找 `templates/`；找不到则按骨架生成，并提示从本仓库取完整模板。

## 快速上手（新项目）

```bash
mkdir my-project && cd my-project && git init
cp /path/to/vibe-methodology/templates/DESIGN.md .
cp /path/to/vibe-methodology/templates/PROGRESS.md .
cp /path/to/vibe-methodology/templates/ARCHITECTURE.md .
cp /path/to/vibe-methodology/CORE.md ./AGENTS.md   # 或 CLAUDE.md
```

填 DESIGN（目标 / 范围 / 明确不做）→ 拆里程碑（**M0 = `{test_cmd}` 能红能绿**）→ 对 AI 说「按 VCM 初始化」，再请示第一次提交。

已有仓库不要覆盖现有 `AGENTS.md`：对 AI 说「把 VCM 接到这个已有仓库」。

## 轻量版（小项目/原型）

只保留三样：`CORE.md` + `PROGRESS.md`（DESIGN/ARCHITECTURE 内容合并进去）+ git。里程碑 3 个就够（含 M0）。

## 人侧口令

- 「按 VCM 初始化这个项目」/「把 VCM 接到这个已有仓库」
- 「按 VCM 做 M3，先列 Ready」
- 「这是范围外，记入已知问题」
- 「先交接再走」/「从 PROGRESS 恢复」
- 「验收：M2 我确认了」/「没过，现象是…」
- 「回滚到检查点」/「现在可以提交」

## 核心理念一句话

> 让 AI **犯错有代价**（验证 + git 回滚）、**进展有记录**（状态只落文件）、**上下文可接力**（交接块 + 恢复流程）。

## License

[MIT](LICENSE)。可以合规地用在自己的项目里（含商用）；再分发时保留版权声明和本许可即可。
