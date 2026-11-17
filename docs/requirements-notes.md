# Tension Sim App – Requirements Notes

Collected step by step from the user's walkthrough of the old Windows app
(INSITE for Well Intervention). Notes only – no design decisions yet.

---

## 0. Old app main screen

Tree menu:
- **Job Setup:** Job/Customer Information, Fluid Manager, IWI Design (with WLI SS), IWI Design (without WLI SS)
- **Displays:** Depth-Based Plotting
- **Reports:** Reports, Attachment Manager
- **Transfer:** Design Exporter
- **Utilities:** Units of Measure, Unit Converter, Well Manager, Cable Editor, WLI Service Selector,
  Tool String Editor, Tool Fit Analyzer, Material Library, Engineering Best Practices, Windows Calculator

---

## 1. Cable

### Cable Selector (library)
- Columns: Cable Name, Cable Type, Diameter (mm), Weight per 1000 ft (kg), Stretch Coefficient, Breaking Strength (lbf)
- Actions: New cable by name (max 50 characters) → Create; Open, Import, Export, Delete
- Filter "Show Only": All Cables, Mono-Conductor, Multi-Conductor, Slickline, Fiber Optic, Opto-Electrical, Custom Cables
- Built-in examples (slickline):

| Cable | Type | Dia mm | Wt/1000 ft kg | Stretch | Break lbf |
|---|---|---|---|---|---|
| Bridon 316 SS (0.082) | Slickline | 2.1 | 8.3 | 7.08 | 1100 |
| Bridon 316 SS (0.092) | Slickline | 2.3 | 10.5 | 5.58 | 1400 |
| Bridon 316 SS (0.108) | Slickline | 2.7 | 14.5 | 4.08 | 1920 |
| Bridon 316 SS (0.125) | Slickline | 3.2 | 19.4 | 3.00 | 2500 |
| Bridon Plain Carbon (0.082) | Slickline | 2.1 | 8.3 | 7.08 | 1240 |
| Bridon Plain Carbon (0.092) | Slickline | 2.3 | 10.5 | 5.58 | 1545 |
| Bridon Plain Carbon (0.108) | Slickline | 2.7 | 14.5 | 4.08 | 2110 |
| Bridon Plain Carbon (0.125) | Slickline | 3.2 | 19.4 | 3.00 | 2830 |
| Bridon Supa 75 (0.082) | Slickline | 2.1 | 8.3 | 7.08 | 1240 |
| Bridon Supa 75 (0.092) | Slickline | 2.3 | 10.5 | 5.58 | 1550 |
| Bridon Supa 75 (0.108) | Slickline | 2.7 | 14.5 | 4.08 | 2030 |
| Bridon Supa 75 (0.125) | Slickline | 3.2 | 19.4 | 3.00 | 2560 |
| Bridon UHT Carbon (0.082) | Slickline | 2.1 | 8.3 | 7.08 | 1610 |

### Cable Editor – user's cable (DEFAULT in new app)
| Field | Value |
|---|---|
| Cable Name | Camesa 7Q49RTZZ-EEHSnew |
| Cable Type | Multi-Conductor |
| Nominal Diameter | 12.4 mm |
| Weight per 1000 ft | 187.3 kg |
| Stretch Coefficient | 0.6 ft/kft/klb |
| Breaking Strength | 30000 lbf |

### Requirements
- Load the Camesa cable as the default when the app opens.
- Select another cable from the library, and set any cable as the new default (remembered).
- Create a new cable (name, type, diameter, weight, stretch, breaking strength).
- Old app mixes units (mm, kg/1000 ft, lbf) – note for unit handling.

---

## 2. Tool String

### Tool String Selector (main page)
- Left filters: Tool Source (Tool Strings / Tools; All Tools), name search,
  Advanced filter min/max on Weight (kg), Length (m), OD (mm), Reset
- List columns: Tool String Name, Tool OD (mm), Tool Length (m), Tool Weight (kg)
- Buttons: WLI Tool Import, Delete, Import, Export, Edit Selected Tool String, Create New Tool String, Exit
- User mainly uses: Edit, Create New, Exit

| # | Tool String | OD mm | Length m | Weight kg |
|---|---|---|---|---|
| 1 | HQC-HWST CTEP | 127.0 | 43.11 | 1113.62 |
| 2 | HQC-SO-JAR-HDIR-HWST-SERPENTINE | 120.7 | 50.71 | 1212.36 |
| 3 | HSFT-II Chambers CTEP | 108.0 | 27.30 | 700.95 |
| 4 | IWI-101-HTC IW JAR-SERPENTINE | 120.7 | 35.66 | 910.65 |
| 5 | IWI-101-HTC SO JAR-SERPENTINE | 120.7 | 33.57 | 872.84 |
| 6 | IWI-301-HSFT JAR CHAMBERS-ATCHF | 108.0 | 26.76 | 659.09 |
| 7 | IWI-401-HSFT2 JAR CHAMBERS-SERP | 108.0 | 30.47 | 802.20 |
| 8 | IWI-401-HSFT2 JAR IW CHAMBERS-SERP | 108.0 | 30.47 | 808.68 |
| 9 | IWI-501-275in-CBL-168x12ftWB | 69.8 | 8.42 | 174.63 |
| 10 | MRIL_PRIME_4875IN | 123.8 | 16.42 | 614.62 |
| 11 | RDT_2MCS_1FPS | 127.0 | 29.49 | 1329.71 |
| 12 | RDT_2MCS_2FPS | 127.0 | 33.17 | 1533.82 |
| 13 | T-15-HWST-JAR-SERPENTINE | 127.0 | 25.02 | 608.93 |
| 14 | T15-HQC-HWST-JAR-SERPENTINE | 127.0 | 45.90 | 1146.11 |

