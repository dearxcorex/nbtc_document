---
title: คู่มือใช้งาน R&S NESTOR
tags:
  - guide
  - nestor
  - cell tower
---

# คู่มือใช้งาน R&S NESTOR สำหรับงาน กสทช.

## 1. NESTOR คืออะไร

**R&S NESTOR** (Network Analysis Software) คือซอฟต์แวร์จาก Rohde & Schwarz สำหรับ **วัด วิเคราะห์ และรายงาน** คุณภาพเครือข่ายโทรศัพท์เคลื่อนที่ รองรับเทคโนโลยี:

| เทคโนโลยี | ค่าที่วัดได้ (Power) | ค่าที่วัดได้ (Quality) |
|-----------|---------------------|----------------------|
| **GSM** | RxLev (dBm) | C/I (dB) |
| **UMTS** | RSCP (dBm) | Ec/No (dB) |
| **LTE** | RSRP (dBm) | RSRQ (dB), SINR (dB) |
| **5G NR** | NB-RSRP (dBm) | NB-RSRQ (dB), NB-SINR (dB) |

### Workflow หลักของ NESTOR

```
  ┌──────────────────┐
  │  R&S NESTOR GUI   │
  └───────┬──────────┘
          │
    ┌─────┴─────┐
    ▼           ▼
┌────────┐  ┌──────────────┐
│วัดสนาม │  │ Replay &     │
│Measure │  │ Analysis     │
└───┬────┘  └──────┬───────┘
    │              │
    ▼              ▼
┌────────────────────┐
│     DB-Files       │
│  (.db ไฟล์ผลวัด)   │
└────────┬───────────┘
         ▼
┌────────────────────┐
│  Post Processing   │
│  วิเคราะห์ภายหลัง   │
└────────────────────┘
```

### อุปกรณ์ที่ใช้ร่วมกับ NESTOR

| อุปกรณ์ | ประเภท | หน้าที่ |
|---------|--------|--------|
| **R&S TSME / TSME6** | Scanner | สแกนความถี่ วัด RF ของ cell tower |
| **R&S TSMW** | Scanner | สแกนความถี่ (รุ่นเก่า) |
| **R&S TSMA / TSMA6** | Scanner + NUC | scanner แบบ standalone มี PC ในตัว |
| **GPS Receiver** | Navigation | ระบุตำแหน่ง lat/long ระหว่างวัด |
| **QualiPoc** | UE Device | สมาร์ทโฟนวัดคุณภาพเครือข่ายฝั่ง user |

---

## 2. คำศัพท์สำคัญ

| คำศัพท์ | ความหมาย |
|--------|---------|
| **Scenario** | โหมดการทำงานหลัก เช่น Cellular Network Analysis, Data Investigation, Monitoring |
| **Use Case** | โมดูลย่อยในแต่ละ Scenario เช่น ACD, SCN, COV, CPE |
| **Workspace** | ชุดการตั้งค่า Use Case ที่บันทึกไว้ใช้ซ้ำ |
| **Template** | ชุดค่า preset ของ Use Case แยกตามเทคโนโลยี (GSM, UMTS, LTE) |
| **BTS List** | ฐานข้อมูลสถานีฐานที่ import เข้า NESTOR (ใช้เทียบกับผลวัด) |
| **Bin** | พื้นที่กริดบนแผนที่สำหรับแบ่งข้อมูลวัดตามตำแหน่ง |
| **TopN** | การจัดอันดับ cell ที่แรงสุด N อันดับแรกในแต่ละ bin |
| **MCC/MNC** | รหัสประเทศ/ผู้ให้บริการ (ไทย: MCC=520, AIS=01, DTAC=05, TRUE=04) |

---

## 3. Scenario หลักที่ใช้งาน

### 3.1 Cellular Network Analysis (วัดสนาม)

ใช้สำหรับ **ออกวัดจริงในสนาม** พร้อม scanner

```
ขั้นตอน:

1.  เปิด NESTOR → เลือก "Cellular Network Analysis"
         │
2.  เลือก Workspace:
         ├→ Quickstart (เริ่มเร็วใช้ค่า default)
         ├→ General Workspace (เลือก workspace ที่บันทึกไว้)
         └→ Open Workspace (เปิดจากไฟล์)
         │
3.  เลือก Use Case ที่ต้องการ → ลากไปใส่ Active Use Cases
         │
4.  กดปุ่ม ► เริ่มวัด
         │
5.  ขับรถ/เดินวัด → NESTOR บันทึกข้อมูลลงไฟล์ .db อัตโนมัติ
```

### 3.2 Data Investigation (วิเคราะห์ภายหลัง)

ใช้สำหรับ **เปิดไฟล์ผลวัดที่บันทึกไว้** แล้ววิเคราะห์ภายหลัง

```
ขั้นตอน:

1.  เปิด NESTOR → เลือก "Data Investigation"
         │
2.  เลือก Workspace
         │
3.  ลาก "Measurement Files" มาวาง → เลือกไฟล์ .db ที่ต้องการ
         │
4.  เลือก Use Case สำหรับวิเคราะห์ (COV, CPE, SCN ฯลฯ)
         │
5.  ดูผลวิเคราะห์ → Export ออกเป็น .csv, .kml, PDF
```

### 3.3 Monitoring (ตรวจวัดต่อเนื่อง)

ติดตั้ง scanner ไว้ที่จุดคงที่ วัดเครือข่ายต่อเนื่องตลอด 24 ชม. เหมาะกับงาน **Base Station Monitoring (BSM)**

---

## 4. Use Case สำหรับวิเคราะห์ Cell Tower

### 4.1 ACD — Automatic Channel Detection (ตรวจจับช่องสัญญาณอัตโนมัติ)

**หน้าที่**: สแกนทุกย่านความถี่ หาว่ามี cell tower ไหนส่งสัญญาณอยู่บ้าง แล้ว map เป็น channel number (ARFCN, UARFCN, EARFCN, NR-ARFCN)

```
ACD ทำงานอัตโนมัติเมื่อเริ่ม measurement
         │
         ▼
┌──────────────────────────────┐
│  สแกนทุกย่านความถี่          │
│  (GSM 900/1800, UMTS 2100,  │
│   LTE 700/900/1800/2100/    │
│   2600, 5G NR n28/n78)      │
└──────────┬───────────────────┘
           ▼
┌──────────────────────────────┐
│  ตรวจพบ channel:             │
│  • Center Frequency (MHz)   │
│  • Bandwidth (LTE)          │
│  • Channel Number           │
│  • Operator                 │
│  • Duplex Mode (FDD/TDD)    │
└──────────────────────────────┘
```

**ข้อมูลที่ได้**:

- จำนวน channel ที่ตรวจพบ แยกตาม GSM, UMTS, LTE, 5G NR
- Band View — แสดงผังย่านความถี่ทั้งหมดที่ตรวจพบ
- ระบุ Operator ที่ใช้งานแต่ละช่องสัญญาณ

!!! tip "เคล็ดลับสำหรับประเทศไทย"

    เลือก Template = **Asia** เพื่อให้สแกนเฉพาะ band ที่ใช้ในไทย ไม่ต้องสแกนทุก band ทั่วโลก (ลดเวลาสแกน)

    Band ที่ใช้ในไทย:

    | เทคโนโลยี | Band |
    |-----------|------|
    | GSM | 900, 1800 |
    | UMTS | Band I (2100 MHz) |
    | LTE | Band 1 (2100), Band 3 (1800), Band 8 (900), Band 28 (700), Band 41 (2600 TDD) |
    | 5G NR | n28 (700), n78 (3500) |

### 4.2 SCN — Scanner Expert (วัด RF ละเอียด)

**หน้าที่**: วัดค่า RF ของแต่ละ cell อย่างละเอียด พร้อม decode System Information (SI/SIB) เพื่อระบุ cell identity

```
SCN แสดงผล:
         │
         ├→ TopN — จัดอันดับ cell ที่แรงสุด (แสดงค่า Power + Quality)
         │
         ├→ SystemInfo View — แสดง SI/SIB ที่ decode ได้
         │         ├→ GSM: SI 1-20 (ARFCN, LAI, Cell ID, BCCH)
         │         ├→ UMTS: MIB, SIB 1-20 (PLMN, LAC, Cell ID, SC)
         │         ├→ LTE: MIB, SIB 1-11 (PLMN, TAC, Cell ID, PCI, BW)
         │         └→ 5G NR: MIB, SIB 1-2 (PLMN, Cell ID, TAC)
         │
         └→ Map — แสดงตำแหน่ง cell บนแผนที่
```

**ค่าที่ SCN วัดได้ (แยกตามเทคโนโลยี)**:

| ค่า | GSM | UMTS | LTE | 5G NR |
|-----|-----|------|-----|-------|
| **Channel ID** | ARFCN, BSIC | UARFCN, SC | EARFCN, PCI | NR-ARFCN, PCI |
| **Cell Identity** | MCC, MNC, LAC, CI | MCC, MNC, LAC, CI | MCC, MNC, TAC, eNB ID, ECI | MCC, MNC, TAC, Cell ID |
| **Power** | RxLev (dBm) | RSCP (dBm) | RSRP (dBm) | SS-RSRP (dBm) |
| **Quality** | C/I (dB) | Ec/No (dB) | RSRQ (dB) | SS-RSRQ (dB) |
| **อื่นๆ** | C1, C2 | SIR (dB) | SINR (dB) | SS-SINR (dB) |

