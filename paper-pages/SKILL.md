---
name: paper-pages
description: 私人论文精读与GitHub Pages发布一站式工作流。对学术论文进行深度解读，生成自包含网页报告，自动推送到GitHub Pages并更新README目录索引。当用户说"读论文并发布"、"把这篇论文做成网页"、"论文解读并推送"、"paper to pages"、"分析这篇论文并上传"、"帮我解读这篇文章并发布到网上"时使用。即使用户只给了论文链接/PDF/DOI/标题，说"下载文章精读并推送"、"分析这篇paper"，也应触发此技能。
---

# Paper Pages — 私人论文精读与 GitHub Pages 发布

## 概述

本技能编排一个完整的论文→网页工作流，通过链式调用其他 skill 实现：

```
论文来源 (PDF/URL/文本)
  → [pdf skill] 提取文本 (如需要)
  → [paper-interpreter skill] 12维度深度解读 → 生成 HTML
  → 保存到本地 Documents 仓库
  → git push → GitHub Pages 上线
  → 更新 README.md 目录索引
```

**核心原则：不修改任何已有 skill，仅通过 Skill 工具链式调用它们。**

## 固定路径

| 项目 | 值 |
|------|-----|
| 仓库根目录 | `C:\Users\WZH\Desktop\documents` |
| 论文目录 | `C:\Users\WZH\Desktop\documents\paper\<slug>\` |
| HTML 文件名 | `index.html` (固定) |
| README | `C:\Users\WZH\Desktop\documents\README.md` |
| GitHub 远程 | `git@github.com:WangZihao-Hub/Document.git` |
| Pages 根 URL | `https://wangzihao-hub.github.io/Document/` |

## 完整工作流

### 步骤 1：确定论文内容来源

根据用户提供内容的方式，采取不同策略：

- **用户提供了 PDF 文件路径** → 调用 `pdf` skill 提取全文文本
- **用户提供了 URL / DOI** → 使用 WebFetch 获取页面内容，配合 WebSearch 搜索论文元数据、摘要、及尽可能多的公开信息（如 Semantic Scholar、Google Scholar、arXiv）
- **用户直接粘贴了论文全文** → 直接使用
- **用户仅给了标题** → 使用 WebSearch 搜索公开摘要；**必须告知用户**"基于公开摘要解读，深度有限，建议提供全文或PDF以获得更全面的分析"

### 步骤 2：调用 paper-interpreter 进行深度解读

调用 `paper-interpreter` skill。该 skill 会按照以下 12 个维度生成结构化 HTML 报告：

1. 元信息（作者/期刊/DOI/引用量）
2. 研究背景与动机（领域演进脉络 + 核心痛点）
3. 核心问题/目标（Research Question 精确表述）
4. 方法论/技术路线（组件拆解 + 公式 + 伪代码 + 对比表）
5. 实验设计与结果（数据集/指标/对比表/CSS图表）
6. 主要贡献（卡片网格）
7. 讨论与局限性
8. 未来工作建议
9. 批判性思考（7+ 角度系统批判）
10. 可视化总结（CSS流程图/对比表/概念图）
11. 相关资源（参考文献/代码/后续工作）
12. 可复现性提示（超参数表/环境配置/复现陷阱）

### 步骤 3：确定文件夹名 (slug)

根据论文标题生成 kebab-case 短名，格式建议：`<核心关键词>-<第一作者姓氏><年份>`

**示例：**
- "Attack vulnerability of scale-free networks due to cascading failures" (Wang et al. 2008) → `cascading-failure-wang2008`
- "GraspNet-1Billion: A Large-Scale Benchmark" → `GraspNet-1Billion`
- "Deep Residual Learning for Image Recognition" → `resnet-he2016`

原则：简短、可读、全小写英文和数字，用连字符分隔。如不确定，可以用 `<第一作者姓氏>-<年份>-<主题词>` 格式。

### 步骤 4：保存 HTML 并提交推送

```bash
# 1. 将 HTML 内容写入文件
#    目标路径: C:\Users\WZH\Desktop\documents\paper\<slug>\index.html

# 2. 提交并推送
cd "C:\Users\WZH\Desktop\documents"
git add "paper/<slug>/index.html"
git commit -m "Publish paper/<slug>/index.html - <论文简称>"
git push
```

### 步骤 5：更新 README.md

1. **先 Read** `C:\Users\WZH\Desktop\documents\README.md` 查看当前结构
2. 在 `### 📝 Paper（论文）` 小节末尾追加新条目，格式如下（保持与已有条目一致）：

```markdown
- **[<论文中文简称>](./paper/<slug>/)** — [网页版](https://wangzihao-hub.github.io/Document/paper/<slug>/) · <一句话描述 | 第一作者 et al. (年份)>
```

3. 提交并推送 README 更新：
```bash
cd "C:\Users\WZH\Desktop\documents"
git add README.md
git commit -m "Update README: add <论文简称> entry"
git push
```

### 步骤 6：输出总结

向用户报告：
- 📄 论文标题及作者
- 📁 本地文件路径
- 🔗 在线访问链接：`https://wangzihao-hub.github.io/Document/paper/<slug>/`
- 📊 报告亮点（覆盖了哪些维度、有什么特色图表或分析）

## 注意事项

- **不要修改 `paper-interpreter`、`pdf` 等已有 skill** — 通过 Skill 工具调用它们
- 如果论文仅基于摘要解读，报告中会自动标注"基于公开摘要解读，深度有限"
- `<slug>` 确定后展示给用户确认再创建目录
- 推送前检查是否有未提交的变更，如有先告知用户
- README 追加条目时保持与已有条目的格式完全一致
- HTML 文件必须是 `index.html`，这样 GitHub Pages 才能直接以目录 URL 访问
- 如果目标文件夹已存在 `index.html`，询问用户是否覆盖