### Tool String Editor (layout)
- Header: Tool String Name, "Originated from WLI", Total Weight (kg), Total Length (m), Max OD (mm)
- Table (top = cable head → bottom): Qty, Name, Tool OD (mm), Tool ID (mm, always 0.0), Tool Length (m), Tool Weight (kg)
- Last row "New – Select Tool..." dropdown adds a tool from the tool library
- Right panel: tool diagram (shows only one rectangle with first tool name – not very useful)
- Export button
- Totals are simple sums (verified on IWI-301: weights sum to 659.07 ≈ 659.09, lengths 26.73 ≈ 26.76)
- Standoffs / wheels / decentralisers are stored as **separate rows** (e.g. "I-Wheel Assembly" 100.6 mm,
  "Hostile Metal Standoff" 95.3 mm, "HSFT-II Standoff" 108.0 mm, "HDSN Bowspring Decentraliser",
  "HSDL Rollover Sleeve", "HSDL_M Pad", "Sleeve Protector"). No explicit friction/contact-type field
  is visible in this editor.
- No weak point rating visible in this editor.

### IWI-101-HTC IW JAR-SERPENTINE (Total 910.65 kg, 35.66 m, Max OD 120.7 mm)
Complete (37 rows; sums check: 910.67 kg vs 910.65 shown, 35.65 m vs 35.66 shown).

| Name | OD mm | ID | Len m | Wt kg |
|---|---|---|---|---|
| HRCH1 | 69.8 | 0 | 1.35 | 27.80 |
| HRCH2 I-Wheel Assembly | 100.6 | 0 | 0.09 | 6.42 |
| HRCH3 | 69.8 | 0 | 0.79 | 16.31 |
| HMCS1 | 69.8 | 0 | 1.19 | 30.00 |
| MFT Jar1 | 85.7 | 0 | 2.95 | 95.25 |
| H4TG-A1 | 69.8 | 0 | 2.19 | 52.42 |
| H4TG-A2 I-Wheel Assembly | 100.6 | 0 | 0.09 | 6.72 |
| H4TG-A3 | 69.8 | 0 | 1.91 | 45.64 |
| FJHA1 | 69.8 | 0 | 0.88 | 22.00 |
| HGNI1 | 69.8 | 0 | 0.58 | 10.66 |
| HGNI2 HDSN Bowspring Decentraliser | 95.3 | 0 | 1.49 | 30.52 |
| HGNI3 | 69.8 | 0 | 0.27 | 4.91 |
| HDSN1 | 69.8 | 0 | 1.91 | 39.01 |
| HSDL1 | 69.8 | 0 | 1.03 | 19.96 |
| HSDL2 HSDL Rollover Sleeve | 95.3 | 0 | 0.47 | 18.77 |
| HSDL3 | 69.8 | 0 | 1.32 | 25.41 |
| HSDL_M1 | 88.9 | 0 | 0.01 | 0.42 |
| HSDL_M2 HSDL Transition Sleeve | 95.3 | 0 | 0.23 | 11.45 |
| HSDL_M3 | 88.9 | 0 | 2.20 | 75.77 |
| HSDL_M4 HSDL_M Pad | 120.7 | 0 | 0.78 | 56.26 |
| HSDL_M5 | 88.9 | 0 | 0.46 | 15.89 |
| HSDL_M6 HSDL Transition Sleeve | 95.3 | 0 | 0.23 | 11.45 |
| HSDL_M7 | 88.9 | 0 | 0.16 | 5.57 |
| FJHA1 | 69.8 | 0 | 0.88 | 22.00 |
| HACRT Instrument1 | 79.4 | 0 | 3.31 | 22.68 |
| HACRT Sonde1 | 79.4 | 0 | 0.02 | 0.73 |
| HACRT Sonde2 Hacrt Hostile Metal Standoff | 108.0 | 0 | 0.30 | 13.96 |
| HACRT Sonde3 | 79.4 | 0 | 0.15 | 4.34 |
| HACRT Sonde4 HACRt Sleeve Protector 3_5 | 88.9 | 0 | 0.05 | 1.80 |
| HACRT Sonde5 | 79.4 | 0 | 5.14 | 151.10 |
| HACRT Sonde6 HACRt Sleeve Protector 3_5 | 88.9 | 0 | 0.05 | 1.80 |
| HACRt Temperature Sub1 | 69.8 | 0 | 0.03 | 0.87 |
| HACRt Temperature Sub2 I-Wheel Assembly | 100.6 | 0 | 0.09 | 7.19 |
| HACRt Temperature Sub3 Hacrt Hostile Metal Standoff | 108.0 | 0 | 0.14 | 6.23 |
| HACRt Temperature Sub4 | 69.8 | 0 | 0.11 | 3.09 |
| HSMHF1 | 69.8 | 0 | 0.67 | 9.98 |
| Sinker Tung 7ft1 | 69.9 | 0 | 2.13 | 36.29 |

