---
name: hk-facilities
description: Search Hong Kong public sports and leisure facilities from the Leisure and Cultural Services Department (LCSD). Use when the user asks about sports centres, gyms, swimming pools, parks, courts, or any public recreational facility in Hong Kong. Covers 116+ venues across all 18 districts.
---

# HK Facilities Skill

Search LCSD public sports and leisure facilities. No API key needed.

## API Endpoint

### Sports Centres & Facilities
```bash
# Bilingual (English + Chinese fields) — 116 venues
curl -s "https://www.lcsd.gov.hk/datagovhk/facility/facility-sc.json"
```

Traditional Chinese (courts/sports halls subset — 55 venues):
```bash
curl -s "https://www.lcsd.gov.hk/datagovhk/facility/facility-tc.json"
```

## Response Format

```json
[
  {
    "District_en": "Kowloon City",
    "District_cn": "九龍城區",
    "Name_en": "Fat Kwong Street Sports Centre",
    "Name_cn": "佛光街體育館",
    "Address_en": "1 Fat Kwong Street, Ho Man Tin, Kowloon.",
    "Address_cn": "九龍何文田佛光街1號",
    "GIHS": "...",
    "Facilities_en": "Arena, Table Tennis Table, Badminton Court...",
    "Facilities_cn": "主場、乒乓球枱、羽毛球場...",
    "Ancillary_facilities_en": "Changing Room, Locker...",
    "Ancillary_facilities_cn": "更衣室、儲物櫃...",
    "Opening_hours_en": "7:00a.m. - 11:00p.m.",
    "Opening_hours_cn": "上午7時至晚上11時",
    "Maintenance_day_en": "...",
    "Maintenance_day_cn": "...",
    "Phone": "2762 4506",
    "Photo_1": "...",
    "Remarks_en": "...",
    "Remarks_cn": "...",
    "Longitude": "114-10-52",
    "Latitude": "22-18-53"
  }
]
```

**Fields**:
- `District_en` / `District_cn` — District name
- `Name_en` / `Name_cn` — Facility name
- `Address_en` / `Address_cn` — Full address
- `Phone` — Contact number
- `Facilities_en` / `Facilities_cn` — Main facilities (comma-separated)
- `Ancillary_facilities_en` / `Ancillary_facilities_cn` — Supporting facilities
- `Opening_hours_en` / `Opening_hours_cn` — Operating hours
- `Maintenance_day_en` / `Maintenance_day_cn` — Scheduled maintenance
- `Longitude` / `Latitude` — Coordinates in DMS format (e.g., `"114-10-52"` = 114°10'52"E). Convert: `deg + min/60 + sec/3600` for decimal.
- `Photo_1`, `Photo_2`, `Photo_3` — Photo URLs
- `Remarks_en` / `Remarks_cn` — Additional notes

## Searching

The API returns all facilities at once (116 records). Filter client-side:

```bash
curl -s "https://www.lcsd.gov.hk/datagovhk/facility/facility-sc.json" | python3 -c "
import sys,json
facilities = json.load(sys.stdin)
# Filter by district
district = 'Wan Chai'
results = [f for f in facilities if district.lower() in f['District_en'].lower()]
for f in results:
    print(f'🏊 {f[\"Name_en\"]}')
    print(f'   {f[\"Address_en\"]}')
    print(f'   📞 {f[\"Phone\"]}')
    print(f'   Facilities: {f[\"Facilities_en\"][:80]}...')
    print()
"
```

## Tips

- Filter by district name or facility type (e.g., "Swimming Pool", "Badminton")
- Use `Longitude`/`Latitude` fields to find facilities near a specific location
- Combine with hk-geodata for address-to-coordinate lookups
- Show phone number for booking enquiries
- Use 🏊 🏸 🏋️ ⚽ 🎾 emoji based on facility type