!!! note "SCN สำคัญมาก — เป็นพื้นฐานของ Use Case อื่น"

    ข้อมูลจาก SCN จะถูกใช้ต่อโดย **COV** (วิเคราะห์ coverage) และ **CPE** (ประมาณตำแหน่ง cell) — ถ้า SCN ไม่ถูกตั้งค่าหรือ decode ไม่ได้ Use Case อื่นจะทำงานไม่สมบูรณ์

### 4.3 COV — Coverage Analysis (วิเคราะห์พื้นที่ครอบคลุม)

**หน้าที่**: วิเคราะห์ว่าเครือข่ายมีสัญญาณครอบคลุมพื้นที่ดีแค่ไหน โดยแบ่งพื้นที่เป็น **grid (bin)** แล้วจัดอันดับ cell ที่แรงสุดในแต่ละ bin

**เกณฑ์ Coverage (ค่า default)**:

| เทคโนโลยี | ค่า | Good | Fair | Bad |
|-----------|-----|------|------|-----|
| **GSM** | SCH Power | > -85 dBm | > -105 dBm | < -105 dBm |
| **GSM** | C/I | > 14 dB | > 5 dB | < 5 dB |
| **UMTS** | RSCP | > -95 dBm | > -105 dBm | < -105 dBm |
| **UMTS** | Ec/No | > -10 dB | > -18 dB | < -18 dB |
| **LTE** | RSRP | > -115 dBm | > -125 dBm | < -125 dBm |
| **LTE** | RSRQ | > -12 dB | > -18 dB | < -18 dB |
| **LTE** | SINR | > 20 dB | > 5 dB | < 5 dB |

**ผลวิเคราะห์ที่ได้**:

```
COV แสดงผลใน 4 tab:
         │
         ├→ Overview
         │    ├→ Bin Power Chart — กราฟ % พื้นที่ที่ Good/Fair/Bad (Power)
         │    ├→ Power Line View — กราฟเส้นกำลังสัญญาณ Top1 cell ตามเส้นทาง
         │    └→ TopN — รายละเอียด cell แรงสุดแต่ละ bin
         │
         ├→ Coverage Map — แผนที่สีแสดง coverage
         │    ├→ เขียว = Good
         │    ├→ เหลือง = Fair
         │    └→ แดง = Bad
         │
         ├→ Coverage Verdict
         │    ├→ Power/Quality Scatter View — กราฟจุด Power vs Quality
         │    ├→ Bin Power Chart View — stacked bar chart
         │    └→ Combined Bin Power/Quality — รวม Power + Quality
         │
         └→ Cell Table — รายการ cell ทั้งหมดที่ตรวจพบ
              ├→ Operator, Cell ID, EARFCN, PCI, Band
              ├→ RSRP, RSRQ, SINR (LTE)
              └→ จำนวน Bins ที่ cell นั้นเป็น Top1
```

!!! tip "วิธีอ่าน Coverage Map"

    - **เขียว (Good)**: สัญญาณแรงพอ ใช้งานได้ดี
    - **เหลือง (Fair)**: สัญญาณพอใช้ อาจมีปัญหาถ้าอยู่ในอาคาร
    - **แดง (Bad)**: สัญญาณอ่อน อาจหลุดสาย/หลุดการเชื่อมต่อ
    - **ไม่มีสี**: ไม่มี coverage เลยในจุดนั้น

### 4.4 CPE — Cell Position Estimation (ประมาณตำแหน่ง Cell Tower)

**หน้าที่**: ประมาณตำแหน่ง (lat/long) ของ cell tower จากข้อมูลที่วัดได้ โดยไม่ต้องมี BTS list ล่วงหน้า

**ข้อกำหนด**:

- ต้องมี scanner + GPS receiver
- ต้อง **เคลื่อนที่** ระหว่างวัด (ขับรถรอบพื้นที่)
- ต้องรอ ~2 นาทีหลังเริ่มวัด (scanner sync PPS pulse กับ GPS)

```
ขั้นตอน CPE:
         │
1. เลือก Use Case "Cell Position Estimation"
         │
2. เลือกโหมด:
         ├→ Manual Estimation: กด "Estimate cell position" เอง
         └→ Live Estimation: ประมาณตำแหน่ง real-time ระหว่างขับ
         │
3. ผลลัพธ์:
         ├→ Position Estimation Status View — สถิติความคืบหน้า
         │    ├→ ● 19.2% (51) — High precision (วงรีเล็ก)
         │    ├→ ● 34.7% (92) — By power (ประมาณจากกำลังสัญญาณ)
         │    └→ ● 46.0% (122) — Low precision (วงรียาว)
         │
         ├→ Estimated Cells Map — แผนที่แสดงตำแหน่งที่ประมาณ
         │    ├→ วงกลม = error ellipse (ความไม่แน่นอน)
         │    ├→ วงเล็ก = high precision
         │    └→ วงใหญ่ = low precision
         │
         └→ Cell List — รายการ cell พร้อม lat/long ที่ประมาณ
              └→ Export เป็น .kml เปิดใน Google Earth ได้
```

**Filter Estimation Progress**:

| ฟิลเตอร์ | ความหมาย |
|----------|---------|
| **By power** | ประมาณจากค่า power เท่านั้น (accuracy ต่ำสุด) |
| **High precision** | error ellipse เล็ก — ตำแหน่งน่าเชื่อถือ |
| **Low precision** | error ellipse ใหญ่ — ตำแหน่งมีความไม่แน่นอนสูง |

!!! tip "เพิ่มความแม่นยำของ CPE"

    - **ขับรอบ cell tower หลายรอบ** — ยิ่งขับรอบมาก ยิ่งได้ error ellipse เล็ก
    - **ขับจากหลายทิศทาง** — ไม่ใช่ขับผ่านทิศเดียว
    - **ใช้ GPS accuracy ดี** — accuracy < 5 m
    - Export เป็น **.kml** แล้วเปิดใน Google Earth เทียบกับตำแหน่งจริง

### 4.5 BSA — Base Station Analysis (วิเคราะห์สถานีฐาน)

**หน้าที่**: วิเคราะห์ประสิทธิภาพของสถานีฐานแต่ละแห่ง (เน้นเปรียบเทียบ cell หลายๆ ตัว)

```
BSA แสดงผล:
         │
         ├→ BSA Map — แผนที่แสดง cell tower พร้อม sector direction
         │
         ├→ BSA TopN Grid Chart — เปรียบเทียบ TopN ของแต่ละ cell
         │
         ├→ BSA Finding View — สรุปปัญหาที่พบ
         │    ├→ Coverage Problem — cell ให้ coverage ไม่ดี
         │    └→ Interference Problem — cell รบกวนกัน
         │
         ├→ BSA Compare View — เปรียบเทียบ cell 2 ตัว
         │
         └→ BSA Status View — สถานะรวมของทุก cell
```

---

## 5. Cell Parameters — ค่าสำคัญที่ใช้วิเคราะห์

### 5.1 Cell Identity (ระบุตัวตน cell)

NESTOR แสดง Cell Identity เป็นรูปแบบ **Global Cell ID** ซึ่งรวมข้อมูลหลายส่วนเข้าด้วยกัน:

```
รูปแบบที่แสดงใน NESTOR:

    LTE 520/03/411/232/148
     │   │  │   │   │   │
     │   │  │   │   │   └── Sector ID (หมายเลข sector ของ eNB)
     │   │  │   │   └────── Cell ID (CI) — ค่ารวมที่ใช้ระบุ cell
     │   │  │   └────────── eNB ID — หมายเลข eNodeB (สถานี)
     │   │  └────────────── MNC — รหัสผู้ให้บริการ (03 = AIS Fixed)
     │   └───────────────── MCC — รหัสประเทศ (520 = ไทย)
     └───────────────────── RAT — เทคโนโลยี (LTE, GSM, UMTS, 5G NR)
```

#### แต่ละค่าคืออะไร — อธิบายทีละตัว

##### Technology (RAT — Radio Access Technology)

**Technology** คือเทคโนโลยีที่ cell tower ใช้ส่งสัญญาณ — แต่ละเทคโนโลยีมีวิธีระบุตัวตน cell ต่างกัน

```
  ปี ~1990       ~2000        ~2010         ~2020
     │            │            │             │
     ▼            ▼            ▼             ▼
  ┌──────┐   ┌──────┐    ┌──────┐     ┌──────────┐
  │ GSM  │   │ UMTS │    │ LTE  │     │ 5G NR    │
  │ (2G) │   │ (3G) │    │ (4G) │     │ (5G)     │
  └──────┘   └──────┘    └──────┘     └──────────┘
   เสียง+SMS   3G data     4G data     5G high-speed
   64 kbps    384 kbps    150 Mbps     1+ Gbps
```

| Technology | ชื่อเต็ม | หน้าที่หลัก | ใช้ในไทย |
|-----------|---------|-----------|---------|
| **GSM** (2G) | Global System for Mobile | เสียง, SMS | ยังใช้ (AIS 900, TRUE 900/1800) |
| **UMTS** (3G) | Universal Mobile Telecom System | 3G data, เสียง | กำลังทยอยปิด |
| **LTE** (4G) | Long Term Evolution | 4G data, VoLTE | หลักของทุก operator |
| **5G NR** | New Radio | 5G high-speed | เริ่มขยาย (n28, n78) |

