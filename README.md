# HanoiAQ — Chất lượng không khí Hà Nội

Interactive map showing Hanoi's air quality by ward, with PM2.5 concentration and cigarette-equivalent exposure. Published as part of the VnExpress article *"Người Hà Nội hút thụ động bao nhiêu điếu thuốc mỗi ngày do ô nhiễm"*.

**Live:** embedded in VnExpress via `index.html`

---

## What's in this repo

| File | Description |
|------|-------------|
| `index.html` | Interactive ward map (Leaflet + Tailwind) |
| `HanoiAQ.geojson` | Ward-level GeoJSON with AQI and PM2.5 values pre-merged |

---

## How the data was built

Data comes from **three third-party sources** — no proprietary data is included. The processing work (done in `Code.ipynb`, not in this repo) involved:

1. **Converting all sources to a common VN AQI scale**
   - US Embassy: raw PM2.5 daily averages → VN AQI (TCMT QĐ 1459/2019)
   - AQICN: US AQI → back-convert to PM2.5 → VN AQI
   - OpenAQ API (`/v3/sensors/{id}/days`): PM2.5 daily avg → VN AQI

2. **Filtering to the pollution season**: Oct 1 – Jan 9 (2020–2025), when Hanoi's air quality is typically worst

3. **Spatial matching**: Station readings were matched to Hanoi ward boundaries to produce `HanoiAQ.geojson`

### VN AQI formula (PM2.5, per TCMT QĐ 1459/2019)

| PM2.5 range (µg/m³) | AQI range |
|---|---|
| 0 – 25 | 0 – 50 |
| 25 – 50 | 51 – 100 |
| 50 – 80 | 101 – 150 |
| 80 – 150 | 151 – 200 |
| 150 – 250 | 201 – 300 |
| > 250 | > 300 |

### Cigarette equivalence

Per [Berkeley Earth](http://berkeleyearth.org/archive/air-pollution-and-cigarette-equivalence/): **22 µg/m³ PM2.5 daily avg ≈ 1 cigarette/day**.

---

## Data sources

| Source | What it provides | Access |
|--------|-----------------|--------|
| US Embassy Hanoi | Hourly PM2.5 at embassy station | [Public download](https://www.stateair.net/web/historical/1/4.html) |
| AQICN | Daily AQI for multiple Hanoi stations | [aqicn.org/data-platform/token/](https://aqicn.org/data-platform/token/) |
| OpenAQ v3 API | Daily PM2.5 for 10 suburban Hanoi stations | `GET /v3/sensors/{id}/days` — requires API key |

### OpenAQ station IDs used

| Station | Sensor ID |
|---------|-----------|
| An Khánh | 7772012 |
| Liên Quan | 7772053 |
| Minh Khai - Bắc Từ Liêm | 7772087 |
| Pháp Vân | 7772016 |
| Sài Sơn | 7772076 |
| Lưu Quang Vũ | 7772032 |
| Sóc Sơn | 7772037 |
| Vân Đình | 7771984 |
| Vân Hà | 7772059 |
| Xuân Mai | 7772015 |

---

## To update the data

1. Download new data from the three sources above
2. Re-run `Code.ipynb` (kept separately — ask the Spotlight team for a copy)
3. Replace `HanoiAQ.geojson` with the newly generated file
