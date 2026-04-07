# nbtc@23

คู่มือปฏิบัติงานตรวจสอบและกำกับดูแลกิจการโทรคมนาคม — สำนักงาน กสทช. ภาค/เขต

## Guides

| Guide | Description |
|-------|-------------|
| [คู่มือตรวจ FM](docs/guide/parameter.md) | Occupied BW, Frequency Deviation, Spurious, Intermodulation, NBTC Deviation Mask (มท. ๘๐๐๑-๒๕๕๕) |
| [Frequency Occupancy](docs/guide/frequency-occupancy.md) | FCO, FBO, Threshold ตาม ITU-R SM.1880 |
| [R&S NESTOR](docs/guide/nestor.md) | วิเคราะห์ Cell Tower (ACD, SCN, COV, CPE, BSA), Cell ID/PCI/eNB ID, ตรวจจับ Fake BTS / Repeater / SIM Box |
| [ตรวจวัด EMF](docs/guide/emf-guide.md) | พื้นฐาน EMF, มาตรฐาน ICNIRP, วิธีวัด, เกณฑ์ผ่าน/ไม่ผ่าน |
| [พื้นฐาน Spectrum Analyzer](docs/guide/spectrum-analyzer.md) | RBW, VBW, Span, Trace Mode, Detector, Marker |
| [เตรียมตัวลงพื้นที่](docs/guide/fieldwork.md) | เตรียมอุปกรณ์และขั้นตอนก่อนออกสนาม |
| [แผนปฏิบัติงาน 2569](docs/guide/plan-2569.md) | แผนตรวจสอบและกำกับดูแลประจำปี |

## Tech Stack

- [Zensical](https://zensical.org) + [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) — static site generator
- [Obsidian](https://obsidian.md) — markdown editor
- Google Calendar embed — ปฏิทินงาน

## Development

```bash
# Install dependencies
uv sync

# Dev server
uv run zensical serve

# Build
uv run zensical build
```

## Deploy

Site published at: https://dearxcorex.github.io/nbtc_document/
