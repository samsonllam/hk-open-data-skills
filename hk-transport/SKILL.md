---
name: hk-transport
description: Query Hong Kong public transport data including KMB bus, Citybus, and MTR real-time arrival times and route information. Use when the user asks about bus arrival times, MTR schedules, route planning, which bus to take, next train time, or any public transport question in Hong Kong. Covers KMB (1600+ routes), Citybus (400+ routes), and MTR (all lines).
---

# HK Transport Skill

Query real-time Hong Kong public transport data. No API key needed.

## APIs Overview

| Provider | Routes | Base URL |
|----------|--------|----------|
| KMB 九巴 | 1,621 | `https://data.etabus.gov.hk/v1/transport/kmb/` |
| Citybus 城巴 | 416 | `https://rt.data.gov.hk/v2/transport/citybus/` |
| MTR 港鐵 | All lines | `https://rt.data.gov.hk/v1/transport/mtr/getSchedule.php` |

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

## Presentation Tips

- Show next 2-3 arrivals with minutes remaining
- Mention remarks (Last Bus, Scheduled, etc.)
- For MTR, show both UP and DOWN directions
- Use 🚌 for bus, 🚇 for MTR
- Format times relative to now: "3 分鐘後", "12:45 (5 min)"
