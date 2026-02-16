---
name: hk-geodata
description: Search and look up Hong Kong geographic locations, addresses, landmarks, and places using the GeoData API. Use when the user asks about locations in Hong Kong, needs to find an address, wants to know which district a place is in, or needs coordinates for any HK location. Also useful for resolving place names to coordinates for other HK data skills.
---

# HK GeoData Skill

Search Hong Kong locations using the official GeoData.gov.hk API. No API key needed.

## API Endpoint

```
https://geodata.gov.hk/gs/api/v1.0.0/locationSearch?q={QUERY}
```

## How to Query

```bash
curl -s "https://geodata.gov.hk/gs/api/v1.0.0/locationSearch?q=central" | python3 -m json.tool
```

## Response Format

Returns a JSON array of matching locations:

```json
[
  {
    "nameEN": "Central",
    "nameZH": "中環",
    "addressEN": "",
    "addressZH": "",
    "districtEN": "Central and Western District",
    "districtZH": "中西區",
    "x": 834463,
    "y": 815890
  }
]
```

**Fields**:
- `nameEN` / `nameZH` — Place name (English / Chinese)
- `addressEN` / `addressZH` — Street address if available
- `districtEN` / `districtZH` — HK district
- `x`, `y` — HK 1980 Grid coordinates (not WGS84 lat/lon)

## Coordinate Conversion (HK Grid → WGS84)

The API returns HK 1980 Grid System coordinates. To convert to latitude/longitude (WGS84), use this approximation:

```python
def hk_grid_to_wgs84(x, y):
    """Approximate conversion from HK 1980 Grid to WGS84."""
    lat = 22.0 + (y - 800000) / 111320.0
    lon = 114.0 + (x - 832000) / (111320.0 * 0.9272)
    return round(lat, 6), round(lon, 6)
```

For precise conversion, use the Lands Department's coordinate transformation tool or the `pyproj` library with EPSG:2326 (HK 1980 Grid) → EPSG:4326 (WGS84).

## Usage Tips

- Search works with English and Chinese place names
- Returns multiple results; present the most relevant ones
- Use district info to disambiguate similar names
- Combine with other HK skills (e.g., find location → check nearby parking)

## HK Districts (18)

Central & Western, Wan Chai, Eastern, Southern, Yau Tsim Mong, Sham Shui Po, Kowloon City, Wong Tai Sin, Kwun Tong, Tsuen Wan, Tuen Mun, Yuen Long, North, Tai Po, Sha Tin, Sai Kung, Kwai Tsing, Islands
