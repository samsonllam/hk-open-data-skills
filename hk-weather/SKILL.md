---
name: hk-weather
description: Query real-time Hong Kong weather data from the Hong Kong Observatory (HKO) API. Use when the user asks about Hong Kong weather, temperature, rainfall, humidity, forecasts, typhoon signals, rainstorm warnings, weather warnings, UV index, or any weather-related question specific to Hong Kong. Also triggers for questions about whether to bring an umbrella, what to wear, or outdoor activity planning in HK.
---

# HK Weather Skill

Query Hong Kong weather using the official HK Observatory (天文台) API. No API key needed.

## API Base

```
https://data.weather.gov.hk/weatherAPI/opendata/weather.php?dataType={TYPE}&lang={LANG}
```

Languages: `en` (English), `tc` (繁體中文), `sc` (简体中文). Default to user's language.

## Quick Reference

| Query | dataType | What you get |
|-------|----------|-------------|
| Current conditions | `rhrread` | Temperature, humidity, rainfall by district, UV, warnings |
| 9-day forecast | `fnd` | Daily forecast with min/max temp, weather, wind |
| Today's forecast | `flw` | General situation, forecast description, outlook |
| Active warnings | `warnsum` | List of active warnings (empty `{}` = none) |
| Warning details | `warningInfo` | Detailed warning descriptions |
| Special tips | `swt` | Special weather tips from HKO |

For earthquake data: `https://data.weather.gov.hk/weatherAPI/opendata/earthquake.php?dataType=qem&lang=en`

## How to Query

1. Use `curl` to call the appropriate endpoint
2. Parse the JSON response
3. Present relevant info to user in a readable format

### Example: Current Weather

```bash
curl -s "https://data.weather.gov.hk/weatherAPI/opendata/weather.php?dataType=rhrread&lang=en" | python3 -m json.tool
```

Key fields in response:
- `temperature.data[]` → `{ place, value, unit }` (station temperatures)
- `humidity.data[]` → `{ value, unit }` (humidity %)
- `rainfall.data[]` → `{ place, max, unit }` (district rainfall in mm)
- `uvindex.data[]` → UV readings
- `icon[]` → weather condition icon numbers
- `warningMessage` → active warning text (if any)

### Example: 9-Day Forecast

```bash
curl -s "https://data.weather.gov.hk/weatherAPI/opendata/weather.php?dataType=fnd&lang=en" | python3 -m json.tool
```

Key fields:
- `weatherForecast[]` → `{ forecastDate, week, forecastWeather, forecastMaxtemp, forecastMintemp, forecastWind, ForecastIcon }`
- `generalSituation` → overall weather description

## Presentation Guidelines

- Show temperature range (min-max) when available
- Mention rainfall if > 0mm in any district
- Always mention active warnings prominently (typhoon/rainstorm signals are critical in HK)
- For forecasts, group by date and show weather + temp range
- Use weather emoji: ☀️ 🌤️ ⛅ 🌥️ ☁️ 🌧️ ⛈️ 🌪️ as appropriate
- For Chinese responses, use 繁體中文 (match HK style)

## Sunrise / Sunset / Moonrise
```bash
# Sunrise & sunset (full year)
curl -s "https://data.weather.gov.hk/weatherAPI/opendata/opendata.php?dataType=SRS&lang=en&rformat=json&year=2026"
# Moonrise & moonset (full year)
curl -s "https://data.weather.gov.hk/weatherAPI/opendata/opendata.php?dataType=MRS&lang=en&rformat=json&year=2026"
```
Response: `{ fields: ["YYYY-MM-DD","RISE","TRAN.","SET"], data: [["2026-02-17","06:54","12:37","18:21"], ...] }`

## Lunar Calendar
```bash
curl -s "https://data.weather.gov.hk/weatherAPI/opendata/lunardate.php?date=20260217"
```
Response: `{ "LunarYear": "丙午年，馬", "LunarDate": "正月初一" }`

Date format: `YYYYMMDD`. Useful for Chinese festivals and traditional calendar queries.

## Historical Climate Data
```bash
# Temperature (since 1884!)
curl -s "https://data.weather.gov.hk/weatherAPI/opendata/opendata.php?dataType=CLMTEMP&lang=en&rformat=json&station=HKO"
# Max temperature
curl -s "...?dataType=CLMMAXT&lang=en&rformat=json&station=HKO"
# Min temperature
curl -s "...?dataType=CLMMINT&lang=en&rformat=json&station=HKO"
```
Response: `{ fields: ["Year","Month","Day","Value","Completeness"], data: [["1884","3","1","***",""], ...] }`

49,000+ records of daily temperature data. Use for historical comparisons and trends.

## Full API Details

See [references/api-endpoints.md](references/api-endpoints.md) for complete endpoint documentation, response schemas, and weather icon mappings.
