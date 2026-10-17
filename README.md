# DC–DC converter efficiency measurement data

This repository contains electrical measurement records from an efficiency-test workbook. The source labels the measured quantities as input voltage (`Vin`), input current (`IIN`), output voltage (`VO`), and output current (`IO`), and calculates efficiency (`EFF`). There are 72 records arranged in six blocks of 12 load-sweep points.

The files are intended to make the numerical data behind the efficiency calculations directly reusable. They do not require access to a paper or a plotted figure to be understood.

The workbook does not explicitly state physical units. The working interpretation is volts for `Vin` and `VO`, amperes for `IIN`, and milliamperes for `IO`, consistent with the recorded calculation. These units require confirmation before public release.

## Repository structure

```text
.
├── data/
│   ├── measurements.csv
│   └── groups.csv
├── results/
│   ├── recomputed.csv
│   ├── group_peaks.csv
│   └── efficiency_preview.svg
├── README.md
```

## Data dictionary

`data/measurements.csv` uses the following columns:

| Column | Meaning | Unit |
| --- | --- | --- |
| `record_id` | Unique identifier derived from the source block and row | None |
| `group_id` | Source block identifier, G1–G6 | None |
| `source_sheet` | Original worksheet name | None |
| `source_row` | Original one-based worksheet row number | None |
| `Vin` | Input voltage, source column A | V |
| `IIN` | Input current, source column B | A |
| `VO` | Output voltage, source column C | V |
| `IO` | Output current, source column D | mA |
| `EFF_cached_ratio` | Cached result of the workbook efficiency formula | Ratio under the working unit interpretation |

The source formula is:

```text
EFF = (VO × IO × 0.001) / (Vin × IIN)
Efficiency (%) = 100 × EFF
```

Under the working unit interpretation, the factor `0.001` converts output current from mA to A. It does not establish the units independently. Efficiency is calculated from the stored inputs, rather than an independent efficiency-meter reading. Whether auxiliary supply power is included is not documented.

All 72 exported records contain the four numeric inputs and a cached efficiency value.

## Measurement coverage

All six groups are stored in `data/measurements.csv`. The ranges below are the recorded numerical values; their physical units follow the provisional interpretation above.

| Group | Source rows | Vin range | VO range | IO range | Records |
| --- | --- | --- | --- | --- | --- |
| G1 | 9–20 | 2.496–2.503 | 2.997–3.019 | 50–600 | 12 |
| G2 | 25–36 | 2.474–2.503 | 5.996–6.003 | 50–600 | 12 |
| G3 | 40–51 | 2.448–2.503 | 9.832–10.004 | 50–600 | 12 |
| G4 | 55–66 | 2.468–2.502 | 11.987–12.087 | 50–600 | 12 |
| G5 | 70–81 | 3.967–4.003 | 9.990–10.004 | 50–600 | 12 |
| G6 | 85–96 | 3.957–4.002 | 11.089–12.000 | 50–600 | 12 |