##### PCI — Physical Cell Identifier

**PCI** คือ "ชื่อเล่น" ของ cell ที่ส่งออกอากาศ — scanner ตรวจจับได้ **ทันที** โดยไม่ต้อง decode อะไร

```
PCI มีค่า 0 ถึง 503 (รวม 504 ค่า)

  คำนวณจาก:  PCI = 3 × SSS + PSS

  โดย PSS = Primary Sync Signal    (0, 1, 2)     → 3 ค่า
      SSS = Secondary Sync Signal  (0 ถึง 167)   → 168 ค่า

  รวม: 3 × 168 = 504 PCI
```

**ทำไม PCI สำคัญ**:

- scanner วัด PCI ได้ **เร็วที่สุด** (ไม่ต้องรอ decode SIB)
- ใช้แยก cell เบื้องต้น — แต่ **PCI ซ้ำได้!** (504 ค่าไม่พอสำหรับทุก cell)
- cell 2 ตัวที่อยู่ใกล้กันและใช้ PCI เดียวกัน → เกิด **PCI collision** → มือถือสับสน

```
ตัวอย่าง PCI ในพื้นที่เดียวกัน:

  Cell A (AIS)     PCI = 346   ← ไม่ซ้ำ → ปกติ
  Cell B (AIS)     PCI = 125   ← ไม่ซ้ำ → ปกติ
  Cell C (TRUE)    PCI = 346   ← ซ้ำกับ Cell A! → PCI collision ถ้าอยู่ใกล้กัน
```

##### MNC — Mobile Network Code (รหัสผู้ให้บริการ)

**MNC** คือรหัส 2 หลักที่ระบุว่า cell เป็นของ **operator ไหน** — ใช้คู่กับ MCC (รหัสประเทศ) เสมอ

```
  MCC = 520 (ไทย) + MNC = ?

  ┌──────────────────────────────────────┐
  │  MNC    Operator                     │
  │  ────────────────────────────────    │
  │   01    AIS (เดิม)                   │
  │   03    AIS (AWN — LTE หลัก)         │
  │   04    TRUE (TrueMove H)            │
  │   05    DTAC (dtac TriNet)           │
  │   15    TOT Mobile                   │
  │   18    DTAC (dtac เดิม)             │
  │   23    NT (National Telecom)        │
  └──────────────────────────────────────┘
```

!!! note "ทำไม AIS มี MNC ทั้ง 01 และ 03"

    - **MNC 01** = AIS เดิม (ใบอนุญาตเก่า สัมปทาน) — ส่วนใหญ่เป็น GSM/UMTS
    - **MNC 03** = AWN (Advanced Wireless Network) — บริษัทลูกของ AIS ที่ถือใบอนุญาต LTE/5G
    - ในทางปฏิบัติ: ถ้าเห็น **520/03** ใน NESTOR = **AIS LTE/5G**, ถ้าเห็น **520/01** = AIS GSM/UMTS เก่า

##### eNB ID — eNodeB Identifier (หมายเลขสถานีฐาน LTE)

**eNB ID** คือหมายเลขของ **ตัวสถานีฐาน LTE** (eNodeB = evolved Node B) — สถานี 1 แห่งมี eNB ID เดียว แต่มีหลาย sector

```
  สถานีฐาน 1 แห่ง (eNB ID = 164)
  ตั้งอยู่ที่: lat 14.xx, long 100.xx

            ╲  Sector 1  ╱
             ╲  (0°)    ╱
              ╲        ╱
               ╲      ╱
                ╲    ╱
                 ╲  ╱
      Sector 3   ╲╱   Sector 2
      (240°)    ╱╲    (120°)
               ╱  ╲
              ╱    ╲
             ╱      ╲
            ╱        ╲

  Sector 1: ECI = 164 × 256 + 1 = 41985, PCI = 344
  Sector 2: ECI = 164 × 256 + 2 = 41986, PCI = 345
  Sector 3: ECI = 164 × 256 + 3 = 41987, PCI = 346

  ← ทั้ง 3 sector มี eNB ID = 164 เหมือนกัน
     แต่ ECI, PCI, และ Azimuth ต่างกัน
```

**ทำไม eNB ID สำคัญ**:

- ใช้ระบุ **ตัวสถานีฐานทางกายภาพ** (เสาจริงในพื้นที่)
- cell หลายตัวที่มี eNB ID เดียวกัน = **อยู่บนเสาต้นเดียวกัน** แค่หันคนละทิศ
- ช่วยตรวจสอบว่า operator ตั้ง cell site ตรงที่ขออนุญาตหรือไม่

!!! tip "eNB ID สำหรับ 5G NR"

    5G NR ใช้ **gNB ID** (gNodeB ID) แทน eNB ID — หลักการเดียวกัน แต่ขนาด bit ต่างกัน

##### CI — Cell Identity / ECI (หมายเลข cell เฉพาะ)

**CI (Cell Identity)** คือหมายเลขที่ **ระบุ cell ได้แน่นอน** — ไม่ซ้ำกันภายใน operator เดียวกัน

```
  ความแตกต่างในแต่ละเทคโนโลยี:

  GSM:  CI = 16-bit (0 ถึง 65,535)
        → cell ทั้งหมดของ operator ต้องอยู่ใน 65,535 ค่า
        → ระบุ cell ได้เมื่อรู้ MCC + MNC + LAC + CI

  UMTS: CI = 28-bit (0 ถึง 268,435,455)
        → มี cell ได้มากกว่า GSM
        → ระบุ cell ได้เมื่อรู้ MCC + MNC + LAC + CI

  LTE:  ECI = 28-bit = eNB ID × 256 + Sector ID
        → ECI ทำหน้าที่เป็น CI ของ LTE
        → ระบุ cell ได้เมื่อรู้ MCC + MNC + ECI
        → แถมแยก eNB ID กับ Sector ID ได้ด้วย
```

**ความต่างระหว่าง PCI กับ CI/ECI**:

| | PCI | CI / ECI |
|---|-----|---------|
| **ได้มาจากไหน** | สแกนจากอากาศได้ทันที | ต้อง decode จาก SIB 1 |
| **ซ้ำได้ไหม** | ซ้ำได้ (มีแค่ 504 ค่า) | ไม่ซ้ำ (ภายใน operator) |
| **ใช้ระบุ cell ได้ไหม** | ไม่แน่นอน (อาจซ้ำ) | แน่นอน 100% |
| **ได้เร็วไหม** | เร็วมาก (< 1 วินาที) | ช้ากว่า (ต้องรอ decode) |
| **ใช้ทำอะไร** | แยก cell คร่าวๆ, handover | ระบุตัวตน, ตรวจสอบใบอนุญาต |

```
ตัวอย่างจริง — scanner เห็น 3 cell ที่มี PCI = 346:

  PCI 346 → decode SIB 1 → ECI = 42124 (AIS, eNB 164, sector 140)  ← cell A
  PCI 346 → decode SIB 1 → ECI = 78901 (TRUE, eNB 308, sector 85)  ← cell B
  PCI 346 → decode SIB 1 → ECI = 55555 (DTAC, eNB 217, sector 11)  ← cell C

  → PCI เดียวกัน แต่เป็นคนละ cell คนละ operator!
  → ต้องดู ECI ถึงจะรู้ตัวตนจริง
```

##### สรุปความสัมพันธ์ทุกค่า

```
                    ┌─────────────────────────────┐
                    │     สถานีฐาน (Cell Site)     │
                    │     eNB ID = 164             │
                    │     ตำแหน่ง: 14.xx, 100.xx   │
                    └──────────┬──────────────────┘
                               │
          ┌────────────────────┼────────────────────┐
          ▼                    ▼                    ▼
   ┌──────────────┐    ┌──────────────┐    ┌──────────────┐
   │  Sector 1    │    │  Sector 2    │    │  Sector 3    │
   │  Azimuth 0°  │    │  Azimuth 120°│    │  Azimuth 240°│
   │              │    │              │    │              │
   │  PCI = 344   │    │  PCI = 345   │    │  PCI = 346   │
   │  ECI = 41985 │    │  ECI = 41986 │    │  ECI = 41987 │
   │  EARFCN=1450 │    │  EARFCN=1450 │    │  EARFCN=1450 │
   │  BW = 20 MHz │    │  BW = 20 MHz │    │  BW = 20 MHz │
   │  Band 3      │    │  Band 3      │    │  Band 3      │
   │  (1800 MHz)  │    │  (1800 MHz)  │    │  (1800 MHz)  │
   └──────────────┘    └──────────────┘    └──────────────┘
          │                    │                    │
          ▼                    ▼                    ▼
    Operator: AIS         Operator: AIS       Operator: AIS
    MCC/MNC: 520/03       MCC/MNC: 520/03     MCC/MNC: 520/03
    TAC: 26676            TAC: 26676          TAC: 26676
```

#### วิธี Identify Cell — ดูค่าอะไรบ้าง ตามลำดับ

เวลาจะ **ระบุตัวตน cell** ให้แน่ชัด ต้องดูค่าหลายตัว **เรียงลำดับ** จากกว้างไปแคบ:

