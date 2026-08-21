---
name: ai-delta
description: >-
  Accumulates personal vocab, project topology, and failure/success cases
  while working with AI; loads prior cards on invoke and never writes phrasing.md.
  Use when the user invokes ai-delta, asks what a term means, asks how a
  project is layered, marks a good or bad AI outcome, or starts a task that
  should reuse prior anti-goals, acceptance, and splitting lines.
---

# ai-delta

生成变便宜了，验证没有。这个 skill 只做当场能写完的记录，不替用户提炼判断。

细则在 [README.md](README.md)。为什么长这样见 [why.md](why.md)。

## On invoke

1. 读 `vocab.md`、`topology.md` 里所有 `## ` 标题（`rg '^## '`）。只展开看起来相关的小节。
2. 把 [phrasing.md](phrasing.md) **全文**抄进当前任务的问题陈述。skill 不许改这份文件。
3. 不要默认加载 [why.md](why.md) 或 [cases.md](cases.md)。

## Write (only when the user speaks)

信息不够就少写半行。不许反问要补什么字段。说不出「换个人 + 同一个模型会差很多」就不记。

| 用户随口说的 | 写到哪 |
|---|---|
| 「什么是 X」「X 是什么意思」 | [vocab.md](vocab.md) 加一行：`日期｜场景｜它指什么，不指什么｜session` |
| 「这项目分几层」「这结构怎么回事」 | [topology.md](topology.md) 建一节或补一行：`层名 — 判据` |
| 「这个例子很好」「这次很对」 | [cases.md](cases.md) 加一行 `(+)` |
| 「又碰到那个问题了」「它又修错层了」 | [cases.md](cases.md) 加一行 `(a/b/c/d)` |

新词/新项目类型的标题必须自带一句话摘要：`## 词 · 摘要`。

**禁止写入 [phrasing.md](phrasing.md)。** 同一类问题大约第五次进 `cases.md` 时，随口提醒一句可以提炼说法。不严格计数。

## Association (vocab / topology only)

只提候选，最多两条，一行一条，仅两种情况：

- 这次的用法和已记的某条不一致
- 这次的概念是已记概念的上位或下位

用户确认之后才给对应词条补一层。宁可漏，不要每次塞三条像是有关的。
