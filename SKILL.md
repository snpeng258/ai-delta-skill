---
name: ai-delta
description: >-
  Accumulates personal vocab, project topology, and failure/success cases
  while working with AI. Loads only vocab and topology headings on invoke.
  Use when the user invokes ai-delta, asks what a term means, asks how a
  project is layered, or marks a good or bad AI outcome.
---

# ai-delta

生成变便宜了，验证没有。这个 skill 只做当场能写完的记录，不替用户提炼判断。

细则在 [README.md](README.md)。为什么长这样见 [why.md](why.md)。两份都不要默认加载。

## 允许读写的文件

只有这三份：

- 读：`vocab.md`、`topology.md` 的 `## ` 标题；相关小节才展开正文
- 写：上面两份，以及 `cases.md`（只在用户说话时追加一行）

不要读这个仓库里的其他 markdown。用户的说法笔记本不在这个仓库里，不要去找。

## On invoke

1. `rg '^## '` 读 `vocab.md`、`topology.md` 的标题。只展开看起来相关的小节。
2. 停。不要打开别的文件当约束。

## Write (only when the user speaks)

信息不够就少写半行。不许反问要补什么字段。说不出「换个人 + 同一个模型会差很多」就不记。

| 用户随口说的 | 写到哪 |
|---|---|
| 「什么是 X」「X 是什么意思」 | [vocab.md](vocab.md) 新建一节或往「叠加」加一行。新节必须有：外行说法、首次 `ob:<source>/<session_id>`、搞混过、拓扑（没有写「无」）。拓扑有值时附检索：`rg '^## <节标题>' topology.md` |
| 「这项目分几层」「这结构怎么回事」 | [topology.md](topology.md) 建一节或补一行：`层名 — 判据` |
| 「这个例子很好」「这次很对」 | [cases.md](cases.md) 加一行 `(+)`，末栏写 `ob:<source>/<session_id>` |
| 「又碰到那个问题了」「它又修错层了」 | [cases.md](cases.md) 加一行 `(a/b/c/d)`，末栏写 `ob:<source>/<session_id>` |

写 `ob:` 时先 `overview()`。`current.session_id` 有值就用。Cursor 经常是 `null`（nonce 认不出当前对话）：改搜本项目、标题带 `[Cursor]` 的最新一条，写成 `ob:claude/<id>`。不要写「本次对话」。查不到就空着末栏，不要编。

新词/新项目类型的标题必须自带一句话摘要：`## 词 · 摘要`。

同一类问题大约第五次进 `cases.md` 时，随口提醒用户可以去整理案例。不严格计数。不要为此打开别的文件。

## Association (vocab / topology only)

只提候选，最多两条，一行一条，仅两种情况：

- 这次的用法和已记的某条不一致
- 这次的概念是已记概念的上位或下位

用户确认之后才给对应词条补一层。宁可漏，不要每次塞三条像是有关的。

词条若写了拓扑字段，分析时用它给出的 `rg '^## …' topology.md` 去读那一节的 `L<n>` 判据，不要猜层名。
