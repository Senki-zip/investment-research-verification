# Investment Research Verification

面向上市公司、行业、指数、ETF、基金、商品和宏观主题的 Codex skill，用于投资信息检索、事实核验、估值比较、事件影响分析和资金流/流动性判断。

该 skill 的目标不是给出确定性的买卖指令，而是帮助 Codex 基于公开信息形成结构化、可追溯、标注置信度的初步投资研究结论。

## 适用场景

- 分析某家公司当前估值是否偏高或偏低
- 验证某个公告、订单、政策或行业事件对公司的实际影响
- 判断行业景气度是否已经传导到公司收入、利润和现金流
- 比较目标公司与历史估值、行业均值和同业公司的差异
- 分析 ETF 或基金的份额变化、折溢价、成交活跃度和资金流
- 区分已确认事实、管理层指引、市场一致预期和未经证实信息

## 核心原则

- 优先使用公告、财报、交易所文件、监管文件、基金公司披露、政府和央行数据等一级来源
- 媒体报道只作为线索，关键数据尽量回到原始披露验证
- 券商研报和机构观点必须标记为预测或第三方估计
- 不把行业利好直接等同于公司利好
- 不把框架协议、送样、验证中、意向协议直接视为收入或利润
- 不把静态低 PE 直接判断为低估
- 在关键数据缺失时降低置信度，并明确说明哪些结论暂时不能成立

## 安装

将本仓库放入 Codex skills 目录，例如：

```bash
mkdir -p ~/.codex/skills
git clone https://github.com/Senki-zip/investment-research-verification.git ~/.codex/skills/investment-research-verification
```

也可以手动下载本仓库，并确保目录结构类似：

```text
~/.codex/skills/investment-research-verification/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── research-framework.md
```

## 使用方式

在 Codex 中明确调用：

```text
Use $investment-research-verification to analyze whether NVIDIA is overvalued based on recent earnings, valuation, peer comparison, and fund flows.
```

中文示例：

```text
使用 $investment-research-verification 分析宁德时代目前是否高估，重点看估值分位、同业比较、订单兑现、自由现金流和近期资金流。
```

也可以提出更具体的问题：

```text
使用 $investment-research-verification 验证某公司 AI 订单传闻是否已经转化为收入和利润。
```

```text
使用 $investment-research-verification 分析某半导体 ETF 最近 20 日是否有真实资金净流入，区分二级市场成交和基金份额变化。
```

## 输出内容

典型输出会包括：

- 不超过 200 字的执行摘要
- 关键事件表，标注信息级别、影响方向、时间范围和置信度
- 基本面验证链：行业需求、客户资本开支、公司订单、收入、利润率、净利润、自由现金流
- 估值比较表：当前估值、历史中位数、行业中位数、同业水平和初步判断
- 资金流和流动性表：1 日、5 日、20 日、60 日维度
- 支持因素和风险因素
- 最终结论：估值判断、事件判断、流动性判断、综合判断和结论置信度
- 下一步应重点跟踪的数据

## 文件说明

- `SKILL.md`：Codex skill 的入口文件，包含触发描述、核心流程、风险边界和输出风格
- `references/research-framework.md`：详细研究框架，包括指标清单、估值规则、事件模板、资金流判断和输出模板
- `agents/openai.yaml`：Codex UI 元数据

## 免责声明

本 skill 仅用于基于公开信息的初步投资研究和信息核验，不构成确定性的投资建议、买卖建议或收益承诺。任何投资决策都应结合个人风险承受能力、完整尽调和专业意见。
