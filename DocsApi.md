# AMS1One API Documentation

**Base URL:** `/api/v1/`

All endpoints require `Authorization: Bearer <access_token>`.

---

## Table of Contents

### Authentication
1. [Login Endpoint](#login-endpoint)
2. [Logout Endpoint](#logout-endpoint)

### Sales — Dashboard
3. [Writer Statistics Endpoint](#writer-statistics-endpoint)
4. [Detailed Tickets Endpoint](#detailed-tickets-endpoint)
5. [Game Types Endpoint](#game-types-endpoint)
6. [Today's Sales Endpoint](#todays-sales-endpoint)
7. [Today's Top Ups Endpoint](#todays-top-ups-endpoint)
8. [Available Float Endpoint](#available-float-endpoint)
9. [Today's Claims Endpoint](#todays-claims-endpoint)
10. [Today's Wins Endpoint](#todays-wins-endpoint)
11. [Winning Events Endpoint](#winning-events-endpoint)
12. [Winners List Endpoint](#winners-list-endpoint)

### Draw — Dashboard
16. [Draws & Winnings Table](#draws--winnings-table)
17. [Draw Event Tickets](#draw-event-tickets)
29. [Draws & Winnings Dashboard Card](#draws--winnings-dashboard-card)

### Analysis — Dashboard
13. [Active Writer Daily Stats Endpoint](#active-writer-daily-stats-endpoint)
14. [Download Active Writer Daily Stats Endpoint](#download-active-writer-daily-stats-endpoint)
15. [Top Writers Endpoint](#top-writers-endpoint)
18. [Top-Up Statistics](#top-up-statistics)
19. [Winning Statistics](#winning-statistics)
20. [Best & Worst Performance](#best--worst-performance)
21. [Retention Rate](#retention-rate)
22. [Retention Rate Trend](#retention-rate-trend)
23. [Sales Card](#sales-card)
24. [Net Top-Ups Card](#net-top-ups-card)
25. [Writers at Work Card](#writers-at-work-card)
26. [Wins Card](#wins-card)
27. [Liquidation Card](#liquidation-card)
28. [Settlements Card](#settlements-card)

### Supervisors
- [Register Supervisor](#register-supervisor)
- [Edit Supervisor](#edit-supervisor)
- [List Supervisors with Owner Details](#list-supervisors-with-owner-details)
- [Detail Cards](#detail-cards)
- [Snapshot](#supervisor-snapshot)
- [Summary](#summary)
- [Writers Overview](#writers-overview)
- [Transactions](#transactions)
- [Dashboard](#dashboard)
- [Statistics](#statistics)
- [Writers (Authenticated Supervisor)](#writers-authenticated-supervisor)
- [Create Writer](#create-writer)
- [Writer Detail](#writer-detail)
- [Writer Top-Ups](#writer-top-ups)
- [Writer Sales](#writer-sales)
- [Writer Sales Summary](#writer-sales-summary)
- [Today's Top-Up](#todays-top-up)
- [Transaction Summary](#transaction-summary)

### Reports
- [List Reports](#list-reports)
- [Get Report Schema](#get-report-schema)
- [Execute Report](#execute-report)
- [Download Report (Excel)](#download-report-excel)

#### Report Catalogue
| ID | Name | Category |
|---|---|---|
| 1 | [30 Days Sales Tracker](#report-1-30-days-sales-tracker) | Operations |
| 2 | [Bank Transfer - Batch Details](#report-2-bank-transfer---batch-details) | Finance |
| 3 | [Bank Transfers](#report-3-bank-transfers) | Finance |
| 5 | [Commission Payments](#report-5-commission-payments) | Finance |
| 6 | [Ticket Query](#report-6-ticket-query) | Finance |
| 7 | [Daily Sales](#report-7-daily-sales) | Finance |
| 8 | [Daily Sales & Winnings](#report-8-daily-sales--winnings) | Finance |
| 9 | [Finance - Payout](#report-9-finance---payout) | Finance |
| 14 | [Revenue Per Play](#report-14-revenue-per-play) | Finance |
| 17 | [Ticket Status](#report-17-ticket-status) | General |
| 18 | [Active Writers](#report-18-active-writers) | General |
| 19 | [Terminal History](#report-19-terminal-history) | General |
| 20 | [Winning Stakes Report](#report-20-winning-stakes-report) | General |
| 21 | [All Stakes Report](#report-21-all-stakes-report) | Operations |
| 23 | [Topup - Claims as Credit](#report-23-topup---claims-as-credit) | Finance |
| 24 | [Topup - Supervisor Transfers](#report-24-topup---supervisor-transfers) | Finance |
| 25 | [Topup - Mobile Money](#report-25-topup---mobile-money) | Finance |
| 26 | [Writers - Active Writers](#report-26-writers---active-writers) | General |
| 27 | [Export Top-Ups](#report-27-export-top-ups) | Finance |
| 28 | [Export Sales](#report-28-export-sales) | Finance |
| 29 | [Export Wins](#report-29-export-wins) | Finance |


---

## Login Endpoint

**`POST /api/v1/auth/login/`**

**Permission:** Public

Accepts `phone` + `password` or `email` + `password`.

**Request**

```json
{ "phone": "+233244979958", "password": "secret" }
```

or

```json
{ "email": "frank@writer.ams1one.com", "password": "secret" }
```

**Response `200 OK`**

```json
{
  "access": "<access_token>",
  "refresh": "<refresh_token>"
}
```

---

## Logout Endpoint

**`POST /api/v1/auth/logout/`**

**Permission:** Public

Blacklists the refresh token, invalidating the session.

**Request**

```json
{ "refresh": "<refresh_token>" }
```

**Response `200 OK`**

```json
{}
```

---

## Writer Statistics Endpoint

**`GET /api/v1/writers/statistics/`**

**Permission:** Writer or above

Returns per-writer sales, stakes, and top-up totals.

```json
{
  "totalwriterFloat": "GHS 14,826.43",
  "totalwriters": "42",
  "writers": [
    {
      "sales": "GHS 14,826.43",
      "total_stakes": "20",
      "topup": "GHS 10,291.00",
      "writer": {
        "id": "32",
        "name": "Frank Mawuli",
        "profileImage": null,
        "online": false,
        "lastSeen": "2026-04-27 22:24:53",
        "contact": {
          "email": "frank@writer.ams1one.com",
          "phone": "+233244979958"
        }
      }
    }
  ]
}
```

---

## Detailed Tickets Endpoint

**`GET /api/v1/sales/tickets/detailed-list/`**

**Permission:** Operator or above

Paginated detailed ticket list with stake and writer information.

```json
{
  "count": 2425,
  "next": "...?page=2",
  "previous": null,
  "results": [
    {
      "ticket_no": "202604272216000084067183",
      "time": "2026-04-27 22:16:00",
      "total_stake_amount": "0.50",
      "total_stake": 1,
      "play_group": "Direct",
      "writer_name": "SANI HUSEMA",
      "stakes": [
        {
          "stake_id": "03dc36ce-c5fc-48bd-8130-0b40aa9a6e78",
          "created_at": "2026-04-27 22:16:00",
          "game": { "id": "0cd0be0b-...", "name": "VAG", "code": "MVG" },
          "event_id": "e2234779-...",
          "event": "VAG Tuesday",
          "play_group": "Direct",
          "play": "Direct 2 (2 Sure)",
          "numbers": "24,69",
          "stake_amount": "0.50",
          "original_numbers": "24,69",
          "player_phone": "",
          "stake_status": "ACTIVE",
          "writer": { "id": "84", "name": "SANI HUSEMA", "phone": "+233504300691" }
        }
      ]
    }
  ]
}
```

---

## Game Types Endpoint

**`GET /api/v1/games/types/`**

**Permission:** Writer or above

Returns all active game types.

---

## Today's Sales Endpoint

**`GET /api/v1/sales/tickets/today_sales/`**

**Permission:** Operator or above

Returns today's ticket sales totals, excluding cancelled tickets.

```json
{
  "date": "2026-04-27",
  "total_sales": 20322.10,
  "ticket_count": 200,
  "currency": "GHS"
}
```

| Field | Type | Description |
|---|---|---|
| `date` | string | Today's date (ISO 8601) |
| `total_sales` | number | Sum of all non-cancelled ticket amounts today |
| `ticket_count` | integer | Count of non-cancelled tickets today |
| `currency` | string | Always `"GHS"` |

---

## Today's Top Ups Endpoint

**`GET /api/v1/writers/today-topup/`**

**Permission:** Writer or above

Returns total top-ups for today.

---

## Available Float Endpoint

**`GET /api/v1/writers/available-float/`**

**Permission:** Operator or above

Returns the total available float — sum of `airtime_balance` across all writer airtime wallets.

```json
{
  "available_float": "GHS 124,064.43",
  "available_float_amount": 124064.43,
  "currency": "GHS"
}
```

| Field | Type | Description |
|---|---|---|
| `available_float` | string | Formatted total airtime balance across all writers |
| `available_float_amount` | number | Raw decimal value |
| `currency` | string | Always `"GHS"` |

---

## Today's Claims Endpoint

**`GET /api/v1/sales/wins/today_claims/`**

**Permission:** Operator or above

Returns today's claimed win amounts and successful withdrawals.

```json
{
  "date": "2026-04-07",
  "total_claims": 242846.40,
  "claims_withdrawn": 156942.60,
  "currency": "GHS"
}
```

| Field | Type | Description |
|---|---|---|
| `total_claims` | number | Sum of wins claimed today |
| `claims_withdrawn` | number | Sum of successful withdrawals today |
| `currency` | string | Always `"GHS"` |

---

## Today's Wins Endpoint

**`GET /api/v1/sales/wins/today_wins/`**

**Permission:** Operator or above

Returns today's total win amount and unique winning players.

```json
{
  "date": "2026-04-07",
  "total_win_amount": 287738.40,
  "unique_players": 749,
  "currency": "GHS"
}
```

| Field | Type | Description |
|---|---|---|
| `total_win_amount` | number | Sum of all wins computed today |
| `unique_players` | integer | Distinct player phones on winning tickets today |
| `currency` | string | Always `"GHS"` |

---

## Winning Events Endpoint

**`GET /api/v1/sales/wins/winning_events/`**

**Permission:** Operator or above

Returns today's draw events with their winning numbers.

```json
{
  "date": "2026-04-07",
  "events": [
    {
      "event_id": 828,
      "event_name": "Morning VAG",
      "event_no": 828,
      "winning_numbers": [43, 5, 90, 56, 70]
    }
  ]
}
```

| Field | Type | Description |
|---|---|---|
| `event_id` | integer | Draw event number |
| `event_name` | string | Draw event name |
| `event_no` | integer | Draw event number |
| `winning_numbers` | array | Numbers drawn for the event |

---

## Winners List Endpoint

**`GET /api/v1/sales/wins/winners_list/`**

**Permission:** Operator or above

Returns today's full winners list with staked and winning numbers.

```json
{
  "date": "2026-04-27",
  "winners": [
    {
      "numbers_staked": [[11, 19, 34, 45]],
      "winning_lines": [[11, 19, 34, 45]],
      "win_amount": 24.0,
      "player_phone": "",
      "writer_name": "Thompson Lord",
      "event_name": "Monday Special",
      "event_no": 1027
    }
  ]
}
```

| Field | Type | Description |
|---|---|---|
| `numbers_staked` | array | All numbers on each stake of the winning ticket |
| `winning_lines` | array | Stakes that were marked as winners |
| `win_amount` | number | Total win amount |
| `player_phone` | string | Player phone (empty for POS tickets) |
| `writer_name` | string | Writer who sold the ticket |
| `event_name` | string | Draw event name |
| `event_no` | integer | Draw event number |

---

---

# Writers Endpoints

---

## Active Writer Daily Stats Endpoint

**`GET /api/v1/writers/active-writer-daily-stats/`**

**Permission:** Operator or above

Returns day-by-day active writer counts for the last N days (default 30), or a monthly breakdown when `?days=365`.

**Query Parameters**

| Parameter | Type | Default | Description |
|---|---|---|---|
| `days` | integer | `30` | `30` for daily breakdown, `365` for monthly breakdown |

**Response — `?days=30` (daily)**

```json
{
  "totals": {
    "total_writers": 84,
    "active_writers": 12
  },
  "period": {
    "start_date": "2026-03-30",
    "end_date": "2026-04-28",
    "days": 30
  },
  "download_url": "http://example.com/api/v1/writers/download-active-writer-daily-stats/?days=30",
  "days": [
    { "day": "2026-03-30", "active_writers": 0 },
    { "day": "2026-04-27", "active_writers": 3 },
    { "day": "2026-04-28", "active_writers": 5 }
  ]
}
```

**Response — `?days=365` (monthly)**

```json
{
  "totals": {
    "total_writers": 84,
    "active_writers": 12
  },
  "period": {
    "start_date": "2025-05-01",
    "end_date": "2026-04-28",
    "days": 365
  },
  "download_url": "http://example.com/api/v1/writers/download-active-writer-daily-stats/?days=365",
  "months": [
    { "month": "May '25", "active_writers": 0 },
    { "month": "Apr '26", "active_writers": 7 }
  ]
}
```

| Field | Type | Description |
|---|---|---|
| `totals.total_writers` | integer | All writers in the system |
| `totals.active_writers` | integer | Writers with at least one active ticket in the period |
| `period.start_date` | string | First date of the period (ISO 8601) |
| `period.end_date` | string | Last date of the period (ISO 8601) |
| `download_url` | string | URL to download the same data as `.xlsx` |
| `days` | array | Present when `days=30` — one entry per calendar day |
| `months` | array | Present when `days=365` — one entry per calendar month |

---

## Download Active Writer Daily Stats Endpoint

**`GET /api/v1/writers/download-active-writer-daily-stats/`**

**Permission:** Operator or above

Downloads the active writer stats as an Excel (`.xlsx`) file. Accepts the same `?days=` parameter as the JSON endpoint.

**Query Parameters**

| Parameter | Type | Default | Description |
|---|---|---|---|
| `days` | integer | `30` | `30` for daily sheet, `365` for monthly sheet |

**Response**

Binary `.xlsx` file download.

```
Content-Type: application/vnd.openxmlformats-officedocument.spreadsheetml.sheet
Content-Disposition: attachment; filename="active_writers_2026-03-30_2026-04-28.xlsx"
```

The workbook contains two sheets:
- **Active Writers** — main data (Day/Month + Active Writers columns)
- **Summary** — period metadata (start date, end date, days, total writers, active writers in period)

---

## Top Writers Endpoint

**`GET /api/v1/writers/top-writers/`**

**Permission:** Operator or above

Returns the top N writers ranked by total sales (all-time).

**Query Parameters**

| Parameter | Type | Default | Max | Description |
|---|---|---|---|---|
| `limit` | integer | `10` | `50` | Number of writers to return |

**Response `200 OK`**

```json
{
  "writers": [
    {
      "rank": 1,
      "writer_id": 32,
      "writer_name": "Frank Mawuli",
      "photo_url": null,
      "total_sales": {
        "formatted": "GHS 16,951.43",
        "amount": 16951.43
      },
      "net_profit": {
        "formatted": "GHS 16,951.43",
        "amount": 16951.43
      }
    },
    {
      "rank": 2,
      "writer_id": 5,
      "writer_name": "Occc Appiah",
      "photo_url": null,
      "total_sales": {
        "formatted": "GHS 13,267.70",
        "amount": 13267.70
      },
      "net_profit": {
        "formatted": "GHS 13,267.70",
        "amount": 13267.70
      }
    }
  ]
}
```

| Field | Type | Description |
|---|---|---|
| `rank` | integer | Position by total sales (1 = highest) |
| `writer_id` | integer | Writer's numeric ID |
| `writer_name` | string | Writer's full name |
| `photo_url` | string \| null | Absolute URL to writer photo, or `null` |
| `total_sales.formatted` | string | Formatted total ticket sales (ACTIVE/WON/LOST/CLAIMED) |
| `total_sales.amount` | number | Raw total sales amount |
| `net_profit.formatted` | string | Formatted net profit (`total_sales - wins_paid`) |
| `net_profit.amount` | number | Raw net profit amount |

---

---

# Financials — Dashboard Endpoints

All dashboard endpoints are under **`/api/v1/financials/dashboard/`** and require **Operator or above** permission.

YTD figures are read from the `YTDSummary` snapshot (updated nightly at 05:00 UTC). Period-based figures (last week / last month / last 3 months) are computed live.

---

## Top-Up Statistics

**`GET /api/v1/financials/dashboard/topup-statistics/`**

Returns total writer top-ups for YTD and recent periods.

**Response `200 OK`**

```json
{
  "ytd":           { "label": "YTD",           "total": "GHS 124,064.43", "total_amount": 124064.43 },
  "last_week":     { "label": "Last Week",     "total": "GHS 4,200.00",   "total_amount": 4200.00 },
  "last_month":    { "label": "Last Month",    "total": "GHS 18,500.00",  "total_amount": 18500.00 },
  "last_3_months": { "label": "Last 3 Months", "total": "GHS 52,000.00",  "total_amount": 52000.00 }
}
```

| Field | Type | Description |
|---|---|---|
| `ytd.total_amount` | number | YTD gross top-ups from the nightly snapshot |
| `last_week.total_amount` | number | Top-ups in the last full calendar week (Mon–Sun) |
| `last_month.total_amount` | number | Top-ups in the last full calendar month |
| `last_3_months.total_amount` | number | Top-ups over the last 3 calendar months |

---

## Winning Statistics

**`GET /api/v1/financials/dashboard/winning-statistics/`**

Returns total win amounts for YTD and recent periods.

**Response `200 OK`**

```json
{
  "ytd":           { "label": "YTD",           "total": "GHS 98,000.00", "total_amount": 98000.00 },
  "last_week":     { "label": "Last Week",     "total": "GHS 3,100.00",  "total_amount": 3100.00 },
  "last_month":    { "label": "Last Month",    "total": "GHS 14,200.00", "total_amount": 14200.00 },
  "last_3_months": { "label": "Last 3 Months", "total": "GHS 40,500.00", "total_amount": 40500.00 }
}
```

| Field | Description |
|---|---|
| `ytd.total_amount` | YTD total win amounts from the nightly snapshot |
| Period fields | Live aggregation of `Win.win_amount` per period |

---

## Best & Worst Performance

**`GET /api/v1/financials/dashboard/best-worst-performance/`**

Returns the best and worst performing calendar months year-to-date, by net profit (`net_topups - total_wins_paid`).

**Response `200 OK`**

```json
{
  "best_month": {
    "month": "Jan '26",
    "performance": "+ GHS 12,500.00",
    "net_profit": 12500.00,
    "topups": 30000.00,
    "wins": 17500.00
  },
  "worst_month": {
    "month": "Mar '26",
    "performance": "- GHS 2,000.00",
    "net_profit": -2000.00,
    "topups": 18000.00,
    "wins": 20000.00
  }
}
```

| Field | Type | Description |
|---|---|---|
| `month` | string | Month label e.g. `"Jan '26"` |
| `performance` | string | Formatted net profit with sign prefix |
| `net_profit` | number | `topups - wins` for the month |
| `topups` | number | Total net top-ups for the month |
| `wins` | number | Total wins paid for the month |

Returns `null` for either field if no data exists yet for the current year.

---

## Retention Rate

**`GET /api/v1/financials/dashboard/retention-rate/`**

Returns the YTD gross sales, net income (wins claimed), retention amount, and retention rate percentage.

**Response `200 OK`**

```json
{
  "gross_sales": 124064.43,
  "net_income": 98000.00,
  "retention_amount": 26064.43,
  "retention_rate": "21.01%"
}
```

| Field | Type | Description |
|---|---|---|
| `gross_sales` | number | YTD `total_wins` from snapshot |
| `net_income` | number | YTD `wins_claimed` from snapshot |
| `retention_amount` | number | `gross_sales - net_income` |
| `retention_rate` | string | YTD retention rate as a percentage string |

---

## Retention Rate Trend

**`GET /api/v1/financials/dashboard/retention-rate-trend/`**

Returns retention rate over time — daily for the last 30 days, or monthly for the last 12 months.

**Query Parameters**

| Parameter | Type | Default | Allowed values | Description |
|---|---|---|---|---|
| `days` | integer | `30` | `30`, `365` | `30` = daily entries, `365` = monthly entries |

**Response — `?days=30`**

```json
{
  "ytd_retention_rate": 21.01,
  "period": { "start_date": "2026-03-30", "end_date": "2026-04-28", "days": 30 },
  "days": [
    { "day": "2026-03-30", "retention_rate": 0 },
    { "day": "2026-04-27", "retention_rate": 74 }
  ]
}
```

**Response — `?days=365`**

```json
{
  "ytd_retention_rate": 21.01,
  "period": { "start_date": "2025-05-01", "end_date": "2026-04-28", "days": 365 },
  "months": [
    { "month": "May '25", "retention_rate": 0 },
    { "month": "Apr '26", "retention_rate": 74 }
  ]
}
```

| Field | Type | Description |
|---|---|---|
| `ytd_retention_rate` | number | YTD retention rate from snapshot (2 d.p.) |
| `days` | array | Daily entries — present only when `?days=30` |
| `months` | array | Monthly entries — present only when `?days=365` |
| `retention_rate` | integer | Rounded retention rate percentage for the entry |

---

## Sales Card

**`GET /api/v1/financials/dashboard/sales/`**

Returns YTD total and net sales figures.

**Response `200 OK`**

```json
{
  "total_sales": "GHS 200,000.00",
  "total_sales_amount": 200000.00,
  "net_sales": "GHS 102,000.00",
  "net_sales_amount": 102000.00,
  "currency": "GHS"
}
```

| Field | Description |
|---|---|
| `total_sales_amount` | YTD gross ticket sales (ACTIVE/WON/LOST/CLAIMED) |
| `net_sales_amount` | `total_sales - wins_claimed` |

---

## Net Top-Ups Card

**`GET /api/v1/financials/dashboard/net-topups/`**

Returns YTD gross and net top-up totals.

**Response `200 OK`**

```json
{
  "gross_topups": "GHS 124,064.43",
  "gross_topups_amount": 124064.43,
  "net_topups": "GHS 124,064.43",
  "net_topups_amount": 124064.43,
  "currency": "GHS"
}
```

---

## Writers at Work Card

**`GET /api/v1/financials/dashboard/writers-at-work/`**

Returns active and total writer headcount for the current year.

**Response `200 OK`**

```json
{
  "active_writers": 42,
  "total_writers": 84
}
```

| Field | Description |
|---|---|
| `active_writers` | Writers with at least one ticket sold this year |
| `total_writers` | All writers registered up to end of current year |

---

## Wins Card

**`GET /api/v1/financials/dashboard/wins/`**

Returns YTD total win amount and number of winning stakes.

**Response `200 OK`**

```json
{
  "total_wins": "GHS 98,000.00",
  "total_wins_amount": 98000.00,
  "winning_stakes": 1204,
  "currency": "GHS"
}
```

| Field | Description |
|---|---|
| `total_wins_amount` | Sum of all win amounts computed this year |
| `winning_stakes` | Count of `Win` records computed this year |

---

## Liquidation Card

**`GET /api/v1/financials/dashboard/liquidation/`**

Returns YTD total liquidation amount and unclaimed win balance.

**Response `200 OK`**

```json
{
  "total_liquidation": "GHS 5,200.00",
  "total_liquidation_amount": 5200.00,
  "unclaimed_coupons": "GHS 1,800.00",
  "unclaimed_coupons_amount": 1800.00,
  "currency": "GHS"
}
```

| Field | Description |
|---|---|
| `total_liquidation_amount` | YTD sum of expired unclaimed wins that were liquidated |
| `unclaimed_coupons_amount` | YTD sum of wins still in PENDING status |

---

## Settlements Card

**`GET /api/v1/financials/dashboard/settlements/`**

Returns YTD total settlements and live claims wallet balance across all writers.

**Response `200 OK`**

```json
{
  "total_settlements": "GHS 30,000.00",
  "total_settlements_amount": 30000.00,
  "claim_wallet_balance": "GHS 12,500.00",
  "claim_wallet_balance_amount": 12500.00,
  "currency": "GHS"
}
```

| Field | Description |
|---|---|
| `total_settlements_amount` | YTD sum of `Settlement.amount_due` from snapshot |
| `claim_wallet_balance_amount` | Live sum of `ClaimsWallet.claims_balance` across all writers |

---

## Draws & Winnings Dashboard Card

**`GET /api/v1/financials/dashboard/draws-and-winnings/`**

Returns three YTD card groups covering sales volume, winnings breakdown, and gross gaming revenue. Most figures are read from the nightly `YTDSummary` snapshot; `unique_players` is a live distinct query.

**Response `200 OK`**

```json
{
  "ytd_sales": {
    "total_sales": "GHS 200,000.00",
    "total_sales_amount": 200000.00,
    "unique_players": 1204,
    "total_tickets": 3500,
    "total_stakes": 7200
  },
  "ytd_winnings": {
    "total_winnings": "GHS 98,000.00",
    "total_winnings_amount": 98000.00,
    "claimed": "GHS 80,000.00",
    "claimed_amount": 80000.00,
    "unclaimed": "GHS 18,000.00",
    "unclaimed_amount": 18000.00
  },
  "ytd_ggr": {
    "gross_gaming_revenue": "GHS 102,000.00",
    "gross_gaming_revenue_amount": 102000.00,
    "retention_rate": "51.00%",
    "retention_rate_value": 51.0,
    "retention_value": "GHS 102,000.00",
    "retention_value_amount": 102000.00
  }
}
```

**`ytd_sales`**

| Field | Type | Description |
|---|---|---|
| `total_sales_amount` | number | YTD gross ticket sales (ACTIVE/WON/LOST/CLAIMED) from snapshot |
| `unique_players` | integer | Distinct non-empty `player_phone` values on valid tickets — live query |
| `total_tickets` | integer | Count of valid tickets from snapshot |
| `total_stakes` | integer | Count of stakes across valid tickets from snapshot |

**`ytd_winnings`**

| Field | Type | Description |
|---|---|---|
| `total_winnings_amount` | number | Sum of all win amounts computed this year |
| `claimed_amount` | number | Wins in CLAIMED status |
| `unclaimed_amount` | number | Wins still in PENDING status |

**`ytd_ggr`**

| Field | Type | Description |
|---|---|---|
| `gross_gaming_revenue_amount` | number | `total_sales - total_wins` |
| `retention_rate` | string | YTD retention rate as a percentage string |
| `retention_rate_value` | number | Raw retention rate (2 d.p.) |
| `retention_value_amount` | number | `total_sales × retention_rate / 100` |

---

---

# Games Endpoints

---

## Draws & Winnings Table

**`GET /api/v1/games/events/draws-and-winnings/`**

**Permission:** Operator or above

Paginated table of all drawn events with their result data — draw numbers, machine numbers, pre/post-draw sales, payout ratio, and total winnings. Ordered newest-first by draw date.

**Query Parameters**

| Parameter | Type | Description |
|---|---|---|
| `game_type` | integer | Filter by game type ID |
| `status` | string | Filter by event status (always `DRAWN` for this endpoint, but accepted) |
| `draw_date` | date | Filter by exact draw date (`YYYY-MM-DD`) |
| `draw_date__gte` | date | Draw date on or after this date |
| `draw_date__lte` | date | Draw date on or before this date |
| `page` | integer | Page number (25 results per page) |

**Response `200 OK`**

```json
{
  "count": 48,
  "next": "https://example.com/api/v1/games/events/draws-and-winnings/?page=2",
  "previous": null,
  "results": [
    {
      "event_id": "a88957b3-0d7c-4014-8c93-18dc50f924bc",
      "event_no": 137,
      "draw_date": "Tue, 28 Apr 2026",
      "event_name": "Tuesday Noon Rush",
      "draw_time": "12:00",
      "pre_draw": "GHS 7,268.56",
      "pre_draw_amount": 7268.56,
      "draw_numbers": [80, 45, 77, 12, 30],
      "machine_numbers": null,
      "post_draw_1": "GHS 0.00",
      "post_draw_1_amount": 0.0,
      "post_draw_2": "GHS 0.00",
      "post_draw_2_amount": 0.0,
      "payout_ratio": "118.21%",
      "payout_ratio_value": 118.21,
      "total_winnings": "GHS 8,592.00",
      "total_winnings_amount": 8592.0
    }
  ]
}
```

| Field | Type | Description |
|---|---|---|
| `event_id` | UUID | Draw event UUID |
| `event_no` | integer | Sequential event number |
| `draw_date` | string | Formatted draw date e.g. `"Tue, 28 Apr 2026"` |
| `event_name` | string | Draw event name |
| `draw_time` | string \| null | Draw time as `"HH:MM"`, or `null` if not set |
| `pre_draw` | string | Formatted pre-draw sales |
| `pre_draw_amount` | number | Raw pre-draw sales amount |
| `draw_numbers` | array | Numbers drawn for the event |
| `machine_numbers` | array \| null | Machine numbers (last set), or `null` |
| `post_draw_1` | string | Formatted Post Draw I sales |
| `post_draw_1_amount` | number | Raw Post Draw I amount |
| `post_draw_2` | string | Formatted Post Draw II sales |
| `post_draw_2_amount` | number | Raw Post Draw II amount |
| `payout_ratio` | string | Payout ratio as a percentage string e.g. `"118.21%"` |
| `payout_ratio_value` | number | Raw payout ratio value |
| `total_winnings` | string | Formatted total winnings paid out |
| `total_winnings_amount` | number | Raw total winnings amount |

---

## Draw Event Tickets

**`GET /api/v1/games/events/{id}/tickets/`**

**Permission:** Operator or above

Returns the event header and a paginated list of tickets for a specific draw event. Used for the Pre Draw Tickets modal.

**Path Parameters**

| Parameter | Type | Description |
|---|---|---|
| `id` | UUID | Draw event UUID |

**Query Parameters**

| Parameter | Type | Description |
|---|---|---|
| `search` | string | Filter by ticket number, writer first/last name, or player phone |
| `page` | integer | Page number (25 results per page) |

**Response `200 OK`**

```json
{
  "event": {
    "event_no": 137,
    "event_name": "Tuesday Noon Rush",
    "draw_numbers": [80, 45, 77, 12, 30],
    "total_wins": "GHS 8,592.00",
    "total_wins_amount": 8592.0,
    "payout_ratio": "118.21%",
    "payout_ratio_value": 118.21,
    "draw_date": "Tue, 28 Apr 2026",
    "draw_time": null
  },
  "tickets": {
    "count": 256,
    "next": "https://example.com/api/v1/games/events/{id}/tickets/?page=2",
    "previous": null,
    "results": [
      {
        "ticket_id": "af23b3bf-6542-40a6-91ae-931d9010ffb6",
        "ticket_no": "202604281329370090447304",
        "stake_count": 1,
        "stake_value": "GHS 3.00",
        "stake_value_amount": 3.0,
        "datetime": "Tue, 28 Apr 2026",
        "staked_by": "Raindolf Borketey",
        "phone_number": "",
        "status": "lost",
        "stakes": [
          {
            "stake_id": "71a30ad8-45ab-4d19-ba51-f526122d24a7",
            "created_at": "2026-04-28 13:29:37",
            "game": {
              "id": "486e1097-cf18-4ed8-97fc-3a7721c848ce",
              "name": "Noonrush",
              "code": "590_NR"
            },
            "event_id": "a88957b3-0d7c-4014-8c93-18dc50f924bc",
            "event": "Tuesday Noon Rush",
            "play_group": "Perm",
            "play": "Perm 2",
            "numbers": "20,30,50",
            "stake_amount": "1.00",
            "original_numbers": "20,30,50",
            "player_phone": "",
            "stake_status": "LOST",
            "writer": {
              "id": "90",
              "name": "Raindolf Borketey",
              "phone": "+233595539729"
            }
          }
        ]
      }
    ]
  }
}
```

**`event` object**

| Field | Type | Description |
|---|---|---|
| `event_no` | integer | Sequential event number |
| `event_name` | string | Draw event name |
| `draw_numbers` | array | Drawn numbers, empty array if not yet drawn |
| `total_wins` | string | Formatted total winnings for the event |
| `total_wins_amount` | number | Raw total winnings amount |
| `payout_ratio` | string | Payout ratio as a percentage string |
| `payout_ratio_value` | number | Raw payout ratio value |
| `draw_date` | string | Formatted draw date e.g. `"Tue, 28 Apr 2026"` |
| `draw_time` | string \| null | Draw time as `"HH:MM:SS"`, or `null` if not set |

**`tickets.results[]` object**

| Field | Type | Description |
|---|---|---|
| `ticket_id` | UUID | Ticket UUID |
| `ticket_no` | string | Unique ticket number |
| `stake_count` | integer | Number of stakes on the ticket |
| `stake_value` | string | Formatted total ticket amount |
| `stake_value_amount` | number | Raw total ticket amount |
| `datetime` | string | Formatted sale date e.g. `"Tue, 28 Apr 2026"` |
| `staked_by` | string | Writer's full name |
| `phone_number` | string | Player phone (empty for POS tickets) |
| `status` | string | Ticket status: `active`, `won`, `lost`, `claimed` |
| `stakes` | array | Individual stakes on the ticket (see below) |

**`stakes[]` object**

| Field | Type | Description |
|---|---|---|
| `stake_id` | UUID | Stake UUID |
| `created_at` | string | Stake creation datetime (`YYYY-MM-DD HH:MM:SS`) |
| `game` | object | `{ id, name, code }` — game type |
| `event_id` | UUID | Draw event UUID |
| `event` | string | Draw event name |
| `play_group` | string | Play group e.g. `"Direct"`, `"Perm"`, `"Banker to All"` |
| `play` | string | Play type name e.g. `"Direct 2 (2 Sure)"`, `"Perm 2"` |
| `numbers` | string | Comma-separated staked numbers |
| `stake_amount` | string | Amount staked on this line |
| `original_numbers` | string | Numbers as originally entered |
| `player_phone` | string | Player phone (empty for POS tickets) |
| `stake_status` | string | `ACTIVE`, `WON`, `LOST`, or `CLAIMED` |
| `writer` | object | `{ id, name, phone }` — writer who sold the ticket |

---

---

# Reports Endpoints

All reports endpoints are under **`/api/v1/financials/reports/`** and require **Operator or above** permission.

Reports are stateless — every `execute` or `download` call runs the query fresh. Filters are passed as a JSON body on `execute`, or as query parameters on `download`.

---

## List Reports

**`GET /api/v1/financials/reports/`**

**Permission:** Operator or above

Returns the full report registry — all available report IDs, names, categories, column schemas, and filter definitions.

**Response `200 OK`**

```json
{
  "status": true,
  "message": "Reports retrieved successfully",
  "data": [
    {
      "reportId": 1,
      "name": "30 Days Sales Tracker",
      "schema": {
        "category": "Operations",
        "columns": [...],
        "filters": [...]
      }
    }
  ]
}
```

---

## Get Report Schema

**`GET /api/v1/financials/reports/{reportId}/`**

**Permission:** Operator or above

Returns the schema for a single report — columns and filter definitions.

**Path Parameters**

| Parameter | Type | Description |
|---|---|---|
| `reportId` | integer | Report ID |

**Response `200 OK`**

```json
{
  "reportId": 1,
  "name": "30 Days Sales Tracker",
  "schema": {
    "category": "Operations",
    "columns": [
      { "key": "Writer ID", "label": "Writer ID", "required": true },
      { "key": "Writer Name", "label": "Writer Name", "required": true }
    ],
    "filters": [
      { "key": "entity_name", "label": "Writer Name", "type": "text", "required": false }
    ]
  }
}
```

**`schema.columns[]`**

| Field | Type | Description |
|---|---|---|
| `key` | string | Data key used in each result row |
| `label` | string | Display label |
| `required` | boolean | Whether the column is considered a primary/required field |

**`schema.filters[]`**

| Field | Type | Description |
|---|---|---|
| `key` | string | Filter key to pass in the request body |
| `label` | string | Display label for the filter input |
| `type` | string | Input type: `"text"`, `"date"` |
| `required` | boolean | Whether the filter must be supplied to run the report |

---

## Execute Report

**`POST /api/v1/financials/reports/{reportId}/execute/`**

**Permission:** Operator or above

Runs the report with the supplied filters and returns all matching rows as JSON, plus a `download_url` for the Excel file.

**Path Parameters**

| Parameter | Type | Description |
|---|---|---|
| `reportId` | integer | Report ID |

**Request Body**

Pass any filters defined in the report's `schema.filters` as a flat JSON object. Omit optional filters to use their defaults (usually no restriction).

```json
{
  "start_date": "2026-01-01",
  "end_date": "2026-04-29"
}
```

**Response `200 OK`**

```json
{
  "status": true,
  "message": "Report executed successfully",
  "report_name": "Daily Sales & Winnings",
  "count": 120,
  "download_url": "https://example.com/api/v1/financials/reports/8/download/",
  "data": [
    { "Date": "2026-01-01", "Sales": "1200.00", "Winning": "800.00" }
  ]
}
```

| Field | Type | Description |
|---|---|---|
| `status` | boolean | Always `true` on success |
| `message` | string | Human-readable status message |
| `report_name` | string | Name of the report |
| `count` | integer | Number of rows returned |
| `download_url` | string | Absolute URL to download the same data as `.xlsx` |
| `data` | array | Report rows — keys match `schema.columns[].key` |

**Error Responses**

| Status | Description |
|---|---|
| `400` | `reportId` is not a valid integer |
| `404` | Report ID does not exist or executor not implemented |

---

## Download Report (Excel)

**`GET /api/v1/financials/reports/{reportId}/download/`**

**Permission:** Operator or above

Re-runs the report with optional query-string filters and streams the result as an Excel (`.xlsx`) file.

**Path Parameters**

| Parameter | Type | Description |
|---|---|---|
| `reportId` | integer | Report ID |

**Query Parameters**

Pass any filters defined in `schema.filters` as query-string parameters. e.g. `?start_date=2026-01-01&end_date=2026-04-29`

**Response**

Binary `.xlsx` file download.

```
Content-Type: application/vnd.openxmlformats-officedocument.spreadsheetml.sheet
Content-Disposition: attachment; filename="Daily_Sales_&_Winnings.xlsx"
```

The workbook contains a single sheet named after the report. The header row is styled with a dark-blue background and bold white text. Column widths are auto-fitted (capped at 40 characters).

---

---

# Report Catalogue

Full column and filter reference for all 21 available reports.

---

## Report 1: 30 Days Sales Tracker

**Category:** Operations

A per-writer breakdown of sales over the last 30 days, including lifetime stats, device info, and a daily column for each of the past 30 days (Day-1 = today).

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `entity_name` | Writer Name | text | No |

**Columns**

`Writer ID`, `Writer Name`, `Writer Phone`, `Supervisor`, `Device`, `Serial`, `State`, `Days on Platform`, `Days-to-Start`, `Operation Days`, `Lifetime Sales`, `Avg Lifetime Sales`, `30 Days Total`, `30 Day Average`, `Date Onboarded`, `First Transaction`, `Last Transaction`, `Day-1` … `Day-30`

---

## Report 2: Bank Transfer - Batch Details

**Category:** Finance

Detailed breakdown of top-up transactions within a specific batch.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `batch_number` | Batch Number | text | No |
| `reference` | Reference | text | No |

**Columns**

`Writer Name`, `Writer Phone`, `Supervisor Name`, `Supervisor Phone`, `Phone Number`, `Network`, `Client Reference`, `Amount`, `Description`, `Datetime`, `Updated At`, `Batch Number`, `UUID`

---

## Report 3: Bank Transfers

**Category:** Finance

All bank transfer records, optionally filtered by date range and account number.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `from` | From Date | date | No |
| `to` | To Date | date | No |
| `account` | Account Number | text | No |

**Columns**

`Datetime`, `Account Number`, `Reference`, `Amount`, `Success`, `Reason`, `Batch Number`, `Batch Date`

---

## Report 5: Commission Payments

**Category:** Finance

Commission amounts per writer for a given reference date.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `reference_date` | Reference Date | date | **Yes** |

**Columns**

`Writer`, `Phone Number`, `Supervisor`, `Supervisor Phone Number`, `Sales`, `Commission`

---

## Report 6: Ticket Query

**Category:** Finance

All stake lines on a specific ticket.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `ticket` | Ticket | text | **Yes** |

**Columns**

`Datetime`, `Ticket No.`, `Play`, `Original Stake`, `Stake`, `Amount`

---

## Report 7: Daily Sales

**Category:** Finance

All tickets sold on a given date (defaults to today).

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `date` | Date | date | No |

**Columns**

`Ticket Number`, `Game`, `Writer Name`, `Writer Number`, `Supervisor Name`, `Datetime of Ticket`, `Ticket Amount`

---

## Report 8: Daily Sales & Winnings

**Category:** Finance

Day-by-day summary of sales, winnings, and retention over a date range.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `start_date` | Start Date | date | **Yes** |
| `end_date` | End Date | date | **Yes** |

**Columns**

`Date`, `Total Writers`, `Sales`, `Gross Income`, `Winning`, `Net Income`, `Retention Rate`

---

## Report 9: Finance - Payout

**Category:** Finance

Writer withdrawal (payout) transactions, optionally filtered by date range.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `start_date` | Start Date | date | No |
| `end_date` | End Date | date | No |

**Columns**

`Writer ID`, `Writer Name`, `Writer Phone #`, `Transaction Date`, `Withdrawal`, `Bank Reference`

---

## Report 14: Revenue Per Play

**Category:** Finance

Total ticket count and amount grouped by game and play variety over a date range.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `start_date` | Start Date | date | **Yes** |
| `end_date` | End Date | date | **Yes** |

**Columns**

`Date`, `Game`, `Play Variety`, `Total Tickets`, `Total Amount`

---

## Report 17: Ticket Status

**Category:** General

Full stake-level status history for a single ticket number.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `ticket` | Ticket | text | **Yes** |

**Columns**

`Ticket`, `Event ID`, `Event Name`, `Event Display`, `Occurrence Date`, `Writer Phone`, `Player Phone`, `Stake No.`, `Stake Amount`, `Payout`, `Status`, `Reason`, `Created At`, `Payout Time`

---

## Report 18: Active Writers

**Category:** General

All currently active writers — ID, name, phone, terminal number, location, and join date.

**Filters**

None

**Columns**

`ID`, `Name`, `Phone Number`, `Terminal Number`, `Location`, `Joined Date`

---

## Report 19: Terminal History

**Category:** General

Device/terminal usage history, optionally filtered by terminal number or MAC address.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `terminal_number` | Terminal Number | text | No |
| `mac_address` | MAC Address | text | No |

**Columns**

`Terminal`, `MAC Address`, `Writer Name`, `Phone Number`, `First Use`

---

## Report 20: Winning Stakes Report

**Category:** General

Winning stake details optionally filtered by date range or ticket number.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `start_date` | Start Date | date | No |
| `end_date` | End Date | date | No |
| `ticket` | Ticket | text | No |

**Columns**

`Writer`, `Supervisor`, `Ticket`, `Purchase Amount`, `Stake`, `Draw`, `Event`, `Play`, `Variety`, `Payout`, `Date of Stake`, `Date of Winning`

---

## Report 21: All Stakes Report

**Category:** Operations

Every stake across all tickets within a date range. Can be large — use the download endpoint for bulk exports.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `start_date` | Start Date | date | **Yes** |
| `end_date` | End Date | date | **Yes** |

**Columns**

`Ticket`, `Ticket Number`, `Writer Name`, `Writer Phone`, `Supervisor Name`, `Supervisor Phone`, `Game`, `Event`, `Round`, `Variety`, `Status`, `Numbers`, `Amount`, `Payout`, `Date`

---

## Report 23: Topup - Claims as Credit

**Category:** Finance

Top-up transactions funded from a writer's claims wallet, optionally filtered by date range.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `from` | Date From | date | No |
| `to` | Date To | date | No |

**Columns**

`Writer Name`, `Writer Phone`, `Supervisor Name`, `Supervisor Phone`, `Phone Number`, `Network`, `Client Reference`, `Amount`, `Description`, `Datetime`, `Updated At`, `Batch Number`, `UUID`

---

## Report 24: Topup - Supervisor Transfers

**Category:** Finance

Top-up transactions processed via supervisor transfers, optionally filtered by date range.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `from` | Date From | date | No |
| `to` | Date To | date | No |

**Columns**

`Writer Name`, `Writer Phone`, `Supervisor Name`, `Supervisor Phone`, `Phone Number`, `Network`, `Client Reference`, `Amount`, `Description`, `Datetime`, `Updated At`, `Batch Number`, `UUID`

---

## Report 25: Topup - Mobile Money

**Category:** Finance

Top-up transactions processed via mobile money, optionally filtered by date range.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `from` | Date From | date | No |
| `to` | Date To | date | No |

**Columns**

`Writer Name`, `Writer Phone`, `Supervisor Name`, `Supervisor Phone`, `Phone Number`, `Network`, `Client Reference`, `Amount`, `Net Value`, `Description`, `Batch Number`, `UUID`, `Datetime`, `Updated At`

---

## Report 26: Writers - Active Writers

**Category:** General

Active writer count and writer details grouped by period over a date range.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `start_date` | Start Date | date | **Yes** |
| `end_date` | End Date | date | **Yes** |

**Columns**

`Period`, `Active Writers`, `Writer Name`, `Phone Number`

---

## Report 27: Export Top-Ups

**Category:** Finance

All top-up transactions within a date range — suitable for bulk export.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `from_date` | From Date | date | **Yes** |
| `to_date` | To Date | date | **Yes** |

**Columns**

`Date`, `Writer ID`, `Writer Name`, `Writer Phone`, `Supervisor`, `Amount (GHS)`, `Airtime Credited`, `Method`, `Reference`, `Created By`

---

## Report 28: Export Sales

**Category:** Finance

All ticket sales within a date range — suitable for bulk export.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `from_date` | From Date | date | **Yes** |
| `to_date` | To Date | date | **Yes** |

**Columns**

`Date`, `Ticket No`, `Writer ID`, `Writer Name`, `Writer Phone`, `Supervisor`, `Game`, `Draw Event`, `Stakes`, `Amount (GHS)`, `Status`, `Channel`, `Player Phone`

---

## Report 29: Export Wins

**Category:** Finance

All winning tickets within a date range — suitable for bulk export.

**Filters**

| Key | Label | Type | Required |
|---|---|---|---|
| `from_date` | From Date | date | **Yes** |
| `to_date` | To Date | date | **Yes** |

**Columns**

`Date Won`, `Ticket No`, `Writer ID`, `Writer Name`, `Writer Phone`, `Supervisor`, `Game`, `Draw Event`, `Ticket Amount (GHS)`, `Win Amount (GHS)`, `Status`, `Claimed At`, `Expires At`

---

---

# Supervisor Endpoints

All supervisor endpoints are under **`/api/v1/supervisors/`** and require `Authorization: Bearer <token>`.

---

## Standard CRUD

| Method | URL | Permission | Description |
|---|---|---|---|
| `GET` | `/api/v1/supervisors/` | Supervisor or above | List all supervisors. Supervisor role sees own record only. |
| `GET` | `/api/v1/supervisors/{pk}/` | Supervisor or above | Retrieve a single supervisor. |
| `POST` | `/api/v1/supervisors/` | Operator or above | Create supervisor (requires existing user UUID via `owner` field). |
| `PATCH` | `/api/v1/supervisors/{pk}/` | Supervisor or above | Partial update via write serializer (`address`, `owner`, `is_active`). |
| `DELETE` | `/api/v1/supervisors/{pk}/` | Operator or above | Soft-delete a supervisor. |

---

## Register Supervisor

**`POST /api/v1/supervisors/register/`**

**Permission:** Supervisor or above

Creates a new `User` (role=`supervisor`) and a linked `Supervisor` record in one atomic transaction. Auto-generates the supervisor code (e.g. `SUP-0002`).

**Request Body**

```json
{
  "email": "john.doe@example.com",
  "first_name": "John",
  "last_name": "Doe",
  "phone": "+233501234567",
  "password": "securepass123",
  "address": "45 Ring Road Central, Accra"
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `email` | string | Yes | Must be unique across all users |
| `first_name` | string | Yes | — |
| `last_name` | string | Yes | — |
| `phone` | string | Yes | E.164 format, must be unique |
| `password` | string | Yes | Minimum 8 characters |
| `address` | string | No | Operating address |

**Response `201 Created`**

```json
{
  "id": "3efd7ffb-5aff-4bc9-adcd-f421c58b8065",
  "code": "SUP-0002",
  "name": "John Doe",
  "phone": "+233501234567",
  "address": "45 Ring Road Central, Accra",
  "owner": { "id": "...", "email": "john.doe@example.com", "full_name": "John Doe" },
  "is_active": true,
  "created_at": "2026-04-29T10:00:00Z",
  "updated_at": "2026-04-29T10:00:00Z"
}
```

---

## Edit Supervisor

**`PATCH /api/v1/supervisors/{pk}/edit/`**

**Permission:** Supervisor or above

Updates a supervisor's personal details and/or active status. All fields are optional — send only the fields to change.

**Path Parameters**

| Parameter | Type | Description |
|---|---|---|
| `pk` | UUID | Supervisor UUID |

**Request Body**

```json
{
  "first_name": "John",
  "last_name": "Smith",
  "phone": "+233501234568",
  "address": "12 Liberation Road, Accra",
  "is_active": true
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `first_name` | string | No | Updates the owner user's first name |
| `last_name` | string | No | Updates the owner user's last name |
| `phone` | string | No | E.164 format; validated for uniqueness (skipped if unchanged) |
| `address` | string | No | Supervisor's operating address |
| `is_active` | boolean | No | Activate or deactivate the supervisor |

**Response `200 OK`**

Full `SupervisorSerializer` output (same shape as Register response).

---

## List Supervisors with Owner Details

**`GET /api/v1/supervisors/owners/`**

**Permission:** Supervisor or above

Returns all supervisors ordered by name, each with their full owner object.

**Response `200 OK`**

```json
[
  {
    "id": "3efd7ffb-5aff-4bc9-adcd-f421c58b8065",
    "code": "SUP-0001",
    "name": "Super Visor",
    "phone": "+233200000003",
    "address": "123 Test Street, Accra",
    "owner": { "id": "...", "email": "sup@example.com", "full_name": "Super Visor" },
    "is_active": true,
    "created_at": "2026-01-01T00:00:00Z",
    "updated_at": "2026-04-29T10:00:00Z"
  }
]
```

---

## Detail Cards

**`GET /api/v1/supervisors/detail-cards/`**

**Permission:** Supervisor or above

Returns all supervisors with their latest operational snapshot and current-month financial aggregates. Supervisor role sees their own card only; operator and above sees all.

**Response `200 OK`**

```json
[
  {
    "id": "3efd7ffb-5aff-4bc9-adcd-f421c58b8065",
    "code": "SUP-0001",
    "name": "Super Visor",
    "phone": "+233200000003",
    "address": "123 Test Street, Accra",
    "photo_url": null,
    "is_active": true,
    "operational": {
      "snapshot_date": "2026-04-28",
      "active": 3,
      "passive": 0,
      "inactive": 0,
      "recover": 0,
      "no_use": 0,
      "writers_total": 3,
      "pos_issued": 0,
      "pos_trading": 2,
      "pos_recovery": 0
    },
    "financial": {
      "wallet_balance": 500.00,
      "monthly_topups": 1200.00,
      "monthly_sales": 3400.00,
      "monthly_commissions": 0.0
    }
  }
]
```

**`operational` object**

| Field | Type | Description |
|---|---|---|
| `snapshot_date` | string \| null | Date of the latest daily snapshot, or `null` if none yet |
| `active` / `passive` / `inactive` / `recover` / `no_use` | integer | Live writer counts by status |
| `writers_total` | integer | Total writers under this supervisor |
| `pos_issued` / `pos_trading` / `pos_recovery` | integer | POS device counts from the latest snapshot |

**`financial` object**

| Field | Type | Description |
|---|---|---|
| `wallet_balance` | number | Sum of current `airtime_balance` across all writers' airtime wallets |
| `monthly_topups` | number | Total top-up amount for the current calendar month |
| `monthly_sales` | number | Total ticket sales for the current calendar month |
| `monthly_commissions` | number | Always `0.0` (supervisors have no commission model) |

---

## Supervisor Snapshot

**`GET /api/v1/supervisors/{pk}/snapshot/`**

**Permission:** Supervisor or above

Returns the most recent daily operational snapshot for a supervisor.

**Response `200 OK`**

```json
{
  "id": "...",
  "supervisor": "3efd7ffb-5aff-4bc9-adcd-f421c58b8065",
  "snapshot_date": "2026-04-28",
  "active": 3,
  "passive": 0,
  "inactive": 0,
  "recover": 0,
  "no_use": 0,
  "writers_total": 3,
  "total_writers": 3,
  "pos_issued": 0,
  "pos_trading": 2,
  "pos_recovery": 0,
  "created_at": "2026-04-28T23:00:00Z"
}
```

Returns `404` if no snapshot has been computed yet.

---

## Summary

**`GET /api/v1/supervisors/{pk}/summary/`**

**Permission:** Supervisor or above

Returns supervisor info and YTD performance cards for a single supervisor, including contribution ratios against global totals.

**Response `200 OK`**

```json
{
  "supervisor_info": {
    "name": "Super Visor",
    "address": "123 Test Street, Accra",
    "phone": "+233200000003",
    "pos_issued": 0,
    "pos_trading": 2,
    "writers_total": 3
  },
  "summary": {
    "ytd_sales": "3400.00",
    "ytd_topups": "1200.00",
    "ytd_winnings": "200.00",
    "writers_count": 3,
    "ytd_sales_ratio": 12,
    "ytd_topups_ratio": 8,
    "ytd_winnings_ratio": 5,
    "writers_ratio": 4
  }
}
```

**`supervisor_info` fields**

| Field | Type | Description |
|---|---|---|
| `name` | string | Supervisor's full name |
| `address` | string | Operating address |
| `phone` | string | Phone number |
| `pos_issued` | integer | POS devices issued (from latest snapshot) |
| `pos_trading` | integer | POS devices trading (from latest snapshot) |
| `writers_total` | integer | Total writers from latest snapshot |

**`summary` fields**

| Field | Type | Description |
|---|---|---|
| `ytd_sales` | decimal string | YTD ticket sales across this supervisor's writers |
| `ytd_topups` | decimal string | YTD top-ups across this supervisor's writers |
| `ytd_winnings` | decimal string | YTD win amounts on this supervisor's writers' tickets |
| `writers_count` | integer | Current live writer count |
| `ytd_sales_ratio` | integer | This supervisor's sales as % of global YTD sales |
| `ytd_topups_ratio` | integer | This supervisor's top-ups as % of global YTD top-ups |
| `ytd_winnings_ratio` | integer | This supervisor's winnings as % of global YTD winnings |
| `writers_ratio` | integer | This supervisor's writers as % of all writers system-wide |

---

## Writers Overview

**`GET /api/v1/supervisors/{pk}/writers-overview/`**

**Permission:** Supervisor or above

Paginated, filterable writer table for a single supervisor. Each writer is annotated with YTD sales, YTD top-ups, and days-on-task (distinct days with a valid ticket).

**Query Parameters**

| Parameter | Type | Description |
|---|---|---|
| `status` | string | Filter by writer status: `active`, `passive`, `inactive`, `recover`, `no_use` |
| `search` | string | Filter by writer first or last name (case-insensitive) |
| `page` | integer | Page number |

**Response `200 OK`**

```json
{
  "count": 3,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": "...",
      "writer_id": 32,
      "name": "Frank Mawuli",
      "phone": "+233244979958",
      "status": "active",
      "location_address": "Madina Market",
      "ytd_sales": "14826.43",
      "ytd_topups": "10291.00",
      "dot": 28,
      "created_at": "2026-01-15T08:00:00Z"
    }
  ]
}
```

| Field | Type | Description |
|---|---|---|
| `id` | UUID | Writer UUID |
| `writer_id` | integer | Human-readable writer ID |
| `name` | string | Writer's full name |
| `phone` | string | Writer's phone number |
| `status` | string | Current writer status |
| `location_address` | string | Writer's operating location |
| `ytd_sales` | decimal string | YTD ticket sales (ACTIVE/WON/LOST/CLAIMED) |
| `ytd_topups` | decimal string | YTD total top-up amount |
| `dot` | integer | Days-on-task — distinct days with at least one valid ticket this year |
| `created_at` | datetime | Date the writer was registered |

---

## Transactions

**`GET /api/v1/supervisors/{pk}/transactions/`**

**Permission:** Supervisor or above

Paginated, unified transaction ledger for a single supervisor — merges writer top-ups (money in) and successful withdrawals (money out) across all writers, sorted newest-first.

**Query Parameters**

| Parameter | Type | Description |
|---|---|---|
| `type` | string | Filter by type: `topup` or `withdrawal` |
| `date_from` | date | Filter from this date (`YYYY-MM-DD`) inclusive |
| `date_to` | date | Filter to this date (`YYYY-MM-DD`) inclusive |
| `search` | string | Filter by writer name or phone (case-insensitive) |
| `page` | integer | Page number |

**Response `200 OK`**

```json
{
  "count": 42,
  "next": "...?page=2",
  "previous": null,
  "results": [
    {
      "created_at": "2026-04-28T14:30:00Z",
      "type": "topup",
      "writer_name": "Frank Mawuli",
      "writer_phone": "+233244979958",
      "reference": "TXN-00123",
      "amount": "200.00",
      "is_credit": true
    },
    {
      "created_at": "2026-04-27T11:00:00Z",
      "type": "withdrawal",
      "writer_name": "Occc Appiah",
      "writer_phone": "+233501234567",
      "reference": "WD_abc123",
      "amount": "150.00",
      "is_credit": false
    }
  ]
}
```

| Field | Type | Description |
|---|---|---|
| `created_at` | datetime | Transaction timestamp |
| `type` | string | `topup` — airtime top-up; `withdrawal` — claims wallet withdrawal |
| `writer_name` | string | Writer's full name |
| `writer_phone` | string \| null | Writer's phone number |
| `reference` | string \| null | Transaction reference, or `null` if none |
| `amount` | decimal string | Transaction amount in GHS |
| `is_credit` | boolean | `true` for top-ups (money in), `false` for withdrawals (money out) |

---

## Dashboard

**`GET /api/v1/supervisors/dashboard/`**

**Permission:** Supervisor or above

Returns current-month aggregates for the authenticated supervisor's writers.

**Response `200 OK`**

```json
{
  "monthly_topups": "1200.00",
  "monthly_sales": "3400.00",
  "active_writers": 3,
  "total_writers": 3,
  "month_start": "2026-04-01",
  "month_end": "2026-04-30"
}
```

---

## Statistics

**`GET /api/v1/supervisors/statistics/`**

**Permission:** Supervisor or above

Returns per-writer sales, stake count, and top-up totals for the authenticated supervisor's writers, ordered by total sales descending.

**Response `200 OK`**

```json
{
  "totalwriterFloat": "GHS 14,826.43",
  "totalwriters": "3",
  "writers": [
    {
      "sales": "GHS 14,826.43",
      "total_stakes": "20",
      "topup": "GHS 10,291.00",
      "writer": {
        "id": "32",
        "name": "Frank Mawuli",
        "profileImage": null,
        "online": false,
        "lastSeen": "2026-04-27 22:24:53",
        "contact": { "email": "frank@writer.ams1one.com", "phone": "+233244979958" }
      }
    }
  ]
}
```

---

## Writers (Authenticated Supervisor)

**`GET /api/v1/supervisors/writers/`**

**Permission:** Supervisor or above

Paginated, filterable list of writers belonging to the authenticated supervisor.

**Query Parameters:** Supports all `WriterFilter` fields (status, search, etc.)

---

## Create Writer

**`POST /api/v1/supervisors/create-writer/`**

**Permission:** Supervisor or above

Creates a new writer under the authenticated supervisor. Also creates the writer's `AirtimeWallet` and `ClaimsWallet`.

**Request Body**

```json
{
  "phone": "+233501234567",
  "first_name": "Kwame",
  "last_name": "Mensah",
  "password": "securepass123",
  "date_of_birth": "1990-05-15",
  "location_address": "Tema Station, Accra"
}
```

| Field | Type | Required | Description |
|---|---|---|---|
| `phone` | string | Yes | E.164 format, must be unique |
| `first_name` | string | Yes | — |
| `last_name` | string | Yes | — |
| `password` | string | Yes | Minimum 8 characters |
| `date_of_birth` | date | Yes | `YYYY-MM-DD`, used for KYC |
| `location_address` | string | No | Writer's operating location |

**Response `201 Created`** — Full `WriterSerializer` output.

---

## Writer Detail

**`GET /api/v1/supervisors/writer-detail/?writer={uuid}`**

**Permission:** Supervisor or above

Returns the full profile for a single writer under the authenticated supervisor, including active POS device and wallet balances.

**Query Parameters**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `writer` | UUID | Yes | Writer UUID |

**Response `200 OK`**

```json
{
  "id": "...",
  "writer_id": 32,
  "full_name": "Frank Mawuli",
  "status": "active",
  "phone": "+233244979958",
  "airtime_balance": "500.00",
  "claims_balance": "120.00",
  "days_on_platform": 104,
  "pos_device": {
    "serial_number": "POS-001",
    "device_type": "pos",
    "status": "trading"
  },
  "date_of_birth": "1990-05-15",
  "location_address": "Madina Market",
  "created_at": "2026-01-15T08:00:00Z"
}
```

---

## Writer Top-Ups

**`GET /api/v1/supervisors/writer-topups/?writer={uuid}`**

**Permission:** Supervisor or above

Paginated top-up history for a specific writer under the authenticated supervisor.

**Query Parameters**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `writer` | UUID | Yes | Writer UUID |

Supports all `WriterTopUpFilter` fields for further filtering.

---

## Writer Sales

**`GET /api/v1/supervisors/writer-sales/?writer={uuid}`**

**Permission:** Supervisor or above

Paginated ticket sales history for a specific writer under the authenticated supervisor.

**Query Parameters**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `writer` | UUID | Yes | Writer UUID |

Supports all `WriterSalesFilter` fields for further filtering.

---

## Writer Sales Summary

**`GET /api/v1/supervisors/writer-sales-summary/?writer={uuid}&date_from=YYYY-MM-DD&date_to=YYYY-MM-DD`**

**Permission:** Supervisor or above

Date-ranged sales summary for a specific writer.

**Query Parameters**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `writer` | UUID | Yes | Writer UUID |
| `date_from` | date | Yes | Start date (`YYYY-MM-DD`) |
| `date_to` | date | Yes | End date (`YYYY-MM-DD`) |

---

## Today's Top-Up

**`GET /api/v1/supervisors/today-topup/`**

**Permission:** Supervisor or above

Returns today's top-up totals for the authenticated supervisor's writers.

**Response `200 OK`**

```json
{
  "date": "2026-04-29",
  "total_topup": "GHS 400.00",
  "total_topup_amount": 400.00,
  "topup_count": 4,
  "currency": "GHS"
}
```

---

## Transaction Summary

**`GET /api/v1/supervisors/transaction-summary/?date_from=YYYY-MM-DD&date_to=YYYY-MM-DD`**

**Permission:** Supervisor or above

Date-ranged top-up and sales totals for the authenticated supervisor's writers.

**Query Parameters**

| Parameter | Type | Required | Description |
|---|---|---|---|
| `date_from` | date | Yes | Start date (`YYYY-MM-DD`) |
| `date_to` | date | Yes | End date (`YYYY-MM-DD`) |

**Response `200 OK`**

```json
{
  "date_from": "2026-04-01",
  "date_to": "2026-04-29",
  "topups": "1200.00",
  "sales": "3400.00"
}
```
