# Investment Research Verification Framework

Use this reference after `SKILL.md` triggers. It contains the detailed checklists, formulas, templates, and classification rules for investment research verification.

## Input Schema

Extract as much as possible from the user request:

```yaml
research_target:
  type: company | industry | index | etf | fund | commodity | portfolio
  name:
  ticker:
  market: CN | HK | US | GLOBAL
  industry:

research_focus:
  - valuation
  - earnings
  - orders
  - capital_expenditure
  - policy
  - supply_demand
  - fund_flow
  - liquidity
  - risk
  - event_impact

time_range:
  event_window:
  financial_period:
  valuation_period:

comparison:
  historical_average: true
  industry_average: true
  peer_companies: []
  benchmark_index:

output_depth: brief | standard | deep
```

## Event Retrieval

For companies, check:

- Earnings reports, previews, and guidance
- Revenue, profit, and cash-flow changes
- Major contracts, orders, backlog, customer validation, product certification
- Capacity expansion, capex, R&D, acquisitions, disposals
- Buybacks, cancellations, dividends, equity incentives
- Management changes, insider/major-holder increases or decreases
- Penalties, litigation, debt, refinancing, goodwill impairment
- Customer/supplier concentration changes
- Sanctions, export restrictions, tariffs

For industries, check:

- Sales, shipments, prices, inventories, utilization, new capacity
- Capex cycle, upstream raw materials, downstream demand
- Subsidies, regulation, import/export data, technology route changes
- Competition, concentration, price war, over/under-supply
- Guidance from industry leaders

For ETFs and funds, check:

- Latest NAV and premium/discount
- Share/unit changes and estimated net subscriptions/redemptions
- Turnover, spread, tracking error, fund size
- Top holdings, industry/region exposure, index rules
- Manager changes, subscription restrictions, QDII quota limits, fees, distributions

## Financial Data

Collect where relevant:

```yaml
income_statement:
  - revenue
  - revenue_growth
  - gross_profit
  - gross_margin
  - operating_profit
  - net_profit
  - adjusted_net_profit
  - eps

balance_sheet:
  - cash
  - total_debt
  - net_debt
  - inventory
  - accounts_receivable
  - contract_liabilities
  - goodwill
  - construction_in_progress

cash_flow:
  - operating_cash_flow
  - capital_expenditure
  - free_cash_flow

operating_metrics:
  - order_backlog
  - shipment_volume
  - average_selling_price
  - utilization_rate
  - customer_concentration
```

Compute:

```text
Free cash flow = operating cash flow - capital expenditure
Net cash = cash and equivalents - interest-bearing debt
Cash earnings match = operating cash flow / net profit
Receivable growth gap = accounts receivable growth - revenue growth
Inventory growth gap = inventory growth - revenue growth
```

If operating cash flow remains far below net profit, flag earnings-quality risk.

## Verification Closure Matrix

Use this matrix before finalizing. Do not leave a material claim as "to be verified" until the available proxy evidence has been checked.

| Claim | Direct evidence | Proxy evidence when direct evidence is unavailable | Red flags |
| -- | -- | -- | -- |
| Orders are growing | Announced contract value, backlog, signed orders, disclosed customer awards | Contract liabilities, advances from customers, management commentary, capex plans of customers, revenue by product line | Only framework agreements, samples, validation, or rumors |
| Orders converted to revenue | Segment/product revenue growth, shipment volume, utilization | Inventory drawdown, receivable collection, customer concentration stability | Revenue growth with receivables/contract assets rising much faster |
| Revenue converted to profit | Gross profit, operating profit, net profit, margin expansion | Mix change, utilization improvement, price data | Revenue growth with margin decline or one-off gains |
| Profit converted to cash | Operating cash flow, free cash flow, cash earnings match | Receivable turnover, inventory turnover, advances, capex cycle | Net profit up but operating cash flow negative or deteriorating |
| Growth is sustainable | Repeat orders, backlog, customer diversification, capacity plans | R&D progress, new product certification, downstream capex cycle | High customer concentration, subsidy dependence, inventory buildup |
| Valuation is justified | Current valuation vs history, peers, and forward growth | PEG, FCF yield, ROIC, earnings revision trend | Valuation expansion without earnings revision or cash-flow improvement |
| Fund inflow is real | ETF shares outstanding, disclosed holdings, northbound/southbound holdings, fund reports | Trading value, turnover, margin balance | "Main force inflow" only, no share/holding confirmation |