```
ขั้นตอนที่ 1: ใครเป็นเจ้าของ?
─────────────────────────────────────────────────

  MCC / MNC
  520 / 03  →  AIS (AWN)       ← รู้แล้วว่าเป็น cell ของ AIS

  ★ ดูค่านี้ก่อนเสมอ — ถ้า MNC ไม่ตรงกับ operator ที่ขออนุญาต = ผิดปกติ


ขั้นตอนที่ 2: อยู่พื้นที่ไหน?
─────────────────────────────────────────────────

  TAC (LTE/5G) หรือ LAC (GSM/UMTS)
  TAC = 26676  →  กลุ่มพื้นที่ที่ cell อยู่

  ★ cell ของ operator เดียวกัน ในพื้นที่ใกล้กัน → TAC เดียวกัน
    ถ้า TAC แปลก ไม่เคยเห็นในพื้นที่นี้ → อาจเป็น fake cell


ขั้นตอนที่ 3: สถานีฐานไหน?
─────────────────────────────────────────────────

  eNB ID (LTE) หรือ gNB ID (5G NR)
  eNB ID = 164  →  สถานีฐานหมายเลข 164

  ★ eNB ID เดียวกัน = เสาต้นเดียวกัน
    ใช้ตรวจสอบว่าสถานีนี้อยู่ในฐานข้อมูล BTS List หรือไม่


ขั้นตอนที่ 4: sector ไหนของสถานี?
─────────────────────────────────────────────────

  ECI (LTE) = eNB ID × 256 + Sector ID
  ECI = 42124  →  eNB 164, Sector 140

  ★ ECI คือค่า unique ที่ระบุ cell ได้แน่นอน 100%
    ใช้คู่กับ MCC+MNC → ไม่ซ้ำกันทั่วโลก


ขั้นตอนที่ 5: ยืนยันทางกายภาพ
─────────────────────────────────────────────────

  PCI + EARFCN + Bandwidth + Band
  PCI = 346, EARFCN = 1450, BW = 20 MHz, Band 3 (1800 MHz)

  ★ PCI ใช้ยืนยันว่า scanner เห็น cell ตัวเดียวกับที่ decode ได้
    EARFCN/BW/Band ใช้ตรวจว่าส่งสัญญาณตรงกับที่ขออนุญาต
```

##### ตารางสรุป — ค่าไหนตอบคำถามอะไร

| คำถาม | ค่าที่ต้องดู | ตัวอย่าง | หมายเหตุ |
|-------|------------|---------|---------|
| **ของ operator ไหน?** | MCC + MNC | 520/03 = AIS | ดูก่อนเสมอ |
| **อยู่พื้นที่ไหน?** | TAC (LTE) / LAC (GSM) | 26676 | ใช้ตรวจ fake cell |
| **สถานีไหน? (เสาต้นไหน)** | eNB ID | 164 | เทียบกับ BTS List |
| **sector ไหน?** | ECI หรือ Sector ID | 42124 (sector 140) | ระบุ cell ได้แน่นอน |
| **ส่งที่ความถี่อะไร?** | EARFCN + Band | 1450 = Band 3 (1800 MHz) | ตรวจสอบใบอนุญาต |
| **BW เท่าไร?** | Bandwidth | 20 MHz | fake cell มักใช้ BW แคบ |
| **PCI อะไร?** | PCI | 346 | ใช้ยืนยัน + ตรวจ collision |
| **หันทิศไหน?** | Antenna Azimuth | 239° | CPE ประมาณให้ |
| **อยู่ตรงไหน?** | Lat / Long | 14.xx / 100.xx | CPE ประมาณให้ |

##### ตัวอย่างจริง — Identify cell จากหน้าจอ NESTOR

```
สถานการณ์: ขับรถวัดในจังหวัดระยอง เจอ cell LTE น่าสงสัย

ข้อมูลที่ NESTOR แสดง:
┌──────────────────────────────────────────────────┐
│  Global CellId:   LTE 520/03/164/140             │
│  Operator:        AIS          RAT: LTE          │
│  MCC/MNC:         520/03       TAC: 26676        │
│  eNB ID:          164          ECI: 42124        │
│  PCI:             346          Band: LTE 3       │
│  EARFCN:          1450         BW:  5 MHz  ⚠️    │
│  Center Freq:     1830 MHz                       │
└──────────────────────────────────────────────────┘

วิเคราะห์ทีละขั้น:

✓ ขั้น 1: MCC/MNC = 520/03 → AIS (AWN) → operator ไทย ตรง
✓ ขั้น 2: TAC = 26676 → ต้องเทียบกับ TAC อื่นในพื้นที่
                         → ถ้าเป็น TAC ที่ AIS ใช้จริง = ปกติ
                         → ถ้าไม่เคยเห็นในพื้นที่นี้ = น่าสงสัย
✓ ขั้น 3: eNB ID = 164 → ตรวจใน BTS List ว่ามีสถานี 164 ไหม
                         → ถ้าไม่มี = สถานีไม่อยู่ในฐานข้อมูล → น่าสงสัย
✓ ขั้น 4: ECI = 42124 → cell นี้ unique ภายใน AIS → ระบุตัวตนได้
⚠ ขั้น 5: BW = 5 MHz  → cell AIS อื่นบน Band 3 ใช้ 20 MHz
                         → cell นี้ใช้แค่ 5 MHz → ผิดปกติ!
```

##### เทียบกับงาน กสทช. — identify cell เพื่ออะไร

| งาน กสทช. | ค่าที่ focus | เหตุผล |
|----------|------------|--------|
| **ตรวจใบอนุญาต** | MCC/MNC + EARFCN + Band + BW | ตรวจว่า operator ส่งสัญญาณตรงย่านที่ได้รับอนุญาต + BW ไม่เกิน |
| **ตรวจตำแหน่งสถานี** | eNB ID + lat/long (จาก CPE) | เทียบกับตำแหน่งที่จดทะเบียน → ตรงหรือเปล่า |
| **ตรวจ sector** | ECI + Sector ID + Azimuth | ตรวจว่า sector หันทิศตรงกับที่แจ้งไว้ |
| **ตรวจ coverage** | RSRP + RSRQ ต่อ cell | ตรวจว่า operator ให้บริการ coverage ตามเงื่อนไข |
| **จับ fake BTS** | TAC (unique?), BW (แคบผิดปกติ?), Power ramp-up | BSA วิเคราะห์อัตโนมัติ |
| **ตรวจรบกวนข้ามช่อง** | PCI + EARFCN ของ cell ใกล้กัน | PCI collision, same-frequency interference |
| **ตรวจ 5G** | gNB ID + NR-ARFCN + Band (n28/n78) | ตรวจว่า 5G ส่งจริงตามที่แจ้ง |

!!! tip "ค่าที่ต้องจำ 4 ตัว สำหรับ identify cell LTE"

    เวลาจดบันทึก cell ในสนาม ให้จด **อย่างน้อย 4 ค่านี้**:

    ```
    1. MCC/MNC    → ใคร     (520/03 = AIS)
    2. eNB ID     → สถานีไหน (164)
    3. ECI        → cell ไหน (42124)
    4. PCI        → ยืนยัน   (346)
    ```

    มี 4 ค่านี้ = ระบุ cell ได้ 100% ไม่มีซ้ำ

    เพิ่มเติมถ้ามีเวลา:

    ```
    5. EARFCN     → ความถี่   (1450)
    6. Bandwidth  → BW        (20 MHz)
    7. TAC        → พื้นที่    (26676)
    ```

#### MCC / MNC ของประเทศไทย

| MCC | MNC | Operator |
|-----|-----|----------|
| 520 | 01 | AIS (Advanced Info Service) |
| 520 | 03 | AIS (AWN - Advanced Wireless Network) |
| 520 | 04 | TRUE (TrueMove H / Real Future) |
| 520 | 05 | DTAC (dtac TriNet) |
| 520 | 15 | TOT (TOT Mobile) |
| 520 | 18 | DTAC (dtac) |
| 520 | 23 | NT (National Telecom) |

#### Cell Identity แยกตามเทคโนโลยี

**GSM**:

```
MCC / MNC / LAC / CI
520    01    1234   5678
                ▲      ▲
                │      └── Cell Identity (16-bit, 0-65535)
                └── Location Area Code — กลุ่มของ cell ในพื้นที่เดียวกัน
```

**UMTS**:

```
MCC / MNC / LAC / CI
520    01    1234   56789
                        ▲
                        └── Cell Identity (28-bit, ค่ามากกว่า GSM)
```

**LTE** (สำคัญที่สุด — ซับซ้อนกว่า GSM/UMTS):

```
MCC / MNC / TAC / eNB ID / ECI / Sector ID
520    03   26676   164     42124    140
                ▲       ▲       ▲       ▲
                │       │       │       └── Sector = ECI mod 256
                │       │       └────────── E-UTRAN Cell ID (28-bit)
                │       └───────────────── eNodeB ID = ECI ÷ 256
                └───────────────────────── Tracking Area Code
```

!!! note "วิธีคำนวณ ECI ↔ eNB ID"

    ```
    ECI = eNB ID × 256 + Sector ID

    ตัวอย่าง: eNB ID = 164, Sector ID = 140
    ECI = 164 × 256 + 140 = 41,984 + 140 = 42,124

    กลับกัน: ECI = 42,124
    eNB ID  = 42,124 ÷ 256 = 164 (ปัดลง)
    Sector  = 42,124 mod 256 = 140
    ```

    **eNB ID เดียวกัน = สถานีเดียวกัน** — sector ต่างกันคือเสาต้นเดียวกันแต่หันคนละทิศ

