---
name: markitdown
description: 把 PDF、Word、PPT、Excel、图片等文件转成 Markdown 格式，让 AI 更好读取和处理。使用 Microsoft MarkItDown 引擎。当用户说"把这个文件转成 md"、"转成 Markdown"、"提取成 Markdown"、"转换文件格式"、"把 PDF 变成 Markdown"、"把 Word 转成 Markdown"时触发，也适用于用户想把各种办公文档变成纯文本格式。
---

# MarkItDown — 通用文件转 Markdown 转换器

将各种办公文档、图片、音频等文件转换成 Markdown 格式，方便 AI 读取和处理。

## 安装

```bash
pip install markitdown
```

## 基本用法

```bash
# 转换单个文件
markitdown 输入文件.pdf -o 输出文件.md

# 或使用重定向
markitdown 输入文件.pdf > 输出文件.md

# 从标准输入读取
cat 输入文件.pdf | markitdown

# 直接输出到终端查看
markitdown 输入文件.pdf
```

## 支持的格式

| 类型 | 格式 | 说明 |
|------|------|------|
| PDF | `.pdf` | 文字 PDF 直接提取，含表格 |
| Word | `.docx` | 含标题、段落、列表、表格 |
| PPT | `.pptx` | 提取每张幻灯片的文字内容 |
| Excel | `.xlsx` | 提取表格数据 |
| 图片 | `.jpg`, `.png`, `.bmp` | OCR 识别图片中的文字 |
| 音频 | `.wav`, `.mp3`, `.m4a` | 语音转文字 |
| HTML | `.html`, `.htm` | 清理 HTML 转 Markdown |
| 文本 | `.txt`, `.csv`, `.json`, `.xml` | 结构化文本转换 |
| 电子书 | `.epub` | 电子书提取 |
| ZIP | `.zip` | 批量处理压缩包内的文件 |
| YouTube | 视频 URL | 提取视频字幕/文字记录 |

## 工作流程

1. **确认文件路径** — 用户提供需要转换的文件
2. **转换** — 执行 `markitdown <文件> -o <输出>.md`
3. **读取结果** — Markdown 输出到文件后，直接读取使用
4. **后续处理** — 可以用转换后的 Markdown 做：总结、翻译、分析、提取关键信息等

## 示例

### 转换 PDF 学术论文

```bash
markitdown paper.pdf -o paper.md
```

### 转换 Word 文档

```bash
markitdown 报告.docx > 报告.md
```

### 转换 PPT 并总结

```bash
markitdown 演示文稿.pptx -o 演示文稿.md
# 然后读取 演示文稿.md 进行总结
```

### 批量转换

```bash
# 转换目录下所有 PDF
for f in *.pdf; do
  markitdown "$f" -o "${f%.pdf}.md"
done
```

## 与现有技能配合

- **pdf 技能**：处理扫描件 OCR 或复杂 PDF 布局时，可先用 pdf 技能提取，再用 markitdown 转为结构化文本
- **docx/pptx/xlsx 技能**：当需要精确控制格式时用对应技能，快速提取内容用 markitdown

## 注意事项

- 对复杂排版的 PDF（多栏、复杂表格），转换效果可能不完美，可以配合 pdf 技能使用
- OCR 功能需要安装额外的语言包（自动检测）
- 音频转文字需要联网（调用 Azure AI 服务）或本地模型支持