### IWI-401-HSFT2 JAR CHAMBERS-SERP (Total 802.2 kg, 30.47 m, Max OD 108 mm)
| Name | OD mm | ID | Len m | Wt kg |
|---|---|---|---|---|
| HRCH1 | 69.8 | 0 | 1.16 | 23.92 |
| HRCH2 Hostile Metal Standoff | 95.3 | 0 | 0.30 | 10.27 |
| HRCH3 | 69.8 | 0 | 0.77 | 15.80 |
| HMCS1 | 69.8 | 0 | 1.19 | 30.00 |
| MFT Jar1 | 85.7 | 0 | 2.95 | 95.25 |
| HPSU-II1 | 79.4 | 0 | 2.31 | 54.43 |
| H4TG-A1 | 69.8 | 0 | 2.16 | 51.69 |
| H4TG-A2 Hostile Metal Standoff | 95.3 | 0 | 0.30 | 11.27 |
| H4TG-A3 | 69.8 | 0 | 1.73 | 41.28 |
| FJHA1 | 69.8 | 0 | 0.88 | 22.00 |
| HSFT-II-E1 | 79.4 | 0 | 2.46 | 67.27 |
| HSFT-II-E2 HSFT-II Standoff | 108.0 | 0 | 0.12 | 7.87 |
| HSFT-II-E3 | 79.4 | 0 | 1.54 | 42.12 |
| HSFT-II1 | 79.4 | 0 | 3.44 | 104.27 |
| HSFT-II2 HSFT-II Standoff | 108.0 | 0 | 0.12 | 8.24 |
| HSFT-II3 | 79.4 | 0 | 2.64 | 80.17 |
| HSFT-II4 HSFT-II Standoff | 108.0 | 0 | 0.12 | 8.24 |
| HSFT-II5 | 79.4 | 0 | 0.73 | 22.25 |
| HSFT UPR CHMBR-HUSC1 | 79.4 | 0 | 2.16 | 38.56 |
| HSFT LWR CHMBR-HLSC1 | 79.4 | 0 | 2.14 | 38.56 |
| HSFT J-Latch1 | 79.4 | 0 | 0.08 | 2.54 |
| Hostile Cabbage Head Steel1 | 108.0 | 0 | 0.27 | 10.80 |
| Thermometer Sub1 | 69.8 | 0 | 0.21 | 5.44 |
| HSMHF1 | 69.8 | 0 | 0.67 | 9.98 |

### IWI-401-HSFT2 JAR IW CHAMBERS-SERP (Total 808.68 kg, 30.47 m, Max OD 108 mm)
| Name | OD mm | ID | Len m | Wt kg |
|---|---|---|---|---|
| HRCH1 | 69.8 | 0 | 1.28 | 26.35 |
| HRCH2 I-Wheel Assembly | 100.6 | 0 | 0.09 | 6.42 |
| HRCH3 | 69.8 | 0 | 0.86 | 17.76 |
| HMCS1 | 69.8 | 0 | 1.19 | 30.00 |
| MFT Jar1 | 85.7 | 0 | 2.95 | 95.25 |
| HPSU-II1 | 79.4 | 0 | 2.31 | 54.43 |
| H4TG-B1 | 79.4 | 0 | 2.50 | 59.69 |
| H4TG-B2 I-Wheel Assembly | 100.6 | 0 | 0.09 | 6.72 |
| H4TG-B3 | 79.4 | 0 | 1.61 | 38.36 |
| FJHA1 | 69.8 | 0 | 0.88 | 22.00 |
| HSFT-II-E1 | 79.4 | 0 | 2.46 | 67.27 |
| HSFT-II-E2 HSFT-II Standoff | 108.0 | 0 | 0.12 | 7.87 |
| HSFT-II-E3 | 79.4 | 0 | 1.54 | 42.12 |
| HSFT-II1 | 79.4 | 0 | 3.44 | 104.27 |
| HSFT-II2 HSFT-II Standoff | 108.0 | 0 | 0.12 | 8.24 |
| HSFT-II3 I-Wheel Assembly | 100.6 | 0 | 0.00 | 0.31 |
| HSFT-II4 | 79.4 | 0 | 2.64 | 80.05 |
| HSFT-II5 HSFT-II Standoff | 108.0 | 0 | 0.12 | 8.24 |
| HSFT-II6 I-Wheel Assembly | 100.6 | 0 | 0.01 | 1.07 |
| HSFT-II7 | 79.4 | 0 | 0.72 | 21.84 |
| HSFT UPR CHMBR-HUSC1 | 79.4 | 0 | 2.16 | 38.56 |
| HSFT LWR CHMBR-HLSC1 | 79.4 | 0 | 2.14 | 38.56 |
| HSFT J-Latch1 | 79.4 | 0 | 0.01 | 0.28 |
| HSFT J-Latch1 I-Wheel Assembly | 100.6 | 0 | 0.07 | 5.61 |
| Hostile Cabbage Head Steel1 I-Wheel Assembly | 100.6 | 0 | 0.02 | 2.14 |
| Hostile Cabbage Head Steel2 | 108.0 | 0 | 0.24 | 9.84 |
| Thermometer Sub1 | 69.8 | 0 | 0.21 | 5.44 |
| HSMHF1 | 69.8 | 0 | 0.67 | 9.98 |