**5G NR**:

```
MCC / MNC / TAC / Cell ID / gNB ID
520    01    ????   ?????    ???
                              ▲
                              └── gNodeB ID (คล้าย eNB ID ของ LTE)
```

#### ข้อมูล Cell ที่ NESTOR แสดง (ตัวอย่างจากหน้าจอจริง)

จากการอบรม 5G ระยอง (NESTOR Scenario Workshop):

```
┌─────────────────────────────────────────────────────────┐
│  Global CellId:    LTE 520/03/164/140                   │
│                                                         │
│  Operator(s):      AIS              RAT:    LTE         │
│  Band:             LTE 3 (1800 MHz)                     │
│  MCC / MNC Code(s): 520/03                              │
│  TAC:              26676            ECI:    42124        │
│  EARFCN:           1450                                  │
│  Center Frequency: 1830 MHz                              │
│  Bandwidth:        5 MHz            PCI:    346          │
│                                                         │
│  First measured:   3/12/2024 10:58:40 AM                │
│  Last measured:    3/12/2024 12:05:08 PM                │
└─────────────────────────────────────────────────────────┘
```

**แต่ละค่าหมายความว่าอะไร**:

| ค่า | ความหมาย | ตัวอย่าง | อธิบาย |
|-----|---------|---------|--------|
| **RAT** | Radio Access Technology | LTE | เทคโนโลยีที่ cell ใช้ |
| **MCC** | Mobile Country Code | 520 | รหัสประเทศ (520 = ไทย) |
| **MNC** | Mobile Network Code | 03 | รหัส operator (03 = AIS/AWN) |
| **TAC** | Tracking Area Code | 26676 | กลุ่มพื้นที่ที่ cell อยู่ (LTE ใช้แทน LAC) |
| **ECI** | E-UTRAN Cell Identifier | 42124 | หมายเลข cell เฉพาะ (unique ทั่วโลกเมื่อรวม MCC+MNC) |
| **eNB ID** | eNodeB Identifier | 164 | หมายเลขสถานีฐาน (1 สถานี = หลาย sector) |
| **Sector ID** | Sector Identifier | 140 | หมายเลข sector (ทิศทางของเสา) |
| **PCI** | Physical Cell Identifier | 346 | รหัสทางกายภาพ (0-503) ใช้แยก cell ในอากาศ |
| **EARFCN** | E-UTRA Absolute Radio Freq Channel Number | 1450 | หมายเลขช่องสัญญาณ → คำนวณเป็น frequency ได้ |
| **Center Frequency** | ความถี่กลาง | 1830 MHz | ความถี่จริงที่ cell ส่ง |
| **Bandwidth** | แบนด์วิดท์ | 5 MHz | ความกว้างช่องสัญญาณ (5/10/15/20 MHz) |
| **Band** | LTE Band | LTE 3 (1800 MHz) | ย่านความถี่ตามมาตรฐาน 3GPP |

#### ความสัมพันธ์ระหว่าง eNB ID กับ Sector

```
             eNB ID = 164
         (สถานีฐาน 1 แห่ง)
                │
    ┌───────────┼───────────┐
    ▼           ▼           ▼
Sector 1    Sector 2    Sector 3
ECI=42001   ECI=42002   ECI=42003
PCI=344     PCI=345     PCI=346
Azimuth=0°  Azimuth=120° Azimuth=240°
    │           │           │
    ▼           ▼           ▼
  ╱   ╲       ╱   ╲       ╱   ╲
 ╱ 120°╲     ╱ 120°╲     ╱ 120°╲
╱       ╲   ╱       ╲   ╱       ╲
```

!!! tip "PCI vs ECI — ต่างกันอย่างไร"

    - **PCI (Physical Cell ID)**: ค่า 0-503 ที่ cell ส่งออกอากาศ — scanner ตรวจจับได้ทันทีโดยไม่ต้อง decode SI — แต่ **ซ้ำได้** (cell คนละตัวอาจใช้ PCI เดียวกัน)
    - **ECI (E-UTRAN Cell ID)**: ค่า unique ที่ต้อง decode จาก SIB 1 — ใช้ระบุ cell ได้แน่นอน
    - ในทางปฏิบัติ: PCI ใช้แยก cell คร่าวๆ แต่ ECI ใช้ยืนยันตัวตน

### 5.2 RF Parameters (ค่า RF)

| ค่า | เทคโนโลยี | ความหมาย | ช่วงปกติ |
|-----|-----------|---------|---------|
| **RSRP** | LTE, 5G NR | Reference Signal Received Power — กำลังสัญญาณอ้างอิง | -44 ถึง -140 dBm |
| **RSRQ** | LTE, 5G NR | Reference Signal Received Quality — คุณภาพสัญญาณ | -3 ถึง -19.5 dB |
| **SINR** | LTE, 5G NR | Signal to Interference + Noise Ratio | -23 ถึง +40 dB |
| **RSCP** | UMTS | Received Signal Code Power | -25 ถึง -120 dBm |
| **Ec/No** | UMTS | Energy per chip to Noise ratio | 0 ถึง -24 dB |
| **RxLev** | GSM | Received Signal Level | -47 ถึง -110 dBm |
| **C/I** | GSM | Carrier to Interference ratio | -10 ถึง +40 dB |

### 5.3 System Information ที่ decode ได้

**GSM** (SI = System Information):

| SI | ข้อมูลสำคัญ |
|----|-----------|
| SI 1 | ARFCN ของ cell, frequency hopping |
| SI 2 | Neighbor cell BCCH frequencies |
| SI 3 | LAI (MCC+MNC+LAC), Cell ID |
| SI 4 | CBCH, LAI, cell selection parameters |

**LTE** (SIB = System Information Block):

| SIB | ข้อมูลสำคัญ |
|-----|-----------|
| MIB | DL bandwidth, PHICH config, SFN |
| SIB 1 | PLMN list, TAC, Cell ID, access control |
| SIB 2 | Radio resource config (RACH, paging) |
| SIB 3 | Cell reselection parameters |
| SIB 4 | Intra-frequency neighbor cells |
| SIB 5 | Inter-frequency neighbor info |

**5G NR**:

| SIB | ข้อมูลสำคัญ |
|-----|-----------|
| MIB | SFN, cell barred flag |
| SIB 1 | PLMN, Cell ID, TAC, RAN area code, access control, TDD UL/DL config |
| SIB 2 | Cell re-selection info |

---

## 6. วิธีตั้งค่าสำหรับวัดสนาม (Step-by-Step)

### 6.1 เตรียมอุปกรณ์

```
1.  ต่อ Scanner (TSME/TSME6) เข้า PC ผ่าน Ethernet
         │
2.  ต่อ GPS Receiver (ในตัว TSME หรือ NMEA GPS ภายนอก)
         │
3.  ต่อเสาอากาศ Scanner → ติดบนหลังคารถ
         │
4.  เปิด NESTOR → ตรวจ "Connected Devices"
         │
         ├→ ต้องเห็น: IP, Serial No, Firmware, Device Type
         └→ ถ้าไม่เห็น → ตรวจสาย/driver
```

### 6.2 สร้าง Workspace สำหรับวัด LTE

```
1.  เลือก Scenario: "Cellular Network Analysis"
         │
2.  เลือก "Quickstart"
         │
3.  จากหน้า Available Use Cases:
         │
         ├→ ลาก ACD (Template: Asia) ไปใส่ Active Use Cases
         ├→ ลาก SCN (Template: LTE) ไปใส่ Active Use Cases
         ├→ ลาก COV (Template: LTE) ไปใส่ Active Use Cases
         └→ ลาก CPE (Template: LTE) ไปใส่ Active Use Cases
         │
4.  ตั้งค่า Measurement ของ SCN:
         │
         ├→ กด Measurement charm → เลือก Technology = LTE
         ├→ เลือก "Manually configure measured radio channels"
         ├→ เพิ่ม Band: 1 (2100), 3 (1800), 8 (900), 28 (700)
         └→ หรือเลือก "Automatically configure from ACD" ให้ ACD หาเอง
         │
5.  Save Workspace → ตั้งชื่อ เช่น "NBTC_LTE_DriveTest"
```

### 6.3 เริ่มวัด

```
1.  ตรวจ GPS: Navigation Use Case → GPS Status = "3D Fix"
         │
2.  กดปุ่ม ► เริ่ม Measurement
         │
3.  ขับรถตามเส้นทางที่กำหนด
         │
4.  ระหว่างขับ ดูหน้าจอ:
         ├→ Scanner Expert → TopN: ดู cell ที่แรงสุด
         ├→ Coverage Map → ดูสี coverage ตามเส้นทาง
         └→ CPE Map → ดูตำแหน่ง cell ที่ประมาณได้
         │
5.  จบการวัด → กด ■ หยุด → ไฟล์ .db ถูกบันทึกอัตโนมัติ
         │
         ไฟล์ชื่อ: Measurement_YYYYMMDD_HHMMSS.db
         ที่อยู่: C:\Users\<username>\Documents\NESTOR\Measurements\
```

### 6.4 วิเคราะห์ผลภายหลัง (Data Investigation)

