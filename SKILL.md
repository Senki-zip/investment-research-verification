---
name: investment-research-verification
description: Investment research verification for listed companies, industries, indices, ETFs, funds, commodities, macro themes, and portfolios. Use when Codex needs to retrieve and verify public investment information, distinguish confirmed facts from guidance/consensus/rumor, compare valuation against history/peers/benchmarks, assess event impact, fund flows, liquidity, fundamentals, risks, and produce structured high/low valuation, positive/negative catalyst, and confidence conclusions without issuing deterministic buy/sell advice.
---

# Investment Research Verification

## Purpose

Use this skill to perform preliminary investment research from public information. Verify facts from authoritative sources, connect events to fundamentals and cash flow, compare valuation across time and peers, evaluate liquidity and fund flows, and return a structured, uncertainty-aware conclusion.

Do not provide deterministic buy/sell instructions. Do not equate short-term price movement with business value change.

## Default Scope

When the user does not specify parameters, use:

- Event window: latest 30 days
- Financial comparison: latest 3 years
- Valuation history: latest 5 years
- Fund flow windows: 1 day, 5 days, 20 days, 60 days
- Peer set for company research: 3 to 8 comparable companies
- ETF review: NAV, premium/discount, turnover, volume, shares outstanding, fund size, index valuation, top holdings

If multiple securities share a name or ticker, verify the exact identity, exchange, market, and product type before analyzing.

## Source Hierarchy

Prefer sources in this order:

1. Official filings and primary data: company announcements, annual/interim/quarterly reports, exchange filings, regulator filings, investor relations records, official government data, central bank/statistics/customs data, fund company disclosures, ETF creation/redemption files, index methodology documents.
2. Authoritative market databases: Wind, Bloomberg, Refinitiv, Choice, iFinD, TradingView, Koyfin, FRED, Trading Economics, exchange market data, fund company data.
3. Authoritative media as event leads: Reuters, Bloomberg News, Financial Times, Wall Street Journal, 第一财经, 财联社, 证券时报, 上海证券报, 中国基金报. Verify material numbers back to filings or official data where possible.
4. Broker research and institutional views: use for industry sizing, forecasts, peer selection, and consensus estimates only. Label clearly as third-party estimates.
5. Social media, forums, and self-media: use only as search leads, never as the sole basis for an important conclusion.

Every important claim must identify whether it is:

- `confirmed_fact`: filing, official data, or directly disclosed financial/operating data
- `management_guidance`: company management forecast, target, or commentary
- `consensus_estimate`: analyst or market estimate
- `unverified`: media leak, supply-chain rumor, or social-media claim

## Required Time Discipline

For key events and data, record:

- Event date
- Publication date
- Reporting/data period
- Whether it is the latest available period
- Whether later disclosures revised or contradicted it

Use current data for current conclusions. Do not use stale filings to explain a business that has materially changed.

## Core Workflow

1. Identify the target: formal name, ticker, market, business lines, revenue drivers, industry, peers, benchmarks, and time range.
2. Build the investment logic chain:

   ```text
   Industry demand
   -> customer capex or end demand
   -> company orders
   -> revenue
   -> gross margin
   -> operating profit
   -> free cash flow
   -> EPS
   -> dividends/buybacks
   -> current valuation
   ```

3. Retrieve authoritative disclosures and recent events.
4. Extract financials, operating metrics, valuation, and liquidity/fund-flow data.
5. Compare against historical ranges, industry averages, peers, and relevant market benchmarks.
6. Test whether growth has been realized in orders, revenue, profit, and free cash flow.
7. Convert every "needs further verification" item into an immediate evidence search unless the data is genuinely unavailable.
8. Classify each key event as positive, neutral, or negative with confidence and time horizon.
9. Check risk factors and value-trap conditions before giving any favorable or undervalued conclusion.
10. Write a Markdown research/statistical report file for standard or deep research requests.
11. Output a structured conclusion with verified evidence, failed checks, missing data, assumptions, uncertainty, next verification points, and a link to the saved report file.

For detailed metric checklists, scoring rules, event templates, and output tables, read `references/research-framework.md`.

## Verification Closure

Do not stop at a list of "items to verify" when the user asks for research. For each material uncertainty, perform the next available evidence search before finalizing:

- If order data is unavailable, check revenue growth, contract liabilities, advance receipts, backlog, management commentary, customer concentration, and inventory/prepayment changes.
- If revenue growth is claimed, verify whether gross margin, operating margin, net profit, operating cash flow, and free cash flow improved in the same period.
- If profit growth is claimed, verify whether accounts receivable, contract assets, inventory, and capitalized costs grew faster than revenue.
- If fund inflow is claimed, distinguish ETF share changes or disclosed holdings from secondary-market trade-size estimates.
- If valuation is claimed low or high, compare current valuation to history, peers, expected growth, cash-flow quality, and balance-sheet risk.

Assign each verification item one of:

- `verified`: direct primary-source evidence supports the claim.
- `partially_verified`: proxy indicators support the claim, but direct evidence is incomplete.
- `not_verified`: available evidence does not support the claim.
- `contradicted`: available evidence points against the claim.
- `unavailable`: the required data is not disclosed or accessible; provide the best proxy and lower confidence.

Final answers must include the evidence status for material claims. Only put an item into "next things to track" after attempting available verification and explaining why it cannot be closed now.

## Non-Optional Modules