### IWI-301-HSFT JAR CHAMBERS-ATCHF (Total 659.09 kg, 26.76 m, Max OD 108 mm)
| Name | OD mm | ID | Len m | Wt kg |
|---|---|---|---|---|
| HRCH1 | 69.8 | 0 | 1.16 | 23.92 |
| HRCH2 Hostile Metal Standoff | 95.3 | 0 | 0.30 | 10.27 |
| HRCH3 | 69.8 | 0 | 0.77 | 15.80 |
| HMCS1 | 69.8 | 0 | 1.19 | 30.00 |
| MFT Jar1 | 85.7 | 0 | 2.95 | 95.25 |
| HPSU1 | 69.8 | 0 | 2.17 | 54.43 |
| H4TG-B1 | 79.4 | 0 | 1.86 | 44.53 |
| H4TG-B2 Hostile Metal Standoff | 95.3 | 0 | 0.30 | 11.27 |
| H4TG-B3 | 79.4 | 0 | 2.03 | 48.43 |
| FJHA1 | 69.8 | 0 | 0.88 | 22.00 |
| HSFT Electronics-HFTE1 | 69.8 | 0 | 1.13 | 22.36 |
| HSFT Electronics-HFTE2 Hostile Metal Standoff | 95.3 | 0 | 0.30 | 10.00 |
| HSFT Electronics-HFTE3 | 69.8 | 0 | 2.06 | 40.57 |
| HSFT Hydraulics Sub-HFTH1 | 79.4 | 0 | 2.96 | 87.13 |
| HSFT Hydraulics Sub-HFTH2 HSFT Standoff | 108.0 | 0 | 0.12 | 8.12 |
| HSFT Pad Std-HFTP1 | 79.4 | 0 | 0.78 | 18.10 |
| HSFT Pad Std-HFTP2 Hostile Strain Gauge | 85.7 | 0 | 0.00 | 0.01 |
| HSFT Pad Std-HFTP3 | 79.4 | 0 | 1.06 | 24.43 |
| HSFT Pad Std-HFTP4 HSFT Standoff | 108.0 | 0 | 0.12 | 7.35 |
| HSFT UPR CHMBR-HUSC1 | 79.4 | 0 | 2.16 | 38.56 |
| HSFT LWR CHMBR-HLSC1 | 79.4 | 0 | 2.14 | 38.56 |
| HSFT J-Latch1 | 79.4 | 0 | 0.08 | 2.54 |
| Thermometer Sub1 | 69.8 | 0 | 0.21 | 5.44 |

