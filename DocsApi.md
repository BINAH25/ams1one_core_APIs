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