```
1.  เปิด NESTOR → เลือก "Data Investigation"
         │
2.  เลือก Workspace (เดิมที่สร้างไว้ หรือ Quickstart)
         │
3.  ลาก Measurement Files → เลือกไฟล์ .db ที่ต้องการ
         │
4.  เลือก Use Case:
         ├→ COV → ดู Coverage Map + Coverage Verdict
         ├→ CPE → ดู Estimated Cell Map → Export KML
         └→ SCN → ดู TopN + Cell Info
         │
5.  Export ผล:
         ├→ Coverage Map → Screenshot → PDF Report
         ├→ Cell Table → Export → .csv
         ├→ CPE → Commands → Export KML → เปิดใน Google Earth
         └→ TopN Details → More → Export → .csv
```

---

## 7. Export ข้อมูลออก

| วิธี Export | ผลลัพธ์ | ใช้ทำอะไร |
|------------|--------|----------|
| **Export KML** (CPE/APE) | ไฟล์ .kml | เปิดใน Google Earth ดูตำแหน่ง cell |
| **Export Cell Data** (SCN) | ไฟล์ .txt | รายการ cell พร้อมค่า RF |
| **Export TopN** (COV) | ไฟล์ .csv | ตาราง TopN details |
| **Screenshot Report** | ไฟล์ PDF/RTF | จับภาพหน้าจอทำรายงาน |
| **Raw Data Export** | ไฟล์ .csv | ข้อมูลดิบแบ่งตาม time-based cycle |
| **Bin Data Export** | ไฟล์ .csv + .kml | ข้อมูลแบ่งตาม geographical bin |

---

## 8. Import BTS List (ฐานข้อมูลสถานีฐาน)

NESTOR รองรับ import ฐานข้อมูล BTS เพื่อ **เทียบผลวัดกับตำแหน่งจริง** ของสถานีฐาน

**รูปแบบไฟล์**: CSV ตาม NESTOR BTS List format

```
ขั้นตอน Import:

1.  Settings → BTS Lists → Import BTS List
         │
2.  เลือกไฟล์ .csv ที่เตรียมไว้
         │
3.  Map คอลัมน์ (MCC, MNC, LAC, CI, Lat, Long, ...)
         │
4.  BTS จะแสดงเป็นจุดบนแผนที่ทุก Use Case
```

!!! tip "ประโยชน์ของ BTS List"

    - **Coverage Map** แสดงตำแหน่ง BTS จริงเทียบกับ coverage
    - **CPE** เทียบตำแหน่งที่ประมาณกับตำแหน่งจริง → วัดความแม่นยำ
    - **BSA** ระบุ cell ที่มีปัญหาได้ตรงจุด

---

## 9. เคล็ดลับสำหรับงาน กสทช.

!!! tip "การเลือก Band สำหรับประเทศไทย"

    ตั้งค่าให้สแกนเฉพาะ band ที่ใช้ในไทย — ลดเวลาสแกน เพิ่มความเร็ว cycle:

    | Operator | GSM | UMTS | LTE | 5G NR |
    |----------|-----|------|-----|-------|
    | **AIS** (520/01) | 900 | 2100 | 700, 900, 1800, 2100, 2600 | 700, 2600, 3500 |
    | **DTAC** (520/05) | 1800 | 2100 | 700, 900, 2100, 2300 | 700, 2300 |
    | **TRUE** (520/04) | 900, 1800 | 850, 2100 | 700, 900, 1800, 2100, 2600 | 700, 2600, 3500 |

!!! warning "ข้อควรระวัง"

    - **อย่าเปิดสแกนทุก band** — ทำให้ scanner ทำงานช้า cycle time เพิ่ม
    - **บันทึก workspace** ที่ตั้งค่าดีแล้ว — ใช้ซ้ำทุกครั้งที่ออกวัดสนาม
    - **ตรวจ GPS ก่อนวัดทุกครั้ง** — ถ้า GPS ไม่ fix ข้อมูลจะไม่มีตำแหน่ง
    - **ไฟล์ .db ขนาดใหญ่** — วัดนาน 8 ชม. ไฟล์อาจ > 1 GB → ใช้ SSD

---

## 10. Scenario จริงจากการอบรม กสทช. (Workshop 5G ระยอง)

จากเอกสาร R&S "Cellular Network Analysis Workshop" ที่ใช้อบรม กสทช. มี 3 scenario หลักที่ใช้งานจริง:

### 10.1 Scenario 1 — Sector Verification (ตรวจสอบ sector ด้วย CPE)

**วัตถุประสงค์**: ตรวจสอบว่า sector ของ cell site ทำงานถูกต้อง โดยใช้ CPE ประมาณตำแหน่งและทิศทางของ sector

```
ขั้นตอน:

1.  ขับรถวัดรอบพื้นที่เป้าหมาย (1st RUN)
         │
         ├→ NESTOR + CPE ประมาณตำแหน่ง + ทิศทาง sector
         ├→ ดูผล: Error ellipse, Antenna Azimuth, Azimuth Confidence
         └→ จดข้อมูล cell: Technology, PCI, MNC, eNB ID, CI
         │
2.  แจ้ง operator ปิด/ปรับ sector ที่สงสัย
         │
3.  ขับรถวัดรอบเดิมอีกครั้ง (2nd RUN)
         │
4.  เปรียบเทียบ 1st RUN vs 2nd RUN ด้วย Data Investigation
         │
         └→ ถ้า sector ถูกปิดจริง → cell นั้นจะหายไปจากผลวัด
```

**ข้อมูลที่ CPE แสดงสำหรับแต่ละ cell**:

```
┌─────────────────────────────────────────┐
│  Global CellId:  LTE 520/03/411/232/148 │
│  Cell type:      Estimated              │
│  Latitude:       14.xx°                 │
│  Longitude:      100.xx°                │
│                                         │
│  Error ellipse geometry:                │
│    Semi-major axis:  153.2m  ← วงรียาว  │
│    Semi-minor axis:  146.8m  ← วงรีสั้น │
│    Phi (°):          176.6°             │
│                                         │
│  Antenna Azimuth:    239°   ← ทิศเสา   │
│  Azimuth Confidence: +/- 37.5°          │
│  PCI:                168                │
│  Center Frequency:   2145 MHz           │
│  EARFCN:             250                │
│                                         │
│  First measured:  1/31/2024 10:12 AM    │
│  Last measured:   1/31/2024 10:56 AM    │
└─────────────────────────────────────────┘
```

**Position Estimation Status** (ตัวอย่างจาก workshop):

```
  Unidentified Cells: 42.4% (103)
  ├→ ● 55.7% (78)  by power + high precision  ← ดีที่สุด
  └→ ●  6.4% (9)   low precision              ← ข้อมูลน้อย

  Identified Cells: 57.6% (140)
  ├→ ● 37.9% (53)  by power                   ← ประมาณจาก power เท่านั้น
  └→ ● ...         high precision
```

### 10.2 Scenario 2 — RSCP / Coverage Verification (ตรวจสอบ coverage ด้วย COV)

**วัตถุประสงค์**: ตรวจสอบว่า coverage ของ sector เปลี่ยนแปลงหลังปรับกำลังส่ง โดยเปรียบเทียบ RSRP ใน bin เดียวกันก่อน/หลังปรับ

```
ขั้นตอน:

1.  ขับรถวัด Coverage รอบพื้นที่ cell site เป้าหมาย (1st RUN)
         │
         ├→ COV แสดง Coverage Map (สี เขียว/เหลือง/แดง)
         └→ คลิก bin ที่สนใจ → อ่านค่า RSRP ของ cell เป้าหมาย
         │
         เช่น: TRUE (520/03), RSRP = -67.71 dBm, Count = 2
         │
2.  แจ้ง operator ลดกำลังส่งของ sector เป้าหมาย
         │
3.  ขับรถวัดรอบเดิมอีกครั้ง (2nd RUN)
         │
4.  Data Investigation → เปิดทั้ง 2 ไฟล์ → Filter ที่ bin เดิม
         │
         ├→ RSRP เปลี่ยนจาก -67.71 → -72.18 dBm
         └→ ยืนยันว่า operator ลดกำลังจริง (ลดลง ~4.5 dB)
```

**เกณฑ์ Coverage สำหรับ LTE** (ที่ใช้ใน workshop):

| สี | ระดับ | NB-RSRP |
|----|------|---------|
| เขียว | **Good** | > -115 dBm |
| เหลือง | **Fair** | -115 ถึง -125 dBm |
| แดง | **Bad** | < -125 dBm |

### 10.3 Scenario 3 — False Base Station Detection (ตรวจจับสถานีฐานปลอม ด้วย BSA)

**วัตถุประสงค์**: ตรวจจับ **สถานีฐานปลอม (IMSI Catcher / Fake BTS)** ที่ตั้งขึ้นมาเพื่อดักจับข้อมูลโทรศัพท์ โดยใช้ BSA วิเคราะห์ cell ที่ **ผิดปกติ** จากค่ามาตรฐานของ operator

```
ขั้นตอน:

1.  เปิด NESTOR → เลือก BSA (Base Station Analysis)
         │
2.  ขับรถวัดในพื้นที่เป้าหมาย
         │
3.  BSA วิเคราะห์ทุก cell อัตโนมัติ
         │
         ├→ ถ้า cell ปกติ → "Good" (เขียว)
         ├→ ถ้า cell น่าสงสัย → "Check" (เหลือง) ← ตรวจสอบเพิ่ม
         └→ ถ้า cell ผิดปกติชัดเจน → "Bad" (แดง) ← สงสัยว่าเป็น fake
         │
4.  คลิก cell ที่ถูก flag → ดูรายละเอียด criterion ที่ผิดปกติ
```

