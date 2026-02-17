---
name: hk-holidays
description: Query Hong Kong public holidays and calendar events from the 1823 government hotline. Use when the user asks about public holidays in Hong Kong, when the next holiday is, whether a specific date is a holiday, long weekends, or festival dates. Covers all gazetted public holidays in English and Chinese.
---

# HK Holidays Skill

Query Hong Kong public holidays. No API key needed.

## API Endpoints

### English
```bash
curl -s "https://www.1823.gov.hk/common/ical/en.json"
```

### 繁體中文
```bash
curl -s "https://www.1823.gov.hk/common/ical/tc.json"
```

## Response Format

```json
{
  "vcalendar": [{
    "vevent": [
      {
        "dtstart": ["20260101"],
        "dtend": ["20260102"],
        "summary": "The first day of January"
      },
      {
        "dtstart": ["20260217"],
        "dtend": ["20260218"],
        "summary": "Lunar New Year's Day"
      }
    ]
  }]
}
```

**Fields per event**:
- `dtstart` — Start date (YYYYMMDD format)
- `dtend` — End date (day after, exclusive)
- `summary` — Holiday name

## Parsing

```bash
curl -s "https://www.1823.gov.hk/common/ical/en.json" | python3 -c "
import sys,json
from datetime import datetime, date
d = json.load(sys.stdin)
events = d['vcalendar'][0]['vevent']
today = date.today().strftime('%Y%m%d')
upcoming = [e for e in events if e['dtstart'][0] >= today]
upcoming.sort(key=lambda e: e['dtstart'][0])
print(f'Upcoming holidays:')
for e in upcoming[:5]:
    ds = e['dtstart'][0]
    dt = datetime.strptime(ds, '%Y%m%d')
    delta = (dt.date() - date.today()).days
    print(f'  📅 {dt.strftime(\"%b %d\")} ({delta}d): {e[\"summary\"]}')
"
```

## Tips

- Data covers approximately 3 years of holidays
- Use to check if a date is a working day
- Combine with weather data for holiday planning
- Show days remaining until next holiday
- For Chinese festival names, use the TC endpoint