For each material claim, output:

```yaml
verification:
  claim:
  status: verified | partially_verified | not_verified | contradicted | unavailable
  evidence:
  proxy_used:
  confidence: high | medium | low
  implication:
```

## Minimum Research Depth

For a standard or deep company/industry request, include at least:

- Industry demand evidence
- Company financial statistics table with revenue, profit, margins, and cash-flow fields
- Cash-flow and balance-sheet quality evidence
- Valuation evidence with current valuation fields and a relative judgment
- Liquidity/fund-flow evidence with source type and confidence
- Risk checks with explicit pass/fail/unknown state

If time or source access is limited, state that the output is scoped and list which modules were not completed.

## Required Report Artifact

For standard or deep research, write the final research/statistical report to a Markdown file. Prefer the current workspace `outputs/` directory for user-facing deliverables. The filename should be stable and descriptive:

```text
outputs/<target-or-theme>-research-YYYY-MM-DD.md
```

Minimum report structure:

```markdown
# <Target or Theme> Investment Research Verification Report

Date:
Scope:
Method:
Disclaimer:

## Executive Summary
## Research Target and Coverage
## Key Events and Evidence Levels
## Fundamental Verification Chain
## Financial Quality and Cash-Flow Checks
## Valuation Table
## Fund-Flow and Liquidity Table
## Peer/Benchmark Comparison
## Supportive Factors
## Risk Factors and Failed Checks
## Data Gaps and Proxy Indicators
## Final Conclusion
## Source List
```

The chat response should summarize the key conclusion and link to the report file. Do not omit the report file for standard/deep work unless filesystem writing is impossible.

## Required Company Financial Statistics Module

For standard or deep company, industry, or theme research, include a company financial statistics table. Do not replace this table with a narrative or with only a "fundamental verification chain."

Minimum fields:

```yaml
financial_statistics_row:
  company:
  ticker:
  segment_or_role:
  reporting_period:
  revenue:
  revenue_growth:
  gross_margin:
  operating_profit:
  net_profit_or_adjusted_net_profit:
  profit_growth:
  operating_cash_flow:
  free_cash_flow:
  accounts_receivable:
  inventory:
  contract_liabilities_or_advance_receipts:
  financial_quality_judgment: strong | improving | mixed | weak | unavailable
  confidence: high | medium | low
  source:
```

For theme research with many companies, include at least the representative companies used in valuation and fund-flow tables. If a field is not disclosed or cannot be found, keep the column and mark the cell `unavailable`; do not drop the metric silently.

Interpretation rules:

- Revenue growth without profit growth is incomplete realization.
- Profit growth without operating cash-flow improvement is lower-quality realization.
- Receivables, contract assets, inventory, or prepayments growing much faster than revenue should be flagged.
- One-off investment income, disposal gains, subsidy gains, or fair-value changes should be separated from operating profit quality when material.

## Required Combined Valuation, Fund-Flow and Liquidity Module

For every standard or deep listed-security research output, include one combined table named `Valuation, Fund-Flow and Liquidity Table`. Do not split valuation and liquidity into separate tables unless the user explicitly asks for separate tables. Do not replace this table with narrative text.

Minimum fields:

```yaml
valuation_fund_flow_liquidity_row:
  security_or_fund:
  date:
  opening_price:
  closing_or_latest_price:
  price_change:
  trading_value:
  turnover_rate:
  market_cap:
  pe_ttm:
  forward_pe:
  pb:
  ps:
  ev_ebitda:
  fcf_yield:
  dividend_yield:
  relative_volume_or_percentile:
  margin_financing:
  northbound_southbound_or_foreign_holding:
  etf_share_change:
  fund_size_change:
  institutional_holding_change:
  main_force_flow:
  evidence_type: confirmed_flow | market_liquidity | sentiment_proxy | unavailable
  historical_percentile:
  peer_percentile_or_premium:
  growth_context:
  cash_flow_quality:
  valuation_judgment: undervalued | slightly_undervalued | fair | slightly_overvalued | overvalued | unavailable
  liquidity_judgment: strong_positive | positive | neutral | negative | strong_negative | unavailable
  confidence: high | medium | low
  source:
```

Use sector-appropriate substitutions when fields are not meaningful:

