# 📖 Data Dictionary — `market_week_data.csv`

This document describes the structure and columns of the simulated market-week panel provided for this Challenge Project. The data is fully simulated; no real customer, transaction, or proprietary ELC data is used.

---

## 🗂️ Panel Structure

| Property | Value |
|----------|-------|
| **File** | [market_week_data.csv](market_week_data.csv) |
| **Format** | CSV, comma-separated, UTF-8, header row included |
| **Rows** | 728 (one row per market per week) |
| **Columns** | 16 |
| **Grain** | One row = one market in one week |
| **Markets** | 7 (Australia, Canada, France, Germany, Japan, United Kingdom, United States) |
| **Weeks** | 104 consecutive weeks, 2025-01-06 through 2026-12-28 |
| **Balance** | Balanced panel — every market appears in all 104 weeks |
| **Missing values** | None |
| **Pilot market** | United States |
| **Launch week** | 2026-04-06 (`week_index` 65) |

The pair `market` + `week_index` uniquely identifies every row.

---

## 📋 Columns

### Identifiers and timing

| Column | Type | Description |
|--------|------|-------------|
| `market` | text | Name of the market. One of seven values listed above. |
| `week_index` | integer | Sequential week counter, 0–103. Comparable across markets: the same `week_index` refers to the same calendar week in every market. |
| `week_start` | date (`YYYY-MM-DD`) | Monday on which the week begins. |
| `weeks_from_launch` | integer | Weeks relative to the launch week, −65 to +38. Negative before launch, `0` in the launch week, positive after. Defined for every market on the same calendar timeline. |

### Business performance metrics

| Column | Type | Description |
|--------|------|-------------|
| `sessions` | integer | Site sessions in the market-week. |
| `fragrance_orders` | integer | Fragrance orders placed in the market-week. |
| `fragrance_revenue` | decimal (2 dp) | Fragrance revenue in the market-week, in simulated currency units. |
| `conversion_rate` | decimal | Share of sessions resulting in a fragrance order. Equals `fragrance_orders / sessions`. |
| `average_order_value` | decimal (2 dp) | Average value of a fragrance order, in simulated currency units. Equals `fragrance_revenue / fragrance_orders`. |
| `revenue_per_session` | decimal (4 dp) | Fragrance revenue per session, in simulated currency units. Equals `fragrance_revenue / sessions`. |

### Context variables

| Column | Type | Description |
|--------|------|-------------|
| `paid_media_index` | decimal (2 dp) | Relative paid media activity for the market-week, expressed as an index scaled around 100. Higher values mean more paid media activity. |
| `promo_intensity` | decimal (4 dp) | Promotional depth for the market-week, expressed as a share between 0 and 1. Higher values mean deeper or broader promotion. |
| `new_visitor_share` | decimal (4 dp) | Share of sessions from visitors new to the site, between 0 and 1. |

### Indicators

| Column | Type | Description |
|--------|------|-------------|
| `pilot_market` | 0/1 | `1` for the pilot market in **every** week of the panel, `0` for all other markets. Identifies *which* market, not *when*. |
| `post_launch` | 0/1 | `1` for **all seven markets** from the launch week onward, `0` before it. Identifies *when*, not *which market*. |
| `digital_feature_available` | 0/1 | `1` only where the digital fragrance advisor was actually available to visitors: the pilot market, from the launch week onward. `0` everywhere else, including every control-market row. |

> 💡 **These three indicators are not interchangeable.** `pilot_market` is `1` in 104 rows, `post_launch` is `1` in 273 rows across all seven markets, and `digital_feature_available` is `1` in only 39 rows. Check which one each part of your analysis needs before you use it.

---

## ⚠️ Things to Watch For

The panel was constructed to resemble a real, messy feature rollout rather than a clean experiment. As noted in the [Challenge Project Overview](../Challenge-Project-Overview.md), the markets were not randomly assigned, activity around the launch window is not limited to the feature itself, and behavior in the weeks immediately before launch is worth inspecting rather than assuming. Part of your work is to identify these issues in the data yourself and decide how they affect your conclusions.
