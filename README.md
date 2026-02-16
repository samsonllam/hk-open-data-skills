# 🇭🇰 HK Open Data Skills

Agent skills for querying Hong Kong government open data. Built for [OpenClaw](https://openclaw.ai) / [Claude Code](https://claude.ai) and compatible AI agents.

**No API keys required. All data from official HK government sources.**

## Skills

| Skill | Description | Install |
|-------|-------------|---------|
| 🌤️ **hk-weather** | HK Observatory weather, forecasts, warnings | `npx skills add samsonllam/hk-open-data-skills@hk-weather` |
| 🚌 **hk-transport** | KMB, Citybus, GMB, NLB, MTR, Light Rail | `npx skills add samsonllam/hk-open-data-skills@hk-transport` |
| 🅿️ **hk-parking** | Real-time car park vacancy (541+ parks) | `npx skills add samsonllam/hk-open-data-skills@hk-parking` |
| 📍 **hk-geodata** | Location search, address lookup | `npx skills add samsonllam/hk-open-data-skills@hk-geodata` |
| 🏥 **hk-hospital** | A&E waiting times (18 hospitals) | `npx skills add samsonllam/hk-open-data-skills@hk-hospital` |
| 📰 **hk-news** | RTHK latest news headlines | `npx skills add samsonllam/hk-open-data-skills@hk-news` |
| 💰 **hk-finance** | HKMA monetary data, HIBOR, exchange rates | `npx skills add samsonllam/hk-open-data-skills@hk-finance` |
| 🏊 **hk-facilities** | LCSD sports centres & leisure facilities | `npx skills add samsonllam/hk-open-data-skills@hk-facilities` |
| 🇭🇰 **hk-open-data** | All-in-one combined skill | `npx skills add samsonllam/hk-open-data-skills@hk-open-data` |

## Data Sources

- [Hong Kong Observatory](https://data.weather.gov.hk/) — Weather API
- [KMB](https://data.etabus.gov.hk/) — Bus routes & ETA
- [Citybus](https://rt.data.gov.hk/) — Bus routes & ETA
- [GMB](https://data.etagmb.gov.hk/) — Minibus routes & ETA
- [NLB](https://rt.data.gov.hk/) — Lantau bus routes
- [MTR](https://rt.data.gov.hk/) — Train & Light Rail schedules
- [GeoData.gov.hk](https://geodata.gov.hk/) — Location search
- [Transport Department](https://api.data.gov.hk/) — Parking vacancy
- [Hospital Authority](https://www.ha.org.hk/) — A&E wait times
- [RTHK](https://rthk.hk/) — News RSS feeds
- [HKMA](https://api.hkma.gov.hk/) — Financial data
- [LCSD](https://www.lcsd.gov.hk/) — Leisure facilities
- [DATA.GOV.HK](https://data.gov.hk/) — Historical archives

## License

MIT