### T15-HQC-HWST-JAR-SERPENTINE (Total 1146.11 kg, 45.9 m, Max OD 127 mm)
| Name | OD mm | ID | Len m | Wt kg |
|---|---|---|---|---|
| HRCH1 | 69.8 | 0 | 1.04 | 21.35 |
| HRCH2 Hostile Metal Standoff | 95.3 | 0 | 0.30 | 10.27 |
| HRCH3 | 69.8 | 0 | 0.89 | 18.37 |
| HMCS1 | 69.8 | 0 | 1.19 | 30.00 |
| MFT Jar1 | 85.7 | 0 | 2.95 | 95.25 |
| H4TG-B1 | 79.4 | 0 | 2.20 | 52.48 |
| H4TG-B2 Hostile Metal Standoff | 95.3 | 0 | 0.30 | 11.27 |
| H4TG-B3 | 79.4 | 0 | 1.70 | 40.49 |
| FJHA1 | 69.8 | 0 | 0.88 | 22.00 |
| HGNI1 | 69.8 | 0 | 0.58 | 10.66 |
| HGNI2 HDSN Bowspring Decentraliser | 95.3 | 0 | 1.49 | 30.52 |
| HGNI3 | 69.8 | 0 | 0.27 | 4.91 |
| HDSN1 | 69.8 | 0 | 1.91 | 39.01 |
| HSDL1 | 69.8 | 0 | 1.03 | 19.96 |
| HSDL2 HSDL Rollover Sleeve | 95.3 | 0 | 0.47 | 18.77 |
| HSDL3 | 69.8 | 0 | 1.32 | 25.41 |
| HSDL_M1 | 88.9 | 0 | 0.01 | 0.42 |
| HSDL_M2 HSDL Transition Sleeve | 95.3 | 0 | 0.23 | 11.45 |
| HSDL_M3 | 88.9 | 0 | 2.20 | 75.77 |
| HSDL_M4 HSDL_M Pad | 120.7 | 0 | 0.78 | 56.26 |
| HSDL_M5 | 88.9 | 0 | 0.46 | 15.89 |
| HSDL_M6 HSDL Transition Sleeve | 95.3 | 0 | 0.23 | 11.45 |
| HSDL_M7 | 88.9 | 0 | 0.16 | 5.57 |
| FJHA1 | 69.8 | 0 | 0.88 | 22.00 |
| HWST1 | 79.2 | 0 | 0.92 | 19.88 |
| HWST2 HFWS Caged Standoff | 127.0 | 0 | 0.41 | 12.33 |
| HWST3 | 79.2 | 0 | 5.09 | 109.70 |
| HWST4 HFWS Caged Standoff | 127.0 | 0 | 0.41 | 12.33 |
| HWST5 | 79.2 | 0 | 4.70 | 101.31 |
| HWST6 HFWS Caged Standoff | 127.0 | 0 | 0.41 | 12.33 |
| HWST7 | 79.2 | 0 | 0.59 | 12.80 |
| HACRT Instrument1 | 79.4 | 0 | 3.31 | 22.68 |
| HACRT Sonde1 | 79.4 | 0 | 0.42 | 12.50 |
| HACRT Sonde2 Hacrt Hostile Metal Standoff | 108.0 | 0 | 0.30 | 13.96 |
| HACRT Sonde3 | 79.4 | 0 | 4.67 | 137.39 |
| HACRT Sonde4 Hacrt Hostile Metal Standoff | 108.0 | 0 | 0.30 | 13.96 |
| Thermometer Sub1 | 69.8 | 0 | 0.21 | 5.44 |
| HSMHF1 | 69.8 | 0 | 0.67 | 9.98 |

### T-15-HWST- JAR-SERPENTINE (Total 608.93 kg, 25.02 m, Max OD 127 mm)
| Name | OD mm | ID | Len m | Wt kg |
|---|---|---|---|---|
| HRCH1 | 69.8 | 0 | 1.04 | 21.35 |
| HRCH2 Hostile Metal Standoff | 95.3 | 0 | 0.30 | 10.27 |
| HRCH3 | 69.8 | 0 | 0.89 | 18.37 |
| HMCS1 | 69.8 | 0 | 1.19 | 30.00 |
| MFT Jar1 | 85.7 | 0 | 2.95 | 95.25 |
| H4TG-B1 | 79.4 | 0 | 2.11 | 50.50 |
| H4TG-B2 Hostile Metal Standoff | 95.3 | 0 | 0.30 | 11.27 |
| H4TG-B3 | 79.4 | 0 | 1.78 | 42.46 |
| FJHA1 | 69.8 | 0 | 0.88 | 22.00 |
| HWST1 | 79.2 | 0 | 0.53 | 11.49 |
| HWST2 HFWS Caged Standoff | 127.0 | 0 | 0.41 | 12.33 |
| HWST3 | 79.2 | 0 | 5.39 | 116.16 |
| HWST4 HFWS Caged Standoff | 127.0 | 0 | 0.41 | 12.33 |
| HWST5 | 79.2 | 0 | 4.16 | 89.57 |
| HWST6 HFWS Caged Standoff | 127.0 | 0 | 0.41 | 12.33 |
| HWST7 | 79.2 | 0 | 1.23 | 26.47 |
| Hostile Terminator Jlatch1 | 69.8 | 0 | 0.09 | 0.91 |
| Hostile Steel Cabbage Head 5.5In1 | 127.0 | 0 | 0.28 | 15.88 |
| HSMHF1 | 69.8 | 0 | 0.67 | 9.98 |

### Answers from user (tool string)
- **Friction / contact type:** there is no friction setting for wheels or standoffs. The user just adds the
  wheel/standoff rows to the tool string and the old app works out the friction automatically.
  → New app must do the same: detect wheel / standoff / decentraliser rows (by tool type in the library,
  or by name keywords such as "Wheel", "Standoff", "Decentraliser") and apply a built-in friction value.
  The user should not need to enter friction per tool.
- **Weak point rating:** entered on the last calculation page (IWI Design), not in the tool string editor.
- IWI-101 rows: resolved – full list received.
- **Strings in use:** only the 6 detailed strings above. The other 8 are not used now, but the new app
  must be able to add new tool strings later.
- **Where strings come from:** the user builds logging tool strings in another app/section (WLI –
  "Originated from WLI", "WLI Tool Import" button). They appear here automatically. The user never
  edits strings in this editor.
  → New app needs an easy **import** of tool strings from that source, plus manual create/edit as backup.
  → Open question: what file format can the other app export (Excel, CSV, XML, other)?

---

## 3. Fluid (Fluid Manager)

