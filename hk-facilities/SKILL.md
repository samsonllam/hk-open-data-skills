---
name: hk-facilities
description: Search Hong Kong public sports and leisure facilities from the Leisure and Cultural Services Department (LCSD). Use when the user asks about sports centres, gyms, swimming pools, parks, courts, or any public recreational facility in Hong Kong. Covers 116+ venues across all 18 districts.
---

# HK Facilities Skill

Search LCSD public sports and leisure facilities. No API key needed.

## API Endpoint

### Sports Centres & Facilities
```bash
curl -s "https://www.lcsd.gov.hk/datagovhk/facility/facility-sc.json"
```

English version:
```bash
curl -s "https://www.lcsd.gov.hk/datagovhk/facility/facility-en.json"
```

## Response Format

```json
[
  {
    "District_en": "Kowloon City",
    "District_cn": "九龍城區",
    "Name_en": "Ho Man Tin Sports Centre",
    "Name_cn": "何文田體育館",
    "Address_en": "1 Chung Yee Street, Ho Man Tin, Kowloon.",
    "Address_cn": "九龍何文田忠義街1號",
    "Phone": "2762 4506",
    "Facility_en": "Arena, Table Tennis, Badminton Court...",
    "Facility_cn": "主場、乒乓球枱、羽毛球場..."
  }
]
```

**Fields**:
- `District_en` / `District_cn` — District name
- `Name_en` / `Name_cn` — Facility name
- `Address_en` / `Address_cn` — Full address
- `Phone` — Contact number
- `Facility_en` / `Facility_cn` — Available facilities (comma-separated)

## Searching

The API returns all facilities at once (116 records). Filter client-side:

```bash
curl -s "https://www.lcsd.gov.hk/datagovhk/facility/facility-en.json" | python3 -c "
import sys,json
facilities = json.load(sys.stdin)
# Filter by district
district = 'Wan Chai'
results = [f for f in facilities if district.lower() in f['District_en'].lower()]
for f in results:
    print(f'🏊 {f[\"Name_en\"]}')
    print(f'   {f[\"Address_en\"]}')
    print(f'   📞 {f[\"Phone\"]}')
    print(f'   Facilities: {f[\"Facility_en\"][:80]}...')
    print()
"
```

## Tips

- Filter by district name or facility type (e.g., "Swimming Pool", "Badminton")
- Combine with hk-geodata to find facilities near a specific location
- Show phone number for booking enquiries
- Use 🏊 🏸 🏋️ ⚽ 🎾 emoji based on facility type
