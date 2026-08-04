---
layout: post
title: "Agent Loop 不是 Agent OS"
date: 2026-08-04 10:00:00 +0800
---

> 状态：架构定位  
> 当前判断：Atoll 已实现持久协作 substrate；完整 Agent OS 系统层尚未实现。

## 1. Loop 解决了什么

成熟 coding agent 通常都有一条核心反馈循环：

```text
user input
   ↓
model sampling
   ↓
tool calls
   ↓
tool results
   └──────────▶ next sampling
```

Codex 和 OpenCode 已经证明，这条循环还需要 input queue、steer、interrupt、approval、compaction、tool concurrency 和 subagent 等精细语义。这些设计非常重要，但它们回答的是：

> 一个 Agent session 怎样有效地继续执行？

它们没有单独回答：

- 谁拥有长期身份和权限；
- 多个参与者共享的事实写在哪里；
- 进程崩溃后协作关系如何恢复；
- Agent 和数据如何跨机器而保持同一语义；
- 多个自治空间如何交接工作而不共享可变真相；
- 一个构建出来的 App 如何安装、升级、回滚和持续运行。

因此，session loop 是优秀 Agent Runtime 的执行器，不是整个 Agent OS 的架构。

## 2. Agent OS 需要一个世界

OS 级问题不是“下一次调用哪个模型”，而是为长期存在的主体和应用提供稳定环境：

| 问题 | Atoll 的基础回答 |
|---|---|
| 身份 | Actor identity 与 incarnation |
| 协作事实 | Channel-local append-only truth |
| 主体间影响 | Message |
| 数据访问 | Resource + Access |
| 权限 | 焊死身份的 capability 与门内判定 |
| 生命周期 | membership、embodiment、reconcile、closure |
| 物理位置 | cell/port 对称与 Home/compute 分离 |
| 恢复 | durable facts + projection/reconcile |

这些结构使 Agent loop 可以崩溃、替换或升级，而世界中的身份、事实与义务仍然可解释。

## 3. 正确的嵌入方向

```text
Application / Coding Workflow
  └─ Channel-local Agent Runtime
       ├─ queue / steer / interrupt / approval
       ├─ model/tool continuation
       ├─ child/team policy
       └─ Agent root actor
            └─ Atoll substrate
                 identity / truth / authority / lifecycle
```

Agent Runtime 可以参考 Codex/OpenCode，甚至直接复用其设计经验；但它属于 Channel 内的系统层。Atoll substrate 应保持 agent-blind，只执法跨 workload 都必须成立的不变量。

## 4. 这不意味着 Atoll 已经是 Agent OS

当前更准确的说法是：

- **已实现**：一个扎实的 Channel/Actor 协作 substrate。
- **架构可支持**：Codex-like、OpenCode-like 和 durable multi-agent runtime。
- **探索中**：跨 Channel 关系、安装式 App Runtime、实时 build-to-run 产品闭环。

Atoll 的价值不在于比 Codex 多一层 loop，而在于让不同 Agent Runtime 可以生活在同一套持久世界规则中，并让多个局部世界能够形成联邦。