### Fluid Manager list
- Toolbar: save, cut/copy/paste, help, move up/down, colour; Material Selection Filters: "Material Library", "Well Intervention"
- Columns: Fluid Name, Fluid Description, Fluid Color, Properties (Edit...), Proppant (Select...),
  Absolute Volume Factor (L/kg), Proppant Concentration (kg/m³) – proppant columns empty / not used
- Last row "New – Select..." adds a fluid

| # | Fluid Name | Description | Color |
|---|---|---|---|
| 1 | 1.48 SG | 1.48 SG | DeepSkyBlue |
| 2 | 1.65 SG | 1.65 SG | Blue |
| 3 | 1.75 SG | 1.75 SG | Yellow |
| 4 | 1.49 SG | 1.49 SG | Purple |
| 5 | 1.56 SG | 1.56 SG | White |
| 6 | 1.13 sg | 1.13 sg | Olive |
| 7 | 1.20 SG | 1.20 SG | DarkCyan |
| 8 | 1.18 SG | 1.18 SG | PaleGreen |
| 9 | 1.28 SG | 1.28 SG | LightBlue |
| 10 | 1.3 SG | 1.3 SG | DarkGreen |

### Fluid properties dialog ("Fann Fluid"), example 1.48 SG
| Field | Value |
|---|---|
| Name | 1.48 SG |
| Fluid Type | Mud (options: Mud, Cement, Other) |
| Reference Temperature | 25.0 °C |
| PVT Model – Density | 1480.000 kg/m³ |
| Compressible | unchecked (if checked: Mud Base Fluid, Oil-Water Ratio %, Salt kg/sack, Average Solids Gravity) |
| Rheology Model | Bingham Plastic |
| Cement Classification | Other (disabled) |
| Rheology Parameters | Enter (option: Calculate) |
| PV | 30.000 mPa·s |
| YP | 16.000 lbf/100ft² |

### Requirements
- User creates muds here; names them by density in SG.
- Density entered in **SG by default**, but editable in **ppg** too (1 SG = 8.345 ppg = 1000 kg/m³).
- Proppant / cement / compressible options not used.
- **PV / YP:** user always uses the default **PV 30, YP 16** (not a mud specialist).
  → New app: default PV 30 / YP 16 pre-filled, can be changed if a mud report is available.
  Only Density, PV and YP are needed for the calculation.

### Example mud report (field data, for reference)
"Well TD Mud Properties" – the source of the values. Only Density / PV / YP are used by the sim.

| Field | Value |
|---|---|
| Date | 3-Aug-26 |
| Well | BK-32-N_R |
| Time | 7:00 AM |
| Mud Type | CARBO DRILL SDF |
| **Density, SG** | **1.26** |
| Funnel Viscosity | 65 s/qt |
| **Plastic Viscosity** | **30** |
| **Yield Point** | **17** |
| Gels | 11/15/17 |
| Electrical Stability | 982 V |
| Oil/Water ratio | 82 / 18 |
| Cl- Whole Mud | 34,000 mg/L |
| CaCl2 (%wt) | 25.20 |
| Chlorides in whole mud / water phase | 34,000 / 197,727 ppm |
| Corrected Solids | 12.80 % vol |
| Barite | 6.94 % vol |
| Fluid loss (175C) | 1.8 cc |
| Circulation at btm stopped | 2:00 AM |
| Brine Type / Oil Type | CaCl2 / Saraline 185V |

- Idea: mud entry form shows only Density (SG/ppg), PV, YP – the three numbers to copy from the mud report.

---

## 4. Well (Well Manager)

### Well Manager main window
- Left: list of **Wells** (currently only "Current well")
- Bottom buttons: Delete, Copy..., Export..., Import..., Verify...
- Toolbar: print, new, open/save, import/export
- Right "Editors and Viewers" buttons:
  - Well Information
  - Wellbore Geometry
  - Directional Data
  - Friction Profile Editor
  - Temperature Profile
  - Reservoir Data
  - 3D Wellbore Graph

### How the user uses it
1. **Copy** an existing well to keep the same data (wellbore, directional data, etc.) as a starting point.
2. Update: **Wellbore Geometry**, **Directional Data**, **Friction Profile**, **Temperature Profile**.
3. Close Well Manager.
- **3D Wellbore Graph:** opens a 3D view of the well – can rotate and zoom.
- Not mentioned as used: Well Information, Reservoir Data, Verify (to confirm later).

### Requirements
- Well list with Copy (duplicate a well as a template), plus new/delete/import/export.
- Editors for wellbore geometry, directional survey, friction profile, temperature profile.
- Interactive 3D well view (rotate, zoom).
- (Details of each editor to follow.)

### 4a. Wellbore Geometry
Header options (example "Current well"):
- Deviated Well: ☑ ; Enable Perforation Friction Calculations: ☐
- Offshore: ☑ – Water Depth 76.6 m, Air Gap 39.6 m, Sea Water Density 1031 kg/m³
- Displace To: Bottom of Perfs; Crossover / MPT fields disabled; Column Filter: Required Data
- Volume to Bottom of Perfs: 75.98 m³ (Details...), status "No Errors"
- Right side: diameter-vs-depth diagram ("Fluid Flow Path")

