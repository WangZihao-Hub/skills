# Claude Code Skills

> **使用方法：** 将本仓库的所有技能文件夹直接放入 `~/.claude/skills/` 目录下即可使用。Claude Code 会自动加载该目录下的所有技能。
>
> ```
> ~/.claude/
> └── skills/              ← 本仓库（git clone 到这里）
>     ├── skill1/
>     │   ├── SKILL.md      ← 技能定义文件
>     │   └── ...
>     ├── skill2/
>     │   ├── SKILL.md      ← 技能定义文件
>     │   └── ...
>     └── README.md
> ```

# Claude Code Skills

两个实用的 Claude Code 技能，用于学术论文深度解读和 PDF 文档处理。

## 技能列表

### 📄 paper-interpreter
对学术论文进行 **12 维度深度结构化解读**，生成自包含的 HTML 报告（无外部依赖，浏览器直接打开）。

**功能亮点：**
- 12 个维度：元信息、研究背景、核心问题、方法论、实验、贡献、局限性、未来工作、批判性思考、可视化总结、资源、可复现性
- 纯 CSS 图表（流程图、柱状图、对比表、评分条）
- 响应式设计（桌面/移动端/打印友好）
- 批判性思考维度（11 个审查角度，含模拟审稿评分）

**使用示例：**
```
/paper-interpreter https://arxiv.org/abs/XXXX.XXXXX
解读这篇论文的 PDF
```

### 📑 pdf
全面的 PDF 处理工具集，支持文本提取、表格解析、合并拆分、表单填充、OCR 等。

**功能亮点：**
- 文本和表格提取（pdfplumber）
- PDF 合并/拆分/旋转（pypdf）
- 表单填充（自动检测可填充字段 + 视觉定位）
- OCR 扫描件文字识别
- 命令行和 Python API 双模式

**使用示例：**
```
提取这个 PDF 的文字
把这些 PDF 合并成一个
填一下这个 PDF 表单
```

## 安装

将本仓库克隆到 `.claude/skills/` 目录下：

```bash
git clone https://github.com/WangZihao-Hub/skills.git ~/.claude/skills/
```

## 目录结构

```
skills/
├── paper-interpreter/
│   ├── SKILL.md
│   └── assets/
│       └── template.html
├── pdf/
│   ├── SKILL.md
│   ├── forms.md
│   ├── reference.md
│   ├── LICENSE.txt
│   └── scripts/
│       ├── check_bounding_boxes.py
│       ├── check_fillable_fields.py
│       ├── convert_pdf_to_images.py
│       ├── create_validation_image.py
│       ├── extract_form_field_info.py
│       ├── extract_form_structure.py
│       ├── fill_fillable_fields.py
│       └── fill_pdf_form_with_annotations.py
└── README.md
```
