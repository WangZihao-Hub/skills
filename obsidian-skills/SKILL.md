---
name: obsidian-skills
description: 把 Obsidian 的笔记、资料、Canvas 变成 AI 可调用的工作材料。支持 Obsidian Flavored Markdown（双向链接、Callout、属性）、JSON Canvas 思维导图、Bases 数据库视图、Defuddle 网页抓取。当用户提到 Obsidian 笔记、知识库、Canvas 画布、知识管理、双链笔记时使用。
---

# Obsidian Skills

教你如何创建和编辑 Obsidian 生态中的各种文件格式：Markdown 笔记（含 Obsidian 扩展语法）、JSON Canvas 画布、Bases 数据库视图。

## 核心能力

### 1. Obsidian Flavored Markdown

创建和编辑 Obsidian 扩展 Markdown（`.md` 文件），包含双向链接、嵌入、Callout、属性、注释等。

**基础语法：**

```markdown
# 标题
## 双向链接
[[笔记名称]]                              → 链接到笔记
[[笔记名称|显示文字]]                       → 自定义显示文字
[[笔记名称#标题]]                          → 链接到指定标题
![[图片.png|300]]                         → 嵌入图片（可设宽度）
![[文档.pdf#page=3]]                      → 嵌入 PDF 指定页

## Callout（强调块）
> [!note] 自定义标题
> 这是 Callout 内容。

> [!warning]- 可折叠
> 默认折叠的 Callout（- 折叠，+ 展开）

常用类型：`note`, `tip`, `warning`, `info`, `example`, `quote`, `bug`, `danger`, `success`, `failure`, `question`, `abstract`, `todo`

## 属性（Frontmatter）
---
title: 笔记标题
date: 2024-01-15
tags:
  - project
  - active
aliases:
  - 别名
cssclasses:
  - custom-class
---

## 标签
#tag              → 普通标签
#nested/tag       → 层级标签

## 注释
%%这里是隐藏内容%%

## 高亮
==高亮文字==

## 数学公式
行内：$e^{i\pi} + 1 = 0$
块级：$$ \frac{a}{b} = c $$

## Mermaid 图表
\`\`\`mermaid
graph TD
    A[开始] --> B{决策}
\`\`\`

## 脚注
文字需要脚注[^1]。
[^1]: 脚注内容。
```

### 2. JSON Canvas（思维导图/画布）

创建和编辑 JSON Canvas 文件（`.canvas`），用于可视化知识图谱。

```json
{
  "nodes": [
    {
      "id": "6f0ad84f44ce9c17",
      "type": "text",
      "x": 100,
      "y": 100,
      "width": 400,
      "height": 60,
      "text": "中心主题"
    },
    {
      "id": "a1b2c3d4e5f6a7b8",
      "type": "text",
      "x": 600,
      "y": 100,
      "width": 300,
      "height": 60,
      "text": "子主题"
    }
  ],
  "edges": [
    {
      "id": "edge1",
      "fromNode": "6f0ad84f44ce9c17",
      "toNode": "a1b2c3d4e5f6a7b8",
      "fromSide": "right",
      "toSide": "left",
      "label": "关联"
    }
  ]
}
```

**节点类型：** `text`（文字）、`file`（文件）、`group`（分组）
**边属性：** `fromSide`/`toSide`（top/right/bottom/left）、`label`（标签）、`color`（颜色）

### 3. Obsidian Bases 数据库

创建 `.base` 文件实现结构化数据视图：

```yaml
filters:
  and:
    - 'status == "active"'

views:
  - type: table
    name: "项目看板"
    limit: 20
    order:
      - file.name
      - status
      - priority
```

### 4. Defuddle：网页内容提取

从网页 URL 提取干净的 Markdown 内容：

```bash
defuddle parse <url> --md
```

## 使用示例

- "把这篇笔记改成 Canvas 画布"
- "创建一个 Obsidian 笔记，包含双向链接和 Callout"
- "帮我整理这些内容成思维导图"
- "把这篇 Obsidian 笔记加上属性标签"
- "用 Mermaid 在这个笔记里画个流程图"
