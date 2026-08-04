---
layout: post
title: "五元素、两平面：Atoll 的最小世界"
date: 2026-08-04 11:00:00 +0800
---

> 状态：内核本体介绍  
> **已实现**：五个协议包与主要 runtime enforcement 已存在。

## 1. 本体闭包

Atoll 用五个元素描述一个 Channel 内可以存在和发生的全部基础结构：

```text
容器：Channel

实体：Actor（主体）
      Resource（客体）

关系：Message（主体 ↔ 主体）
      Access（主体 → 客体）
```

记作：

```text
{ channel; actor, resource; message, access }
```

这不是功能清单，而是主体/客体维度上的闭包。

## 2. 五个元素

| 元素 | 范畴 | 本质 |
|---|---|---|
| Channel | 容器 | 一个局部世界及其信任、数据与因果边界 |
| Actor | 主体实体 | 有私有可变态、串行生命并能够施动 |
| Resource | 客体实体 | 被动、可寻址、被访问的数据 |
| Message | 横向关系 | Actor 对另一个 Actor 的协作影响，进入 truth log |
| Access | 纵向关系 | Actor 对 Resource 的受控操作，走授权门 |

Actor 是唯一施动实体。它只有两个向外方向：

```text
横向够到另一个主体：Message
纵向够到一个被动客体：Access
```

两种实体与两种关系覆盖了主体能够产生的基础作用，因此不需要为 manager、scheduler、connection、timer 等实现概念不断增加本体元素。

## 3. 两个平面

### Plane 1：协作平面

```text
Actor ── Message ──▶ Actor
```

- Message 有作者和 audience；
- 通过 Pen/harness 写入 Channel truth；
- request/response/terminal 形成可审计因果链；
- 投递是已提交事实的物理 projection。

### Plane 2：访问平面

```text
Actor ── Access ──▶ Resource
```

- Resource ID 只是名字，不是权限；
- 每次操作由 welded caller 经过 access door 判断；
- Resource 的字节与 driver 细节不进入 proto；
- 授权关系与资源生命周期由 runtime 执法。

## 4. 薄实体、重关系

Atoll 的一个重要设计取向是：不要把行为和政策塞进实体定义。

- Resource 保持为薄的 opaque identity，复杂性进入 Access。
- Channel ID 也保持 opaque；未来跨 Channel 的复杂性应进入 ChannelRelation。
- Actor 的实现可以更换，但其身份、能力和消息关系保持稳定。

关系承担语义，实体保持可组合，这使窄腰不会随产品功能增长而持续变宽。

## 5. 哪些东西不是新元素

- Timer 是延迟的 self-message，不是独立客体。
- 活连接是 adapter actor；连接配置才可能是 resource。
- Scheduler 是执行政策，不是世界实体。
- Store、manager、dispatcher 是实现结构，不是跨边界契约。
- Agent 是 Actor 承载的一种系统层运行语义，不要求内核新增 AgentKind 本体。

判断一个概念是否应进入 proto 的标准是：

> 它是否在跨边界契约中被双方引用，并且双方必须对它具有同一个稳定含义？

不能满足这个条件的概念，应留在 runtime、system layer 或 application 中。

