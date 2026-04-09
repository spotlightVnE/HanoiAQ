# HanoiAQ — Chất lượng không khí Hà Nội

Interactive map showing Hanoi's air quality by ward, with PM2.5 concentration and cigarette-equivalent exposure. Published as part of the VnExpress article *"Người Hà Nội hút thụ động bao nhiêu điếu thuốc mỗi ngày do ô nhiễm"*.

**Live:** embedded in VnExpress via `index.html`

---

## What's in this repo

| File | Description |
|------|-------------|
| `index.html` | Interactive ward map (Leaflet + Tailwind) |
| `HanoiAQ.geojson` | Ward-level GeoJSON with AQI and PM2.5 values pre-merged (126 wards/communes) |

---

## Data source

Ward-level PM2.5 and AQI data was provided by the **[GEOI research group](https://geoi.edu.vn/vi/)** (Trường Đại học Công nghệ, Đại học Quốc gia Hà Nội — VNU University of Engineering and Technology).

GEOI computes annual average PM2.5 concentrations at ward/commune resolution for Hanoi. Their methodology should be referenced for details on how PM2.5 values are modeled (likely combining ground station measurements, satellite-derived AOD, and spatial interpolation/dispersion modeling).

This data was provided to VnExpress Spotlight in exchange for the administrative boundary dataset (post-merger ward boundaries) that the Spotlight team had prepared.

### GeoJSON properties

| Property | Description |
|---|---|
| `Name` | Ward/commune name |
| `Type` | `phường` (urban ward) or `xã` (commune) |
| `Merge` | Which pre-merger wards were combined (post-2025 admin boundary reform) |
| `PM2.5` | Annual average PM2.5 concentration (µg/m³), Vietnamese decimal format (e.g. `45,3`) |
| `AQI` | Corresponding VN AQI value |
| `Pop` | Population |

---

## Background: initial open-source research

Before GEOI provided their data, the initial analysis was built from three open sources using a processing notebook (`Code.ipynb`, not in this repo):

1. **US Embassy Hanoi** — hourly PM2.5 at the embassy station ([public download](https://www.stateair.net/web/historical/1/4.html))
2. **AQICN** — daily AQI for multiple Hanoi stations ([aqicn.org](https://aqicn.org/data-platform/token/))
3. **OpenAQ v3 API** — daily PM2.5 for 10 suburban stations (`GET /v3/sensors/{id}/days`)

This involved converting all sources to VN AQI (per TCMT QĐ 1459/2019), filtering to the pollution season (Oct–Jan, 2020–2025), and spatially matching station readings to ward boundaries.

The GEOI dataset replaced this approach with higher-quality, ward-level modeled data covering all 126 wards/communes in Hanoi.

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

## Notes

- `HanoiAQ.geojson` is self-contained — the viz does not fetch from any external source at runtime.
- To update the data, request updated ward-level PM2.5 from GEOI and regenerate the GeoJSON with the same property schema.
- Contact GEOI for their full methodology documentation if publishing methodology details or extending the analysis to other cities.
