# 🇭🇰 HK Open Data Skills

Agent skills for querying Hong Kong government open data. Built for [OpenClaw](https://openclaw.ai) / [Claude Code](https://claude.ai) and compatible AI agents.

**No API keys required. All data from official HK government sources.**

## Skills

| Skill | Description | Install |
|-------|-------------|---------|
| **hk-weather** | HK Observatory weather, forecasts, warnings | `npx skills add samsonllam/hk-open-data-skills@hk-weather` |
| **hk-transport** | KMB, Citybus, MTR real-time arrivals | `npx skills add samsonllam/hk-open-data-skills@hk-transport` |
| **hk-parking** | Real-time car park vacancy data | `npx skills add samsonllam/hk-open-data-skills@hk-parking` |
| **hk-geodata** | Location search, address lookup | `npx skills add samsonllam/hk-open-data-skills@hk-geodata` |
| **hk-open-data** | All-in-one combined skill | `npx skills add samsonllam/hk-open-data-skills@hk-open-data` |

## Data Sources

- [Hong Kong Observatory](https://data.weather.gov.hk/) — Weather API
- [KMB](https://data.etabus.gov.hk/) — Bus routes & ETA
- [Citybus](https://rt.data.gov.hk/) — Bus routes & ETA
- [MTR](https://rt.data.gov.hk/) — Train schedules
- [GeoData.gov.hk](https://geodata.gov.hk/) — Location search
- [Transport Department](https://resource.data.gov.hk/) — Parking vacancy
- [DATA.GOV.HK](https://data.gov.hk/) — Historical archives

## License

MIT