For standard or deep research on listed companies, industries, indices, ETFs, funds, or portfolios, do not omit valuation or fund-flow/liquidity analysis. If live data is unavailable, include the module with `unavailable` status, the last accessible observation date, proxy indicators, and lowered confidence.

For standard or deep company/industry/theme research, do not omit a company financial statistics table. At minimum include revenue, revenue growth, net profit or adjusted net profit, profit growth, gross margin or net margin where available, operating cash flow, and the reporting period for each representative company. If a field is unavailable, mark it `unavailable` rather than dropping the column.

Valuation must include at least current market cap or price context plus PE/PB/PS or the sector-appropriate alternatives. For high-growth or loss-making companies, include PS, gross margin, revenue growth, cash-flow quality, and forward expectation context. For cyclical companies, include normalized-cycle caveats.

Fund-flow/liquidity must separate:

- Confirmed fund flow: ETF shares outstanding, fund size changes, disclosed holdings, northbound/southbound holdings, institutional filings.
- Market liquidity: trading value, turnover, relative volume, margin financing, short interest where applicable.
- Low-confidence sentiment proxies: "main force net inflow", large-order flow, media-reported hot money.

Never use trading value or "main force" statistics as proof of real fund subscription or institutional buying.

Before generic web search for fund-flow or liquidity, use the source routing in `references/research-framework.md`: exchanges and official fund-company pages first, then reputable market-data sites, and only then low-confidence "main force" or large-order flow pages.

For A-share fund-flow and liquidity tables, use a single market-data vendor across representative companies whenever possible. Prefer 东方财富 as the default source, use 同花顺/iFinD as fallback, and mark rows as `mixed_source` if data must be combined from other vendors. Do not compare vendor-specific "main force flow" values without noting methodology differences.

Numerical market-data fields must be closed with concrete values, not page pointers. For listed securities, any table that includes price or liquidity must include the observation date, opening price, closing price or latest traded price, price change, trading value, turnover rate, market cap, and the source. If 东方财富 is unstable or does not expose a field, immediately use 同花顺/iFinD, exchange pages, 腾讯行情, 新浪财经, 证券时报, 富途/Moomoo, TradingView, or another reputable quote source and label the fallback source. Do not write placeholder phrases such as "估值页可查", "行情页入口可查", "页面可查", "未稳定抓取", or similar text in metric cells. A metric cell must contain a numeric value with unit and period, or the metric should be removed from that table and explained in the data-gap section after fallback sources have been attempted.

## Report File Requirement

For standard or deep research, create a user-facing Markdown report in the active workspace `outputs/` directory when available. Use a descriptive lowercase filename such as:

```text
outputs/<topic>-research-YYYY-MM-DD.md
```

The report must include the complete evidence tables, valuation table, fund-flow/liquidity table, source list, data gaps, confidence labels, and final conclusion. The chat answer may be a concise summary, but it must link to the saved report. If file writing is not available, state that limitation and include the full report in the response.

## Analysis Rules

Never jump directly from "industry outlook is good" to "the company is attractive." Answer:

- Is industry demand actually growing?
- Did the company obtain real orders?
- Did orders convert to revenue?
- Did revenue convert to profit and free cash flow?
- Has the current valuation already priced in that growth?

For growth stories, compare:

```text
Share price change
vs revenue growth
vs net profit growth
vs free cash flow growth
vs analyst earnings forecast revisions
```

Interpretation guardrails:

- Share price rising faster than earnings forecast revisions suggests valuation expansion and higher risk.
- Revenue growth without profit or cash-flow improvement suggests weak growth quality.
- Orders, revenue, profit, and free cash flow rising together suggests better fundamental realization.
- Industry improvement without company revenue improvement suggests the company may not be benefiting.

## Risk Checks

Before any positive or undervalued conclusion, check for:

- Financial fraud or audit risk
- Large goodwill or impairment risk
- High debt, refinancing pressure, or pledge risk
- Controlling shareholder or insider selling
- Major lock-up expiration or dilution
- Customer or supplier concentration
- Abnormal accounts receivable or inventory growth
- Operating cash flow deterioration
- Industry overcapacity or price war
- Policy, regulatory, sanction, tariff, or export-control risk
- Valuation dependence on aggressive forecasts

Before an undervalued conclusion, explicitly test whether the low valuation may be caused by peak-cycle earnings, structural decline, governance problems, debt stress, asset-quality deterioration, or market share loss.

## Prohibited Behavior

Do not:

- Treat news headlines as conclusions.
- Treat an industry catalyst as automatic company benefit.
- Treat framework agreements, intention letters, samples, validation, or pilot projects as booked revenue/profit.
- Treat contract value as net profit.
- Treat static low PE as sufficient evidence of undervaluation.
- Treat price gains as proof of fundamental improvement.
- Treat "main force net inflow" as confirmed institutional buying.
- Treat secondary-market ETF buying volume as fund net subscription.
- Ignore dilution, buyback cancellation, or share-count changes.
- Use future data to explain a past investment decision.
- Fabricate missing data.
- Give high confidence when critical data is missing.
- Base conclusions on a single self-media post or broker report.
- Use phrases such as "guaranteed profit", "must rise", or "risk-free".

## Output Style

Start with the conclusion, then the evidence. Use concrete dates and periods. Separate facts, forecasts, and speculation. Explain why each important data point affects future cash flow. Present both positive and negative interpretations when evidence is contested.

End with:

```text
本结论属于基于公开信息的初步研究，不构成确定性的投资建议。
后续应重点跟踪：订单兑现、盈利预测变化、自由现金流、估值分位和资金份额变化。
```
