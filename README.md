# 中文学术文档工具集

## Chinese Academic Document Toolkit

**面向中文学术文档的隐私优先、可扩展质量检查与自动化工具生态**

本仓库是生态入口、跨项目文档和路线图，不包含检查引擎的主实现。核心实现分别位于下列
三个职责明确的项目中。

这是一组相互协作的开源工具，用于检查中文课程论文、研究报告及其他学术 DOCX 文档。
文档在本地设备或用户选择的 CI 运行器内处理，不会由这些工具上传到外部检查服务。

## 为什么需要这个项目

多数写作工具关注语言润色；本项目关注可以机械检查、可以进入 CI 的中文学术 DOCX
质量问题，例如占位符残留、重复标点、中文字符间异常空格、参考文献章节缺失、段落过长
和机器可读报告。

本项目不判断论证质量、事实准确性、引用真实性，也不声称符合任何学校或期刊的正式格式
规范。

## 项目组成

| 项目 | 职责 | 稳定入口 |
| --- | --- | --- |
| [han-docx-lint](https://github.com/heavenoracle/han-docx-lint) | 解析 DOCX、执行规则并输出检查结果 | [`v0.3.0`](https://github.com/heavenoracle/han-docx-lint/releases/tag/v0.3.0) |
| [han-docx-rules](https://github.com/heavenoracle/han-docx-rules) | 提供可审计、可复用的 JSON 质量配置 | [`main`](https://github.com/heavenoracle/han-docx-rules/tree/main) |
| [han-docx-action](https://github.com/heavenoracle/han-docx-action) | 在 GitHub Actions 中批量检查 DOCX 并上传报告 | [`v1`](https://github.com/heavenoracle/han-docx-action/releases/tag/v1) |

```text
han-docx-rules -- JSON 配置 --> han-docx-lint -- JSON 检查结果
                                      ^
                                      |
                              han-docx-action
                          GitHub CI 批量检查与报告
```

## 解决的问题

- 通用代码 Linter 无法解析或检查 DOCX；
- 英文写作工具通常不覆盖中文标点、CJK 间距和中文标题样式；
- 在线检查服务可能不适合处理未公开论文或课程作业；
- 学校、课程、实验室和开放文档项目需要复用不同质量配置；
- DOCX 文档缺少与代码仓库类似的持续质量检查流程。

## 快速开始

### 本地检查

```bash
git clone https://github.com/heavenoracle/han-docx-lint.git
python -m pip install ./han-docx-lint
han-docx-lint paper.docx
```

### 使用规则配置

```bash
git clone https://github.com/heavenoracle/han-docx-rules.git
han-docx-lint paper.docx \
  --config han-docx-rules/profiles/coursework.json \
  --format json
```

### 在 GitHub Actions 中检查

```yaml
name: document-quality

on:
  push:
    paths:
      - "**/*.docx"

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: heavenoracle/han-docx-action@v1
        with:
          paths: documents
          config: rules/coursework.json
```

## 设计原则

- **隐私优先：** 文档仅在本地或用户选择的 CI 运行器中处理。
- **可审计：** 规则配置使用版本化 JSON，可在执行前审查。
- **可扩展：** 检查引擎、规则配置和 CI 编排由独立项目负责。
- **谨慎判断：** 工具只报告可机械检查的问题，不判断论证真实性或学术质量。
- **不冒充官方规范：** 通用规则集不代表任何学校、期刊或标准组织的官方要求。

## 当前能力

- DOCX 包完整性检查；
- 占位文本、重复标点和中文字符间距检查；
- 过长段落、连续空段落、参考文献章节和标题样式检查；
- 内置规则开关及严重级别覆盖；
- 自定义正则文本规则；
- JSON 检查报告；
- GitHub Actions 批量检查和报告构件上传。

## 路线图

- 收集真实用户反馈并降低误报；
- 增加具有可验证来源的规则说明；
- 改进人类可读的 HTML 报告；
- 评估 PDF 与引用质量检查的独立工具；
- 在规则稳定后评估有限、可回滚的自动修复能力。

## 贡献

请根据改动范围前往对应仓库：

- 检查逻辑和配置协议：[han-docx-lint Issues](https://github.com/heavenoracle/han-docx-lint/issues)
- 通用规则配置：[han-docx-rules Issues](https://github.com/heavenoracle/han-docx-rules/issues)
- GitHub Actions 集成：[han-docx-action Issues](https://github.com/heavenoracle/han-docx-action/issues)

本仓库用于生态总览、跨项目文档与路线图讨论。

## English summary

Chinese Academic Document Toolkit is a privacy-first, extensible open-source
ecosystem for checking and automating quality controls for Chinese academic
DOCX files. It separates the checking engine, auditable rule profiles, and
GitHub Actions orchestration into focused projects.

## License

MIT
