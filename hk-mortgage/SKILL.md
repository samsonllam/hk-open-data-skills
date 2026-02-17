---
name: hk-mortgage
description: Query Hong Kong residential mortgage statistics from the Hong Kong Monetary Authority (HKMA). Use when the user asks about mortgage rates, property loan data, housing loan statistics, mortgage approval numbers, or the state of the Hong Kong property lending market.
---

# HK Mortgage Skill

Query Hong Kong residential mortgage survey data from HKMA. No API key needed.

## API Endpoint

```bash
curl -s "https://api.hkma.gov.hk/public/market-data-and-statistics/monthly-statistical-bulletin/banking/residential-mortgage-survey"
```

### With Filters
```bash
# Latest record only
curl -s "https://api.hkma.gov.hk/public/market-data-and-statistics/monthly-statistical-bulletin/banking/residential-mortgage-survey?sortby=end_of_month&sortorder=desc&pagesize=1"

# Date range
curl -s "...?choose=by_date&from=2025-01&to=2025-12&sortby=end_of_month&sortorder=desc"
```

## Response Format

```json
{
  "header": { "success": true },
  "result": {
    "records": [{
      "end_of_month": "2025-12",
      "outstanding_loans_amt": 1917499,
      "outstanding_loans_amt_mom": 0.002,
      "outstanding_loans_amt_yoy": 0.025,
      "new_loans_drawn_down_amt": 26234,
      "new_loans_approved_amt": 38766,
      "new_loans_approved_no": 9876,
      "avg_loan_size": 3.93
    }]
  }
}
```

**Key Fields**:
- `outstanding_loans_amt` — Total outstanding mortgage loans (HKD million)
- `outstanding_loans_amt_mom` — Month-on-month change (decimal, e.g., 0.002 = 0.2%)
- `outstanding_loans_amt_yoy` — Year-on-year change
- `new_loans_drawn_down_amt` — New loans drawn down (HKD million)
- `new_loans_approved_amt` — New loans approved amount (HKD million)
- `new_loans_approved_no` — Number of new loan applications approved

## Query Parameters

| Param | Description | Example |
|-------|-------------|---------|
| `sortby` | Sort field | `end_of_month` |
| `sortorder` | Direction | `desc` |
| `pagesize` | Results per page | `10` |
| `choose` | Filter mode | `by_date` |
| `from` / `to` | Date range | `2025-01` / `2025-12` |

## Tips

- Amounts are in HKD millions — divide by 1000 for billions
- MoM/YoY changes are decimals (multiply by 100 for percentage)
- Data updates monthly, usually with 1-2 month lag
- Useful for property market analysis and trend tracking
- Use 📊 and trend arrows (📈📉) when presenting