Section table columns: Type, Top MD (m), Bottom MD (m), Length (m), Description, OD (mm), Weight (kg/m), Grade, ID (mm), Data Source

Example rows:
| Type | Top MD | Bottom MD | Length | Description | OD mm | Wt kg/m | Grade | ID mm | Source |
|---|---|---|---|---|---|---|---|---|---|
| Casing | 0.0 | 1687.0 | 1687.0 | 177.8 mm Casing | 177.8 | 38.692 | L-80 | 159.4 | Redbook |
| Open Hole | 1687.0 | 3913.0 | 2226.0 | 155.6 mm Open Hole | | | | 155.6 | |

Type dropdown options: Open Hole, Casing, Liner, Drill Pipe, Tubing, Surface Lines (CT), Surface Lines (WH),
Packer, Blank, Screen, Sump Packer, Bridge Plug, Perforations, Treatment Interval, Swellable Packer, Formation.

**User only uses Casing + Open Hole.** Two standard well types:
1. **6-1/8" section:** 7" casing (177.8 mm OD, 38.692 kg/m = 26 lb/ft, L-80, ID 159.4 mm) + 6-1/8" open hole (155.6 mm)
2. **8.5" section:** 9-5/8" casing (244.5 mm OD, 47 lb/ft = 69.94 kg/m, L-80, ID 8.681" = 220.5 mm,
   drift 8.525" = 216.5 mm) + 8.5" open hole (215.9 mm)

Requirements idea:
- Simple geometry editor: just Casing (shoe depth, size) + Open Hole (TD, bit size).
- Two templates: "6-1/8 section" and "8.5 section" – pick one, enter casing shoe depth and TD.
- Casing ID from a built-in casing table (size + weight → ID).
- Offshore fields (water depth, air gap) kept for depth reference.

### 4b. Directional Data
Main window ("Directional Data – Current well"):
- Deviated Well ☑, row count (142 rows), display mode Dev. / TVD / Build, Azimuth ☑
- Wellbore selector "Main Wellbore", Add / Edit / Delete Lateral (laterals not used)
- Table columns: Length (m), MD (m), Dev. (°), TVD (m), Build (°/100 m), Azimuth (°), DLS (°/100 m)
- Footer: Max values (Length 30.0, MD 3914.0, Dev 41.9, TVD 3102.3, Build 17.81, DLS 17.80),
  Departure 1825.5 m, Wellbore Geometry Bottom MD 3913.0 m

Import Directional Survey dialog:
- File Name + Browse; ☑ Keep this file as an attachment in the database
- Start / End Import at Row
- Measured Depth Column (1) + unit dropdown, Inclination Column (2), ☑ Azimuth Column (3)
- Reducing Filter: ● No Filter / ○ Blocking Algorithm (max blocks 20), Smoothing Factor 1
- Preview grid (Row #, Col. 1 … Col. 7)

User workflow: open Directional Data → Import → pick text file → set rows/columns → OK.

### Sample survey file – `docs/samples/BK-53-V14_Rev2.0_M_Well_plan.txt` (full file, stored at user request)
- Tab-separated text; row 1 = column names, row 2 = units, then 142 data rows
- Columns: MD (m), Inc (°), Az (°), CL (m), TVD (m), TVD (MSL) (m), N.Offset, E.Offset, VS, Northing,
  Easting, Latitude, Longitude, DLS (°/10 m)
- Only MD, Inc, Az are needed (columns 1–3); the rest can be used to check our calculation.
- TD 3913.99 m MD, TVD 3102.3 m, max Inc 41.9°, max DLS 1.78°/10 m (= 17.8°/100 m)
- TVD (MSL) = TVD − 30.3 m → RKB 30.3 m above MSL in this plan
  (Wellbore Geometry showed Air Gap 39.6 m – user: air gap is not kept up to date because it is not
  used in the calculation. → New app: offshore water depth / air gap optional, not needed for tension.)
- **Verified:** minimum curvature from MD/Inc/Az reproduces the file
  (TVD 3102.32 vs 3102.3; max TVD difference 0.03 m; departure 1825.5 m = old app).

Requirements idea:
- Open / drag-and-drop the survey text file; auto-detect header rows and MD/Inc/Az columns
  (manual column choice still possible); instant preview and 2D/3D plot.
- Handle DLS units °/10 m, °/30 m, °/100 ft, °/100 m.
- Blocking/smoothing filter not needed – the new app is fast enough for all rows.

### 4c. Friction Profile Editor
- Friction Profile Name: "Current well"
- Toolbar includes **"Default Friction Coefficients..."** button
- Table columns: Top MD (m), Bottom MD (m), Top TVD (m), Bottom TVD (m), RIH Friction Coefficient,
  POOH Friction Coefficient (TVD columns calculated from survey – shown in blue)
- "Align with Well Geometry" button – makes intervals match casing / open hole sections
- Right side: wellbore schematic (radius vs MD), options Full schematic / Effective ID only

Example (= one row per well section):
| Section | Top MD | Bottom MD | Top TVD | Bottom TVD | RIH µ | POOH µ |
|---|---|---|---|---|---|---|
| Casing | 0.0 | 1687.0 | 0.0 | 1333.7 | 0.30 | 0.25 |
| Open hole | 1687.0 | 3913.0 | 1333.7 | 3101.5 | 0.40 | 0.30 |

Notes:
- Friction is set **per section, separately for RIH and POOH**.
- Wheels / standoffs in the tool string adjust friction automatically (see tool string answers).
- New app idea: friction rows created automatically from the well geometry (casing / open hole), with
  default values pre-filled; user can edit them.
- **User answer:** always the same values for every well → defaults in new app:
  casing RIH 0.30 / POOH 0.25, open hole RIH 0.40 / POOH 0.30. Must still be changeable.
- User does **not** like editing friction in this editor – prefers to edit it on the **last page
  (simulation summary)**, to be shown later. → New app: friction editable on the summary/results page,
  with instant recalculation. ("Default Friction Coefficients..." button not used.)

### 4d. Temperature Profile
User only opens this page because the tension sim gives an **error if temperature is not updated**.

Inputs (°C):
| Field | Value | At depth |
|---|---|---|
| Ambient | 25 | |
| Injected Fluid | 25 | |
| Proppant | 25 | |
| Sea Water | 25 | 39.64 m MD (39 m TVD) |
| Earth, Mudline | 30 | 116.28 m MD (116 m TVD) |
| BH Static | 193 | 3913 m MD (3101 m TVD) |
| Gradient | 54.15 mK/m (calculated) | |

- ☐ Enter Full Temperature Profile; ☐ Specify Pre-Job Wellbore Temperature (disabled)
- Table: MD (m), TVD (m), Undisturbed Temperature (°C):
  30.38 / 30.38 / 25 ; 90.21 / 90.21 / 30 ; 2383.00 / 1895.88 / 173
- Right side: plot of undisturbed temperature vs MD (straight line ~25 °C at surface → 193 °C at TD)

Effect on tension (explained to user): small. Mainly through mud properties (density / PV / YP change
with temperature – but "Compressible" is off in Fluid Manager, so density is probably constant) and
cable thermal stretch (affects depth, not tension). Suggested check: run the old app twice with
different BHT (e.g. 100 vs 193 °C) and compare tension.
**TODO (user, later):** run same job with BHT 193 °C and 100 °C in the old app, send both max tensions.

Requirements idea:
- Temperature optional: just enter BHT (+ surface temp default 25 °C) – never block the simulation.
- Use BHT for warnings (tool / cable temperature rating) rather than tension.

### 4e. 3D Wellbore Graph
- Window with Reset (View), Refresh, Properties..., Zoom dropdown + slider; 3D box with well path (yellow)
  and its projections on the walls (white lines); rotate / zoom.
- **Not needed for single-well logging jobs.** Needed only for the **platform tension sim report**
  (all wells on a platform), where a 3D picture is pasted on each well's slide.

---

## 5. Platform tension sim report (PowerPoint, made by user)
One slide per well. Example well **1_AT-43-A(05)**:

Header table:
| No. | Well Name | 7" CSG MD (m) | 7" CSG TVD (m) | TD MD (m) | TD TVD (m) | MW (sg) | Friction CH | Friction OH | WP (lbs) | Max Tension (lbs) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | 1_AT-43-A(05) | 2672.02 | 2201.16 | 3697.69 | 3194.16 | 1.79 | 0.30/0.30 | 0.30/0.30 | 9000 | 6829 |
(CH = cased hole, OH = open hole, friction shown as RIH/POOH; WP = weak point)

Plots on the slide:
1. **"Run (1_AT-43-A(05)-HTC_1) – Weight [Trip In/Out]"**: depth (m, 0–4000) vs surface weight (lbf, 0–15000+)
   - Blue = RIH tension at logging-down speed
   - Green = POOH tension at normal logging-up speed (max ≈ 6829 lbf at TD)
   - Red = surface tension when cable is estimated to enter compression
   - Magenta = maximum pulling tension (50%) – vertical line ≈ 16,000 lbf
2. **Inclination + DLS×10 (deg/30 m)** vs depth (m)
3. **3D wellbore picture** (screenshot from 3D Wellbore Graph)
- Remark/legend box explaining the colours (text above).

Requirements:
- **Multi-well / platform mode:** run the sim for many wells, then generate a report automatically:
  summary table of all wells + one page per well (same layout as above).
- Export to **PowerPoint** (and PDF).
- 3D well view rendered automatically for the report.
- **Friction for platform sims: 0.30/0.30** for both CH and OH (RIH/POOH). This is different from
  single logging jobs (CH 0.30/0.25, OH 0.40/0.30). → Two friction presets: "Logging job" and "Platform".
- Platform report size: **4 to 30 wells**.
- **Priority:** build platform mode **after** the main single-well tension sim is complete (phase 2).
