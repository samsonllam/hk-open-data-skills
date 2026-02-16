---
name: hk-hospital
description: Query real-time A&E (Accident & Emergency) waiting times at Hong Kong public hospitals from the Hospital Authority. Use when the user asks about emergency room wait times, which hospital has the shortest queue, A&E waiting times, or any question about going to the emergency department in Hong Kong. Covers all 18 public hospitals with A&E services.
---

# HK Hospital A&E Wait Times

Query real-time emergency department waiting times from the Hospital Authority (醫管局). No API key needed.

## API Endpoints

### English
```bash
curl -s "https://www.ha.org.hk/opendata/aed/aedwtdata2-en.json"
```

### 繁體中文
```bash
curl -s "https://www.ha.org.hk/opendata/aed/aedwtdata2-tc.json"
```

## Response Format

```json
{
  "waitTime": [
    {
      "hospName": "Queen Mary Hospital",
      "t1wt": "0 minute",
      "manageT1case": "Y",
      "t2wt": "less than 15 minutes",
      "manageT2case": "Y",
      "t3p50": "25 minutes",
      "t3p95": "68 minutes",
      "manageT3case": "Y"
    }
  ],
  "updateTime": "16/2/2026 1:00PM"
}
```

**Triage Categories**:
- **T1 (危殆 Critical)**: Life-threatening → `t1wt` (wait time)
- **T2 (危急 Emergency)**: Could become critical → `t2wt`
- **T3 (緊急 Urgent)**: Stable but needs prompt attention → `t3p50` (median wait), `t3p95` (95th percentile)
- **T4/T5 (次緊急/非緊急)**: Not separately reported

**Fields**:
- `hospName` — Hospital name
- `t1wt` — Category 1 (Critical) wait time
- `t2wt` — Category 2 (Emergency) wait time
- `t3p50` — Category 3 (Urgent) median wait time (50th percentile)
- `t3p95` — Category 3 (Urgent) 95th percentile wait time
- `manageT1case` / `manageT2case` / `manageT3case` — Whether hospital handles this category (Y/N)

## 18 Public Hospitals with A&E

| Hospital | Region |
|----------|--------|
| Alice Ho Miu Ling Nethersole Hospital 雅麗氏何妙齡那打素醫院 | NT |
| Caritas Medical Centre 明愛醫院 | KLN |
| Kwong Wah Hospital 廣華醫院 | KLN |
| North District Hospital 北區醫院 | NT |
| North Lantau Hospital 北大嶼山醫院 | Islands |
| Pamela Youde Nethersole Eastern Hospital 東區尤德夫人那打素醫院 | HKI |
| Prince of Wales Hospital 威爾斯親王醫院 | NT |
| Princess Margaret Hospital 瑪嘉烈醫院 | NT |
| Queen Elizabeth Hospital 伊利沙伯醫院 | KLN |
| Queen Mary Hospital 瑪麗醫院 | HKI |
| Ruttonjee Hospital 律敦治醫院 | HKI |
| Tseung Kwan O Hospital 將軍澳醫院 | NT |
| Tuen Mun Hospital 屯門醫院 | NT |
| Tin Shui Wai Hospital 天水圍醫院 | NT |
| United Christian Hospital 基督教聯合醫院 | KLN |
| Yan Chai Hospital 仁濟醫院 | NT |
| Pok Oi Hospital 博愛醫院 | NT |
| Tang Shiu Kin Hospital 鄧肇堅醫院 | HKI |

## Presentation Guidelines

- Sort by shortest wait time (t3p50) to help users pick the fastest hospital
- Highlight if t3p95 > 2 hours (long wait warning)
- Show both median (t3p50) and worst case (t3p95)
- Use 🟢 (<30min), 🟡 (30-60min), 🔴 (>60min) for T3 median wait
- Always show the update time
- If user is in a specific area, suggest the nearest hospital (combine with hk-geodata)
- **Important**: Remind users that T1/T2 cases are seen first regardless of wait times
- For Chinese output, use 繁體中文 endpoint