- Loss-making or early-growth companies: prioritize PS, gross margin, revenue growth, operating cash flow, unit economics, and cash runway.
- Banks/insurers/brokers: prioritize PB, ROE, capital adequacy, asset quality, and dividend yield.
- Cyclical sectors: include normalized earnings, cycle position, inventory, utilization, and commodity/input price assumptions.
- ETFs/indices: include index PE, PB, dividend yield, valuation percentile, concentration, and earnings-growth expectations.

Valuation evidence priority:

1. Exchange, company, index provider, fund company, or official data vendor pages.
2. Reputable market-data pages with timestamped PE/PB/PS/market-cap values.
3. Broker consensus estimates, clearly labeled as estimates.

If current valuation fields conflict across sources, show the range and explain likely causes, such as static vs TTM PE, adjusted vs GAAP earnings, negative earnings, different share counts, or delayed data.

Price fields are mandatory numeric fields for listed securities. Include same-day opening price and closing price; if the market has not closed, label the latest traded price with the timestamp and update the report after close when the user asks for a daily report. Do not use vague price context such as "quote page available" or "valuation page available."

Evidence confidence:

- High: ETF share/fund-size changes, disclosed holdings, northbound/southbound holdings, fund reports, exchange margin data, verified short interest, company buyback filings.
- Medium: trading value, turnover, relative volume, block trades, margin balance where available.
- Low: "main force net inflow", large-order flow, retail heat rankings, media summaries without source methodology.

For industries or themes, include both:

- Representative stock liquidity: trading value, turnover, margin financing, relative volume for 5 to 10 leaders.
- Theme fund flow: ETF shares/fund-size changes over 1-day, 5-day, 20-day, and 60-day windows where available.

When ETF share data is unavailable, say so explicitly and use trading value only as a market-liquidity proxy, not proof of inflow.

Every metric included in the liquidity table must be a concrete value with unit, date, and source. Do not put source-navigation text or placeholders in cells. If a desired metric cannot be obtained from the preferred source, try the fallback sequence in the source-routing section. If it remains unavailable, remove that metric from the main table for that security and list it in "Data Gaps and Proxy Indicators" with the attempted sources and why confidence is reduced.

## Fund-Flow and Liquidity Source Routing

Use this source routing before generic web search. Prefer official or primary sources for confirmed fund-flow evidence, then use market-data sites for liquidity and sentiment proxies.

### China A-share equities

Official and high-confidence sources:

- Shanghai Stock Exchange: `https://www.sse.com.cn/` for listed-company pages, daily trading, margin trading, block trades, announcements.
- Shenzhen Stock Exchange: `https://www.szse.cn/` for listed-company pages, daily trading, margin trading, block trades, announcements.
- Beijing Stock Exchange: `https://www.bse.cn/` for BSE-listed securities.
- China Securities Depository and Clearing: `https://www.chinaclear.cn/` for investor/account and market statistics when relevant.
- HKEX Stock Connect: `https://www.hkex.com.hk/Mutual-Market/Stock-Connect/` for northbound/southbound aggregate data and eligible lists.
- Company announcements via exchange pages, 巨潮资讯 `https://www.cninfo.com.cn/`, or official company IR pages for shareholder changes, buybacks, pledges, lock-up releases, and institutional holdings.

Secondary market-data sources:

- 东方财富 quote/data pages: `https://quote.eastmoney.com/`, `https://data.eastmoney.com/` for PE/PB/PS, market cap, turnover, margin financing, large-order flow, sector flow.
- 腾讯行情: `https://qt.gtimg.cn/` for A-share same-day open, close/latest, high, low, price change, trading value, turnover, PE, PB, and market-cap snapshots when 东方财富 quote APIs are unstable.
- 新浪财经: `https://finance.sina.com.cn/` and `https://vip.stock.finance.sina.com.cn/` for quotes, announcements, transaction data, and news.
- 证券时报 quote pages: `https://www.stcn.com/quotes/` for market snapshots, financing balance, and related news.
- 富途/Moomoo: `https://www.futunn.com/` and `https://www.moomoo.com/` for quotes, turnover, valuation fields, and market snapshots.
- 雪球: `https://xueqiu.com/` for quote snapshots and announcement mirrors; use as secondary evidence.

For A-share fund-flow tables, keep the market-data source consistent across all representative stocks whenever possible:

1. Use 东方财富 as the default unified source for A-share market snapshots, valuation fields, sector/stock fund-flow pages, turnover, trading value, margin financing, and large-order/main-force flow.
2. Use 同花顺/iFinD pages as the first fallback when 东方财富 does not provide an accessible field.
3. Use 交易所、腾讯行情、新浪、证券时报、富途/Moomoo、雪球 only as fallback or cross-check sources, and label the row as `mixed_source` if their data is combined with 东方财富/同花顺.
4. Do not compare "main force flow" values across different vendors unless the report explicitly says the methodologies differ.
5. If a table uses a unified source, state it in the table note, for example: `A-share liquidity fields use 东方财富 unless otherwise noted.`
6. For price fields, do not stop at source availability. The table must show numeric opening price and closing/latest price. If the default source fails, use a fallback quote source before finalizing.

### Hong Kong equities

Official and high-confidence sources:

- HKEXnews: `https://www.hkexnews.hk/` for filings, announcements, shareholding changes.
- HKEX market data: `https://www.hkex.com.hk/Market-Data/` for official quote and turnover references.
- CCASS shareholding search: use HKEX CCASS pages for custody/holding changes where relevant.
- Stock Connect data: use HKEX Stock Connect pages for southbound holding and flow context.

Secondary sources:

- 富途/Moomoo, AASTOCKS, 新浪港股, 东方财富港股 for valuation, turnover, and daily market snapshots.

### US-listed equities and ADRs

Official and high-confidence sources:

- SEC EDGAR: `https://www.sec.gov/edgar` for filings, share issuance, buybacks, 13F-related issuer disclosures.
- Nasdaq/NYSE official pages for trading and listing context.
- FINRA margin/short-interest pages when relevant.

Secondary sources:

- Nasdaq, Yahoo Finance, Koyfin, TradingView, CompaniesMarketCap, Macrotrends, Moomoo/Futu for valuation, market cap, turnover, short interest, and analyst estimate context.

### ETFs and funds

Official and high-confidence sources:

- Fund manager official product pages for NAV, shares outstanding, fund size, holdings, premium/discount, tracking error, and creation/redemption information.
- Exchange ETF pages for ETF trading value, turnover, premium/discount, and creation/redemption rules.
- Index provider pages for methodology, constituent weights, index valuation and earnings-growth data.

Secondary sources:

- 天天基金 `https://fund.eastmoney.com/`, 东方财富 ETF data pages, 新浪基金, Wind/Choice/iFinD if available through the user environment, and fund-company announcements mirrored on exchange/CNInfo.

### Data to fetch by source type

For each representative stock, try in this order:

1. Trading value, turnover, price change: exchange quote page or reputable quote page.
2. Margin financing / securities lending: SSE/SZSE official margin pages first; then 东方财富/证券时报 if official page is hard to fetch.
3. Northbound/southbound holdings: HKEX Stock Connect or exchange/market-data mirrors.
4. Block trades, buybacks, pledges, lock-up releases: exchange announcements and company filings.
5. Institutional/fund holdings: latest periodic reports, fund holdings, shareholder top-ten disclosures.
6. Main-force or large-order flow: only after higher-confidence sources; label as `sentiment_proxy` and low confidence.

For each ETF or theme fund, try in this order:

1. Fund-company product page for shares outstanding and fund size.
2. Exchange ETF page for trading value, turnover, premium/discount.
3. Fund periodic report for holdings.
4. Secondary ETF flow pages for 1-day, 5-day, 20-day, 60-day share/fund-size changes.

If a source blocks access or requires a paid terminal, try the fallback sources above before using `unavailable`. Do not leave a metric cell as `unavailable` for same-day price, trading value, turnover, PE/PB, or market-cap fields unless all listed public quote fallbacks fail. In that case, remove the metric from the main table and document the failed sources in "Data Gaps and Proxy Indicators."

## Valuation Metrics

General companies:

- PE TTM, forward PE, PB, PS, EV/EBITDA, EV/Sales, FCF yield, dividend yield, PEG

Financial companies:

- PB, ROE, dividend yield, non-performing ratio, net interest margin, capital adequacy

High-growth or loss-making companies:

- PS, EV/Sales, gross margin, revenue growth, operating cash flow, unit economics

Cyclical sectors:

- Do not rely on static PE alone.
- Analyze normalized earnings, commodity price cycle, utilization, inventory, capex, PB, EV/EBITDA, and cycle position.

ETFs:

- Index PE, PB, dividend yield, historical percentile, component earnings growth, top-holding concentration

