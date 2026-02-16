---
name: hk-parking
description: Query real-time parking vacancy data for Hong Kong car parks from the Transport Department. Use when the user asks about parking availability, car park vacancies, where to find parking spots, or parking information in Hong Kong. Covers 541+ government and private car parks across HK.
---

# HK Parking Skill

Query real-time parking vacancy data from HK government. No API key needed.

## API Endpoints

### Car Park Info (static data)
```bash
curl -s "https://api.data.gov.hk/v1/carpark-info-vacancy?data=info&vehicleTypes=privateCar"
```
Returns: name, address, district, lat/lon, opening hours, facilities, height limits.

### Car Park Vacancy (real-time)
```bash
curl -s "https://api.data.gov.hk/v1/carpark-info-vacancy?data=vacancy&vehicleTypes=privateCar"
```
Returns: real-time vacancy counts for 541+ car parks.

### Combined Info + Vacancy
```bash
curl -s "https://api.data.gov.hk/v1/carpark-info-vacancy?data=info,vacancy&vehicleTypes=privateCar"
```

## Response Format

### Info response
```json
{
  "results": [{
    "park_Id": "12",
    "name": "Amoy Plaza",
    "nature": "commercial",
    "carpark_Type": "multi-storey",
    "displayAddress": "77 Ngau Tau Kok Road, Kowloon Bay, KLN",
    "district": "Kwun Tong District",
    "latitude": 22.3247,
    "longitude": 114.2168,
    "opening_status": "OPEN",
    "facilities": ["evCharger", "disabilities", "unloading", "washing"],
    "heightLimits": [{ "height": 1.9 }]
  }]
}
```

### Vacancy response
```json
{
  "results": [{
    "park_Id": "12",
    "privateCar": [{
      "vacancy_type": "A",
      "vacancy": 5,
      "vacancyEV": 0,
      "vacancyDIS": 1,
      "lastupdate": "2026-02-15 20:40:23"
    }],
    "motorCycle": [{ "vacancy_type": "A", "vacancy": 0 }],
    "LGV": [{ "vacancy_type": "A", "vacancy": 0 }]
  }]
}
```

**Fields**:
- `vacancy` — available spaces (for private cars)
- `vacancyEV` — EV charger spaces
- `vacancyDIS` — disabled parking spaces
- `vacancy_type` — A (Available), L (Last few), F (Full)
- `lastupdate` — data freshness timestamp

## Query Parameters

| Param | Values | Description |
|-------|--------|-------------|
| `data` | `info`, `vacancy`, or `info,vacancy` | What data to return |
| `vehicleTypes` | `privateCar`, `LGV`, `HGV`, `motorCycle` | Filter by vehicle type |
| `carparkIds` | comma-separated IDs | Filter specific car parks |

## Workflow

1. Fetch vacancy data with `data=info,vacancy&vehicleTypes=privateCar`
2. Filter by district or use lat/lon to find nearest car parks
3. Sort by vacancy count (descending)
4. Present with name, address, available spaces, and last update time

## Tips

- Use `data=info,vacancy` to get both name/location and vacancy in one call
- `vacancy_type: "F"` means FULL — highlight this to users
- Combine with hk-geodata skill to find car parks near a specific location
- Show EV charger availability when relevant
- Use 🅿️ emoji and colour-code: green (>10), yellow (1-10), red (full)
