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
├── docs/
│   ├── data_dictionary.md
│   ├── measurement_setup.md
│   ├── processing.md
│   └── excluded_rows.csv
├── scripts/
│   ├── reproduce.py
│   └── update_checksums.py
├── results/
│   ├── recomputed.csv
│   ├── group_peaks.csv
│   ├── validation.json
│   └── efficiency_preview.svg
├── LICENSE_PENDING.md
├── metadata.json
├── README.md
├── requirements-plot.txt
└── SHA256SUMS.txt
```

## Data dictionary

`data/measurements.csv` uses the following columns:

| Column | Meaning | Unit |
| --- | --- | --- |
| `record_id` | Unique identifier derived from the source block and row | None |
| `group_id` | Source block identifier, G1–G6 | None |
| `source_sheet` | Original worksheet name | None |
| `source_row` | Original one-based worksheet row number | None |
| `Vin` | Input voltage, source column A | V, pending confirmation |
| `IIN` | Input current, source column B | A, pending confirmation |
| `VO` | Output voltage, source column C | V, pending confirmation |
| `IO` | Output current, source column D | mA, pending confirmation |
| `EFF_cached_ratio` | Cached result of the workbook efficiency formula | Ratio under the working unit interpretation |

The source formula is:

```text
EFF = (VO × IO × 0.001) / (Vin × IIN)
Efficiency (%) = 100 × EFF
```

Under the working unit interpretation, the factor `0.001` converts output current from mA to A. It does not establish the units independently. Efficiency is calculated from the stored inputs, rather than an independent efficiency-meter reading. Whether auxiliary supply power is included is not documented.

All 72 exported records contain the four numeric inputs and a cached efficiency value. Sixteen source rows contain no measurement inputs and have cached division-by-zero errors; they are excluded and listed in `docs/excluded_rows.csv`. Missing inputs must not be interpreted as measured zeros.

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

Group identifiers represent contiguous source blocks. They do not establish nominal settings, separate devices, independent repetitions, or operating modes. The recorded current values, including 151, 451 and 502, are retained without adjustment.

The source workbook does not specify instrumentation, acquisition procedure, ambient temperature, switching frequency, operating mode, auxiliary supply accounting, or uncertainty. These details are therefore not inferred here. Authors should complete `docs/measurement_setup.md` with the actual measurement context before public release.

## Validate the files

Python 3.10 or later is required. Run from the repository root:

```bash
python scripts/reproduce.py
```

The script checks the expected 72 records and six groups, unique record identifiers, finite numeric values, nonzero denominators, and agreement between recomputed efficiency and cached values within an absolute tolerance of `1e-12`. It writes `results/recomputed.csv`, `results/group_peaks.csv`, and `results/validation.json`.

Validation checks arithmetic consistency. It does not independently verify measurement accuracy, physical units, or power-accounting boundaries.

## Plot the efficiency curves

Install the plotting dependency and run:

```bash
python -m pip install -r requirements-plot.txt
python scripts/reproduce.py --plot
```

The script creates `results/efficiency_preview.svg`. It plots 100 times the source formula ratio against `IO`, with one curve per source block. The current unit remains marked as unconfirmed. All 72 valid records are included, with no smoothing, fitting, or outlier removal. Group peaks are maxima among the sampled points, not verified global maxima. No uncertainty intervals are available.

After updating repository contents, refresh the checksums:

```bash
python scripts/update_checksums.py
```

## License

The rights holder has not yet selected a license for this dataset or its accompanying code and documentation. See [LICENSE_PENDING.md](LICENSE_PENDING.md). Before public release, include the selected license text, define its scope, and update this section and `metadata.json`.

## Preservation

For long-term availability, archive the completed release with a DOI provider such as Zenodo and add the DOI here after archival. The public repository URL, archival reference, and maintainer have not yet been supplied.
