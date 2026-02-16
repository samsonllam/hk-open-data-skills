---
name: hk-transport
description: Query Hong Kong public transport data including KMB bus, Citybus, GMB minibus, NLB (Lantau bus), MTR heavy rail, and Light Rail real-time arrival times and route information. Use when the user asks about bus arrival times, MTR schedules, minibus routes, light rail, route planning, which bus to take, next train time, or any public transport question in Hong Kong. Covers KMB (1600+ routes), Citybus (400+), GMB minibus (569), NLB (64), MTR (all lines), and Light Rail.
---

# HK Transport Skill

Query real-time Hong Kong public transport data. No API key needed.

## APIs Overview

| Provider | Routes | Base URL |
|----------|--------|----------|
| KMB 九巴 | 1,621 | `https://data.etabus.gov.hk/v1/transport/kmb/` |
| Citybus 城巴 | 416 | `https://rt.data.gov.hk/v2/transport/citybus/` |
| GMB 小巴 | 569 | `https://data.etagmb.gov.hk/` |
| NLB 嶼巴 | 64 | `https://rt.data.gov.hk/v1/transport/nlb/` |
| MTR 港鐵 | All lines | `https://rt.data.gov.hk/v1/transport/mtr/getSchedule.php` |
| Light Rail 輕鐵 | All routes | `https://rt.data.gov.hk/v1/transport/mtr/lrt/getSchedule` |

## KMB Bus API

### List All Routes
```bash
curl -s "https://data.etabus.gov.hk/v1/transport/kmb/route/" | python3 -m json.tool
```
Response: `{ data: [{ route, bound, service_type, orig_en, orig_tc, dest_en, dest_tc }] }`

### Route Stops
```bash
curl -s "https://data.etabus.gov.hk/v1/transport/kmb/route-stop/{ROUTE}/{inbound|outbound}/{SERVICE_TYPE}"
```
Response: `{ data: [{ route, bound, service_type, seq, stop }] }` — `stop` is stop ID.

### Stop Details
```bash
curl -s "https://data.etabus.gov.hk/v1/transport/kmb/stop/{STOP_ID}"
```
Response: `{ data: { stop, name_en, name_tc, lat, long } }`

### ETA at Stop
```bash
curl -s "https://data.etabus.gov.hk/v1/transport/kmb/stop-eta/{STOP_ID}"
```
Response: `{ data: [{ route, dir, service_type, seq, dest_en, dest_tc, eta, rmk_en, rmk_tc }] }`
- `eta` — ISO timestamp of estimated arrival (null if no data)
- `rmk_en` — remarks (e.g., "Last Bus", "Scheduled")

### ETA for Specific Route at Stop
```bash
curl -s "https://data.etabus.gov.hk/v1/transport/kmb/eta/{STOP_ID}/{ROUTE}/{SERVICE_TYPE}"
```

## Citybus API

### List Routes
```bash
curl -s "https://rt.data.gov.hk/v2/transport/citybus/route/ctb"
```
Response: `{ data: [{ co, route, orig_en, orig_tc, dest_en, dest_tc }] }`

### Route Stops
```bash
curl -s "https://rt.data.gov.hk/v2/transport/citybus/route-stop/ctb/{ROUTE}/{inbound|outbound}"
```

### Stop Details
```bash
curl -s "https://rt.data.gov.hk/v2/transport/citybus/stop/{STOP_ID}"
```

### ETA
```bash
curl -s "https://rt.data.gov.hk/v2/transport/citybus/eta/ctb/{STOP_ID}/{ROUTE}"
```

## MTR API

### Train Schedule
```bash
curl -s "https://rt.data.gov.hk/v1/transport/mtr/getSchedule.php?line={LINE}&sta={STATION}"
```
Response: `{ data: { "{LINE}-{STATION}": { UP: [...], DOWN: [...] } } }`

Each entry: `{ seq, dest, plat, time, ttnt, valid, source }`
- `ttnt` — time to next train (minutes)
- `dest` — destination station code

### MTR Line Codes
| Code | Line |
|------|------|
| AEL | Airport Express |
| TCL | Tung Chung Line |
| TKL | Tseung Kwan O Line |
| TML | Tuen Ma Line |
| EAL | East Rail Line |
| SIL | South Island Line |
| TWL | Tsuen Wan Line |
| ISL | Island Line |
| KTL | Kwun Tong Line |
| DRL | Disneyland Resort Line |

### MTR Station Codes
See [references/mtr-stations.md](references/mtr-stations.md) for full station code list.

## Workflow: Finding Bus ETA

1. Search for the route: `GET /kmb/route/` and filter by route number
2. Get route stops: `GET /kmb/route-stop/{ROUTE}/{DIRECTION}/1`
3. Find the stop nearest to user (by name or use hk-geodata skill)
4. Get ETA: `GET /kmb/eta/{STOP_ID}/{ROUTE}/1`
5. Parse `eta` timestamps and calculate minutes until arrival

## GMB Minibus API

### List All Routes
```bash
curl -s "https://data.etagmb.gov.hk/route"
```
Response: `{ data: { routes: { HKI: [...], KLN: [...], NT: [...] } } }` — 569 routes total.

### Route Detail
```bash
curl -s "https://data.etagmb.gov.hk/route/{REGION}/{ROUTE_CODE}"
```
Regions: `HKI` (Hong Kong Island), `KLN` (Kowloon), `NT` (New Territories).
Response includes `route_id`, directions with origin/destination, headways.

### Route Stops
```bash
curl -s "https://data.etagmb.gov.hk/route-stop/{ROUTE_ID}/{ROUTE_SEQ}"
```

### Stop ETA
```bash
curl -s "https://data.etagmb.gov.hk/eta/route-stop/{ROUTE_ID}/{ROUTE_SEQ}/{STOP_SEQ}"
```

## NLB (Lantau Bus) API

### List Routes
```bash
curl -s "https://rt.data.gov.hk/v1/transport/nlb/route.php?action=list"
```
Response: `{ routes: [{ routeId, routeNo, routeName_e, routeName_c, overnightRoute, specialRoute }] }` — 64 routes.

### Route Stops
```bash
curl -s "https://rt.data.gov.hk/v1/transport/nlb/stop.php?action=list&routeId={ROUTE_ID}"
```

### ETA
```bash
curl -s "https://rt.data.gov.hk/v1/transport/nlb/stop.php?action=estimatedArrivals&routeId={ROUTE_ID}&stopId={STOP_ID}&language=en"
```

## Light Rail (LRT) API

### Station Schedule
```bash
curl -s "https://rt.data.gov.hk/v1/transport/mtr/lrt/getSchedule?station_id={STATION_ID}"
```
Response: `{ platform_list: [{ platform_id, route_list: [{ route_no, dest_en, dest_ch, time_en, time_ch, train_length }] }] }`

Station IDs are 3-digit numbers (e.g., 001=Tuen Mun Ferry Pier). See [references/lrt-stations.md](references/lrt-stations.md) for full list.

## Presentation Tips

- Show next 2-3 arrivals with minutes remaining
- Mention remarks (Last Bus, Scheduled, etc.)
- For MTR, show both UP and DOWN directions
- Use 🚌 for bus, 🚐 for minibus, 🚇 for MTR, 🚃 for Light Rail
- Format times relative to now: "3 分鐘後", "12:45 (5 min)"