**เกณฑ์ตรวจจับ (BSA Finding Criteria)** — ตัวอย่างจริงจาก workshop:

| Criterion | ค่าที่ตรวจพบ | Score | ความหมาย |
|-----------|-------------|-------|---------|
| **Bandwidth criterion** | 5 MHz | 0.60 | cell ใช้ BW 5 MHz แต่ช่องเดียวกัน operator ใช้ 20 MHz → **ผิดปกติ** |
| **TAC unique criterion** | 26086 | 0.30 | TAC 26086 ไม่เคยพบใน network ของ operator MCC=520 MNC=03 → **TAC แปลก** |
| **Channel power ramp-up** | -3.01 dB | 0.25 | กำลังสัญญาณพุ่งขึ้น 27.79 dB ทันที → **เหมือนเปิดเครื่องกะทันหัน** |
| **T301 criterion** | 400 ms | 0.10 | T301 timer = 400 ms แต่ปกติ operator ตั้ง 200 ms → **ค่า timer ผิดปกติ** |

!!! warning "วิธีอ่านผล BSA Finding"

    **Score** คือค่าถ่วงน้ำหนักความผิดปกติ (0 ถึง 1) — ยิ่งสูงยิ่งผิดปกติ:

    - **Bandwidth criterion (0.60)**: cell ปลอมมักใช้ bandwidth แคบ (5 MHz) เพราะอุปกรณ์ราคาถูก ในขณะที่ operator จริงใช้ 20 MHz
    - **TAC unique criterion (0.30)**: cell ปลอมมักใช้ TAC ที่ไม่มีอยู่จริงใน network → NESTOR ตรวจพบว่า TAC นี้ "unique" (ไม่เคยเห็นมาก่อน)
    - **Channel power ramp-up (0.25)**: กำลังส่งพุ่งขึ้นทันทีจาก -45.55 dBm → -17.77 dBm — เหมือนเพิ่งเปิดเครื่อง ไม่ใช่ cell ถาวร
    - **T301 criterion (0.10)**: timer T301 ตั้ง 400 ms แต่ operator ปกติใช้ 200 ms — ค่า config ไม่ตรงกับ network profile

```
ตัวอย่าง Cell ที่ BSA flag ว่าน่าสงสัย:

┌─────────────────────────────────────────────────────────┐
│  Cell: 26676/164/140         Last measured: 18 min ago  │
│                                                         │
│  Global CellId:    LTE 520/03/164/140                   │
│  Operator(s):      AIS              RAT: LTE            │
│  Band:             LTE 3 (1800 MHz)                     │
│  MCC / MNC:        520/03           TAC:   26676        │
│  EARFCN:           1450             ECI:   42124        │
│  Center Frequency: 1830 MHz         PCI:   346          │
│  Bandwidth:        5 MHz    ← ผิดปกติ! ปกติ = 20 MHz    │
│                                                         │
│  Actions:  [ Check ]  [ Good ]  [ Bad ]                 │
│                                                         │
│  Findings:                                              │
│  ├→ Bandwidth criterion    5 MHz     [0.60] ← สูงสุด   │
│  ├→ TAC unique criterion   26086     [0.30]             │
│  ├→ Channel power ramp-up  -3.01 dB  [0.25]             │
│  └→ T301 criterion         400 ms    [0.10]             │
└─────────────────────────────────────────────────────────┘
```

!!! tip "การใช้ BSA ตรวจจับ Fake BTS ในงาน กสทช."

    1. **ก่อนออกตรวจ**: import BTS List จาก operator เพื่อให้ BSA รู้ว่า cell ไหนเป็นของจริง
    2. **ระหว่างวัด**: BSA จะ flag cell ที่ไม่อยู่ใน BTS List หรือมีค่าผิดปกติ
    3. **ดู BSA Finding View**: cell ที่มี score รวมสูง → **สงสัยมากที่สุด**
    4. **ยืนยัน**: ตรวจสอบ TAC, Bandwidth, Power pattern ว่าตรงกับ operator จริงหรือไม่
    5. **ประสานงาน**: แจ้ง operator และหน่วยงานที่เกี่ยวข้อง (CyberCop/DSI)

---

## 11. ใช้ NESTOR ตรวจจับ Repeater และ SIM Box

### 11.1 Illegal Repeater (เครื่องขยายสัญญาณเถื่อน)

**Repeater คืออะไร**: อุปกรณ์ที่รับสัญญาณจาก cell tower แล้ว **ขยายแล้วส่งซ้ำ** — ทำให้มีสัญญาณในจุดที่ปกติไม่ถึง (เช่น ในอาคาร ห้องใต้ดิน)

```
  Cell Tower (ต้นทาง)             Repeater (ผิดกฎหมาย)
       │                              │
       │  สัญญาณจริง                    │  สัญญาณซ้ำ (amplified)
       ▼                              ▼
  ┌─────────┐     ขยาย+ส่งซ้ำ     ┌─────────┐
  │  PCI=346 │ ──────────────── │  PCI=346 │  ← PCI เดียวกัน!
  │  ECI=42124│                  │  ECI=42124│  ← ECI เดียวกัน!
  │  f=1830  │                  │  f=1830  │  ← ความถี่เดียวกัน!
  │  BW=20   │                  │  BW=20   │
  │  Power:  │                  │  Power:  │
  │  -80 dBm │                  │  -40 dBm │  ← แรงผิดปกติ!
  │  (ปกติ)  │                  │  (ผิดปกติ)│
  └─────────┘                  └─────────┘
```

**ปัญหาของ Repeater**:

- ส่งซ้ำสัญญาณที่ **PCI/ECI/EARFCN เหมือน cell ต้นทางทุกอย่าง** — ไม่สามารถแยกจาก cell จริงได้จาก identity
- แต่สังเกตได้จาก **พฤติกรรมของสัญญาณ** ที่ผิดปกติ

#### วิธีตรวจจับ Repeater ด้วย NESTOR

##### วิธีที่ 1: Coverage Analysis (COV) — สังเกตสัญญาณแรงผิดที่

```
ขั้นตอน:

1.  ขับรถวัด Coverage ในพื้นที่เป้าหมาย (COV + SCN)
         │
2.  ดู Coverage Map — มองหาจุดที่ "เขียว" แรงผิดปกติ
         │
         ปกติ:  สัญญาณอ่อนลงตามระยะห่างจาก cell tower
         │
         ├→ cell tower อยู่ตรงนี้
         │   -60 ──── -70 ──── -80 ──── -90 ──── -100 dBm
         │   ▲ แรง                               อ่อน ▲
         │
         ผิดปกติ (มี repeater):
         │
         ├→ cell tower อยู่ตรงนี้           repeater อยู่ตรงนี้
         │   -60 ── -70 ── -80 ── -90 ── -40 ── -50 ── -70
         │                            ▲          ▲
         │                            อ่อน       แรงอีกครั้ง!
         │                                      (ผิดปกติ)
         │
3.  คลิก bin ที่แรงผิดปกติ → ดู cell ใน bin นั้น
         │
         ├→ PCI/ECI เดียวกับ cell tower ที่อยู่ไกล → สงสัย repeater
         └→ Power แรงกว่าที่ควรจะเป็นตามระยะทาง
```

##### วิธีที่ 2: CPE — ตำแหน่ง cell ประมาณไม่ตรง

```
ขั้นตอน:

1.  เปิด CPE ระหว่างขับรถวัด
         │
2.  ดู Estimated Cells Map — มองหา:
         │
         ├→ cell เดียวกัน (ECI เดียวกัน) แต่ CPE ประมาณตำแหน่ง
         │  ได้ 2 จุดที่ห่างกัน
         │
         │  ● ตำแหน่งจริงของ cell tower
         │         (estimated position 1)
         │
         │             ● ตำแหน่งของ repeater
         │                (estimated position 2 — ไม่ควรมี)
         │
         ├→ Error ellipse ใหญ่ผิดปกติ
         │  (เพราะสัญญาณมาจาก 2 ทิศ → CPE สับสน)
         │
         └→ Azimuth Confidence ต่ำ
            (ทิศทางสัญญาณไม่สอดคล้อง)
```

##### วิธีที่ 3: SCN + Power Analysis — วิเคราะห์ค่า power ผิดปกติ

```
ขั้นตอน:

1.  เปิด Scanner Expert (SCN) → ดู TopN
         │
2.  สังเกต cell ที่ power ไม่สัมพันธ์กับระยะทาง:
         │
         ├→ ปกติ: cell tower อยู่ไกล 5 km → RSRP ≈ -100 dBm
         │
         └→ ผิดปกติ: cell tower อยู่ไกล 5 km → RSRP ≈ -50 dBm
                     (แรงเกินไป → มี repeater ขยายอยู่ใกล้)
         │
3.  ดู power ของ cell นั้นตลอดเส้นทาง:
         │
         ├→ ปกติ: power ลดลงสม่ำเสมอเมื่อห่างจาก cell
         │
         └→ ผิดปกติ: power ลดลง แล้ว "กระโดด" แรงขึ้นอีกครั้ง
                     → จุดที่กระโดด = ตำแหน่ง repeater
```