## Historical Comparison

Compare at least:

- Current value
- Latest 3-year median
- Latest 5-year median
- Historical 25th percentile
- Historical 75th percentile
- Historical high/low range

Use:

```text
Historical percentile = count(historical observations below current value) / total observations
```

Classification:

```yaml
valuation_percentile:
  0-20: significantly_below_history
  20-40: below_history
  40-60: neutral
  60-80: above_history
  80-100: significantly_above_history
```

Valuation percentile alone must not determine whether something is overvalued or undervalued. Reduce the weight of historical averages if business mix, earnings quality, or growth stage has materially changed.

## Peer Comparison

Select peers with similar:

- Main business, revenue structure, market region, profit model
- Growth stage, capital intensity, customer type, leverage profile

Compare:

```yaml
growth:
  - revenue_growth
  - profit_growth
  - eps_growth
profitability:
  - gross_margin
  - operating_margin
  - net_margin
  - roe
  - roic
quality:
  - operating_cash_flow_to_net_profit
  - free_cash_flow_margin
  - debt_ratio
valuation:
  - pe
  - ps
  - pb
  - ev_ebitda
  - fcf_yield
shareholder_return:
  - dividend_yield
  - buyback_yield
  - net_share_count_change
```

Explain whether a premium or discount versus peers is justified by growth, margins, ROIC, balance sheet quality, or shareholder returns.

## Event Impact Template

Use this for each key event:

```yaml
event:
  title:
  date:
  source:
  evidence_level: confirmed_fact | management_guidance | consensus_estimate | unverified
  affected_variables:
    - revenue
    - margin
    - cash_flow
    - valuation
    - liquidity
  impact_horizon: short_term | medium_term | long_term
  preliminary_impact: strong_positive | positive | neutral | negative | strong_negative
  confidence: high | medium | low
  reasoning:
```

Potential positives include real order growth, earnings beats, margin improvement, free-cash-flow improvement, higher utilization, supply/demand improvement, buyback cancellation at reasonable valuation, lower debt, market-share gain, and policy that directly increases demand or reduces cost.

Potential negatives include order cancellation or miss, revenue growth with margin decline, receivables/inventory growing much faster than revenue, unclear capex returns, price war, customer loss, overcapacity, dilution, major selling, penalties, debt/refinancing pressure, and free-cash-flow deterioration.

## Valuation Scoring

Use scoring only as an aid, not a mechanical substitute for judgment:

```yaml
valuation_score:
  historical_valuation: -2 to +2
  peer_valuation: -2 to +2
  earnings_growth: -2 to +2
  earnings_quality: -2 to +2
  balance_sheet: -2 to +2
  shareholder_return: -2 to +2
```

Meaning:

- `+2`: clearly favorable
- `+1`: mildly favorable
- `0`: neutral or insufficient information
- `-1`: mildly unfavorable
- `-2`: clearly unfavorable

Indicative conclusion:

```yaml
total_score:
  7_to_12: likely_undervalued
  3_to_6: slightly_undervalued
  -2_to_2: fair
  -6_to_-3: slightly_overvalued
  -12_to_-7: likely_overvalued
```

Always provide confidence.

## Fund Flow and Liquidity

For individual stocks, check:

- Latest 1-day, 5-day, 20-day, 60-day turnover and trading value
- Trading value percentile versus the past year
- Turnover rate, margin balance, block trades
- northbound/southbound holdings where relevant
- Institutional holding changes, ETF passive ownership effect
- Major-holder/management changes, pledges, lock-up expirations

Treat "main force net inflow" as a model-based estimate from trade size, not confirmed institutional behavior. Give it low or medium credibility unless independently corroborated.

For ETFs, estimate fund flow primarily from:

```text
change in fund shares * fund NAV
```

Do not infer ETF net subscriptions from secondary-market buy orders alone.

ETF interpretation:

```text
price up + shares up: net inflow and rising market acceptance
price up + shares down: possible redemptions or profit taking
price down + shares up: possible contrarian accumulation
price down + shares down: stronger outflow signal
```

For industry fund flow, check:

- Industry index performance, turnover, turnover share of total market
- Industry ETF share changes
- Active fund allocation, financing balance, northbound/southbound preference
- New thematic fund issuance, IPO/refinancing size, industrial capital flows

Liquidity classification:

```yaml
liquidity_state:
  strong_inflow:
    - trading value rising
    - ETF shares increasing
    - financing balance increasing
    - price trend up
  speculative_inflow:
    - trading value surging
    - ETF shares not clearly increasing
    - valuation expanding quickly
    - fundamentals not improving in sync
  neutral:
    - indicators conflict
  outflow:
    - trading value falling
    - ETF shares falling
    - financing balance falling
    - performance weaker than benchmark
```

Macro liquidity by market:

- China: policy rates, government bond yields, social financing, M1/M2, interbank rates, RMB exchange rate, northbound/southbound flow, market turnover, margin balance, new fund issuance
- United States: federal funds rate, 2-year/10-year Treasury yields, real rates, DXY, Fed balance sheet, bank reserves, money-market fund assets, ETF flows, VIX, credit spreads
- Commodities: DXY, real rates, inventory, futures positioning, term structure, spot premium/discount, CFTC positioning, key ETF share changes

Liquidity conclusion:

```yaml
liquidity_conclusion:
  strong_positive: sustained inflow with trading and share/unit improvement
  positive: marginal improvement, durability uncertain
  neutral: mixed indicators, no clear trend
  negative: redemption, volume contraction, or weaker risk appetite
  strong_negative: sustained withdrawal with price and turnover deterioration
```

## Confidence Rules

High confidence generally requires official or primary-source data, cross-source verification, complete financial data, reasonable peers, clear fund-flow methodology, and conclusions not dependent on one forecast.

Medium confidence applies when some data comes from third parties, industry data lags, order details are undisclosed, valuation is cycle-sensitive, or fund-flow definitions differ.

Low confidence applies when conclusions depend on rumor, disclosure is insufficient, real-time fund-flow data is unavailable, peers are weak, business is changing quickly, or assumptions dominate.

## Data Gaps

If key data is unavailable:

1. State exactly what is missing.
2. Do not fill gaps with guesses.
3. Lower confidence.
4. Offer proxy indicators.
5. State which conclusions cannot currently be supported.

Example:

```text
公司未披露AI业务收入和订单金额，因此无法确认AI需求是否已转化为实质业绩。
目前只能根据资本开支、合同负债和管理层表述进行间接判断，结论置信度为低至中等。
```

## Output Template

Begin with an executive summary under 200 Chinese characters or a similarly concise English paragraph covering the most important fact, valuation state, positive/negative events, fund-flow state, and largest risk.

Then include these sections as relevant:

```markdown
## 关键事件表

| 日期 | 事件 | 信息级别 | 影响 | 时间范围 | 置信度 |
| -- | -- | -- | -- | -- | -- |

## 基本面验证链

| 环节 | 状态 | 证据 | 说明 |
| -- | -- | -- | -- |
| 行业需求 | 已兑现/部分兑现/尚未兑现/数据不足 |  |  |
| 客户资本开支 | 已兑现/部分兑现/尚未兑现/数据不足 |  |  |
| 公司订单 | 已兑现/部分兑现/尚未兑现/数据不足 |  |  |
| 收入 | 已兑现/部分兑现/尚未兑现/数据不足 |  |  |
| 利润率 | 已兑现/部分兑现/尚未兑现/数据不足 |  |  |
| 净利润 | 已兑现/部分兑现/尚未兑现/数据不足 |  |  |
| 自由现金流 | 已兑现/部分兑现/尚未兑现/数据不足 |  |  |

## 估值比较表

| 指标 | 当前 | 历史中位数 | 行业中位数 | 同业水平 | 初步判断 |
| -- | --: | --: | --: | --: | -- |

## 资金流和流动性表

| 指标 | 1日 | 5日 | 20日 | 60日 | 结论 |
| -- | --: | --: | --: | --: | -- |

## 多空因素

### 支持因素

- 因素：证据

### 风险因素

- 风险：证据

## 最终结论

估值判断：
事件判断：
流动性判断：
综合判断：
结论置信度：

当前市场隐含的核心预期：
要使当前估值合理，公司需要实现：
可能推翻当前结论的关键事件：
下一次应重点观察的数据：
```

Overall conclusion schema:

```yaml
conclusion:
  fundamental: positive | neutral | negative
  valuation: undervalued | slightly_undervalued | fair | slightly_overvalued | overvalued
  event_impact: positive | neutral | negative
  liquidity: positive | neutral | negative
  overall: favorable | cautiously_favorable | neutral | cautious | unfavorable
  confidence: high | medium | low
```
