---
name: paper-pages
description: |-
  学术论文深度解读 + GitHub Pages 发布一站式工作流。对论文/文章进行12维度结构化解读，生成自包含HTML网页报告，自动推送到GitHub Pages，更新README目录索引。这是论文/文章分析请求的默认入口技能。

  当用户提出以下任一需求时，必须触发此技能：
  - 论文解读/分析：解读论文、分析paper、论文精读、文献解读、文献精读、读论文、看论文、讲论文、讲paper、分析文章、解读文献、总结论文、研究论文、拆解论文
  - 论文报告生成：生成论文报告、论文汇报、文献汇报、学术分享、组会汇报、做个论文解读
  - 论文+发布：读论文并发布、把论文做成网页、论文解读并推送、paper to pages、分析并上传、解读并发布到网上、下载文章精读并推送
  - 提供论文来源：用户提供论文PDF/链接/DOI/标题/arXiv链接时，即使只说"看看这篇论文"、"帮我分析这篇文章"、"总结一下这篇"、"讲一下这个paper"、"帮我读一下这篇"、"分析一下这篇paper"、"解读一下"、"帮我看看"也应触发
  - 通用HTML发布：把HTML发布到网页、渲染为网页、放到GitHub Pages上、在线看这个页面、publish html to pages

  重要：即使用户没有明确说"发布"或"推送"，只要涉及论文/文章的解读分析请求，默认走完整工作流（解读+发布），除非用户明确说"不需要发布"、"不用推送"、"只解读不发布"。
---

# Paper Pages — 论文精读 + HTML 发布

## 概述

本技能提供两种工作模式，**默认为模式 A**：

- **模式 A（论文工作流，默认）**：论文来源 → 提取文本 → 12维度深度解读 → 生成 HTML → 推送到 GitHub Pages → 更新 README 索引
- **模式 B（通用发布）**：任意 HTML 文件 → 重命名为 index.html → 推送到 GitHub Pages → 输出在线链接

## 模式决策

| 用户意图 | 选用模式 |
|----------|----------|
| 分析/解读/总结论文、文章、文献（无论是否提及"发布"） | **模式 A**（默认） |
| 用户明确说"不用发布"、"不需要推送"、"只解读" | 转交 `paper-interpreter` skill |
| 用户要求把某个已有 HTML 文件发布上网 | 模式 B |
| 用户仅问论文相关问题（非解读） | 按需使用 WebSearch 等工具 |

**核心原则：不修改任何已有 skill，仅通过 Skill 工具链式调用它们。**

---

# 模式 A：论文精读与发布

## 固定路径

| 项目 | 值 |
|------|-----|
| 仓库根目录 | `C:\Users\WZH\Desktop\documents` |
| 论文目录 | `C:\Users\WZH\Desktop\documents\paper\<slug>\` |
| HTML 文件名 | `index.html` (固定) |
| README | `C:\Users\WZH\Desktop\documents\README.md` |
| GitHub 远程 | `git@github.com:WangZihao-Hub/Document.git` |
| Pages 根 URL | `https://wangzihao-hub.github.io/Document/` |

## 工作流

### A-1：确定论文内容来源

根据用户提供内容的方式，采取不同策略：

- **用户提供了 PDF 文件路径** → 调用 `pdf` skill 提取全文文本
- **用户提供了 URL / DOI / arXiv 链接** → 使用 WebFetch 获取页面内容，配合 WebSearch 搜索论文元数据、摘要、及尽可能多的公开信息（如 Semantic Scholar、Google Scholar、arXiv）
- **用户直接粘贴了论文全文** → 直接使用
- **用户仅给了标题** → 使用 WebSearch 搜索公开摘要；**必须告知用户**"基于公开摘要解读，深度有限，建议提供全文或PDF以获得更全面的分析"

### A-2：调用 paper-interpreter 进行深度解读

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

### A-3：确定文件夹名 (slug)

根据论文标题生成 kebab-case 短名，格式建议：`<核心关键词>-<第一作者姓氏><年份>`

**示例：**
- "Attack vulnerability of scale-free networks due to cascading failures" (Wang et al. 2008) → `cascading-failure-wang2008`
- "GraspNet-1Billion: A Large-Scale Benchmark" → `GraspNet-1Billion`
- "Deep Residual Learning for Image Recognition" → `resnet-he2016`

原则：简短、可读、全小写英文和数字，用连字符分隔。如不确定，可以用 `<第一作者姓氏>-<年份>-<主题词>` 格式。

### A-4：保存 HTML 并提交推送

```bash
# 1. 将 HTML 内容写入文件
#    目标路径: C:\Users\WZH\Desktop\documents\paper\<slug>\index.html

# 2. 提交并推送
cd "C:\Users\WZH\Desktop\documents"
git add "paper/<slug>/index.html"
git commit -m "Publish paper/<slug>/index.html - <论文简称>"
git push
```

### A-5：更新 README.md

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

### A-6：输出总结

向用户报告：
- 📄 论文标题及作者
- 📁 本地文件路径
- 🔗 在线访问链接：`https://wangzihao-hub.github.io/Document/paper/<slug>/`
- 📊 报告亮点（覆盖了哪些维度、有什么特色图表或分析）

### A-注意事项

- **不要修改 `paper-interpreter`、`pdf` 等已有 skill** — 通过 Skill 工具调用它们
- 如果论文仅基于摘要解读，报告中会自动标注"基于公开摘要解读，深度有限"
- `<slug>` 确定后展示给用户确认再创建目录
- 推送前检查是否有未提交的变更，如有先告知用户
- README 追加条目时保持与已有条目的格式完全一致
- HTML 文件必须是 `index.html`，这样 GitHub Pages 才能直接以目录 URL 访问
- 如果目标文件夹已存在 `index.html`，询问用户是否覆盖

---

# 模式 B：通用 HTML 发布

将任意本地 HTML 文件发布为 GitHub Pages 网页。

## 工作流

### B-1：确认文件位置

用户需要提供要发布的 HTML 文件路径。确认该文件所在目录是一个 git 仓库，并且已关联 GitHub 远程。

### B-2：重命名为 index.html

将 HTML 文件重命名为 `index.html`（保留在同一目录下）：

```bash
mv "原始文件名.html" "所在目录/index.html"
```

如果该目录已存在 `index.html`，先询问用户是否覆盖。

### B-3：提交并推送

```bash
cd "<repo-root>"
git add "<path-to-index.html>"
git commit -m "Publish <folder-name>/index.html for GitHub Pages"
git push
```

### B-4：输出访问链接

推送成功后，根据远程仓库地址推断 GitHub Pages 链接：

| 远程 | Pages 根域名 |
|------|-------------|
| `git@github.com:<user>/<repo>.git` | `https://<user>.github.io/<repo>/` |

目标 URL = `https://<user>.github.io/<repo>/<子目录>/`

### B-前提条件

- 仓库已开启 GitHub Pages（Settings → Pages → 选择分支保存）
- 该仓库只需开启一次，之后所有子目录的 `index.html` 都能自动渲染

### URL 规则

| 本地路径 | 网页地址 |
|---------|---------|
| `repo/paper/index.html` | `https://<user>.github.io/<repo>/paper/` |
| `repo/index.html` | `https://<user>.github.io/<repo>/` |