!!! warning "สัญญาณเตือนว่ามี Repeater"

    | สิ่งที่สังเกต | อธิบาย |
    |-------------|--------|
    | **RSRP แรงผิดที่** | cell อยู่ไกล 5+ km แต่ RSRP > -60 dBm |
    | **PCI/ECI เหมือน cell ไกล** | สัญญาณแรงแต่ cell identity ชี้ไปที่ cell tower ไกลมาก |
    | **CPE ประมาณตำแหน่งผิด** | cell เดียวกันแต่ CPE เห็นเหมือนมี 2 ตำแหน่ง |
    | **Error ellipse ใหญ่ผิดปกติ** | CPE สับสนเพราะสัญญาณมาจาก 2 ทิศ |
    | **Power กระโดด** | ขับออกจาก cell tower → อ่อนลง → แรงขึ้นอีก (repeater) |
    | **Delay Spread ผิดปกติ** | สัญญาณจาก repeater มี delay เพิ่มจากการ amplify |

### 11.2 SIM Box (กล่องซิม)

**SIM Box คืออะไร**: อุปกรณ์ที่ใส่ SIM card จำนวนมาก (10-100 ซิม) เพื่อ **แปลงสาย VoIP เป็นสายโทรศัพท์มือถือ** — ใช้โกงค่า interconnect หรือหลีกเลี่ยงกฎหมาย

```
  สายจากต่างประเทศ (VoIP ราคาถูก)
         │
         ▼
  ┌──────────────┐
  │   SIM Box    │  ← อุปกรณ์ผิดกฎหมาย
  │              │
  │  ┌───┐ ┌───┐│
  │  │SIM│ │SIM││  ← ใส่ซิมหลายสิบใบ
  │  └───┘ └───┘│
  │  ┌───┐ ┌───┐│
  │  │SIM│ │SIM││
  │  └───┘ └───┘│
  └──────┬───────┘
         │ ส่งสัญญาณเหมือนมือถือปกติ
         ▼
  ┌──────────────┐
  │  Cell Tower  │  ← cell tower เห็น SIM box เป็น "มือถือ" หลายเครื่อง
  └──────────────┘
```

!!! warning "ข้อจำกัดของ NESTOR กับ SIM Box"

    NESTOR เป็นเครื่องมือวัด **ฝั่ง Downlink** (สัญญาณจาก cell tower → มือถือ) ไม่ได้วัด Uplink (มือถือ → cell tower) โดยตรง

    SIM Box ทำตัวเหมือน **มือถือปกติ** → scanner ของ NESTOR **ไม่เห็น SIM Box โดยตรง**

    แต่ NESTOR สามารถช่วยได้ **ทางอ้อม** เมื่อใช้ร่วมกับข้อมูลจาก operator

#### วิธีใช้ NESTOR ช่วยค้นหาตำแหน่ง SIM Box

##### ขั้นที่ 1: ระบุ cell ที่ SIM Box เชื่อมต่อ (ข้อมูลจาก operator)

```
Operator ให้ข้อมูล:
  ├→ SIM Box ใช้ซิม AIS หมายเลข 08x-xxx-xxxx
  ├→ CDR (Call Detail Record) ระบุว่าซิมนี้เชื่อมต่อกับ cell:
  │    ECI = 42124, eNB ID = 164, TAC = 26676
  │    Band 3 (1800 MHz), PCI = 346
  └→ ซิมนี้อยู่กับ cell เดิมตลอด 24 ชม. (ไม่เคย handover)
      → พฤติกรรมผิดปกติ (มือถือจริงจะ handover บ้าง)
```

##### ขั้นที่ 2: ใช้ NESTOR + CPE หาตำแหน่ง cell นั้น

```
ขั้นตอน:

1.  ตั้ง NESTOR: SCN + CPE + COV
         │
2.  ขับรถรอบพื้นที่ของ cell ECI = 42124
         │
3.  CPE ประมาณตำแหน่ง cell → ได้ lat/long ของ eNB ID 164
         │
4.  ดู Coverage Map → รู้ขอบเขต coverage ของ cell นั้น
         │
         → SIM Box ต้องอยู่ภายในพื้นที่ coverage ของ cell นี้
```

##### ขั้นที่ 3: ใช้ COV + Filter จำกัดพื้นที่ค้นหา

```
ขั้นตอน:

1.  Data Investigation → เปิดไฟล์วัด
         │
2.  COV → Coverage Map → Filter เฉพาะ cell ECI = 42124
         │
3.  ดูว่า cell นี้เป็น Best Server ที่ bin ไหนบ้าง
         │
         ├→ bin ที่ cell นี้แรงที่สุด = SIM Box น่าจะอยู่แถวนี้
         │   (เพราะ SIM Box ใช้เสาอากาศภายใน ไม่แรง
         │    → ต้องอยู่ใกล้ cell tower)
         │
         └→ จำกัดพื้นที่ค้นหาจากแผนที่ทั้งเมือง → เหลือพื้นที่เล็กๆ
```

##### ขั้นที่ 4: Narrow down ด้วย CME (Cell Measurement and Evaluation)

```
ขั้นตอน:

1.  ใช้ Use Case CME → ตั้ง Cell of Interest = ECI 42124
         │
2.  ขับรถ (หรือเดิน) ในพื้นที่ที่จำกัดไว้จากขั้นที่ 3
         │
3.  ดู RSRP ของ cell 42124 ตลอดเส้นทาง:
         │
         RSRP (dBm)
         -40 ┤                    ●
         -50 ┤                ●       ●
         -60 ┤            ●               ●
         -70 ┤        ●                       ●
         -80 ┤    ●                               ●
         -90 ┤●                                       ●
             └────────────────────────────────────────────→ เส้นทาง
                                  ▲
                           จุดที่ RSRP แรงสุด
                           = ใกล้ SIM Box มากที่สุด
         │
4.  เดินไปที่จุดที่ RSRP แรงสุด → ค้นหาอาคาร/ห้องในบริเวณนั้น
```

!!! note "ทำไมวิธีนี้ใช้ได้"

    SIM Box ใช้เสาอากาศเล็กๆ ภายในอาคาร → สัญญาณ **Uplink** (จาก SIM Box ไป cell tower) อ่อน → SIM Box ต้องอยู่ในจุดที่ **Downlink แรง** (RSRP สูง) เพื่อให้สื่อสารกับ cell tower ได้

    ดังนั้น: จุดที่ RSRP ของ cell เป้าหมายแรงที่สุด = **ตำแหน่งที่น่าจะอยู่มากที่สุด**

#### เปรียบเทียบ: ตรวจจับ Repeater vs SIM Box

| | Repeater | SIM Box |
|---|---------|---------|
| **NESTOR เห็นโดยตรง?** | ได้ (เห็นสัญญาณที่ repeater ส่งซ้ำ) | ไม่ได้ (SIM Box เป็น UE ไม่ใช่ BTS) |
| **Use Case หลัก** | COV + CPE + SCN | COV + CME (+ ข้อมูลจาก operator) |
| **สัญญาณเตือน** | Power แรงผิดที่, CPE เห็น 2 ตำแหน่ง | ต้องรู้ cell ID จาก CDR ก่อน |
| **หาตำแหน่งได้เอง?** | ได้ (จากจุดที่ power กระโดด) | ต้องมีข้อมูลจาก operator ก่อน |
| **ความยากในการหา** | ปานกลาง | ยาก (ต้องประสานหลายฝ่าย) |
| **NESTOR ช่วยได้แค่ไหน** | ช่วยได้เยอะ — ระบุตำแหน่งได้ค่อนข้างแม่น | ช่วยจำกัดพื้นที่ค้นหา — แต่ต้องค้นหาทางกายภาพเอง |

#### สรุป Workflow ตรวจจับทั้ง 2 แบบ

```
┌───────────────────────────────────────────────────────────┐
│                    ตรวจจับ REPEATER                        │
│                                                           │
│  1. ขับรถวัด COV + CPE + SCN                               │
│                 │                                         │
│  2. สังเกต Coverage Map:                                   │
│     สัญญาณแรงผิดที่?  ──YES──→ 3. ตรวจ PCI/ECI            │
│            │                        │                     │
│           NO                   เหมือน cell ไกล?            │
│            │                   ──YES──→ 4. น่าจะ Repeater  │
│         ปกติ                        │                     │
│                              5. ขับไปจุดนั้น               │
│                              6. หา repeater ทางกายภาพ      │
└───────────────────────────────────────────────────────────┘

┌───────────────────────────────────────────────────────────┐
│                     ตรวจจับ SIM BOX                        │
│                                                           │
│  1. ได้ข้อมูลจาก operator: cell ID ที่ SIM Box เชื่อมต่อ    │
│                 │                                         │
│  2. ใช้ CPE หาตำแหน่ง cell tower                           │
│                 │                                         │
│  3. ใช้ COV ดู coverage area ของ cell นั้น                  │
│                 │                                         │
│  4. ใช้ CME ขับ/เดินในพื้นที่ → หาจุดที่ RSRP แรงสุด        │
│                 │                                         │
│  5. ค้นหาทางกายภาพในอาคารใกล้จุดนั้น                       │
└───────────────────────────────────────────────────────────┘
```

---

!!! abstract "แหล่งอ้างอิง"

    - [R&S NESTOR User Manual V21.2 (1177.5379.02 | Version 28)](../assets/NESTOR_User_Manual_V21_2.pdf)
    - R&S "Cellular Network Analysis Workshop" — เอกสารอบรม 5G ระยอง (NESTOR Scenario Rev01)
