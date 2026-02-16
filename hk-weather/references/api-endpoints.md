# HK Observatory API Reference

Base URL: `https://data.weather.gov.hk/weatherAPI/opendata/weather.php`

## Endpoints

All endpoints use GET with query params `dataType` and `lang` (en/tc/sc).

### Current Weather (`rhrread`)
```
?dataType=rhrread&lang=en
```
Returns: rainfall (by district), temperature, humidity, UV index, weather icon, warning messages.

**Response keys**: `rainfall`, `icon`, `iconUpdateTime`, `uvindex`, `updateTime`, `temperature`, `warningMessage`, `mintempFrom00To09`, `rainfallFrom00To12`, `rainfallLastMonth`, `rainfallJanuaryToLastMonth`, `tcmessage`, `humidity`

**rainfall.data[]**: `{ unit, place, max, main }` — district-level rainfall in mm
**temperature.data[]**: `{ place, value, unit }` — station temperatures in °C
**humidity.data[]**: `{ unit, value, place }` — humidity readings in %

### 9-Day Forecast (`fnd`)
```
?dataType=fnd&lang=en
```
Returns: general weather situation, 9-day forecast, sea temp, soil temp.

**Response keys**: `generalSituation`, `weatherForecast`, `updateTime`, `seaTemp`, `soilTemp`

**weatherForecast[]**: `{ forecastDate, week, forecastWind, forecastWeather, forecastMaxtemp, forecastMintemp, forecastMaxrh, forecastMinrh, ForecastIcon, PSR }`

### Local Weather Forecast (`flw`)
```
?dataType=flw&lang=en
```
Returns: general situation, TC info, fire danger warning, forecast description, outlook.

**Response keys**: `generalSituation`, `tcInfo`, `fireDangerWarning`, `forecastPeriod`, `forecastDesc`, `outlook`, `updateTime`

### Warning Summary (`warnsum`)
```
?dataType=warnsum&lang=en
```
Returns: active weather warnings (empty object `{}` if none).

**Warning types**: Amber/Red/Black Rainstorm, T1/T3/T8/T9/T10 Typhoon, Thunderstorm, Very Hot/Cold Weather, Frost, Landslip, Tsunami, Fire Danger (Yellow/Red)

### Warning Information (`warningInfo`)
```
?dataType=warningInfo&lang=en
```
Returns: detailed warning info with descriptions (empty `{}` if none).

### Special Weather Tips (`swt`)
```
?dataType=swt&lang=en
```
Returns: `{ swt: [...] }` — special weather tips from HKO.

### Weather Icons
Icon numbers map to conditions:
- 50: Sunny
- 51: Sunny Periods
- 52: Sunny Intervals
- 53: Sunny Periods with Showers
- 54: Sunny Intervals with Showers
- 60: Cloudy
- 61: Overcast
- 62: Light Rain
- 63: Rain
- 64: Heavy Rain
- 65: Thunderstorms
- 70: Fine (Night)
- 71: Fine (Night)
- 72: Cloudy (Night)
- 73: Showers (Night)
- 76: Mainly Cloudy
- 77: Mainly Fine
- 80: Windy
- 81: Dry
- 82: Humid
- 83: Fog
- 84: Mist
- 85: Haze
- 90: Hot
- 91: Warm
- 92: Cool
- 93: Cold

## Earthquake API
```
https://data.weather.gov.hk/weatherAPI/opendata/earthquake.php?dataType=qem&lang=en
```
Returns: latest felt/significant earthquake info with lat, lon, magnitude, region, time.

## Language Codes
- `en` — English
- `tc` — Traditional Chinese (繁體中文)
- `sc` — Simplified Chinese (简体中文)

## Rate Limits
- No API key required
- No documented rate limits (be reasonable, cache responses)
- Data updates roughly every 10-60 minutes depending on type
