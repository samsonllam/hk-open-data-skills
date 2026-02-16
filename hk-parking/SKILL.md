---
name: hk-parking
description: Query real-time parking vacancy data for Hong Kong car parks from the Transport Department. Use when the user asks about parking availability, car park vacancies, where to find parking spots, or parking information in Hong Kong. Covers government and private car parks across HK.
---

# HK Parking Skill

Query real-time parking vacancy data from the HK Transport Department. No API key needed.

## API Endpoint

```
https://resource.data.gov.hk/td/parking-vacancy/vacancy_all.xml
```

Returns XML with real-time parking vacancy data for car parks across Hong Kong.

## How to Query

```bash
curl -s "https://resource.data.gov.hk/td/parking-vacancy/vacancy_all.xml"
```

## Response Format (XML)

```xml
<car_park_vacancy>
  <car_park>
    <park_id>1</park_id>
    <name_en>Murray Road Multi-Storey Car Park</name_en>
    <name_tc>美利道多層停車場</name_tc>
    <district_en>Central and Western</district_en>
    <district_tc>中西區</district_tc>
    <lat>22.2793</lat>
    <lng>114.1600</lng>
    <vehicle_type>P</vehicle_type>
    <vacancy>30</vacancy>
    <lastupdate>2026-02-16 12:00:00</lastupdate>
  </car_park>
  ...
</car_park_vacancy>
```

**Fields**:
- `park_id` — Unique car park ID
- `name_en` / `name_tc` — Car park name
- `district_en` / `district_tc` — District
- `lat`, `lng` — WGS84 coordinates
- `vehicle_type` — P (Private car), M (Motorcycle), C (Commercial vehicle)
- `vacancy` — Number of available spaces (-1 = no data)
- `lastupdate` — Last data update timestamp

## Parsing XML

Use Python to parse and filter:

```bash
curl -s "https://resource.data.gov.hk/td/parking-vacancy/vacancy_all.xml" | python3 -c "
import sys, xml.etree.ElementTree as ET
root = ET.parse(sys.stdin).getroot()
for cp in root.findall('.//car_park'):
    name = cp.find('name_en').text
    vacancy = cp.find('vacancy').text
    district = cp.find('district_en').text
    vtype = cp.find('vehicle_type').text
    if vtype == 'P' and vacancy and int(vacancy) > 0:
        print(f'{name} ({district}): {vacancy} spaces')
"
```

## Workflow

1. Fetch the XML data
2. Parse with XML parser
3. Filter by district / location / vehicle type as needed
4. Sort by vacancy count or distance from user's location
5. Present top results with name, district, and available spaces

## Tips

- `vacancy = -1` means no real-time data available for that car park
- Combine with hk-geodata skill to find car parks near a specific location
- Data updates roughly every few minutes
- Use 🅿️ emoji when presenting results
- Show both English and Chinese names when the user's language is Chinese
