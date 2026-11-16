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
Complete (37 rows; sums check: 910.64 kg, 35.66 m).

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

### Open questions (tool string)
- Where does the old app set friction / contact type (wheel vs standoff vs sliding)? Probably in IWI Design.
- Where is the weak point (cable head) rating entered?
